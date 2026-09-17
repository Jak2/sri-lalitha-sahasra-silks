# Stack Research — Perplexity, 2026-09-15

Decision-gate input for [banking.md](banking.md) Step 6. **Research notes, not decisions.**

Source: Perplexity, queried 2026-09-15. Confidence is marked per figure. **Anything below marked
🟡 or 🔴 must be confirmed on the provider's own page before it drives the decision.**

---

## 🟢 Confirmed — Shopify's additional India fee

Shopify Payments does not operate in India, so a third-party gateway is mandatory and Shopify
charges an additional platform fee on top of the gateway's own fee.

| Plan | Extra Shopify fee | Plan cost (annual billing) |
|---|---|---|
| **Basic** | **2.0%** | ₹1,499/mo |
| Grow | 1.0% | ₹5,599/mo |
| Advanced | 0.6% | ₹22,680/mo |
| Plus | 0.2% | — |

Cited from shopify.com/in/pricing, read 2026-09-15. **This was the decisive open number in
04-implementations.md §7.2.** Re-read on the day of the decision — plan pricing changes.

### Plan crossover

Basic (₹1,499 + 2%) vs Grow (₹5,599 + 1%) break even at **~₹4.1 lakh monthly GMV**. Below that,
Basic. Above it, Grow is cheaper despite the higher subscription. Recompute if either figure moves.

---

## 🟡 Blog-sourced — gateway fees. NOT read from pricing pages.

Perplexity could not fetch the official pricing pages and used each provider's blog instead. It
said so, which is to its credit — but these are not confirmed.

| Gateway | Domestic rate | Note |
|---|---|---|
| Razorpay | 2% + 18% GST on the fee ≈ **2.36% effective** | Blog, not pricing page |
| Cashfree | 1.95% + 18% GST ≈ **2.30% effective** | Blog, not pricing page |
| PayU | ~2% | **Not verified at all** |

**Settlement:** all three ~T+2 standard.

**Refunds:** the original MDR is **not returned** when you refund a customer. No separate refund fee
advertised, but the fee on the original payment is lost. On a category where returns are normal,
budget for it.

### 🔴 Cashfree promotional pricing — verify before relying on it

Claim: 0% on eligible domestic GMV up to ₹20 lakh/month, until 31 March 2027, for new merchants.

If true this is material. **It is also a trap if treated as permanent.** Build the margin model on
the standard 1.95%, and treat any promo as temporary upside. A business whose unit economics only
work during a promotional window does not have working unit economics.

---

## GST invoicing — native vs bolted on

The requirement: per-line HSN, per-product GST rate, automatic CGST+SGST vs IGST by place of supply,
gapless sequential numbering.

| Platform | Native? | Reality |
|---|---|---|
| **Odoo** (Accounting + `l10n_in`) | **Yes** | Indian localisation with GST reports, e-invoicing, e-way bills |
| **Zoho Commerce** | Via **Zoho Books** | Commerce alone is not enough; Books does the compliance |
| **Shopify** | **No** | Needs a third-party app, ~₹400–1,500/mo |
| **WooCommerce** | **No** | Needs a plugin or a Tally/Zoho integration |
| **Medusa** | **No** | Would have to be built. No Indian GST plugin exists. |

**Zoho Books free tier:** reportedly free under ₹25 lakh annual turnover, then ~₹749–899/month per
organisation. 🟡 Verify on Zoho's India page — this materially changes the Zoho total.

**Odoo edition question is muddled in the source.** It claims advanced GST needs Enterprise, citing a
partner blog rather than Odoo. Note that Odoo Online's *paid* plans are Enterprise-tier already, so
the Standard plan likely includes it. 🟡 **Ask Odoo directly: "which edition do I need for GST
return filing and e-invoicing."**

**Odoo pricing:** Standard $31.10/user/month after a first-year promo ≈ ₹2,580/user/month, all apps
included. Two internal users ≈ ₹5,160/mo. Customers are free users.

---

## Operations

### Mobile admin — PRD §7.3 hard constraint

