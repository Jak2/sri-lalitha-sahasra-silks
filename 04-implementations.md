# Implementations — How To Build It

**Companion to:** [03-PRD.md](03-PRD.md) (what to build and why) · [05-Changelog.md](05-Changelog.md) (decision log)
**Status:** Stack decision OPEN. Phase 0 can start immediately regardless.
**Last updated:** 2026-09-11

> ⚠️ **Every price, fee, rate, and regulatory figure in this document must be verified against the
> provider's current published pricing before you commit.** Indian platform pricing, gateway fees,
> and GST rules all change. Figures here are *directionally correct as understood at time of
> writing* and are for comparison between options, not for budgeting.

---

## 0. How To Use This Document

```
Phase 0  (start NOW, stack-independent)  ──┐
                                            ├──> Decision Gate ──> Phase 1..4 of the chosen path
Path A / B / C evaluation (this doc)     ──┘
```

Phase 0 is business plumbing. It takes 1–3 weeks of **waiting on other people** (bank, CA, gateway
KYC), which is longer than any of the three builds takes to start. Starting Phase 0 before the
stack is chosen costs nothing and saves the entire critical path.

**Do not read paths A/B/C first. Read §1 and start it today.**

---

## 1. PHASE 0 — Business Prerequisites (stack-independent, critical path)

Nothing in this phase depends on which website is built. All of it blocks launch.

### 1.1 Task Board

| # | Task | Depends on | Realistic elapsed time | Who |
|---|---|---|---|---|
| P0-1 | Engage a CA (PRD Q9) | — | 1–3 days | Owner |
| P0-2 | Decide legal structure: proprietorship vs. private limited (Q2) | P0-1 | 1 week | Owner + CA |
| P0-3 | Resolve GST question (Q1): required or not, and at what rate per product category | P0-1 | 1–2 weeks | CA |
| P0-4 | Register GST if required | P0-3 | 1–3 weeks after filing | CA |
| P0-5 | Open a **business current account** (Q3) | P0-2, often P0-4 | 1–2 weeks | Owner |
| P0-6 | Choose and register a domain (Q8) | brand name decided | 1 day | Jaya |
| P0-7 | Payment gateway account + KYC | P0-5, legal pages live | 3–10 days | Jaya |
| P0-8 | Courier / aggregator account | P0-5 | 2–5 days | Jaya |
| P0-9 | Write the 5 legal pages (PRD §8.3) | P0-2, P0-6, policy decisions | 2–3 days | Jaya, CA reviews |
| P0-10 | Confirm return policy (Q6) | — | 1 day | Owner |
| P0-11 | Audit photo coverage against PRD §3.3 across the real catalog (Q5) | — | 1–2 days | Jaya |
| P0-12 | Build the catalog spreadsheet (every PRD §3.2 field, every product) | P0-3 for HSN/GST | 2–5 days | Jaya + Owner |
| P0-13 | Weigh a sample of sarees, packed, to set the flat shipping rate (PRD §6.1) | — | 1 hour | Owner |
| P0-14 | Stop taking new sales into the personal account; migrate to the current account | P0-5 | — | Owner |
| **P0-15** | **DLT registration (TRAI): entity, sender ID, and every SMS template** | P0-2 | **2–4 weeks, mostly waiting** | Jaya + Owner |
| **P0-16** | **WhatsApp Business API via a BSP: Facebook Business verification + template approval** | P0-2, P0-6 | **1–3 weeks** | Jaya |

> **P0-15 and P0-16 added 2026-09-15.** Discovered while assessing Path B, but they are
> **stack-independent** — Shopify does not exempt you from DLT. PRD §4.5 defines six notification
> triggers; each is a template requiring individual approval, and templates get rejected on wording.
> No transactional SMS can be sent until DLT clears. This is bureaucratic delay, not engineering
> difficulty, and it is the same shape as the CA dependency: start it now, it cannot be compressed
> later.

### 1.2 Critical Path Through Phase 0

```
CA (P0-1) → GST position (P0-3) → [GST registration (P0-4)] → current account (P0-5)
         → gateway KYC (P0-7) → PAYMENT POSSIBLE

parallel, also weeks of external waiting:
legal structure (P0-2) → DLT registration (P0-15) → TRANSACTIONAL SMS POSSIBLE
                      → WhatsApp BSP (P0-16)      → ORDER UPDATES ON WHATSAPP POSSIBLE
```
**Worst case ≈ 6 weeks. Best case ≈ 2 weeks.** This, not the code, is why the site is not live.

⚠️ **A store can launch before P0-15/P0-16 complete** by falling back to email plus manual WhatsApp
messages — but PRD §4.5's automated post-purchase messaging, and therefore the ≤5-minutes-per-order
target, does not hold until they do.

