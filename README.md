# Saree Direct-to-Consumer Website

Moving from *chat-and-trust selling* to *catalog-and-checkout selling*, without losing the personal
relationship that currently drives the sales.

> **A buyer should be able to find a saree, believe it is real and safe to buy, pay for it, and know
> where it is — without the owner typing a single message.**

**Status:** Phase 0 in progress. No code written. **Stack not yet chosen** — deliberately.

> ⚠️ **Budget revised to near-zero on 2026-09-15, which changed the sequence: build the website
> second, not first.** The two highest-severity problems — business money in a personal account, and
> no invoice trail — are fixable this week with free tools and no website.
> **→ [01-START-HERE.md](01-START-HERE.md)** is the checklist. Everything below describes the site you
> build once order volume justifies it.

---

## Documents

> **📍 Not sure what to open? → [00-READ-THIS-FIRST.md](00-READ-THIS-FIRST.md).** One screen, with a
> reading order for the owner and a separate one for an agent picking the project up.
> **Root files are numbered in reading order.**

| Document | Answers |
|---|---|
| **[00-READ-THIS-FIRST.md](00-READ-THIS-FIRST.md)** | **Which file to open, in what order.** |
| **[01-START-HERE.md](01-START-HERE.md)** | **The working checklist.** What to do this week, on ₹0, in order. Start here. |
| **[02-COSTS-AND-TRADEOFFS.md](02-COSTS-AND-TRADEOFFS.md)** | **One ledger.** What ₹0 sacrifices, what it does not, and what every skipped thing costs to buy back. |
| [03-PRD.md](03-PRD.md) | What the system must do, and why. Stack-agnostic. The source of truth. |
| [04-implementations.md](04-implementations.md) | How to build it. Three paths, their trade-offs, and the decision gate. |
| [05-Changelog.md](05-Changelog.md) | Every decision, dated, with what was rejected and what would reverse it. |
| [phase0/](phase0/) | The business prerequisites. 18 artefacts, currently being filled. |
| [Phase 0 plan](../../docs/superpowers/plans/2026-09-11-saree-site-phase0.md) | The 14-task execution plan across three parallel tracks. |
| [06-website_builder.md](06-website_builder.md) | The operating persona this project is built under. |

**Rule:** where this README and the PRD disagree, the PRD wins. Where a decision is changed in
conversation, it is appended to the Changelog the same day or it does not count as decided.

---

## The recommendation, in plain words

There are **two separate recommendations** here, and confusing them is what makes this project sound
complicated. They are not the same decision and they are not made at the same time.

### 1. Do not build the website yet

Over the next six weeks instead: engage a CA, settle GST, open a business current account, set up
**Zoho Books Free** for real invoices, and open a **Razorpay account** so buyers get a payment link
instead of your personal PhonePe. Keep selling through WhatsApp exactly as you do today.

**Cost: roughly ₹10,000–35,000, almost all of it the CA.**

**Why.** The website was never the urgent part. Of the five problems below, the two most dangerous do
not need a website at all:

| Problem | Needs a website? |
|---|---|
| Business money in a personal account | **No** — needs a current account and a payment link |
| No invoices, no order record | **No** — needs Zoho Books Free |
| Every order costs ~20 minutes of your time | **Yes** |
| Nobody new can find you | **Yes** |
| A unique saree could sell twice | Partly |

Business payments landing in a personal savings account is the thing that can freeze your money or
create a tax problem. It is fixable in six weeks for the price of a CA. **Building a website first
would mean spending weeks solving problem #3 while problem #1 keeps running.**

Fix the dangerous things now, cheaply. Build the website when it pays for itself.

### 2. When you do build, use Shopify

Not WooCommerce. Not Odoo. Not writing it yourself in Next.js.

Shopify costs ₹1,499/month plus 2% of each order. **The alternatives cost less money and more of
you.** WooCommerce makes you the sysadmin — updates, backups, security, and the call when checkout
breaks during Diwali, 4–8 hours a month forever. Writing it yourself is 10–14 weeks before the first
sale, then that same burden plus everything you built by hand.

