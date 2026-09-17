# Zero-Budget Path

Written 2026-09-15, after the owner stated investment is **near zero**.

⚠️ **This supersedes the budget assumption behind the Path A recommendation.** The earlier brief was
"budget is not a problem, or else low budget but scaling and features should not suffer." Near-zero
is a different constraint and it changes the sequence — though not, in the end, the eventual
destination.

---

## The reframe

With no money, **you do not need the website first.**

Go back to PRD §1.2. Five ranked problems. The website was the answer to all five at once. But the
two highest-severity ones can be solved **this week, for almost nothing, with no website at all:**

| # | Problem | Zero-cost fix | Cost |
|---|---|---|---|
| 1 | **Personal UPI account risk** | Razorpay/Cashfree **Payment Links** — a link per order, money lands in a business account | ₹0 setup, per-transaction only |
| 2 | **No order record / no invoice** | **Zoho Books Free** — real GST invoices, and GST return filing | **₹0** under ₹25L turnover |
| 3 | Manual labour per order | Not solved. Needs the site. | — |
| 4 | Reach ceiling | WhatsApp Business catalog + Instagram + ONDC | ₹0 |
| 5 | No inventory truth | Partly — Zoho tracks stock | ₹0 |

**Problem #1 is the highest-severity item in the whole PRD and it does not need a single line of
code.** It needs a current account and a payment link. Every week it goes unfixed is a week of
accumulating risk, and no budget is required to fix it.

This is a genuine revision to the 2026-09-11 entry on WhatsApp catalogs. That entry said platform
catalogs "solve browsing, not transacting." **Payment Links plus Zoho Books solve transacting too,**
without a website. What remains unsolved is problem #3 — the manual labour — and that is what
eventually justifies building the site.

---

## 🟢 Genuinely free, verified

| Tool | What it does | Limit |
|---|---|---|
| **Zoho Books Free** | GST invoices, **e-invoicing**, GST return filing (Zoho is a registered GSP — file GSTR-1 and GSTR-3B without the portal) | **Turnover < ₹25 lakh**, 1 user + 1 accountant, 1,000 transactions/year |
| **Razorpay / Cashfree** | Payment collection | ₹0 setup, ₹0 monthly. Per-transaction only (~2%) |
| **Razorpay Payment Pages / Links** | Hosted payment page, no website needed | Included |
| **WhatsApp Business app** | Catalog, business profile, quick replies | Free. (The *API* is not — that is P0-16) |
| **Instagram Business** | Discovery, shop tagging | Free |
| **Google Business Profile** | Local search presence | Free |
| **Google Search Console + Analytics** | Traffic and indexing | Free |
| **Cloudflare** | DNS, CDN, SSL | Free tier |
| **Canva** | Logo, social creatives | Free tier |
| **Photopea / GIMP / Squoosh** | Image editing, crop to 4:5, compression | Free |
| **Shiprocket** | Courier aggregation | Free signup, pay per shipment |

### ONDC — worth a serious look

Government-backed open network. **Onboarding is free.** No central network commission; seller apps
charge roughly **1–3%** versus the 20–30% typical of marketplaces. Reported at 700,000+ merchants
across 1,200+ towns by early 2026.

You list through a seller app (Mystore, Bizcom, e-Samudaay and others). It needs GST and bank details
— which Phase 0 produces anyway.

**Why it fits here:** it attacks problem #4 (reach) at near-zero cost, without building anything, and
without handing a marketplace 25% of every sale. 🟡 Verify current seller-app fees and how unique
quantity-1 stock is handled before committing a catalog to it.

---

## What is NOT free, honestly

| Item | Realistic cost | Avoidable? |
|---|---|---|
| Domain (`.in` or `.com`) | ~₹700–1,200/year | Temporarily — use a free subdomain, but move to a real domain before you build a brand on it |
| CA fees | 🟡 varies, ~₹3,000–10,000 to set up | **No.** Do not skip this to save money |
| Current account minimum balance | Varies by bank | Shop for a low/zero-MAB current account — it is a shortlist criterion in [banking.md](banking.md) |
| Gateway per-transaction | ~2% + GST | No, and it should not be — it replaces the personal-account risk |

**True unavoidable starting cash: roughly ₹4,000–12,000**, almost all of it the CA and the domain.
🟡 Verify locally.

**Do not economise on the CA.** A wrong GST position costs more than every other line here combined,
and it is the one thing on this list you cannot fix later without a correction exercise across every
past invoice.

---

## The trade nobody states plainly

**Zero money means paying in time instead.** That is a real currency and yours is not free — the
personal-account exposure keeps running while you build.

| Path | Cash | Your time |
|---|---|---|
| Shopify Basic | ₹1,499/mo + 2%/order | ~2–3 weeks setup, ~0 maintenance |
| WooCommerce | ~₹1,000/mo hosting | 4–6 weeks setup, **4–8 hrs/month forever** |
| Next.js free tiers (Vercel + Supabase + Cloudflare R2) | ~₹0 at low volume | **10–14 weeks**, 6–12 hrs/month forever, and every failure mode in [nextjs-risk-annex.md](nextjs-risk-annex.md) |