### 1.3 P0-12 — The Catalog Spreadsheet

This is the highest-leverage piece of unglamorous work in the project. It is also the one artefact
that is **100% portable across all three paths** — every path imports from it.

One row per saree, columns exactly matching PRD §3.2:

```
sku | title | price_inr | mrp_inr | stock_qty | is_unique | fabric | weave_technique |
primary_color | secondary_colors | saree_length_m | blouse_piece | care_instructions |
weight_grams | hsn_code | gst_rate | occasion_tags | description | image_folder | status
```

Rules:
- `sku` is generated once and never changes. It is the link between a physical saree and every
  digital record forever.
- `image_folder` points to a folder per SKU containing the §3.3 image set, named
  `01-hero.jpg`, `02-drape.jpg`, `03-pallu.jpg`, `04-border.jpg`, `05-fabric.jpg`, `06-blouse.jpg`.
  Consistent naming is what makes bulk import possible on any of the three paths.
- `weight_grams` is measured **packed**, not bare.
- Do not start this in the website admin. Do it in a spreadsheet, where two people can work at
  once and mistakes are cheap.

### 1.4 Image Pipeline (also stack-independent)

Professional photos exist, so the work is normalisation:

1. Cull to the §3.3 set per SKU.
2. Crop every hero to the same aspect ratio (recommended 4:5).
3. Colour-correct for consistency across the catalog — a grid where every saree has a different
   white balance reads as amateur.
4. Export masters at ≥ 2000px long edge, quality ~85.
5. Write alt text per image into the spreadsheet.

Steps 1–4 are one batch operation in any image tool. Doing it once, before import, avoids doing it
three hundred times inside an admin panel later.

---

## 2. Decision Framework

Score the three paths against what this business actually needs.

| Criterion | Weight | Why it matters here |
|---|---|---|
| Time to first rupee | **High** | Sales are happening now; the site is replacing a working channel |
| Non-technical operability | **High** | PRD §7.1 is a hard constraint |
| Correctness of money and inventory | **High** | PRD §3.6, §5.4 — mistakes here cost trust permanently |
| Total cost of ownership (3 yr) | Medium | Includes Jaya's time, which is not free |
| Feature ceiling | Medium | "Features must not be a problem" per owner |
| Control / no lock-in | Low–Medium | Matters for exit, not for launch |
| Learning value for Jaya | Medium | Explicit secondary goal |

**Important framing:** Paths B and C require *building* what Path A *rents*. The build is not the
expensive part; the **maintaining** is. Every one of: payment webhook reliability, inventory
race-conditions, invoice sequence integrity, admin authentication, image pipeline, backups,
security patching, and the on-call for all of it, becomes Jaya's permanent responsibility in B and
C. That is the real price, and it is paid monthly forever.

---

## 3. PATH A — Shopify (hosted commerce)

### 3.1 What It Is

Rent the entire commerce engine. Build only the storefront look. Shopify owns products, cart,
checkout, orders, inventory, tax, discounts, admin, hosting, PCI scope, and uptime.

### 3.2 Architecture

```
Buyer ──> Shopify CDN ──> Theme (Liquid + CSS + minimal JS)
                              │
                              ├── Shopify Admin ─────> Products, Orders, Inventory, Discounts
                              ├── Payment gateway app (Razorpay / PayU / PhonePe / Cashfree)
                              ├── Shipping app (Shiprocket / Delhivery / Nimbuspost)
                              ├── GST invoicing app
                              └── WhatsApp notification app
```

Everything above the storefront theme is configuration, not code.

### 3.3 Cost Structure — VERIFY CURRENT PRICING

| Item | Nature |
|---|---|
| Shopify Basic subscription | Monthly, per Shopify's current India pricing |
| **Shopify third-party-gateway transaction fee** | **A percentage of every order, charged by Shopify, on top of the gateway's own fee** — see §3.4 |
| Payment gateway fee | Gateway's own percentage per transaction (differs by method; UPI is typically cheapest) |
| Apps (invoicing, WhatsApp, shipping) | Monthly, per app; several have free tiers |
| Domain | Annual |
| Theme | Free themes exist and are good; paid themes are a one-time cost |

### 3.4 The Central Trade-off — Read This Carefully

**Shopify Payments does not operate in India.** Therefore an Indian merchant must use a
third-party gateway, and Shopify levies an **additional transaction fee** on every order for that
privilege — on top of the gateway's own fee. Effective total take rate per order is meaningfully
higher than in markets where Shopify Payments exists.

Consequence to model before committing:

```
per-order platform cost ≈ (Shopify third-party fee %) + (gateway fee %)
monthly cost ≈ subscription + apps + (per-order cost × monthly GMV)
```

Run this against realistic GMV. At low volume the subscription dominates and Path A is cheap in
absolute terms. At high volume the percentage dominates, and that is the point where migrating to
a headless storefront or to Path C starts to pay for itself.

**Verify Shopify's current India third-party gateway fee and each plan's rate before deciding.**
It is the single number that decides A-vs-C economics.

### 3.5 Build Sequence

| Phase | Work | Effort |
|---|---|---|
| A1 | Shopify trial, store settings, domain connect, INR/India setup | 1 day |
| A2 | Theme selection; customise colours, type, spacing per PRD §10 | 2–4 days |
| A3 | Product metafields for PRD §3.2 fields Shopify lacks natively (weave, length, blouse piece, care, is_unique) | 1 day |
| A4 | Bulk-import the catalog spreadsheet (P0-12) via CSV + images | 1–2 days |
| A5 | Collections, filters, navigation per PRD §3.4 | 1 day |
| A6 | Payment gateway app install + live-mode test | 1–2 days (gated by P0-7) |
| A7 | Shipping app, rate config, pickup setup | 1 day |
| A8 | GST invoicing app, HSN mapping, invoice template | 1–2 days |
| A9 | WhatsApp/SMS notification app, message templates per PRD §4.5 | 1 day |
| A10 | Legal pages, About page, trust elements per PRD §8 | 1–2 days |
| A11 | SEO: titles, meta, OG tags, structured data (mostly theme-provided), sitemap, Search Console | 1 day |
| A12 | Full acceptance-criteria pass (PRD §14) | 2 days |

**Realistic total: 2–3 weeks elapsed**, most of it waiting on Phase 0.

### 3.6 How Path A Satisfies the Hard Requirements

| PRD requirement | Path A |
|---|---|
| §3.6 no double-sale on unique items | Shopify's inventory system handles concurrent checkout reservation. **Still test it explicitly** (PRD §14). |
| §5.4 webhook-driven order state | Handled internally by Shopify. Not your code, not your bug. |
| §5.4 no card data on our servers | Guaranteed — checkout is Shopify's/gateway's. PCI scope effectively nil. |
| §5.5 sequential invoice numbers | Provided by the invoicing app. **Verify the app does sequential-without-gaps** before relying on it. |
| §7 non-technical admin | Shopify admin is the benchmark for this. Mobile app included. |
| §10.1 performance | Achievable with a lean theme; Shopify's CDN and image service do the heavy lifting. A bloated theme with ten apps will miss the budget — **app count is the performance risk**. |
| §7.4 data export | Products/orders/customers export as CSV. Theme code is yours. |

### 3.7 Maintenance Cost

Low. Shopify patches, scales, and keeps checkout PCI-compliant. Ongoing work is app upgrades and
occasional theme tweaks. **Approximately zero on-call burden.** This is what the percentage buys.

### 3.8 Failure Modes

- App sprawl degrades performance and adds monthly cost. Cap it: install nothing that is not on
  the PRD.
- The invoicing app is the weak link for GST correctness; it is a third party, not Shopify.
- Theme customisation has a ceiling. Genuinely unusual layouts require fighting Liquid.
- Platform dependency: pricing and policy are Shopify's to change.

### 3.9 Exit Cost

Moderate. Product, customer, and order data export cleanly. The theme does not port. Re-platforming
means rebuilding the storefront but not the data. **Shopify is also the natural base for a headless
upgrade** — keep the Shopify backend, replace the storefront with a custom front-end, without a
data migration. That makes A a reversible decision, which is its strongest argument.

---

## 4. PATH C — Custom storefront + Medusa (self-hosted commerce engine)

Presented before B because it strictly dominates B: same control, far less of the dangerous work.

### 4.1 What It Is

A real open-source commerce engine (carts, orders, inventory, tax, promotions, refunds, admin)
running on your own server, with a custom front-end you fully control.

### 4.2 Architecture

```
Buyer ──> Next.js storefront (Vercel or the same VPS)
              │  Storefront API
              ▼
         Medusa backend  ──> PostgreSQL   (products, orders, inventory)
              │            └> Redis       (sessions, jobs, events)
              ├── Razorpay plugin ──> gateway webhooks
              ├── Shiprocket / courier integration
              ├── S3-compatible object storage ──> product images
              └── Medusa Admin (React) ──> operator UI
```

Infrastructure you now own: VPS, Postgres, Redis, object storage, TLS certificates, backups,
monitoring, OS patching, deploys.

### 4.3 Cost Structure

