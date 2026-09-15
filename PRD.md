# PRD — Saree Direct-to-Consumer Website

**Owner:** Jaya Arun Kumar Tulluri
**Status:** Draft v1.0 — approved for planning, stack not yet chosen
**Last updated:** 2026-09-11
**Related docs:** [implementations.md](implementations.md) (how to build), [Changelog.md](Changelog.md) (decision log)

> This document is **stack-agnostic on purpose**. It describes what the system must do and why.
> Technology choices live in `implementations.md`. If a requirement here can only be met by one
> specific platform, that is a bug in this document — flag it.

---

## 0. Reading Guide

| Section | Answers |
|---|---|
| 1 | Why this exists, what success looks like |
| 2 | Who buys, and what they are actually afraid of |
| 3 | What we sell and how it is modelled as data |
| 4 | What a buyer does, screen by screen |
| 5 | Money: pricing, payment, tax, refunds |
| 6 | Shipping and fulfilment |
| 7 | Who runs the site after launch and what they need |
| 8 | Trust, legal, compliance — non-negotiable in India |
| 9 | Content, SEO, traffic |
| 10 | Quality bars: performance, accessibility, browser support |
| 11 | Analytics — what we measure |
| 12 | Out of scope, explicitly |
| 13 | Assumptions and open questions |
| 14 | Acceptance criteria for launch |

---

## 1. Business Context and Goal

### 1.1 Current State (as of 2026-09-11)

- Sarees are sold **today**, via WhatsApp groups, driven by word of mouth.
- Payment is collected as **individual PhonePe UPI transfers to a personal account**.
- Professional product photography **already exists**.
- There is no website, no catalog system, no order record, no invoice trail.

### 1.2 Problems With the Current State

These are the actual reasons to build, ranked:

1. **Personal UPI account is a business-continuity risk.**
   Personal UPI/savings accounts are not intended for commercial receipts. High inbound
   transaction volume on a personal account can trigger bank scrutiny, limit increases being
   refused, or account freezes, and it creates an income-tax exposure with no matching invoice
   trail. *Verify current NPCI/bank limits before relying on any specific number.*
   **This is the highest-severity item in this document and is stack-independent.**
2. **No order record.** Orders live in chat scrollback. Disputes cannot be resolved. Repeat-buyer
   history is invisible. Nothing can be analysed.
3. **Manual per-order labour.** Every sale costs a conversation, a manual payment check, and a
   manual address collection. This caps sales at the owner's available hours, not at demand.
4. **Reach ceiling.** WhatsApp groups only reach people already known. There is no discovery
   surface — no search traffic, no shareable product link, no Instagram-to-purchase path.
5. **No inventory truth.** Unique pieces can be sold twice. This is the fastest way to lose a
   customer permanently.

### 1.3 Goal

Move from *chat-and-trust selling* to *catalog-and-checkout selling*, without losing the
personal relationship that currently drives the sales.

Stated as one sentence: **a buyer should be able to find a saree, believe it is real and safe to
buy, pay for it, and know where it is — without the owner typing a single message.**

### 1.4 Non-Goals for v1

Building a marketplace. Building an app. Onboarding other sellers. Wholesale/B2B pricing.
International shipping. These are explicitly deferred — see §12.

### 1.5 Success Metrics

Measured 90 days after launch. No traffic or ranking promises are made anywhere in this document.

| Metric | Definition | Target |
|---|---|---|
| Self-serve order rate | Orders completed with zero owner messages | ≥ 60% of total orders |
| Payment success rate | Successful payments ÷ payment attempts | ≥ 85% |
| Checkout completion | Orders ÷ sessions that reached cart | ≥ 25% |
| Double-sale incidents | Unique saree sold twice | 0 |
| Invoice coverage | Orders with a compliant invoice issued | 100% |
| Owner time per order | Minutes of manual work per order | ≤ 5 (pack + label only) |
| Median LCP, mobile | Field or lab measurement on product page | ≤ 2.5s |

If self-serve order rate is below 30% at 90 days, the problem is trust or friction, not traffic.
Diagnose in that order.

---

## 2. Buyers