Free hosting tiers are real and generous at your volume. **The cost was never the hosting.** It is
the 10–14 weeks and the permanent on-call, against a bus factor of 1.

⚠️ **The free-tier trap:** free tiers are free until they aren't. A traffic spike, a database size
limit, or a pricing change lands on you without warning, mid-festival-season. Know each tier's limit
before you depend on it.

### Free store builders — checked, mostly unsuitable

A handful publish a live store for ₹0 (SitesPlaced, Shopexer, StartupInMinutes, Shop2Host). 🟡
Shop2Host's free tier reportedly caps at **10 products**. Your catalog is 50–300 SKUs. Amazon
SmartBiz reportedly waives subscription and transaction fees until October 2026 — a promotional
window, and Amazon lock-in.

**None of these survive PRD §7.3 or the unique-stock requirement.** A free tier that caps at 10
products is not a saree store.

---

## The sequence, with no money

### Now — this week, ₹0

1. **WhatsApp Business app.** Free, ~2 hours. Catalog up, business profile, quick replies.
2. **Instagram Business account.** Free. Post the existing professional photos.
3. **Send the CA enquiries.** Already drafted in [ca-engagement.md](ca-engagement.md).
4. **Total three months of WhatsApp payment messages.** This is Q10, it costs an hour, and every cost
   figure in this project is a guess until it exists.

### Weeks 2–6 — Phase 0, ~₹4,000–12,000

Unchanged. CA, GST position, current account. Plus:

5. **Zoho Books Free the day the GSTIN arrives.** Real invoices from order one.
6. **Razorpay or Cashfree account.** No monthly fee, so open it even before the site exists.
7. **🚨 Cut over from the personal account.** Payment Links instead of personal PhonePe. **This is
   the whole point and it costs nothing.**

### Weeks 6+ — sell with no website

8. WhatsApp catalog for browsing → Payment Link for paying → Zoho Books invoice → Shiprocket label.
9. **Measure.** How many orders? How many hours per order? That is the real business case for the
   site, and it will be evidence rather than assumption.
10. Optional: ONDC listing for reach at ~1–3%.

### Then — build, funded by revenue

**Do not build until step 9 produces a number that justifies it.**

At that point the earlier analysis applies unchanged: Shopify's ₹1,499/mo + 2% is trivially
affordable at any volume that makes the site worth building. **If ₹1,499/mo is unaffordable, the
order volume does not justify a website yet** — and that is information, not failure.

---

## What to hold for later

### Hold until the site exists
Abandoned-cart recovery · discount codes and festival campaigns · back-in-stock notifications ·
reviews with photos · Instagram/Meta catalog sync · loyalty · gift cards

### Hold until revenue justifies it
**DLT + WhatsApp BSP (P0-15/16)** — per-message costs and weeks of approvals. **Manual WhatsApp
messages are free and you are already sending them.** Automate when the volume makes it worth the
cost, not before. *(This is a change from the plan's framing: still stack-independent, still weeks of
waiting, but no longer urgent at 5 orders/week.)*

Live weight-based shipping rates · Google Shopping feed · email marketing tooling · a custom
storefront over any platform

### Hold indefinitely, on the evidence
**COD.** An entire vendor industry (GoKwik, Magic Checkout) exists to manage its damage. At zero
budget you cannot absorb RTO losses. This strengthens the existing no-COD decision.

**International shipping. Mobile app. Marketplace. Wholesale.** Already out of scope per PRD §12.

### Never — regardless of budget
Fabricated reviews · fake MRPs · fake scarcity · skipping the CA · business money into a personal
account.

---

## What this changes in the recommendation

**Path A (Shopify) remains correct — for when you build.** Nothing about zero budget makes
maintaining a VPS cheaper; it makes it more expensive, because your time is now the scarce resource
and the on-call burden lands on the one person who cannot be replaced.

**What changes is the timing.** Build second, not first. Fix the payment rail and the invoice trail
now — near-free — and let order volume decide when the site earns its subscription.

**Reverses if:** order volume or manual-handling pain grows faster than expected, in which case the
site is justified sooner. Or if the free Zoho/Payment-Links loop turns out to be so workable that a
site is not needed for longer than expected. Both are measurable at step 9.

---

## Sources

- [Zoho Books free edition for India](https://www.zoho.com/blog/books/free-edition-of-zoho-books-india.html)
- [Zoho Books free plan with GSTN partnership](https://www.zoho.com/blog/books/zoho-books-introduces-free-plan-for-businesses-with-gstn.html)
- [Razorpay Payment Pages as an online store](https://razorpay.com/blog/payment-pages-online-store-business/)
- [Razorpay for freelancers and individual businesses](https://razorpay.com/freelancer-individual-business/)
- [ONDC seller guide 2026](https://unicommerce.com/blog/what-is-ondc/)
- [ONDC seller onboarding guide](https://www.digicommerce.in/blog/ondc-seller-onboarding-guide/)
- [Free online store builders India 2026](https://sitesplaced.com/best-free-online-store-builders-india)
- [Amazon SmartBiz](https://smartcommerce.amazon.in/smartbiz)