| Item | Nature |
|---|---|
| VPS (4GB+ RAM — Medusa + Postgres + Redis need headroom) | Monthly, low |
| Object storage + CDN for images | Monthly, usage-based, low |
| Storefront hosting | Free tier is usually sufficient |
| Payment gateway fee | Gateway's percentage only — **no platform cut** |
| Domain | Annual |
| **Jaya's time** | The actual dominant cost |

Cash cost is materially lower than Path A. Time cost is materially higher. Whether that is a good
trade depends entirely on the value of Jaya's hours and on GMV.

### 4.4 Build Sequence

| Phase | Work | Effort |
|---|---|---|
| C1 | VPS provisioning, Docker, Postgres, Redis, TLS, firewall, backup job | 2–3 days |
| C2 | Medusa backend install, region/currency/tax config for India | 2 days |
| C3 | Product model extension for PRD §3.2 fields not built in | 2 days |
| C4 | Catalog import script from the P0-12 spreadsheet + image upload | 2–3 days |
| C5 | Next.js storefront: home, collection, product, cart, checkout | 1.5–2 weeks |
| C6 | Razorpay integration: order creation, **signature verification, idempotent webhook** | 3–5 days ⚠️ |
| C7 | Inventory concurrency guarantee for unique items (PRD §3.6) + a test that proves it | 2 days ⚠️ |
| C8 | GST invoice generation: per-item HSN, place-of-supply split, **gapless sequential numbering** | 3–5 days ⚠️ |
| C9 | Shipping integration or a manual label workflow | 2–3 days |
| C10 | WhatsApp/SMS notifications per PRD §4.5 | 2 days |
| C11 | Admin hardening: 2FA, roles, audit log, session policy (PRD §7.3) | 2 days ⚠️ |
| C12 | Legal pages, About, trust elements | 1–2 days |
| C13 | SEO, structured data, OG tags, sitemap | 2 days |
| C14 | Performance pass to hit PRD §10.1 budgets | 2 days |
| C15 | Backup restore drill, monitoring, alerting | 1–2 days |
| C16 | Full acceptance pass (PRD §14) | 3 days |

**Realistic total: 6–9 weeks** of focused work, plus Phase 0 in parallel.

⚠️ marks the phases where a bug costs real money or real trust. C6, C7, C8, C11 are where careful
work is mandatory and where a framework does not save you.

### 4.5 How Path C Satisfies the Hard Requirements

| PRD requirement | Path C |
|---|---|
| §3.6 no double-sale | **You must implement and prove this** — database-level locking or reservation at payment confirmation. C7 exists for this alone. |
| §5.4 webhook-driven state | You write it. Signature verification and idempotency are yours to get right. |
| §5.4 no card data on our servers | Satisfied if using the gateway's hosted/SDK checkout. **Never build a card form.** |
| §5.5 sequential invoicing | You implement gapless numbering, including across restarts and concurrent orders. Non-trivial. |
| §7 non-technical admin | Medusa Admin exists and is decent, but it is developer-flavoured. Expect to train the operator, and expect mobile use to be worse than Shopify's. **Test this against PRD §14's "operator lists a product unaided from a phone" criterion before committing to C.** |
| §10.1 performance | Best ceiling of the three — full control of the front-end. |
| §7.4 data export | Total. It is your Postgres. |

### 4.6 Maintenance Cost

**Real and permanent.** Security patches for OS, Node, Postgres, Medusa and its dependencies.
Backup verification. Monitoring response. Medusa major-version upgrades are not always trivial.
Certificate renewal automation. When checkout breaks at 11pm during a festival sale, it is Jaya's
phone that rings and nobody else's.

Budget **4–8 hours/month steady state**, with spikes.

### 4.7 Failure Modes

- Self-hosted Postgres with an unverified backup. The classic, and it is fatal.
- The webhook handler is not idempotent → duplicate orders or double stock decrement.
- Invoice numbering gap discovered during an audit.
- Admin credentials weak or shared → direct path to the money.
- Jaya becomes unavailable; nobody else can operate or fix it.

### 4.8 Exit Cost

Lowest. Everything is yours. But "exit" is rarely the problem — carrying it is.

---

## 5. PATH B — Fully custom (Next.js + Supabase + Razorpay)

### 5.1 What It Is

Build the commerce engine as well as the storefront. Products, cart, orders, inventory, tax,
discounts, admin — all written from scratch on a database.

### 5.2 Architecture

```
Next.js (storefront + admin + API routes)
   ├── Supabase Postgres ──> products, variants, orders, order_items, customers, invoices
   ├── Supabase Auth     ──> admin accounts
   ├── Supabase Storage  ──> product images
   ├── Razorpay          ──> checkout + webhooks
   └── WhatsApp/SMS provider
```