### 2.1 Persona A — "Existing WhatsApp buyer" (primary, launch-critical)

Knows the owner. Already trusts the product. Currently buys by asking in chat.
- **Wants:** to browse everything at once without scrolling images in a group, and to pay without
  asking "did you get it?"
- **Fears:** that the website is more hassle than just messaging.
- **Implication:** the site must be *faster than sending a WhatsApp message*. One tap from a
  shared link to a product. Guest checkout. No forced account creation. Ever.

### 2.2 Persona B — "Referred stranger" (primary growth path)

Received a product link from a friend. Has never heard of the business.
- **Wants:** proof this is a real business and not a scam page.
- **Fears:** paying and receiving nothing, or receiving a different saree than shown.
- **Implication:** trust signals must be on the product page itself, not buried in an About page.
  See §8.

### 2.3 Persona C — "Search / social discovery" (secondary, post-launch)

Searching for a saree type ("kanjivaram silk saree online", "cotton saree for office").
- **Wants:** to compare, to see fabric detail, to know the real price including delivery.
- **Implication:** drives the content and SEO requirements in §9. Not a launch blocker.

### 2.4 The Buyer's Actual Decision

A saree is a **tactile, high-consideration, gift-or-occasion purchase**. Online, the buyer is
trying to answer three questions from photographs alone:

1. What does this fabric actually feel like?
2. What will this look like on a person, not on a flat surface?
3. Is the colour in the photo the colour in real life?

Every product-page requirement in §4.3 traces back to one of these three questions. A product
page that does not answer all three will not convert, no matter how fast it loads.

---

## 3. Catalog and Data Model

### 3.1 Inventory Reality

Two kinds of stock coexist:

- **Unique pieces** (`is_unique = true`, quantity 1). Handloom, one-off weaves. When sold, gone.
- **Restockable designs** (`is_unique = false`, quantity N). Same design reorderable from the
  supplier, possibly in several colours.

The system must handle both **without the owner having to think about which is which** beyond
setting one flag at listing time.

### 3.2 Product Entity

Required fields marked ✱.

| Field | Type | Notes |
|---|---|---|
| ✱ `sku` | string, unique | Human-readable. Suggested scheme: `<FABRIC>-<YYMM>-<NNN>`, e.g. `KAN-2609-014`. Printed on the packing slip so a physical saree maps to a record. |
| ✱ `title` | string | Buyer-facing. Fabric + distinguishing feature. Not "Saree 14". |
| ✱ `price` | integer, paise | Store money in the smallest unit as an integer. **Never store money as a float.** |
| `mrp` | integer, paise | Strike-through price. Only if genuinely the prior price — see §8.4. |
| ✱ `currency` | enum | `INR` only in v1. |
| ✱ `stock_qty` | integer | Decrements on paid order. Not on cart-add. |
| ✱ `is_unique` | boolean | Drives sold-out behaviour, see §3.5. |
| ✱ `fabric` | enum | Silk / Cotton / Cotton-silk / Linen / Georgette / Chiffon / Crepe / Organza / Tussar / Blended. Controlled vocabulary — free text here kills filtering. |
| `weave_technique` | enum | Kanjivaram / Banarasi / Ikat / Jamdani / Chanderi / Maheshwari / Sambalpuri / Pochampally / Printed / Plain / Other. |
| ✱ `primary_color` | enum | Controlled list of ~14 colour families for filtering. |
| `secondary_colors` | enum[] | Border/pallu colours. |
| ✱ `saree_length_m` | decimal | Typically 5.5 m without blouse, 6.3 m with. Buyers ask. |
| ✱ `blouse_piece` | enum | `attached_unstitched` / `separate_unstitched` / `none`. Single largest source of post-sale complaints if unstated. |
| ✱ `care_instructions` | text | Dry clean only / hand wash cold / machine wash. Affects returns. |
| ✱ `weight_grams` | integer | **Shipping is billed on weight.** Missing weight = wrong shipping cost = lost margin. |
| ✱ `hsn_code` | string | Required on a GST invoice. See §5.5. |
| ✱ `gst_rate` | decimal | Per-item. See §5.5 — rate varies by material and price. |
| ✱ `images` | image[] | See §3.3. |
| `occasion_tags` | enum[] | Wedding / Festive / Office / Daily / Party. Drives collections. |
| `description` | rich text | See §4.3. |
| ✱ `status` | enum | `draft` / `active` / `sold_out` / `archived`. |
| `created_at`, `updated_at` | timestamp | Sort order for "New Arrivals". |