**And you are the only person who could fix any of it.** If you are travelling, ill, or busy, the
business stops. Shopify's outage is Shopify's problem; your server's outage is yours.

> **You are not paying Shopify for features. You are paying them to not be on call.**

### Why this is still a guess

**Shopify does not cost ₹1,499/month. It costs ₹1,499 plus 2% of everything you sell** — and that
depends entirely on how much you sell, which is not yet known.

| Your monthly sales | Shopify costs per year | Verdict |
|---|---|---|
| ₹1,00,000 | ~₹42,000 | Clearly worth it for 7–12 hours a month back |
| ₹10,00,000 | ~₹2,58,000 | Paying a developer starts making sense |

Those are very different answers. So "use Shopify" actually means *"Shopify, assuming you sell
somewhere between ₹1–4 lakh a month"* — **a range that was assumed, not measured.**

→ [02-COSTS-AND-TRADEOFFS.md](02-COSTS-AND-TRADEOFFS.md) has the full ledger.

### What would change each recommendation

| Recommendation | Changes if |
|---|---|
| Don't build yet | You are already doing **50+ orders/month** — then manual handling is costing real money and the site becomes urgent |
| Use Shopify | Sales exceed **~₹8 lakh/month** → the 2% starts hurting; look at self-hosting |
| | **Someone else technical** can help maintain it → self-hosting gets less risky |
| | You have or want a **physical shop** → Odoo, because one system covers shop and website |
| | **Learning to build it is genuinely the goal** → then build it in Next.js, properly. That is a legitimate aim, just a different project from "sell sarees this quarter" |

---

## Why this is being built

Sarees sell **today** — via WhatsApp groups, by word of mouth, paid by individual PhonePe transfers
into a **personal account**. The five problems with that, ranked (PRD §1.2):

1. **The personal UPI account is a business-continuity risk.** Freeze exposure, no reconciliation,
   no invoice trail, tax exposure. Highest severity in the whole project, and **stack-independent** —
   fixing it does not wait for a website.
2. **No order record.** Orders live in chat scrollback. Disputes cannot be resolved.
3. **Manual labour per order.** Caps sales at the owner's available hours, not at demand.
4. **Reach ceiling.** No discovery surface, no shareable product link.
5. **No inventory truth.** A unique piece can be sold twice — the fastest way to lose a customer
   permanently.

A WhatsApp or Instagram catalog solves only #4, and only partially. Catalogs solve *browsing*; the
pain is *transacting*. Both stay as channels **feeding** the site — see the Changelog entry dated
2026-09-11.

---

## Success metrics — measured 90 days after launch

No traffic or ranking promises are made anywhere in this project.

| Metric | Target |
|---|---|
| Orders completed with zero owner messages | ≥ 60% |
| Payment success rate | ≥ 85% |
| Checkout completion (orders ÷ sessions reaching cart) | ≥ 25% |
| **Double-sale incidents** | **0** |
| Invoice coverage | 100% |
| Owner time per order | ≤ 5 min (pack + label only) |
| Median LCP, mobile product page | ≤ 2.5s |

If self-serve order rate is below 30% at 90 days, the problem is trust or friction, not traffic.
Diagnose in that order.

---

# Feature Scope — v1

## Storefront and browse

- Home page
- Collections by **fabric** — Silk, Cotton, Linen, Georgette, and the rest of the controlled list
- Collections by **occasion** — Wedding, Festive, Office, Daily
- Price-band browsing — Under ₹2,000 · ₹2,000–5,000 · ₹5,000–10,000 · ₹10,000+
- **New Arrivals** — automatic, `created_at` descending, last 30 days
- **One-of-a-Kind** collection — `is_unique = true AND stock_qty > 0`
- Filters: fabric, colour, price range, occasion, availability
- Sort: newest, price low→high, price high→low
- On-site search
- **Pagination, not infinite scroll** — deliberate. Infinite scroll destroys the ability to return
  to your place, and is worse for both SEO and accessibility.

## Product page

