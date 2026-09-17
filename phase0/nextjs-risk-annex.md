# Path B Risk Annex — Building on Next.js

Written 2026-09-15. Annex to [04-implementations.md](../04-implementations.md) §5 (Path B) and §4 (Path C).

**Scope:** what you personally own if the storefront is Next.js. Applies in full to **Path B**
(Next.js + Supabase + Razorpay). Items 1, 2, 3 and 5 are removed by **Path C** (Next.js storefront
over Medusa), which is why C ranks above B.

---

## What Next.js genuinely gives you

Credit where due — it is strong on exactly what PRD §9 and §10.1 require:

- `generateMetadata` per product, JSON-LD, Open Graph — clean and fully controlled
- `next/image` with responsive `srcset`, AVIF/WebP, explicit dimensions (kills CLS)
- Real control over the ≤150KB JS budget and the ≤2.5s LCP target — control that neither a Shopify
  theme nor an Odoo storefront gives you

**If the storefront were the whole job, Next.js would win it.** The storefront is roughly 35% of the
work.

---

## The five that actually hurt

### 1. Gapless sequential invoice numbering (PRD §5.5)

The nastiest requirement in the PRD, and the one nobody expects.

**A Postgres sequence will not work.** Sequences are non-transactional by design — a rolled-back
transaction burns its number and leaves a gap. A gap is an audit problem.

So invoice numbers need a counter row with an explicit lock, in the same transaction as the invoice:

```sql
SELECT next_num FROM invoice_counter WHERE fy = '2026-27' FOR UPDATE;
```

**Consequence:** invoice creation is now serialised across the entire store. Every concurrent order
queues behind that lock. Fine at 30 orders/week — but it is a design decision to make consciously,
not to discover under load.

Then handle: restart survival, financial-year rollover, and the case where the number is allocated
but PDF generation fails — which produces the gap anyway unless the allocation is rolled back with it.

### 2. Inventory concurrency on serverless (PRD §3.6)

The requirement: two buyers, one unique saree, **exactly one success**.

The SQL is standard:

```sql
BEGIN;
SELECT stock_qty FROM products WHERE id = $1 FOR UPDATE;
-- verify > 0, decrement, insert order
COMMIT;
```

**The problem is serverless plus connection pooling.** Each function invocation wants a connection;
Postgres runs out. The fix is PgBouncer or Supabase's pooler — and in transaction-pooling mode you
lose prepared statements, and some ORM features break quietly.

Solvable. But "add a row lock" becomes a deployment-architecture decision, and **getting it wrong
fails silently.** The double-sale happens without an error, and you learn about it from a customer.

### 3. Razorpay webhooks (PRD §5.4)

Three specific traps:

- **Raw body.** The signature is HMAC-SHA256 over the raw bytes. Any middleware that parses JSON
  first breaks verification permanently. In App Router: `await req.text()` before touching the body.
- **Idempotency.** Razorpay retries. Requires a `processed_events` table with a unique constraint on
  the event ID, inserted **in the same transaction as the order** — not an `if (exists)` check, which
  races against itself.
- **Event ordering is not guaranteed.** `payment.captured` can arrive before `order.paid`. The state
  machine must be order-independent.

**Plus the timeout.** A webhook handler cannot do payment confirmation + stock decrement + invoice
PDF + email + WhatsApp inside one serverless invocation. That needs a queue — QStash, Inngest, or a
cron-drained table. Another service, another failure mode.

### 4. WhatsApp and SMS — the India tax nobody budgets for

**Bureaucratic delay, not engineering difficulty.** Flag this hardest.

- **SMS requires DLT registration.** TRAI-mandated: register the entity, the sender ID, and every
  message template, each approved individually. **Weeks, not days.** Templates get rejected on
  wording. No transactional SMS can be sent until it clears.
- **WhatsApp Business API requires a BSP** — Gupshup, Interakt, Twilio. Facebook Business
  verification, per-template approval, per-message pricing.

PRD §4.5 defines six notification triggers. Each is a template needing approval.