### 3.3 Image Requirements

Photography exists already, so this is a **standardisation and pipeline** requirement, not a shoot
requirement. Per product, minimum set:

1. **Hero — flat/folded, full saree**, consistent background across the whole catalog.
2. **Drape shot** — on a person or mannequin. Answers persona question 2 (§2.4). Non-optional;
   conversion on saree pages without a drape shot is materially worse.
3. **Pallu detail** — the decorated end.
4. **Border detail.**
5. **Fabric macro** — close enough to read the weave. Answers question 1.
6. **Blouse piece**, if included.
7. Optional: back of saree, alternate lighting.

Technical rules:
- Same aspect ratio for every hero image, or the grid will look broken. **Recommend 4:5** (portrait,
  matches Instagram, matches how a saree hangs).
- Colour accuracy: shoot/edit against a consistent white balance. State in the product page that
  colour may vary slightly by screen — this is a returns-defence, not a disclaimer for bad photos.
- Source files ≥ 2000px on the long edge so zoom is possible.
- Delivered to the browser as WebP/AVIF, responsive `srcset`, explicit width/height to prevent
  layout shift, `loading="lazy"` on everything except the hero.
- Alt text per image, describing the saree — required for accessibility *and* it is the only
  image-SEO signal that matters.

### 3.4 Collections / Navigation Taxonomy

Buyers navigate by **occasion** and **fabric**, in that order. Price is a filter, not a category.

```
By Fabric      → Silk, Cotton, Linen, Georgette, ...
By Occasion    → Wedding, Festive, Office Wear, Daily Wear
By Price       → Under ₹2,000 · ₹2,000–5,000 · ₹5,000–10,000 · ₹10,000+
New Arrivals   → auto, created_at desc, last 30 days
One-of-a-Kind  → is_unique = true AND stock_qty > 0
```

Filters on listing pages: fabric, colour, price range, occasion, availability.
Sort: newest, price low→high, price high→low.
**No infinite scroll.** Paginate. Infinite scroll destroys the ability to return to your place,
and it is worse for both SEO and accessibility.

### 3.5 Sold-Out Behaviour

- `is_unique = true`, stock hits 0 → product page **stays live** (the URL may be shared, and it is
  an SEO asset), marked clearly SOLD, buy button replaced with "Notify me about similar pieces"
  and links to 4 similar sarees. Removed from listing grids by default.
- `is_unique = false`, stock hits 0 → marked "Out of stock", with a back-in-stock email/WhatsApp
  capture. Stays in listing grids, sorted last.

### 3.6 Inventory Integrity — Hard Requirement

Stock decrements **on successful payment confirmation**, not on add-to-cart and not on checkout
start. Two buyers paying for the same unique saree within the same second must result in exactly
one success and one automatic full refund with an apology message.

This requires either a database transaction/row-lock at the decrement, or the platform's built-in
inventory reservation. **This requirement is the single most important piece of correctness in the
system** — see success metric "double-sale incidents = 0". Whichever implementation path is
chosen, it must state explicitly how this is guaranteed.

---

## 4. User Journeys

### 4.1 Journey 1 — Shared link to purchase (the dominant path at launch)

```
WhatsApp message with product link
  → product page loads (must be < 2.5s LCP on 4G mobile)
  → images, price, delivery estimate, trust signals visible without scrolling past the fold
  → Buy Now
  → checkout: name, phone, address, pincode   [no account required]
  → pay via UPI / card / netbanking
  → confirmation screen with order number
  → confirmation message on WhatsApp/SMS + email
  → shipping notification with tracking link
  → delivered
```

Friction budget: **from link tap to payment success, at most 5 screens and one address entry.**