| Platform | Mobile admin |
|---|---|
| **Shopify** | **Dedicated app, best in class.** Full product management, multi-photo upload |
| WooCommerce | Dedicated app, solid |
| Odoo | Real app, full-featured, but dense UI |
| **Zoho Commerce** | **No dedicated Commerce admin app — mobile browser only** |
| Medusa | No app. Developer-oriented web admin. |

⚠️ **This partially cancels Zoho's cost advantage.** PRD §7.3: "Photos are on the phone. Requiring a
laptop to list a product means products do not get listed." Test this personally before deciding.

### Concurrent purchase of a quantity-1 item

| Platform | Verdict |
|---|---|
| Shopify | Safe — inventory committed at order placement, oversell protection |
| Odoo | Safe — single stock pool, reservation at confirmation |
| WooCommerce | Safe **if configured correctly** — `wp_wc_reserved_stock` with row-level locking since 4.3. Misconfiguration (hold_stock = 0, bad caching) causes oversells |
| Zoho | Designed to prevent it; concurrency behaviour less publicly documented |
| **Medusa** | 🔴 **Known oversell risk under concurrent checkout** without custom locking (single blog source, Medusa 2.19.0) |

The Medusa finding, if it holds, directly confirms PRD §3.6 — the requirement that "each path must
state explicitly how this is guaranteed" was the right requirement to impose.

---

## ⚠️ Where the research's own conclusion is wrong

Perplexity ranked the options by **monthly cash cost** and concluded Shopify is a "poor fit" and
WooCommerce the cheapest at ~₹10,500/mo vs Shopify's ~₹18,000/mo.

**It assigned zero cost to maintenance hours.** That is the error.

04-implementations.md §6 puts WooCommerce/Medusa-class options at 4–12 hours per month, forever, with
Jaya on call and a bus factor of 1. The ~₹7,500/month gap buys away 50–100 hours a year of unpaid
sysadmin work plus the outage risk. Perplexity compared *cash*, not *cost*.

It also conflated two separate arguments against Shopify. GST is not the problem — that is an
₹800/month app. **The 2% is the problem**, and only at scale.

### The 2% in perspective

| Monthly GMV | Shopify's extra 2% |
|---|---|
| ₹1,00,000 | ₹2,000/mo |
| ₹3,60,000 | ₹7,200/mo |
| ₹10,00,000 | ₹20,000/mo |

**Cheap insurance now. Expensive later.** Which is exactly why the Path A recommendation was
recorded as *reversible*, with a Phase 4 re-evaluation against real numbers.

---

## What this does and does not change

**Does not change the recommendation.** Path A (Shopify) still leads on the two criteria that
dominate at this stage: fastest removal of the personal-account risk, and best operator experience
on a phone with a bus factor of 1.

**Changes the fee model.** Basic = 2%. The Basic→Grow crossover is ~₹4.1L monthly GMV.

**Adds two genuine contenders** — Odoo and Zoho Commerce — both stronger than Shopify on GST and
neither charging a platform percentage. Zoho is handicapped by the missing mobile admin app; Odoo by
weight and storefront performance.

**Confirms Medusa's risk.** Not disqualifying on its own, but it raises Path C's cost.

---

## Still missing — the number that actually decides it

**Real monthly GMV from the last 3 months of WhatsApp sales.** Every figure above is modelled on an
assumed AOV of ₹3,000 and 120 orders/month. Nobody but the owner has the real number, and without
it the entire fee comparison is arithmetic on a guess.

## Verify before the gate

- [ ] Shopify India pricing page — plan cost and third-party fee, on the day
- [ ] Razorpay and Cashfree fees from their own pricing pages, not blogs
- [ ] Cashfree's 0% promo — real, eligibility, and the cap
- [ ] Zoho Books free-tier turnover threshold, from Zoho India
- [ ] Odoo — which edition for GST filing and e-invoicing, from Odoo
- [ ] **Hands-on test: list one saree with 6 photos from a phone, on Shopify, Zoho, and Odoo trials**
- [ ] Ask the CA which system they would rather receive records in
- [ ] Real GMV, last 3 months