### 5.3 Honest Assessment

📄 **Full engineering detail: [phase0/nextjs-risk-annex.md](phase0/nextjs-risk-annex.md)** — the
specific failure modes, added 2026-09-15. Headlines, because each is the kind of thing discovered at
week nine rather than week one:

- **Gapless invoice numbering cannot use a Postgres sequence** — sequences are non-transactional, so
  a rolled-back transaction leaves a gap. It needs a counter row locked inside the invoice
  transaction, which serialises invoice creation across the whole store.
- **Row-locking inventory collides with serverless connection pooling.** In transaction-pooling mode
  you lose prepared statements and some ORM features break quietly. Getting this wrong **fails
  silently** — the double sale happens without an error.
- **Razorpay webhooks:** signature is HMAC over the *raw* body (any JSON middleware breaks it
  permanently), retries demand a uniquely-constrained `processed_events` insert in the same
  transaction, and event ordering is not guaranteed.
- **Invoice PDF generation exceeds serverless bundle limits** — needs `@sparticuz/chromium` or an
  external render service.
- **ISR versus stock accuracy** — statically generated product pages show a sold unique saree as
  available until revalidation. Must be decided at design time.

Path B builds, by hand, everything Path C gets for free from Medusa: cart semantics, order state
machine, inventory reservation, tax calculation, discount rules, refund flows, and a complete
admin UI. That admin UI alone — meeting PRD §7.2's nine tasks plus §7.3's security requirements,
usable on a phone by a non-technical person — is **2–3 weeks on its own**, and it is the least
interesting code in the project.

**Estimated total: 10–14 weeks.** Maintenance burden is Path C's plus all the commerce logic.

### 5.4 When Path B Is Actually Correct

Only if **learning to build commerce from first principles is a primary goal**, ranked above
launching quickly. As a business decision for a 50–300 SKU saree store with sales happening today,
it is the wrong build — rebuilding solved infrastructure while the existing sales channel keeps
running on a personal UPI account.

### 5.5 If Path B Is Chosen Anyway

Non-negotiables, because these are where custom builds lose real money:

- Money as integer paise. Never floats. Anywhere.
- Order state machine explicit and enumerated: `pending → paid → packed → shipped → delivered`,
  with `payment_failed`, `cancelled`, `refunded` as terminal branches. No boolean soup.
- Stock decrement inside a database transaction with `SELECT ... FOR UPDATE` on the product row.
  A test must prove two concurrent purchases of a unique item yield exactly one success.
- Webhook: verify signature, then look up by gateway payment id and no-op if already processed.
  Store the raw payload for reconciliation.
- Invoice numbers from a dedicated database sequence, never from a count of rows.
- Admin routes behind real authentication with 2FA and an audit log, rate-limited.
- Automated backups with a **tested** restore.

---

## 6. Side-by-Side

| | **A — Shopify** | **C — Medusa** | **B — Fully custom** |
|---|---|---|---|
| Time to launch | **2–3 weeks** | 6–9 weeks | 10–14 weeks |
| Monthly cash cost | Subscription + apps | VPS + storage (lower) | VPS/Supabase (lowest) |
| Per-order platform cut | **Shopify fee + gateway fee** | Gateway fee only | Gateway fee only |
| Non-technical admin | **Best in class** | Adequate, needs training | You build it |
| Money/inventory correctness | **Vendor's problem** | Your problem, engine helps | Entirely your problem |
| PCI scope | **None** | Minimal (hosted checkout) | Minimal (hosted checkout) |
| Design ceiling | Good, with limits | **Unlimited** | **Unlimited** |
| Feature ceiling | Very high (app ecosystem) | High, you build the gaps | You build everything |
| Maintenance hours/month | ~0 | 4–8 | 6–12 |
| On-call burden | None | Jaya | Jaya |
| Lock-in | Moderate (data exports) | None | None |
| Learning value | Low | **High** | **Highest** |
| Bus factor | Safe | Jaya only | Jaya only |

### 6.1 The Recommendation

> **Two separate recommendations, often confused. Clarified 2026-09-15.**
>
> **(a) Sequence — when to build.** Not yet. Fix the payment rail and the invoice trail first, at
> near-zero cost (see [phase0/zero-budget-plan.md](phase0/zero-budget-plan.md)). Of the five problems
> in PRD §1.2, the two most dangerous — business money in a personal account, and no invoice trail —
> **do not need a website.** Building first would mean solving problem #3 while problem #1 keeps
> running.
>
> **(b) Stack — what to build, when the volume justifies it.** Path A, below.
>
> These are decided at different times, on different evidence. (a) is decided now. (b) is decided at
> the gate in §7.