### 4.2 Journey 2 — Browse and discover

```
Home → collection (fabric or occasion) → filter → product → cart → checkout → pay
```
Cart must persist across a page reload and across a browser restart on the same device.

### 4.3 Product Page — Detailed Requirements

Above the fold, mobile:
- Hero image, swipeable gallery, pinch/tap zoom.
- Title, price (inclusive of tax, stated as such), stock state.
- **One primary action.** "Buy Now" or "Add to Cart" — not both competing. Recommend Add to Cart
  as primary with Buy Now secondary, since multi-saree orders reduce per-order shipping cost.
- Delivery estimate by pincode ("Enter pincode → delivered by Sep 18"). Highest-impact single
  element on an Indian commerce product page after price.
- Trust strip: return policy in one line, secure-payment mark, WhatsApp-us link.

Below the fold:
- Full gallery including drape and fabric macro.
- Specifications table — every field from §3.2 the buyer cares about: fabric, weave, length,
  blouse piece, weight, care.
- Description: written for a human, mentioning the occasion and what it pairs with. 80–150 words.
  Not keyword soup.
- Shipping & returns, expanded.
- "You may also like" — 4 items, same fabric or same price band.
- **If unique:** a visible "This is a one-of-a-kind piece — only one available" badge. This is both
  honest and the strongest scarcity signal available, because it is true.

### 4.4 Checkout Requirements

- **Guest checkout mandatory.** Optional account creation *after* payment, never before.
- Single page or a maximum of two steps. Every extra step loses buyers.
- Fields: full name, mobile (10-digit, validated), email (optional but requested for invoice),
  address line 1, line 2, landmark, city, state, pincode (6-digit, validated).
- **Pincode auto-fills city and state.** Small feature, disproportionate effect on completion.
- Order summary always visible: item, price, shipping, total. **No surprise charges at the last
  step** — the top reason for cart abandonment in India.
- Mobile number is the primary order identifier, since WhatsApp is the communication channel.
- Explicit consent checkbox for WhatsApp/SMS order updates.

### 4.5 Post-Purchase

| Trigger | Channel | Content |
|---|---|---|
| Payment success | On-screen + WhatsApp/SMS + email | Order number, items, total, expected delivery |
| Order packed | WhatsApp/SMS | "Your order is packed" |
| Shipped | WhatsApp/SMS + email | Courier name, AWB/tracking number, tracking link |
| Out for delivery | Courier's own message | — |
| Delivered | WhatsApp | Delivery confirmation + a request for a photo/review |
| Payment failed | On-screen + retry link | Never leave a failed payment silent — the buyer assumes money is lost |

Order-status lookup page by order number + mobile, no login required.

---

## 5. Money

### 5.1 Pricing

- Prices displayed **inclusive of GST**, with "Inclusive of all taxes" stated. This is the norm in
  Indian B2C and avoids a last-step price jump.
- MRP strike-through only when it is a real prior price. Fabricated MRPs are a legal risk under
  consumer-protection rules on misleading discounts, and buyers detect them.

### 5.2 Payment Methods — v1

| Method | Included at launch | Why |
|---|---|---|
| UPI (PhonePe/GPay/Paytm/any) | Yes — primary | Buyers already pay this way today |
| Cards (debit/credit) | Yes | Required for higher-value orders |
| Netbanking | Yes | Comes free with the gateway |
| Wallets | Yes if free | Comes free with the gateway |
| **Cash on Delivery** | **No — deliberate** | See §5.3 |
| EMI | No | Revisit if average order value exceeds ~₹8,000 |
| International cards | No | Out of scope, see §12 |

### 5.3 Cash on Delivery — Decision and Reasoning

**COD is excluded at launch.** Reasoning:

- Apparel COD in India carries a high return-to-origin rate. Every RTO costs forward shipping +
  return shipping + handling, on an order that earned nothing. On a thin-margin first-year
  business this is the fastest route to negative unit economics.
- COD also delays cash: the courier remits days-to-weeks later.
- Every COD order needs manual reconciliation — which is the manual labour this project exists to
  remove.