A saree is a tactile, high-consideration purchase. From photographs alone the buyer is answering
three questions: *what does this fabric feel like · what will it look like on a person · is the
colour real*. Every requirement below traces to one of those three.

- Swipeable gallery with pinch/tap zoom
- **Six-shot image set** — hero, **drape**, pallu, border, fabric macro, blouse piece.
  The drape shot is non-optional; conversion without one is materially worse.
- Price inclusive of GST, stated as such
- **One primary action** — Add to Cart primary, Buy Now secondary
- **Delivery estimate by pincode** — the highest-impact element on an Indian product page after price
- Specifications table — fabric, weave, length, blouse piece, weight, care
- Description written for a human, 80–150 words. Not keyword soup.
- Trust strip — returns in one line, secure-payment mark, WhatsApp button
- **One-of-a-kind badge where true** — honest, and the strongest scarcity signal available precisely
  because it is true
- "You may also like" — 4 items, same fabric or price band
- **Sold-out behaviour differs by stock type:** a unique piece keeps its page live, marked SOLD, with
  "notify me about similar pieces" and 4 similar sarees; a restockable design shows "out of stock"
  with back-in-stock capture and sorts last.

## Cart and checkout

- Cart persists across a page reload **and a browser restart** on the same device
- **Guest checkout mandatory.** No account required to buy. Ever.
- Optional account creation offered **only after payment**
- One page, maximum two steps
- **Pincode auto-fills city and state** — small feature, disproportionate effect on completion
- Order summary always visible. **No surprise charges at the last step** — the top cause of cart
  abandonment in India.
- Mobile validated to 10 digits, pincode to 6
- Explicit consent checkbox for WhatsApp/SMS order updates
- Mobile number is the primary order identifier

**Friction budget:** from link tap to payment success — at most 5 screens and one address entry.

## Payments

- UPI (primary — existing buyers already pay this way), cards, netbanking, wallets
- **No Cash on Delivery.** Deliberate: apparel RTO rates, delayed cash, and manual reconciliation —
  which is the exact labour this project exists to remove. Revisit at 90 days with data.
- Payment collected on the gateway's hosted page — **no card data ever touches our servers or
  database.** Keeps PCI-DSS scope minimal. Non-negotiable.
- **Order state driven by a signature-verified, idempotent server-to-server webhook — never by the
  browser redirect.** A buyer who closes the tab after paying still gets their order. The same
  webhook delivered twice creates one order and decrements stock once.
- **Stock decrements on confirmed payment**, not on cart-add. Two buyers paying for the same unique
  saree in the same second produce **exactly one success and one automatic full refund**.
- Every payment attempt logged with its gateway reference, for reconciliation
- Refunds executable from the admin without contacting gateway support

## Tax and invoicing

- **Per-product `gst_rate` and `hsn_code`** — not a global constant. Textile rates vary by material
  and have varied by price threshold.
- **Place of supply computed by the system, not by the owner** — CGST+SGST intra-state, IGST
  inter-state, driven by the buyer's shipping state
- Tax invoice per order, emailed and downloadable
- **Sequential invoice numbering with no gaps**, surviving a system restart. A gap is an audit
  problem.

⚠️ Every rate, code, and threshold comes from the CA in writing. Nothing regulatory is taken from
these documents.

## Shipping and fulfilment

- **India only** in v1
- Flat rate or free-over-threshold — a number, not an integration. Live weight-based rating is
  Phase 2.
- Coarse zone delivery table, set from **measured packed weights** and a real courier rate card
- Tracking number captured per order and sent automatically
- Packing slip carries the SKU, so a physical saree maps to its record
- Waterproof outer packing — monsoon transit damage is the seller's loss

**Target owner loop:** open admin → see paid orders → print slips → pack → bulk labels → hand to
pickup → tracking auto-sent. **≤ 5 minutes per order.**

## Post-purchase messaging

| Trigger | Content |
|---|---|
| Payment success | Order number, items, total, expected delivery — on screen, WhatsApp/SMS, email |
| Order packed | Confirmation |
| Shipped | Courier, AWB, tracking link |
| Delivered | Confirmation + request for a photo or review |
| **Payment failed** | **Retry link. Never leave a failed payment silent — the buyer assumes the money is gone.** |