**Path A**, for this business, at this moment.

Reason: sales are already happening on a personal UPI account. Every week without a proper
merchant setup is a week of accumulating risk. Path A converts that risk to zero in two to three
weeks, hands a non-technical operator a tool they can actually use, and remains reversible — the
data exports, and a headless storefront can be layered on later without a migration.

Trade-off, stated plainly: a percentage of every order, forever, and a ceiling on design
weirdness.

**Path C is the correct second choice**, and becomes the correct first choice if either (a) the
percentage take rate is unacceptable at projected GMV, or (b) building the thing is itself a
primary goal.

**Path B is not recommended for this business.** Keep it as a learning project on a separate,
non-revenue codebase if the itch needs scratching.

#### The argument in one line

**You are not paying Shopify for features. You are paying them to not be on call.**

WooCommerce and Path C cost less cash and more of you — updates, backups, security, and the call when
checkout breaks during a festival sale. Path B costs 10–14 weeks before the first sale, then that
burden plus everything built by hand. **And with a bus factor of 1, an outage has no end date:** if
Jaya is travelling, ill, or busy, the business stops. A hosted platform's outage is the vendor's
problem.

#### What would reverse it

| Reverses to | When |
|---|---|
| **Path C (Medusa)** | Sales exceed ~₹8L/month — the 2% ≈ ₹1.9L/year and self-hosting earns its maintenance |
| **Path C** | A second technical person exists who could operate and repair it |
| **Path E (Odoo)** | A physical shop or POS should share one system with the website |
| **Path B** | Learning to build commerce is explicitly ranked above launching — a legitimate goal, but a different project |

**Stated honestly:** "Path A" currently means *"Path A, assuming monthly GMV of roughly ₹1–4 lakh"* —
**a range that was assumed, not measured.** PRD Q10 closes it. Until then this is an educated
judgement, not arithmetic.

### 6.2 Evidence Added 2026-09-15

Three findings, none of which change the recommendation — two strengthen it, one qualifies it. Full
detail in [phase0/competitor-research.md](phase0/competitor-research.md) and
[phase0/stack-research.md](phase0/stack-research.md).

**Strengthens A — the operator argument is not theoretical.** Suta, a saree brand at $4M+ revenue
supporting ~14,000 weavers, runs on Shopify, and its founders' stated reason is that they "don't even
need a big team to maintain" it. A brand at many multiples of our projected revenue chose not to own
the platform. Our bus factor is 1.

**Strengthens A — the most common headless regret is exactly our forbidden failure mode.** Merchants
who went headless most often report that "every content change is a developer deploy." That is the
Changelog decision of 2026-09-11 almost word for word. Path B and a headless Path C both land there
unless the admin is built to the same standard as the storefront — which is §5.3's second-application
problem.

**Qualifies A — the fee is now a known number, and it scales.** 2% on Basic:

| Monthly GMV | Shopify's extra 2% |
|---|---|
| ₹1,00,000 | ₹2,000/mo |
| ₹3,60,000 | ₹7,200/mo |
| ₹10,00,000 | ₹20,000/mo |

Cheap insurance now, expensive later — which is precisely why the recommendation was recorded as
reversible with a Phase 4 re-evaluation. **The migration trigger should be a GMV number, decided in
advance**, not a feeling that the fee has grown annoying.

**Also noted:** the Indian D2C checkout-tooling industry (GoKwik, Razorpay Magic Checkout, Shiprocket
Checkout) exists largely to manage COD and RTO. The size of that industry is supporting evidence for
the no-COD decision — and a warning that introducing COD in Phase 3 means adopting an RTO-scoring
vendor, not ticking a box.

---

## 7. Decision Gate

### 7.1 Deadline

**Choose by the completion of P0-5 (business current account open).** Gateway selection differs
per path and gateway KYC is on the critical path; the stack decision cannot lag behind it.

### 7.2 Inputs Required Before Deciding

- [x] ~~Shopify's **current** India pricing and third-party gateway fee~~ — **researched 2026-09-15:
      Basic 2.0%, Grow 1.0%, Advanced 0.6%, on top of the gateway's own fee. Basic→Grow crossover at
      ~₹4.1L monthly GMV.** See [phase0/stack-research.md](phase0/stack-research.md). **Re-read
      Shopify's own page on the day of the decision** — this is research, not a quote.
- [ ] Razorpay's (or chosen gateway's) **current** per-method fees — the 2026-09-15 research found
      ~2.36% effective (Razorpay) and ~2.30% (Cashfree), **but from provider blogs, not pricing
      pages.** Still unverified.
