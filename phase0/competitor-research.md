# Competitor Research — What Indian Saree D2C Brands Actually Build On

Researched 2026-09-15 via web search. Decision-gate input for [banking.md](banking.md) Step 6.

⚠️ **Source-quality warning.** Most writing on Indian e-commerce stacks comes from agencies that
sell implementation services, and most performance figures come from vendor case studies, which are
marketing. Confidence is marked per finding. Nothing here is a substitute for the hands-on trials in
[stack-research.md](stack-research.md).

---

## 🟢 The strongest single data point: Suta

**Suta** — modern cotton and linen saree brand, supports ~14,000 weavers.

- **Platform: Shopify.** Confirmed by Shopify's own case study and by third-party tech detection.
- **Revenue: over $4 million**, with >10% from overseas.
- Runs roughly **18 additional tools** alongside Shopify (analytics, wishlist, ads).

The co-founders' stated reason, from Shopify's case study:

> "Shopify is such a sturdy platform that they don't have to be worried that the site is down...
> they don't even need a big team to maintain the Shopify platform."

**Why this matters more than the revenue number:** that quote is a direct answer to the exact
constraint driving our decision — [operator.md](operator.md) question 4, *could anyone other than
Jaya repair this*. A brand at $4M revenue chose not to own the platform. Our bus factor is 1 and our
revenue is a fraction of theirs.

*(Note: Suta sells cotton and linen at volume, mostly restockable — a different inventory shape from
one-of-a-kind handloom. The operational lesson transfers; the catalog lesson does not.)*

---

## 🟡 The industry default

Consistent across multiple independent sources: **Shopify is the default for Indian D2C, and the
reason is ecosystem maturity rather than the platform itself.**

The India-specific integrations — Razorpay Magic Checkout, GoKwik, Shiprocket, and the GST invoicing
apps — are **more mature on Shopify than anywhere else.** That is the real moat. Not features, but
the fact that every Indian commerce vendor ships a Shopify integration first and everything else
later.

This directly qualifies the [stack-research.md](stack-research.md) finding that Shopify lacks native
GST. True — but it has the largest population of GST apps precisely because it has the largest
Indian merchant base.

**Could not verify** the platforms behind Karagiri, Taneira (Tata), or Jaypore. Searches returned
brand marketing, not technical detail. Would need BuiltWith or Wappalyzer against each domain.

---

## 🔴 The finding that most concerns our project

From an analysis of merchants who went headless:

> "A headless store without a CMS means every content change is a developer deploy — the single most
> common operational regret from merchants who went headless."

**This is our Changelog decision of 2026-09-11, independently confirmed by people who learned it the
expensive way.** That entry reads: the operator model "eliminates any design where listing a product
requires editing a file, running a command, or triggering a deploy."

Path B and a headless Path C both land in exactly this trap unless the admin is built to the same
standard as the storefront — which is precisely the "second application" problem in
[nextjs-risk-annex.md](nextjs-risk-annex.md) §5.

---

## 🟡 Cost of the custom route, as quoted by Indian agencies

| Build type | Quoted range |
|---|---|
| D2C site, configured Shopify theme → headless | ₹1.5 lakh – ₹25 lakh |
| Headless specifically | **3–5× a standard theme; ₹5–15 lakh minimum** |

These are agency quotes and therefore self-interested in both directions — they sell both. Treat as
an order of magnitude, not a price.

**Relevance:** if Jaya builds it personally the cash cost is near zero and the cost is time instead.
But it sets the replacement cost — what it would take to hire someone if Jaya becomes unavailable.
That number is the bus-factor risk expressed in rupees.

---

## What competitors solve that we deliberately do not

The entire Indian D2C checkout tooling industry — **GoKwik, Razorpay Magic Checkout, Shiprocket
Checkout** — exists mainly to manage **COD and RTO**.

Reported (vendor case studies, so 🔴 treat as marketing):

- GoKwik: 200+ signals to score each buyer; address autofill for ~90% of shoppers; Pepe Jeans
  reported ~40% RTO reduction and ~20% prepaid lift; one brand moved from 70% COD to 65% prepaid
- Razorpay Magic Checkout: Thirdwatch fraud engine folded in since 2023; Borosil reported 36.36% RTO
  reduction

**Two conclusions:**

1. **Our no-COD decision is validated by the size of this industry.** An entire vendor category
   exists because COD hurts. We avoid the problem rather than buying tools to manage it. The
   Changelog entry excluding COD stands, and this is supporting evidence for the 90-day review.
2. **If COD is ever introduced (Phase 3), it is not a checkbox.** It comes with an RTO-scoring
   vendor, its own fee, and its own integration. Budget it as a project, not a setting.

**Worth noting for later:** address autofill is the one feature in these tools that helps *prepaid*
checkout too, and PRD §4.4 already calls pincode auto-fill "a small feature with a disproportionate
effect on completion." Cheap to do natively.

---

## What this changes

**Recommendation unchanged — Path A.** The evidence strengthens it:

- A $4M saree brand chose Shopify explicitly to avoid maintaining a platform
- The India integration ecosystem is deepest on Shopify, which is where GST, checkout, and shipping
  tooling actually lives
- The most-cited headless regret is exactly the failure mode our operator model forbids

**What it does not settle:** the 2% fee at scale. Nothing here contradicts the crossover maths in
[stack-research.md](stack-research.md), and Odoo and Zoho remain unexamined by this research because
almost nobody writes about them for Indian D2C fashion — which is itself a signal about how much
independent help exists if something breaks.

---

## Gaps in this research

- [ ] Platform detection for Karagiri, Taneira, Jaypore, Forsarees (BuiltWith / Wappalyzer)
- [ ] **Find a brand selling one-of-a-kind handloom at our scale** and see how they present sold
      pieces. Suta's model is restockable volume, not unique stock — the closer analogue is still
      missing
- [ ] Whether any Indian saree brand runs Odoo or Zoho Commerce. Absence of evidence here is
      meaningful for support availability
- [ ] What Shopify GST apps these brands actually use, and their review history

## Sources

- [Suta on Shopify — social impact case study](https://www.shopify.com/blog/suta-social-impact)
- [Suta tech stack detection](https://www.aftership.com/brands/suta.in)
- [Shopify vs WooCommerce for Indian D2C 2026](https://witscode.com/blogs/shopify-vs-woocommerce-india-d2c-2026)
- [Headless Shopify pricing breakdown](https://weaverse.io/blogs/shopify-headless-pricing)
- [D2C website development cost India 2026](https://codevibe.in/blog/d2c-website-development-india)
- [Razorpay vs GoKwik vs Shiprocket checkout guide](https://peakd2c.com/articles/razorpay-gokwik-shiprocket-d2c-checkout/)
- [RTO reduction tools for Indian D2C](https://www.clickpost.ai/blog/rto-reduction-tools)
- [GoKwik Kwik Checkout](https://www.gokwik.co/en_gb/product/kwikcheckout)
- [Why D2C brands switch to headless](https://digitalaptech.com/blogs/why-d2c-brands-are-switching-to-headless-commerce/)