Plus an **order-status lookup by order number + mobile, with no login**.

## Admin

Built for a **non-technical operator working from a phone.** This single constraint eliminates any
design where listing a product means editing a file, running a command, or triggering a deploy.

- Add a product with every field, from a phone
- **Server-side image resizing** — requiring the operator to resize photos is a guaranteed process
  failure
- Edit price, edit stock, mark sold out
- Order list with search and status filters
- Mark packed / shipped, enter tracking
- Full and partial refunds
- Invoice download for any order
- Discount codes
- Static page editing (About, Returns, Contact)
- **CSV export for the accountant**
- Two accounts minimum, either revocable

**Security is not optional here — an admin panel is a direct path to the money.** Strong password
policy, **2FA on the owner account**, session timeout, an audit log of every price/stock/refund
change, and rate limiting on checkout and admin login.

## Trust, legal, compliance

Weighted equally with checkout. A new saree site does not usually fail on technology — it fails
because nobody believed it.

- Real About page: who the owner is, a **real photograph**, where the sarees come from, why
- Verifiable address and a **working phone number**
- Five legally required pages — Privacy · Terms · Returns/Refunds/Cancellation · Shipping · Contact.
  **Payment gateways check for these during onboarding; missing pages delay go-live.**
- GSTIN in the footer once registered
- **No fabricated reviews. No fake MRPs. No fake scarcity. Stated delivery timelines must be
  achievable.** Launch with zero reviews and a strong About page rather than invented ones.

## SEO structure

Not a launch blocker, but the structure must be right at launch — retrofitting URLs is expensive.

- Permanent URLs: `/sarees/<slug>`, `/collections/<slug>`. **URLs never change after publication.**
- One `<h1>` per page, semantic HTML, unique title and meta description per page
- `Product` JSON-LD with price and availability · `Organization` · `BreadcrumbList`
- **Open Graph and Twitter card tags.** These control what appears when a product link is pasted
  into WhatsApp — the primary distribution channel. **A broken OG image directly costs sales.**
- Auto-updating XML sitemap, `robots.txt`, Search Console verified
- Canonical tags on filtered and paginated listings

## Analytics

Enough to diagnose the success metrics, nothing more: funnel from product view → add to cart →
checkout start → payment attempt → payment success · per-product views (tells the owner what to
restock) · failed payments by method · **on-site search terms** — high-signal, low-cost demand data.

## Quality bars

| Metric | Budget |
|---|---|
| LCP, product page | ≤ 2.5s |
| CLS | ≤ 0.1 |
| INP | ≤ 200ms |
| Initial JS | ≤ 150KB compressed |
| Hero image delivered | ≤ 150KB |

**Images are the entire performance story on this site.** Get the pipeline right and the budgets are
met; get it wrong and no framework choice saves it.

**WCAG 2.1 AA baseline** — semantic HTML, contrast ≥ 4.5:1 (beware light-grey-on-white "elegant"
styling, the most common failure on fashion sites), full keyboard operation with visible focus, alt
text everywhere, real labels, 200% zoom, `prefers-reduced-motion` respected.

Tested on a **real mid-range Android**, not a desktop emulator.

---

# Deferred

| Phase | When | Features |
|---|---|---|
| **2** | Weeks 4–10 post-launch | Abandoned-cart recovery · Instagram/Meta catalog sync · Discount codes and festival campaigns · Back-in-stock notifications · Reviews with photos |
| **3** | Months 3–6 | Care guides, fabric explainers, occasion styling · Google Shopping feed · Email list and monthly new-arrivals send · **COD pilot if and only if the data justifies it** |
| **4** | Month 6+ | Re-evaluate the stack against real numbers · headless storefront if on Path A and volume justifies it · NRI/international shipping · wholesale enquiry channel |

**Nothing in Phase 2+ enters the launch scope.** PRD §12 is the fence.

## Explicitly not built