**Mitigation for lost sales:** offer a small prepaid incentive (e.g. free shipping over a
threshold, or a modest prepaid discount), and keep the WhatsApp channel open for buyers who will
not pay online — the owner can still take those the old way. This is a policy decision, reversible
with data. Revisit after 90 days: if measured cart abandonment specifically cites COD absence, run
a limited COD pilot on pincodes with good delivery history and on orders under a value cap.

### 5.4 Payment Processing — Structural Requirements

Independent of which gateway is chosen:

- **A business current account is required.** Settlement must not go to a personal savings
  account. See §1.2 item 1.
- Gateway must support UPI, cards, netbanking, with Indian settlement.
- **Order state must be driven by the gateway's server-to-server webhook, never by the browser
  redirect.** A buyer who closes the tab after paying must still get a confirmed order.
- Webhook handler must verify the gateway's signature, and must be **idempotent** — the same
  webhook delivered twice must not create two orders or decrement stock twice.
- Every payment attempt logged with gateway reference id, for reconciliation.
- **No card data ever touches our servers or our database.** Payment is collected on the gateway's
  hosted page or its SDK. This keeps PCI-DSS scope minimal and is non-negotiable.
- Refund must be executable from the admin without contacting the gateway support.

### 5.5 Tax and Invoicing

⚠️ **This section contains regulatory specifics that change. Confirm every number with a
chartered accountant before launch. Nothing here is tax advice.**

- Textile GST rates in India are **not uniform** — they vary by material and, historically, by
  price threshold. Silk, cotton, and man-made fabrics have differed, and rate slabs have been
  revised more than once. **Do not hardcode a single rate.** Model `gst_rate` per product (§3.2)
  and confirm the current applicable rate per category with the CA.
- Each product needs the correct **HSN code** on the invoice.
- **Place of supply** determines the tax split: intra-state → CGST + SGST; inter-state → IGST. The
  buyer's shipping state drives this. The system must compute it, not the owner.
- A tax invoice must be generated per order and delivered to the buyer (email + downloadable).
  It must carry: seller legal name, address, GSTIN, invoice number (sequential, unbroken), date,
  buyer name and address, place of supply, per-item HSN, taxable value, tax split, total.
- **Invoice numbering must be sequential with no gaps** and must survive a system restart. A gap
  is an audit problem.
- Whether GST registration is required at all depends on turnover thresholds and on whether sales
  are inter-state. **This is an open question** — see §13. It is a **blocking prerequisite** for
  most payment gateway and platform onboarding.

### 5.6 Refunds and Returns Policy

The policy must be decided before launch and published verbatim on the site. Recommended starting
position, to be confirmed by the owner:

- **Return window:** 7 days from delivery.
- **Accepted for:** damaged in transit, wrong item sent, significantly different from the listing.
- **Not accepted for:** minor colour variation across screens (stated on every product page),
  used/washed/altered sarees, blouse pieces already stitched.
- **Unboxing video required** for damage/wrong-item claims. Standard in Indian apparel commerce,
  and it is the only practical defence against fraudulent claims.
- **Refund timeline:** initiated within 48 hours of the return being received and checked;
  reaching the buyer's account per the gateway's timeline (typically 5–7 business days — verify).
- Refund goes back to the original payment method. Always.
- Exchange offered as an alternative to refund for size/blouse issues.

---

## 6. Shipping and Fulfilment

### 6.1 Requirements

