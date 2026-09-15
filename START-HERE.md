# Start Here — Selling Properly on ₹0

**The working checklist.** Reasoning lives in [phase0/zero-budget-plan.md](phase0/zero-budget-plan.md);
this file is what you actually do, in order.

> **The point of everything below:** stop business money landing in a personal account, and start
> producing real invoices. Both are nearly free. Neither needs a website.

**Rule:** do not skip to Stage 4. The website is the last step, not the first.

**Why the website is last.** Of the five problems this project exists to solve, the two most
dangerous — business money in a personal account, and no invoice trail — do not need a website. They
need a bank account and two free accounts. Building first would mean spending weeks solving the
*third* problem while the *first* one keeps running.
→ Full reasoning in [README.md](README.md#the-recommendation-in-plain-words).

---

## Stage 1 — This week · ₹0 · about 4 hours

Nothing here costs money or needs anyone's approval.

- [ ] **Total 3 months of WhatsApp sales** — every PhonePe payment message, added up
  - Orders per month: `_____`
  - Total received per month: ₹`_____`
  - Average order value: ₹`_____`
  - **⏱️ ~1 hour. This is the single most valuable hour in the whole project.** Every cost figure in
    these documents is currently modelled on a guess. This replaces the guess.
- [ ] **WhatsApp Business app** — install, business profile, catalog with your existing photos
  - ⏱️ ~2 hours · Free · Keep your existing number here
- [ ] **Instagram Business account** — convert or create, post the professional shots
  - ⏱️ ~30 min · Free
- [ ] **Send the CA enquiry to two CAs** — text is written and ready in
      [phase0/ca-engagement.md](phase0/ca-engagement.md), just fill in your turnover figure
  - ⏱️ ~15 min · **Do this first — everything in Stage 2 waits on it**

**Done when:** you know your real monthly numbers, two CAs have your enquiry, and a buyer can browse
a catalog instead of scrolling a group chat.

---

## Stage 2 — Weeks 2–6 · ~₹4,000–12,000 · mostly waiting

External timelines. Start everything in parallel; do not sequence it.

- [ ] **Engage a CA** → [phase0/ca-engagement.md](phase0/ca-engagement.md)
  - **Do not economise here.** A wrong GST position costs more than everything else combined and
    cannot be fixed later without correcting every past invoice.
- [ ] **Legal structure decided and registered** → [phase0/legal-structure.md](phase0/legal-structure.md)
- [ ] **Written GST position** — rates and HSN codes per category, in writing, from the CA
      → [phase0/gst-position.md](phase0/gst-position.md)
- [ ] **GST registration**, if required
- [ ] **Business current account** → [phase0/banking.md](phase0/banking.md)
  - Shop on **minimum average balance** first. It is the only criterion that costs money every month
    whether or not you trade.
- [ ] **Brand name + domain** → [phase0/brand.md](phase0/brand.md) · ~₹700–1,200/year
  - Auto-renew **ON**. Registrar 2FA **ON**.

**Meanwhile, free and in parallel:**

- [ ] Weigh 10 sarees packed → [phase0/shipping.md](phase0/shipping.md) · ⏱️ 1 hour
- [ ] Enter 3 pilot catalog rows → [phase0/catalog/](phase0/catalog/) · ⏱️ 1 hour
- [ ] Confirm the return policy line by line → [phase0/legal-pages/return-refund-cancellation.md](phase0/legal-pages/return-refund-cancellation.md)
- [ ] Write the About story, 200–400 words, your voice → [phase0/content/about.md](phase0/content/about.md)

---

## Stage 3 — The cutover · ₹0 monthly · **the whole point**

The moment this stage completes, the highest-severity risk in the project is gone.

- [ ] **Zoho Books Free** — set up the day your GSTIN arrives
  - **Free under ₹25 lakh turnover.** 1 user + 1 accountant, 1,000 transactions/year
  - GST invoices, e-invoicing, and GSTR-1/GSTR-3B filed directly — Zoho is a registered GST Suvidha
    Provider, so no portal
  - Give your CA the accountant login
- [ ] **Razorpay or Cashfree account** — ₹0 setup, ₹0 monthly, per-transaction only (~2%)
  - Open it even with no website. Payment Links work standalone.
  - ☐ **Settlement account set to the business current account — verify it is NOT personal**
- [ ] **🚨 Set a hard cutover date:** `____________`
  - From that date, **no business payment goes to your personal account.** Buyers get a Payment Link.
- [ ] Shiprocket account — free signup, pay per shipment
- [ ] Ship one test parcel to a friend in another state

### Your daily loop after Stage 3

```
WhatsApp catalog  →  buyer picks
      ↓
Payment Link      →  money to the BUSINESS account
      ↓
Zoho Books        →  real GST invoice, emailed
      ↓
Shiprocket        →  label, pickup, tracking
```

**Still manual. But legal, recorded, and invoiced.** That is the gap that mattered.

---

## Stage 4 — Only when the numbers say so

**Do not build before this gate.**

Track for 4–8 weeks after Stage 3:

| Measure | Your number |
|---|---|
| Orders per month | `_____` |
| **Minutes of your time per order** | `_____` |
| Monthly revenue | ₹`_____` |
| Sales lost to "no website" | `_____` |

### The decision rule

> **If ₹1,499/month is unaffordable, the order volume does not yet justify a website.**
> That is information, not failure.

**Build when:** manual handling is eating hours you do not have, or buyers are asking for a site, or
revenue makes ₹1,499/month trivial.

**Then:** [README.md](README.md) for the feature scope, [implementations.md](implementations.md) §7
for the decision gate. **Path A (Shopify) is the standing recommendation** — re-read
[phase0/stack-research.md](phase0/stack-research.md) before committing, and check the fee figures on
Shopify's own page that day.

**The build itself is written up step by step in
[phase0/shopify-build-guide.md](phase0/shopify-build-guide.md)** — including how much design freedom
Shopify actually gives you. Do not open it before this gate clears.

---

## Free tools worth having

| Tool | For | Note |
|---|---|---|
| **Zoho Books Free** | GST invoices + return filing | Free under ₹25L turnover |
| **Razorpay / Cashfree** | Payments, Payment Links | ₹0 monthly, ~2% per transaction |
| **WhatsApp Business app** | Catalog, quick replies | The *API* costs money — not needed yet |
| **Instagram Business** | Discovery | Links out to a destination in India |
| **Google Business Profile** | Local search | Free |
| **Shiprocket** | Courier | Free signup |
| **Photopea / Squoosh** | Crop to 4:5, compress images | Browser-based, free |
| **Canva** | Logo, social posts | Free tier |
| **Cloudflare** | DNS, SSL, CDN | Free tier, for later |
| **ONDC** | Reach, ~1–3% vs 20–30% marketplace | 🟡 Verify unique-stock handling first |

---

## Hold for later — deliberately

**Until the site exists:** abandoned-cart recovery · discount codes · back-in-stock alerts · reviews
with photos · Instagram/Meta catalog sync · loyalty · gift cards

**Until revenue justifies the cost:** **DLT registration + WhatsApp Business API.** Automated order
messages cost per message and take weeks of approval. Manual WhatsApp messages are free and you are
already sending them. → [phase0/messaging.md](phase0/messaging.md)

Also: live shipping rates · Google Shopping feed · email marketing tools · a custom storefront

**Indefinitely, on the evidence:** **COD.** An entire vendor industry exists to manage its damage. At
zero budget you cannot absorb return-to-origin losses.

**Never, regardless of budget:** fabricated reviews · fake MRPs · fake scarcity · skipping the CA ·
business money into a personal account.

---

## If you do only one thing

**Stage 1, item 1.** Total three months of WhatsApp payments.

Every rupee figure across this project rests on an assumed ₹3,000 average order value. You are the
only person who has the real number, and nothing can be decided properly without it.