Mobile app · customer login and wishlist · loyalty points · gift cards · live chat widget (WhatsApp
covers it) · multi-vendor marketplace · wholesale pricing tiers · subscription boxes · AR try-on ·
multi-language · international shipping and multi-currency.

---

# Stack — not yet decided

| Path | Stack | Standing |
|---|---|---|
| **A** | Shopify Basic + custom theme | **Recommended, not decided** |
| **C** | Next.js storefront + self-hosted Medusa (Postgres, Redis, VPS) | Correct second choice |
| **D** | Zoho Commerce + Zoho Books | Contender — GST-native, no platform %, **but no mobile admin app** |
| **E** | Odoo (Website + eCommerce + Accounting) | Contender — GST-native, no platform %, heavy; **best if a physical shop or POS shares the system** |
| **B** | Fully custom Next.js + Supabase + Razorpay | Not recommended for this business |

**Why it is still open:** Phase 0 is stack-independent and is the true critical path — CA → GST
position → current account → gateway KYC is 2–6 weeks of waiting on third parties. Nothing launches
before that completes, so the decision has exactly that long to sit open, and no longer.

**Decision gate:** closes when the business current account opens (Phase 0 Task 5). Inputs and the
decision rule are in [phase0/banking.md](phase0/banking.md) Step 6 and 04-implementations.md §7.

**The number that decides it:** Shopify Payments does not operate in India, so a third-party gateway
is mandatory **and Shopify charges its own per-order fee on top of the gateway's fee.**

Researched 2026-09-15: **Basic 2.0%, Grow 1.0%, Advanced 0.6%.** Basic→Grow crossover at ~₹4.1 lakh
monthly GMV. The fee scales — ₹2,000/mo at ₹1L GMV, ₹7,200 at ₹3.6L, ₹20,000 at ₹10L. **Cheap
insurance now, expensive later**, which is why the Phase 4 migration trigger should be a GMV number
decided in advance rather than a feeling. Re-read Shopify's own page on the day of the decision.

**The number still missing is yours:** real monthly GMV from the last 3 months of WhatsApp sales.
Every cost figure in [phase0/stack-research.md](phase0/stack-research.md) is modelled on an assumed
₹3,000 average order value. Until the real number exists, the whole comparison is arithmetic on a
guess.

**Why the catalog work is safe either way:** `phase0/catalog/catalog.csv` has a frozen 21-column
contract and imports into all three paths. That is the whole reason the decision can be deferred.

---

# Current state

Phase 0 has 14 tasks across three parallel tracks. All owner-independent artefacts are written;
see [phase0/README.md](phase0/README.md) for the live status board.

**Genuinely blocked on third parties:** GST filing · bank account opening · gateway KYC · courier
onboarding · test parcel transit.

**Waiting on the owner only — roughly an hour of work, and these move today:**

1. Send the CA enquiry — drafted verbatim in [phase0/ca-engagement.md](phase0/ca-engagement.md)
2. Three brand-name candidates, checked four ways
3. Confirm the return policy line by line
4. Write the About story, 200–400 words, in the owner's voice
5. Enter three pilot catalog rows
6. Weigh ten sarees, packed

**And the one that waits for nothing:** the **personal-account cutover**
([phase0/banking.md](phase0/banking.md) Step 4). It needs no website, no stack, and no domain — only
a current account and a hard date.

---

## Conventions

- `<<FILL: what is needed | who provides it>>` marks an unfilled value. **A file containing any
  `<<FILL:` marker is a draft and must not be published or treated as decided.**
- Money is **integer paise**, everywhere. Never a float.
- A **SKU, once assigned, never changes.** It is the permanent link between a physical saree and
  every digital record.
- **No regulatory figure** — GST rate, HSN code, threshold — is taken from these documents, from a
  web search, or from an AI. Each comes from the CA in writing, dated and attributed.
- **No secrets in this repository.** No API keys, no account numbers, no IFSC codes, no KYC scans —
  not even in a file named `.example`. If a key is ever pasted anywhere by accident, **rotate it**
  rather than deleting the message.
# sri-lalitha-sahasra-silks