- **Domestic India only** in v1.
- Courier aggregator (so rates can be compared and a single courier's outage is not fatal) or a
  direct courier account. Either is acceptable; the aggregator reduces setup work.
- **Rate model:** flat-rate or free-over-threshold is strongly preferred over live weight-based
  rate calculation at launch. Live rate calculation is a real integration; flat rate is a number.
  Set the flat rate from actual average weight (§3.2 `weight_grams`) and average zone distance.
- Delivery-estimate-by-pincode on the product page (§4.3) — can be a coarse zone table at launch
  (metro 2–4 days, Tier 2 3–6, rest 5–9). Does not need live courier API integration to be useful.
- Tracking number captured per order and sent to the buyer automatically.

### 6.2 Packing Requirements (affects returns, therefore in scope)

- Waterproof outer. Sarees are ruined by monsoon transit damage and the seller eats the loss.
- Packing slip carrying the SKU (§3.2), so the physical item maps to the order record.
- Note that unboxing video is required for damage claims, printed on the slip.

### 6.3 The Owner's Daily Loop — Target

```
Open admin → see new paid orders → print packing slips → pack →
generate shipping labels in bulk → hand to pickup → tracking auto-sent to buyers
```
Target: **≤ 5 minutes of owner time per order** (success metric, §1.5). Any implementation that
requires more manual steps than this has failed the requirement, regardless of how good the
storefront looks.

---

## 7. Administration and Handoff

### 7.1 Who Operates It

Jaya builds and seeds the catalog. A **non-technical person must be able to run day-to-day
operations afterwards.** This is a hard constraint and it eliminates any design where adding a
product means editing a file, running a command, or triggering a deploy.

### 7.2 Admin Must Support, Without Developer Help

| Task | Frequency |
|---|---|
| Add a product: photos, price, stock, all §3.2 fields | Daily/weekly |
| Edit price, edit stock, mark sold out | Daily |
| View and search orders, filter by status | Daily |
| Mark packed / shipped, enter tracking number | Daily |
| Issue a full or partial refund | Weekly |
| Download the invoice for any order | Weekly |
| Create a discount code | Monthly |
| Edit a static page (About, Returns, Contact) | Rarely |
| Export orders to CSV for the accountant | Monthly |

### 7.3 Admin Non-Functional Requirements

- **Usable on a phone.** Photos are on the phone. Requiring a laptop to list a product means
  products do not get listed.
- Image upload must accept a phone photo directly and handle resizing server-side. Requiring the
  operator to resize images is a guaranteed process failure.
- Two accounts minimum (owner, operator), with the ability to revoke one.
- **Mandatory for any admin:** strong password policy, two-factor authentication on the owner
  account, session timeout, and an audit log of price/stock/refund changes. An admin panel is a
  direct path to the money — it gets real authentication, not a shared password.

### 7.4 Data Ownership and Exit

Whatever is built, these must be exportable at any time in a standard format:
products, images, orders, customers, invoices. Document the export procedure before launch, not
after a dispute with a vendor.

---

## 8. Trust, Legal, Compliance

### 8.1 Why This Section Has Equal Weight to Checkout

Persona B (§2.2) is a stranger deciding, in about four seconds, whether this is a real business.
A new saree site does not usually fail on technology. It fails because nobody believed it.

### 8.2 Trust Signals — Required at Launch

On the product page itself, not only on an About page:
- Return policy summarised in one line, linked to the full policy.
- Secure-payment indication, with the gateway's name (buyers recognise it).
- WhatsApp contact button — **preserves the existing relationship channel** and is the single
  highest-trust element available to this specific business.
- "One-of-a-kind" badge where true.

Site-wide:
- A real About page: who the owner is, a photograph, where the sarees come from, why. This
  business's entire current success is relationship-based — do not hide the relationship.
- A verifiable address and a phone number.
- Real customer photos/reviews as they accumulate. **Do not fabricate reviews.** Launch with zero
  reviews and a strong About page rather than with invented ones.
- GSTIN displayed in the footer once registered.

### 8.3 Legally Required Pages (India)

- Privacy Policy — required once personal data is collected. Must cover what is collected, why,
  who it is shared with (gateway, courier), and how to request deletion.
- Terms & Conditions.
- Return, Refund, and Cancellation Policy — §5.6.
- Shipping Policy — timelines, charges, coverage.
- Contact Us — with a working phone number and email.

Payment gateways **check for these pages during merchant onboarding**. Missing pages delay
go-live. Treat them as launch blockers, not filler.

### 8.4 Consumer-Protection Constraints

- No fabricated MRP or fake discount (§5.1).
- No fake scarcity ("2 people viewing this") unless it is literally true.
- Stated delivery timelines must be achievable.
- Country of origin and seller details are required disclosures for goods sold online.

⚠️ Confirm the current e-commerce disclosure requirements with a CA/lawyer. Rules have been
amended repeatedly.

---

## 9. Content and SEO

Not a launch blocker, but the structure must be right at launch because retrofitting URLs is
expensive.

### 9.1 Structural Requirements (launch)

- Clean, permanent URLs: `/sarees/<slug>`, `/collections/<slug>`. **URLs never change after
  publication.** If one must, it gets a 301 redirect.
- One `<h1>` per page. Semantic HTML throughout.
- Unique `<title>` and meta description per product and collection.
- `Product` structured data (JSON-LD) with price, availability, images — enables rich results and
  is a prerequisite for Google Shopping later.
- `Organization` and `BreadcrumbList` structured data.
- Open Graph + Twitter card tags. **Critical**: this controls what the preview looks like when a
  product link is pasted into WhatsApp — which is the primary distribution channel (§4.1). A
  broken OG image directly costs sales.
- XML sitemap, auto-updating. `robots.txt`. Google Search Console verified.
- Canonical tags on filtered/paginated listing pages to avoid duplicate-content dilution.

### 9.2 Content (post-launch)

Care guides, fabric explainers, occasion styling, "how to identify real silk". These match what
Persona C searches for. Deferred to Phase 3 — see implementations.md.

---

## 10. Quality Bars

### 10.1 Performance

**Mobile is the primary canvas.** Budgets, measured on a mid-range Android over 4G:

| Metric | Budget |
|---|---|
| LCP (product page) | ≤ 2.5s |
| CLS | ≤ 0.1 |
| INP | ≤ 200ms |
| Total JS, initial load | ≤ 150KB compressed |
| Hero image, delivered | ≤ 150KB |

Images are the entire performance story on this site. Get the image pipeline right (§3.3) and the
budgets are met; get it wrong and no framework choice saves it.

### 10.2 Accessibility — WCAG 2.1 AA as baseline

- Semantic HTML; heading order never skips.
- Text contrast ≥ 4.5:1. **Beware of light-grey-on-white "elegant" styling** — it is the most
  common failure on fashion sites.
- Every interactive element reachable and operable by keyboard, with a visible focus state.
- Alt text on every image (§3.3).
- Form inputs have real `<label>`s; errors are announced, not only coloured red.
- Zoom to 200% without loss of function.
- Respect `prefers-reduced-motion`.

### 10.3 Browser and Device Support

Last 2 versions of Chrome, Safari, Firefox, Edge; Chrome on Android 10+; Safari on iOS 15+.
Test on a real mid-range Android, not only a desktop emulator.

### 10.4 Reliability and Security

- HTTPS everywhere, HSTS.
- Automated backups of orders and products; a restore must be tested once before launch — an
  untested backup is not a backup.
- Rate limiting on checkout and admin login.
- Uptime monitoring with an alert to the owner's phone.
- Error logging that captures failed payments and failed webhooks specifically.
- Secrets (gateway keys, admin credentials) never committed to the repository. Keys rotated if
  ever exposed.

---

## 11. Analytics

Minimum viable measurement — enough to diagnose §1.5, nothing more.

- Page views, traffic source, device.
- Funnel: product view → add to cart → checkout start → payment attempt → payment success.
- Per-product view count (tells the owner what to restock).
- Failed-payment count by method.
- Search terms used on-site (a high-signal, low-cost source of demand information).

Analytics must comply with the privacy policy (§8.3) and must not block page rendering.

---

## 12. Explicitly Out of Scope for v1

Listed so they do not creep in:

International shipping and multi-currency · COD (§5.3) · Mobile app · Customer login/wishlist
(login is post-purchase-optional only) · Loyalty points · Gift cards · Product reviews with photo
upload (Phase 3) · Live chat widget (WhatsApp covers it) · Multi-vendor/marketplace · Wholesale
pricing tiers · Subscription boxes · AR/virtual try-on · Blog (Phase 3) · Multi-language ·
Abandoned-cart automation (Phase 2) · Instagram/Meta shopping sync (Phase 2).

---

## 13. Assumptions and Open Questions

Per the architect's operating principle, everything guessed is flagged.

### 13.1 Assumptions (stated, unverified)

| # | Assumption | If wrong |
|---|---|---|
| A1 | Catalog is roughly 50–300 active SKUs | Above ~2,000 SKUs, listing/search requirements change materially |
| A2 | Order volume at launch is < 30/day | Higher volume changes fulfilment and possibly infrastructure |
| A3 | Average order value is ₹1,500–₹10,000 | Higher values justify EMI and stronger fraud checks |
| A4 | Owner has or can open a business current account | Blocks all online payment — see Q1 |
| A5 | Sarees are sourced/stocked, not made to order | Made-to-order needs lead-time fields and a different stock model |
| A6 | Existing photos meet the §3.3 set, including drape shots | Missing drape shots become the critical path |
| A7 | Owner can pack and hand over shipments daily | Otherwise delivery promises in §6.1 are not achievable |

### 13.2 Open Questions — Must Be Answered Before Build

| # | Question | Blocks | Owner |
|---|---|---|---|
| **Q1** | **GST registration status?** Registered, required, or below threshold? | Payment gateway onboarding, invoicing (§5.5), platform signup | Owner + CA |
| Q2 | Business legal structure — proprietorship, partnership, private limited? | Current account, gateway KYC | Owner + CA |
| Q3 | Business current account open? In which bank? | All payments | Owner |
| Q4 | Legal business name, address, and phone to publish? | §8.3 pages, invoices | Owner |
| Q5 | Exact catalog size today, and photo coverage per product? | Launch scope, timeline | Owner |
| Q6 | Return policy — confirm the §5.6 recommendation or amend it | Legal pages, launch | Owner |
| Q7 | Who is the non-technical operator (§7.1), and what device do they use? | Admin design | Owner |
| Q8 | Brand name, domain, logo — exist or need creating? | Everything visible | Owner |
| Q9 | Is there a CA engaged already? | Q1, Q2, §5.5 | Owner |

**Q1, Q3, and Q8 are on the critical path and are independent of every technology decision.**
They should be started immediately — see Phase 0 in implementations.md.

---

## 14. Launch Acceptance Criteria

The site ships when all of these are true. Not before.

**Business**
- [ ] Business current account open and set as the settlement account
- [ ] GST question (Q1) resolved, with a CA's written position
- [ ] Payment gateway account live, not in test mode
- [ ] Courier/aggregator account active with pickup confirmed
- [ ] Domain registered, HTTPS live

**Catalog**
- [ ] ≥ 20 products live with the complete §3.3 image set including drape shots
- [ ] Every product has weight, HSN, GST rate, blouse-piece status, care instructions
- [ ] Unique pieces flagged correctly and showing the one-of-a-kind badge

**Commerce**
- [ ] A real ₹1 order placed end to end, by a person who did not build the site
- [ ] Payment webhook verified to confirm the order even when the browser is closed mid-payment
- [ ] Duplicate-webhook test: same webhook delivered twice → one order, stock decremented once
- [ ] Concurrent-purchase test on a unique item → exactly one success, one auto-refund (§3.6)
- [ ] Refund executed successfully from the admin, end to end
- [ ] Invoice generated with correct tax split for both an intra-state and an inter-state order

**Content and legal**
- [ ] Privacy, Terms, Returns, Shipping, Contact pages published and reviewed
- [ ] About page with a real photo and a real story
- [ ] OG preview verified by pasting a product link into WhatsApp

**Quality**
- [ ] LCP ≤ 2.5s on a real mid-range Android over 4G, on a product page
- [ ] Keyboard-only pass through browse → cart → checkout
- [ ] Contrast audit passed
- [ ] Tested on a real phone by someone who is not the developer
- [ ] Backup taken and a restore verified

**Operations**
- [ ] Non-technical operator has listed a product, start to finish, unaided, from a phone
- [ ] Owner has processed a full order in ≤ 5 minutes
- [ ] Order-confirmation and shipping messages verified as actually arriving

---

*Requirements only. Implementation paths and their trade-offs: [implementations.md](implementations.md).
Decision history: [Changelog.md](Changelog.md).*
