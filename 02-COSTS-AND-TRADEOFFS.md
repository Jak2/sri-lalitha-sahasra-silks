# What ₹0 Actually Costs You

One ledger. What you give up, what you do not, and what every skipped thing would cost to buy back.

**Confidence marking:** 🟢 verified from research · 🟡 estimate, verify locally · 🔴 varies widely.
Nothing here is a quote. Prices change; check before committing.

---

## The one-line answer

**You are not sacrificing legitimacy, compliance, or correctness. You are sacrificing growth and
your own time.**

Everything that makes the business *legal and trustworthy* is free or nearly free. Everything that
makes it *scale without you* costs money.

---

# Part 1 — What you are NOT sacrificing

This matters more than the list below it, because these are the things people assume you must pay for.

| Thing | How it stays free | Real limit |
|---|---|---|
| **GST-compliant invoices** | 🟢 Zoho Books Free | Under ₹25L turnover, 1,000 invoices/year |
| **GST return filing (GSTR-1, GSTR-3B)** | 🟢 Zoho is a registered GST Suvidha Provider — file from inside Zoho Books | Same |
| **E-invoicing** | 🟢 Included in Zoho Books Free | Same |
| **Order records, searchable** | 🟢 Zoho Books | Same |
| **Secure payment collection** | 🟢 Razorpay/Cashfree Payment Links | ₹0 monthly, ~2% per transaction |
| **PCI compliance** | 🟢 Gateway-hosted — card data never touches you | None |
| **Business banking legitimacy** | Current account | MAB, see below |
| **Courier + tracking** | 🟢 Shiprocket free signup | Pay per shipment |
| **Catalog data** | Your spreadsheet — 🟢 portable to every platform | None |
| **Legal pages** | 🟢 Already drafted in this repo | Needs CA review |
| **Product photography** | 🟢 Already exists | Verify drape-shot coverage |
| **The option to build later** | Nothing is lost. Data exports. | None |

**Nobody can tell the difference between a ₹0 operation and a ₹50,000 one on any line above.** A
customer receiving a proper GST invoice from Zoho Books cannot tell it did not come from a
₹1,499/month platform.

---

# Part 2 — What you ARE sacrificing

Honestly, in order of how much it costs you.

### 1. Your time — the largest cost by far 🔴

Every order costs a conversation, a manual payment link, a manual address collection, a manual
invoice trigger.

**PRD §1.5 target: ≤5 minutes per order. Realistic manual loop: 15–25 minutes.**

At 30 orders/month that is **7–12 hours a month of your life**, permanently, and it grows linearly
with success. This is PRD problem #3, entirely unsolved by the free path.

> **This is the real price of ₹0.** Not a feature you lack — hours you spend.

### 2. Discovery — no Google traffic

No website means no search presence. Nobody searching "kanjivaram silk saree online" will ever find
you. You reach only people already in your circle or referred into it.

PRD problem #4, unsolved. **Partially recoverable free** via Instagram and ONDC.

### 3. Selling to strangers

Persona B — the referred stranger — decides in about four seconds whether you are real. Right now
there is no About page, no return policy visible, no verifiable address, no trust layer.

A WhatsApp catalog from an unknown number converts a stranger far worse than a product page does.
**You will keep converting friends-of-friends well and strangers badly.**

### 4. Inventory truth for unique pieces

Zoho tracks your stock. **The buyer cannot see it.** Two people can message you about the same
one-of-a-kind saree, and you resolve it by hand, by apology.

PRD calls double-selling "the fastest way to lose a customer permanently." Manual handling makes it
*likely*, not impossible.

### 5. Self-serve buying

Nobody can buy at 11pm while you sleep. Your sales are capped by your waking hours, not by demand.

### 6. Knowing what happened

No analytics. You will not know which sarees were viewed and not bought, where people dropped off, or
what they searched for. **You are flying on instinct.**

### 7. Automated order updates

No DLT, no WhatsApp API — you type every confirmation, every tracking number.

### 8. A ceiling