- [ ] Realistic monthly GMV estimate, from the last 3 months of WhatsApp sales
- [ ] Actual product count and photo coverage (P0-11)
- [ ] Confirmed answer on how many hours/week Jaya can sustain **indefinitely**, not just during
      the build
- [ ] Whether anyone other than Jaya could operate or repair the system
- [ ] **Hands-on trial: list one saree with 6 photos from a phone**, on Shopify, Zoho Commerce and
      Odoo. PRD §7.3 is a hard constraint and this test settles it in an hour per platform.
- [ ] **Which system the CA would rather receive records in** — they are the one who files

### 7.2a Paths D and E — added 2026-09-15

Research surfaced two hosted options that beat Shopify on native Indian GST and charge **no platform
percentage**. They are not yet written up to the depth of A/B/C. Neither displaces the recommendation
in §6.1, but both belong at the gate.

| | **D — Zoho Commerce** | **E — Odoo (Website + eCommerce)** |
|---|---|---|
| GST invoicing | Native **via Zoho Books** (separate product; reportedly free under ₹25L turnover) | **Native** via `l10n_in` — GST reports, e-invoicing, e-way bills |
| Platform % per order | **None** | **None** |
| Subscription | 🟡 unverified; partners cite ₹599–1,799/mo tiers | ~$31.10/user/mo after promo ≈ ₹2,580/user/mo, all apps |
| Mobile admin | 🔴 **No dedicated Commerce app — browser only.** Collides with PRD §7.3 | Real app, full-featured, dense UI |
| Unique-stock concurrency | Designed against oversell; less publicly documented | Single stock pool, reservation at confirmation |
| Storefront performance | Untested against PRD §10.1 | 🔴 Heavy asset bundles; ≤2.5s LCP is a genuine fight |
| Ecosystem in India | Thin for D2C fashion | Strong partner network — **helps the bus-factor problem** |
| Right when | Cost matters and the operator will use a laptop | A physical shop, POS, or weaver purchase orders share the system |

⚠️ **Open question on Odoo:** whether the full Indian GST filing and e-invoicing features require
Enterprise. The source claiming so was a partner blog, and Odoo Online's paid plans are already
Enterprise-tier. **Ask Odoo directly before this influences the decision.**

**Notable absence:** almost nobody writes about running Odoo or Zoho Commerce for Indian D2C fashion.
That is itself a signal about how much independent help exists when something breaks —
see [phase0/competitor-research.md](phase0/competitor-research.md).

### 7.2b PATH F — Odoo as back office + Next.js storefront — added 2026-09-16

**The idea, as asked:** run Odoo headless for inventory, invoicing and accounting; build the
customer-facing store in Next.js; connect the two over Odoo's XML-RPC / JSON-RPC API.

It is a real architecture, used in the wild, and it is the wrong shape for this project today.
Recorded here so it does not get re-proposed.

**What it actually splits:** Odoo replaces **Zoho Books**, not Shopify. The Next.js half remains
**Path B in full**, with every risk in [phase0/nextjs-risk-annex.md](phase0/nextjs-risk-annex.md)
intact.

#### Why it does not hold

| # | Objection | Detail |
|---|---|---|
| 1 | **Replaces something already free** | Zoho Books Free already does GST invoices, e-invoicing and GSTR-1/3B filing at ₹0 under ₹25L turnover, as a registered GSP. Odoo Online's free tier is **one app**; Invoicing + Inventory + Sales is paid — 🟡 ~₹31,000/user/year. Community is free but self-hosted, and the Enterprise question in §7.2a is still open. |
| 2 | **The hard half is untouched** | Odoo does not build the checkout. Razorpay webhook signature over the raw body, idempotency, out-of-order events, the unique-saree row lock, ISR-vs-live-stock, cart, address, auth, paise arithmetic, GST rounding — all still ours. Odoo removes admin screens, and admin screens were never the risky part. |
| 3 | **Two systems, one truth** | No transaction spans the Next.js database and Odoo. An order must land in both. Odoo slow or down at 11pm means checkout fails, or money is taken with no order recorded. That buys a reconciliation job nobody planned. |
| 4 | **Chatty transport** | XML-RPC per page render is slow, so a cache layer appears, so stock goes stale — which is objection 2 again, now with an extra hop. |
| 5 | **Paying for an unused module** | Odoo ships its own website + eCommerce. Front it with Next.js and you either pay for that module unused, or use Odoo's storefront and do not need Next.js. |

#### Cost of each shape at this scale