⚠️ **This is identical on every platform — Shopify does not exempt you from DLT.** The difference is
that on a platform it is an app install, and here it is a BSP API integration. **Start it now
regardless of the stack decision.** It is a genuine critical-path item hiding in plain sight, and it
is not currently a Phase 0 task. It should be.

### 5. The admin panel is a second application

PRD §7.2 lists nine operator tasks. PRD §7.3 requires it to be **usable on a phone**, with
server-side image resizing.

That means building: auth with 2FA, session timeout, role separation, an audit log of price/stock/
refund changes, rate limiting (which on serverless needs an external store such as Upstash Redis), a
mobile-first product form with ~20 fields, multi-image upload with progress, and order management.

**Realistically as much work as the storefront.** Path B is routinely estimated by imagining only
the shop pages.

---

## Integration reality

| Integration | Difficulty | The gotcha |
|---|---|---|
| Razorpay checkout | Easy | — |
| **Razorpay webhooks** | **Hard** | Raw body, idempotency, event ordering |
| Razorpay refunds | Medium | Partial refunds, status polling |
| **GST invoice PDF** | **Hard** | Puppeteer/Chromium exceeds serverless bundle limits — needs `@sparticuz/chromium` or an external render service |
| Place-of-supply tax split | Medium | **Round the total, then split. Never round each half** — CGST + SGST must sum exactly to the total tax |
| Shiprocket / Delhivery | Medium | AWB generation, pickup scheduling, tracking webhooks |
| **DLT + SMS** | **Slow, not hard** | Weeks of approvals |
| **WhatsApp BSP** | **Slow, not hard** | Template approval per trigger |
| Image storage | Medium | 300 SKUs × 6 = 1800 images. Signed uploads from a phone. **iPhones send HEIC** — convert server-side |
| On-site search | Easy | Postgres full-text is genuinely fine at this catalog size |
| Email | Easy | Resend or SES |

---

## The Next.js-specific trap: ISR versus stock accuracy

You will statically generate product pages to hit the 2.5s LCP target. Then a unique saree sells —
and its page still says "Available" until revalidation.

For a one-of-a-kind piece that means a buyer taps Buy on a sold saree. PRD §1.2 calls double-selling
"the fastest way to lose a customer permanently."

Options: on-demand revalidation triggered by every stock change · render stock client-side · or
`dynamic = 'force-dynamic'` on product pages and give up the static win.

**Pick deliberately at design time.** This is the classic Next.js commerce tension and it is not
obvious until it bites.

### Two related items

- **Region pinning.** Put functions and Postgres in the same region (Mumbai / `bom1`). Defaults mean
  every DB call crosses an ocean, twice, per request.
- **WhatsApp's link crawler is fussy** — absolute URLs only, image under ~300KB, does not reliably
  follow redirects. WhatsApp link-sharing is the primary distribution channel (PRD §4.1), so test the
  preview by pasting a real link into a real chat, not by trusting a validator.

---

## Timeline

04-implementations.md estimates **10–14 weeks for Path B**, and that assumes no surprises. Against
Shopify's 2–3 weeks — during all of which sales continue flowing into a personal account.

---

## Verdict

**Path C (Next.js over Medusa)** removes items 1, 2, 3 and 5 — Medusa supplies the commerce engine
and an admin, and the storefront control is retained. That is why C ranks second, not third.

**Path B (fully custom)** means owning every item above, forever, with a bus factor of 1.

Nothing here says it cannot be done. The PRD's question is whether the risk of getting invoice
numbering or stock locking subtly wrong — both of which fail silently and cost money and trust — is
worth the flexibility gained, for a business that loses a customer permanently when one saree sells
twice.

**If learning to build this is itself a goal, that changes the answer and it is a legitimate reason.**
04-implementations.md §6.1 already allows it: Path B "only if learning is explicitly ranked above
launching, with eyes open." It is simply a different project than shipping a store this quarter.

---

## Action item this annex generates

- [ ] **Add DLT registration and WhatsApp BSP onboarding to Phase 0 Track L.** They are
      stack-independent, take weeks of third-party approval, and are currently untracked.