This loop works up to roughly **30–50 orders a month.** Past that it stops being tight budgeting and
starts being a bottleneck that costs you sales.

---

# Part 3 — The cost ledger

## Tier 0 — Unavoidable. Spend this or do not start.

| Item | Cost | What happens if you skip it |
|---|---|---|
| **CA — setup** (structure + GST position + registration) | 🟡 ₹3,000–10,000 one-time | Wrong GST rate on every invoice. A correction exercise across all past orders. **Never skip.** |
| **CA — ongoing compliance** (returns filed on schedule) | 🟡 ₹500–2,000/month | Penalties and notices. Zoho Books Free reduces the work, not the requirement. |
| **Domain** (`.in` or `.com`) | 🟡 ₹700–1,200/year | You build a brand on someone else's subdomain and cannot move it later |
| **Current account minimum balance** | 🔴 ₹0–25,000 **locked, not spent** | No legitimate way to receive business payments. **Shop hard on MAB** — some banks offer zero-balance current accounts |
| **Gateway fees** | 🟢 ~2% + 18% GST on the fee ≈ **2.36% per order** | Only alternative is personal UPI, which is the risk this whole project exists to remove |

**Year-one Tier 0 cash: 🟡 ~₹10,000–35,000**, most of it CA fees, plus whatever MAB is locked.

## Tier 1 — The website. Buys back items 1, 2, 3, 4, 5, 6.

| Item | Cost | Notes |
|---|---|---|
| Shopify Basic | 🟢 ₹1,499/month (annual billing) | ₹17,988/year |
| **Shopify's extra fee** | 🟢 **2% of every order** | Because Shopify Payments does not work in India |
| Gateway fee (unavoidable anyway) | 🟢 ~2.36% | Not a new cost |
| Theme | 🟡 ₹0 free themes, or ₹15,000–25,000 one-time premium | Free themes are genuinely fine at launch |
| GST invoicing | **₹0** — keep Zoho Books Free | 🟡 Or a Shopify GST app at ₹400–1,500/month. **Keep Zoho.** |

**Annual cost at ₹3.6L monthly GMV:** 🟡 ₹17,988 subscription + ~₹86,400 in the 2% ≈ **₹1,04,000/year**

⚠️ **The subscription is the small half.** The 2% is the real cost and it scales:

> **GMV = Gross Merchandise Value** — the total rupees of goods sold in a month, before any costs are
> taken out. Orders × average order value. It is **not** profit. 20 sarees at ₹3,000 = ₹60,000 GMV.
>
> **"2% of GMV" means Shopify takes 2 paise of every ₹1 that passes through the store**, on top of
> the subscription, because Shopify Payments does not operate in India and a third-party gateway is
> therefore mandatory. On a ₹3,000 saree that is **₹60 per order**. It is charged on the sale value,
> not on profit — so on a thin-margin sale it eats a larger share of what you actually keep.

| Monthly GMV | Orders at ₹3,000 | Shopify 2% per month | Shopify 2% per year |
|---|---|---|---|
| ₹1,00,000 | ~33 | ₹2,000 | ₹24,000 |
| ₹3,60,000 | ~120 | ₹7,200 | ₹86,400 |
| ₹10,00,000 | ~333 | ₹20,000 | ₹2,40,000 |

**The gateway's own ~2.36% is separate and unavoidable on every path.** Shopify's 2% is charged
*additionally*, and is the only part of it that disappears on a non-Shopify path.

**This is why Stage 4 of [01-START-HERE.md](01-START-HERE.md) is gated on measured volume.** At ₹1L/month
GMV the whole thing costs ~₹42,000/year and buys back 7–12 hours a month. At ₹10L it costs ₹2.6L/year
and Path C starts making sense.

### The cheaper website options, honestly

| | Cost/year | What you give up instead |
|---|---|---|
| **WooCommerce** | 🟡 ₹12,000–24,000 hosting, no % fee | **4–8 hrs/month of maintenance, forever.** You are the sysadmin |
| **Zoho Commerce** | 🟡 ₹7,000–21,000, no % fee | 🔴 No mobile admin app — collides with listing products from a phone |
| **Odoo** | 🟡 ~₹31,000/user/year, no % fee | Heavy; storefront speed is a fight; overkill unless a shop or POS shares it |
| **Next.js on free tiers** | ~₹0 hosting | **10–14 weeks of your time + 6–12 hrs/month forever** |