| | Cash/year | Build time | Ongoing maintenance |
|---|---|---|---|
| Zoho Books Free + Shopify (Path A) | 🟡 ~₹18,000 + 2% of GMV | ~2 weeks | ~0 |
| Zoho Books Free + Next.js (Path B) | ~₹0 | 10–14 weeks | 6–12 hrs/month |
| **Odoo + Next.js (Path F)** | 🟡 **~₹31,000+** | **12–16 weeks** | **8–15 hrs/month** |
| Odoo alone (Path E) | 🟡 ~₹31,000+ | ~4 weeks | 4–8 hrs/month |

> **Path F is the only combination that loses on both axes** — most expensive in cash *and* most
> expensive in time. Path E gets Odoo's benefits for less of both.

#### What would make Path F correct

Not never. It becomes the right architecture when:

- a physical shop or POS shares the same inventory, **or**
- multiple warehouses, or wholesale and retail price lists coexist, **or**
- purchase orders to weavers, manufacturing or landed-cost tracking are in scope, **or**
- monthly GMV is high enough that Shopify's 2% exceeds Odoo's licence — 🟡 roughly **₹13 lakh/month**

None is true today. All are plausible in three years. **Shelf it, do not delete it.**

#### The pattern worth noticing

Each "doesn't this solve it?" proposal so far adds a system. The two highest-severity problems need
*fewer*: a current account and a Razorpay Payment Link fix the personal-account risk; Zoho Books Free
fixes the invoice trail. Only the third — ~20 minutes per order — needs a website at all, and Path F
reaches it slower than Path A. See [README.md](README.md#the-recommendation-in-plain-words).

### 7.3 Decision Rule

```
IF monthly GMV × (Shopify fee % − 0) > value of 6 additional weeks + 6 hrs/month forever
   AND Jaya can commit to indefinite maintenance
   AND a second person can operate it
THEN Path C
ELSE Path A
```

Path B only if learning is explicitly ranked above launching, with eyes open.

### 7.4 Record the Decision

When chosen, append to [05-Changelog.md](05-Changelog.md): the date, the choice, the numbers that drove
it, what was rejected, and **what would make you reverse it**. That last item is what stops a
decision from being re-litigated every month.

---

## 8. Post-Launch Roadmap (path-independent)

| Phase | When | Contents |
|---|---|---|
| **Phase 2** | Weeks 4–10 post-launch | Abandoned-cart recovery · Instagram/Meta catalog sync · Discount codes and festival campaigns · Back-in-stock notifications · Reviews with photos · Google Search Console remediation |
| **Phase 3** | Months 3–6 | Content: care guides, fabric explainers, occasion styling (PRD §9.2) · Google Shopping feed · Email list and a monthly new-arrivals send · COD pilot **if and only if** the data from PRD §5.3 justifies it |
| **Phase 4** | Month 6+ | Re-evaluate the stack against real numbers · Headless storefront if on Path A and volume justifies it · International/NRI shipping · Wholesale enquiry channel |

Nothing in Phase 2+ is allowed to enter the launch scope. PRD §12 is the fence.

---

## 9. Risk Register

| Risk | Impact | Likelihood | Mitigation |
|---|---|---|---|
| Personal-account payments continue after launch | **Critical** — account freeze, tax exposure | High if not enforced | P0-14. Hard cutover date. Route every buyer to the site or to the current account. |
| GST position wrong | **Critical** — penalties, invoice invalidity | Medium | CA-written position, in writing, before launch (P0-3) |
| Unique saree sold twice | High — trust destroyed | Medium on B/C, low on A | PRD §3.6 test in acceptance criteria, all paths |
| Photos lack drape shots | High — conversion collapses | Unknown until P0-11 | Audit first (P0-11); reshoot is cheap relative to lost sales |
| Gateway KYC delays | Medium — launch slips | Medium | Start P0-7 early; have legal pages ready before applying |
| Jaya unavailable post-launch | High on B/C, low on A | Medium | Weighs heavily toward Path A |
| Untested backup | **Critical** on B/C | Medium | Restore drill is in the launch acceptance criteria |
| Return rate higher than expected | Medium — margin | Medium | No COD at launch (PRD §5.3); unboxing-video policy |
| Site launches, nobody visits | Medium | High | Expected. Launch distribution *is* the WhatsApp groups. Seed with existing buyers. Traffic acquisition is Phase 2–3, and nothing here promises rankings. |

---

## 10. What Is Deliberately Not Being Built

Recorded so it does not get rebuilt by accident: see PRD §12. In addition, at the implementation
level — no custom payment form, no custom image CDN, no custom search engine, no custom
recommendation model, no microservices, no Kubernetes, no GraphQL layer over a single database,
and no framework whose only justification is that it is interesting.

---

*Requirements: [03-PRD.md](03-PRD.md). Decision history: [05-Changelog.md](05-Changelog.md).*