**At ₹1L/month GMV, Shopify's total (~₹42k/year) is close enough to WooCommerce's hosting (~₹18k)
that the ₹24k difference buys away 50–100 hours a year of sysadmin work.** That is ₹240–480/hour for
work you do not want to do, on a system only you can repair.

## Tier 2 — Automation. Buys back item 7.

| Item | Cost | Worth it when |
|---|---|---|
| DLT registration | 🟡 Often free or a small one-time fee via the telecom operator | Required before any SMS |
| Transactional SMS | 🟡 ~₹0.12–0.25 per message | Cheap. ~₹50–100/month at your volume |
| WhatsApp BSP platform fee | 🟡 ₹1,000–3,000/month minimum | **Skip until volume justifies it** |
| WhatsApp per-conversation | 🟡 ₹0.10–0.80 per conversation | Meta's rates, via the BSP |

**Verdict: skip entirely for now.** At 30 orders/month you would pay ~₹1,500/month to avoid typing
30 messages. **Typing 30 messages is cheaper than ₹18,000/year.** Revisit at ~100 orders/month.

## Tier 3 — Growth. Optional, and later.

| Item | Cost | Note |
|---|---|---|
| Drape-shot reshoot, if missing | 🔴 ₹5,000–25,000 | **Could become the critical path.** PRD §3.3 calls the drape shot non-optional. Audit coverage first — it may be ₹0 |
| Instagram/Meta ads | 🔴 Any amount | Do not start until the site converts. Paying to send traffic to a leaky funnel is the most common way small budgets die |
| Logo / brand identity | 🟡 ₹0 (Canva) to ₹15,000 | Canva is genuinely fine at launch |
| ONDC listing | 🟢 Free onboarding, ~1–3% per sale | vs 20–30% on marketplaces. Worth testing |

---

# Part 4 — Three budgets

## ₹0/month — where you are

**Cost:** Tier 0 only. 🟡 ~₹10,000–35,000 in year one.
**You get:** legal, invoiced, recorded, bank-legitimate selling.
**You pay in:** 7–12 hours a month, no discovery, no strangers, a ~30–50 order/month ceiling.
**Right when:** order volume is under ~30/month. **This is where you are, and it is the correct
choice right now.**

## ~₹1,500/month — the website

**Cost:** Tier 0 + Shopify. 🟡 ~₹42,000/year at ₹1L monthly GMV.
**You buy back:** your time, Google discovery, stranger conversion, real-time stock display,
24-hour selling, analytics.
**Right when:** 30+ orders/month, or manual handling is costing hours you do not have.

## ~₹3,000/month — website + automation

**Cost:** adds Tier 2.
**You buy back:** typing every order message.
**Right when:** ~100+ orders/month. **Not before** — the arithmetic does not work below that.

---

# Part 5 — The only number that decides any of this

Everything above is modelled on an assumed ₹3,000 average order value and 120 orders a month.
**Those are guesses.**

**Total three months of your WhatsApp payment messages.** One hour.

| | Your number |
|---|---|
| Orders per month | `_____` |
| Average order value | ₹`_____` |
| Monthly GMV | ₹`_____` |
| **Minutes of your time per order** | `_____` |

With those four numbers, the ₹0 vs ₹1,500 decision stops being a judgement call and becomes
arithmetic:

> **Monthly GMV × 2% + ₹1,499 versus (orders × minutes saved) valued at whatever your hour is worth.**

Until then, everything here — including my recommendation — is an educated guess dressed up in a
table.

---

*Reasoning: [phase0/zero-budget-plan.md](phase0/zero-budget-plan.md) · Fee research and confidence
marking: [phase0/stack-research.md](phase0/stack-research.md) · Action checklist:
[01-START-HERE.md](01-START-HERE.md)*
