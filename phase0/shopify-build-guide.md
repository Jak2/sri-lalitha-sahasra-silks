# Building the Shopify Store — Design Freedom and a Step-by-Step Build

**Read the gate first.** This document is *how*, not *whether*. The decision to build at all is gated
in [../01-START-HERE.md](../01-START-HERE.md) Stage 4: if ₹1,499/month is unaffordable, the order volume
does not yet justify a website. Nothing below should be started before that gate clears.

**Confidence marking:** 🟢 verified · 🟡 estimate, verify before committing · 🔴 varies widely.

---

# Part 1 — Can you use a custom design, or only their templates?

## The short answer

**You can use a fully custom design.** Shopify is not a closed template system. You own the theme
code: HTML, CSS, JavaScript, and Shopify's templating language, **Liquid**. You can edit every file,
or write a theme from scratch with nothing carried over from any template.

What you cannot customise is **checkout** — and that is a deliberate, security-relevant boundary,
not a limitation to fight. See "The real boundaries" below.

## The four levels of design control

Pick the lowest level that gets the look you want. Each level up costs more time and more ongoing
maintenance.

| Level | What you change | Who can do it | Upgrade-safe? |
|---|---|---|---|
| **1. Theme editor** | Colours, fonts, spacing, section order, images, text | The operator, no code | 🟢 Yes |
| **2. Sections and blocks** | Add/remove/reorder page components on *any* page | The operator, no code | 🟢 Yes |
| **3. Custom sections + custom CSS** | New components you design; targeted style overrides | Jaya, some code | 🟡 Mostly |
| **4. Custom theme** | Every template, entire markup and design system | Jaya or an agency | 🔴 No — you maintain it |

**Level 2 is the one people do not know exists.** Since Online Store 2.0, Shopify uses JSON
templates, so **sections work on every page type**, not just the home page — product pages,
collection pages, blog pages. That was the old restriction, and it is gone. Every page type except
`gift_card` and `robots.txt` uses JSON templates now.

Practically: most "I need a custom theme" requests are satisfied at Level 2 or 3.

## The real boundaries

These are hard, on every plan below Shopify Plus:

| Boundary | What it means | Does it matter here? |
|---|---|---|
| **Checkout is not editable** | Below Plus, you cannot change checkout markup. You get branding — logo, colours, fonts — through Checkout Extensibility, plus approved app blocks | **No, and this is good.** An uneditable checkout is why card data never touches you and PCI scope stays minimal — the non-negotiable rule in [payments.md](payments.md) |
| **`checkout.liquid` is retired** | The old way of customising checkout is gone, replaced by Checkout Extensibility | Only affects stores that had it |
| **URL structure is fixed** | Product URLs live under `/products/`, collections under `/collections/` | Cosmetic. No SEO penalty |
| **Some backend logic needs an app** | Anything beyond display — custom pricing rules, complex bundles | Not needed at launch |
| **Shopify's admin is Shopify's admin** | You cannot redesign the order screen the operator uses | Fine. The operator learns one standard screen |

## What this means for a saree store specifically

Sarees need three things a generic template handles badly:

1. **Photo-first product pages.** Drape shot, fabric close-up, border detail, pallu. The product page
   must let a buyer zoom to weave level. Most themes support zoom; verify it on a phone, not a laptop.
2. **One-of-a-kind stock shown honestly.** A unique saree that sells must go to "Sold" visibly, not
   silently disappear. Shopify tracks this natively — the theme decides whether it *reads* well.
3. **Trust for a stranger.** Persona B decides in four seconds. Return policy, real address, GST
   invoice promise — these are content in sections, not custom code.

**None of the three needs a custom theme.** All three are Level 1–3.

## The recommendation

> **Launch on a free theme at Level 1–2. Do not write theme code before the store has taken real
> orders.**

Reasons, in order of weight:

- **A custom theme you maintain is the thing you moved to Shopify to avoid.** Path B was rejected
  largely because 6–12 hours/month of maintenance is real. A custom theme re-imports a smaller
  version of that cost.
- **You do not yet know what the design needs to do.** Design decisions made before any real buyer
  behaviour is observed are guesses. Analytics after 100 orders will tell you more than any mockup.
- **Free themes are genuinely good now.** Dawn and Horizon are Shopify's own, fast, accessible,
  mobile-correct, and updated for free.
- **Premium themes are a one-time ~🟡 ₹15,000–25,000**, and buy a look, not capability. Worth it
  *later*, if the free theme is genuinely the thing limiting sales. It usually is not.

**Revisit a custom theme when:** the store is doing consistent monthly revenue, and you can point at
a specific, measured thing the theme is costing you — page speed, mobile drop-off, a flow buyers
abandon. Not before. "It looks a bit generic" is not that reason.

---

# Part 1b — Making it look Indian

## The thing to understand first

**A theme is structure. Identity is photography, palette, typography and words.**

Two stores on the same free Dawn theme — one selling running shoes, one selling Kanjivarams — do not
look remotely alike, because the photographs are different, the colours are different, and the copy
is different. Almost everything people mean by "it looks Indian" lives in the four things *you*
control, not in the theme file.

Put crudely: **if the sarees are photographed well, the site looks Indian on any good theme. If they
are not, no theme rescues it.** The professional photos already shot are the largest single asset
this store has.

## The trap worth naming

The instinct is ornate: gold filigree borders, deep maroon everywhere, a decorative Devanagari-style
display font, a paisley background pattern. **This reliably reads as cheap, dated, and slow** —
it is the visual signature of low-trust stores, which is the opposite of what Persona B needs in her
four seconds. It also loads badly on mobile data.

**Look at where Indian D2C is actually going.** Taneira, Jaypore, Suta, Raw Mango — premium Indian
textile brands run *restrained* storefronts. Lots of white space, big photographs, a single accent
colour, quiet type. The sarees carry the Indian-ness. The interface gets out of the way.

That restraint is also the cheapest thing to build, which is convenient.

## The four levers, in order of effect

### 1. Photography — 🟡 80% of the effect

Nothing else comes close.

- **Consistent background across every product.** One background, one lighting setup. Mixed
  backgrounds are the single biggest thing that makes a store look amateur.
- **Drape shot first**, always. A saree flat is fabric; a saree draped is a garment. PRD §3.3 calls
  this non-optional.
- **One hero image that sets the tone** — a full drape, generous negative space, shot in daylight.
- **Colour honesty.** Say on every product page that screens vary. It prevents returns.

### 2. Colour — one accent, not a palette

Pick **one** saturated colour from your actual sarees — a real dye colour, not a stock "Indian gold".
Use it for buttons, links and small accents only. Everything else stays white, off-white or very dark
neutral.

A gold-and-maroon *everything* palette fights every photograph on the page. One accent makes the
photographs louder, which is the goal.

### 3. Typography — one serif does most of the work

A good serif for headings against a plain sans for body text reads as heritage and craft without any
ornament. Free on Google Fonts, and Shopify's theme editor sets fonts with no code.

**Do not** use a decorative "Indian-style" display font. It dates immediately and hurts readability
on phones.

### 4. Words — where Indian-ness is cheapest and most real

The theme cannot do this and it matters more than the theme does:

- Name the weave, the region, the cluster. "Kanjivaram, woven in Kanchipuram" says more than any
  border graphic.
- Collections by **occasion** — wedding, festive, everyday, gifting. Buyers browse by occasion.
- The About page in Jaya's own voice → [content/about.md](content/about.md). A real person and a
  real place is the strongest trust signal you have, and it costs ₹0.

## Which theme, concretely

**Free, from Shopify's own Theme Store.** Shopify's free themes come in two families — the **Dawn**
family and the newer **Horizon** family. Several Horizon-family themes are built for exactly this
kind of product:

| Theme | Family | Why it fits |
|---|---|---|
| **Heritage** | Horizon | Editorial, story-led. Built for brands with provenance to talk about |
| **Fabric** | Horizon | Textile-oriented; material and detail shots get room |
| **Atelier** | Horizon | Craft/maker framing — suits one-of-a-kind pieces |
| **Dawn** | Dawn | The safe default. Fast, image-first, the most widely used apparel theme. Nothing wrong with it |
| **Studio** | Dawn | Built to let visual work dominate the page |

🟡 **Shopify renames and retires themes.** Treat this list as where to start browsing, not as final.
Filter the Theme Store by Clothing, sort by free, then apply the phone test in Stage B.

**Pick by trying your own photographs in it.** Every theme has a free preview. Put three real saree
photos into three candidate themes and look at all three on a phone. Twenty minutes, and it settles
the question better than any review.

## On the paid "saree themes"

There are themes marketed specifically at saree and ethnic-wear stores — Vastra, Sareeify, and
others, typically 🟡 ₹5,000–20,000 one-time. Some are good. Two cautions before buying one:

- **Many are sold outside Shopify's official Theme Store.** Theme Store themes are reviewed against
  performance and accessibility standards and get free updates. Marketplace themes are not reviewed,
  and updates depend on the seller still existing in two years.
- **What you are buying is a demo, not the look.** The demo looks good because it is filled with
  professionally shot photographs and finished copy. You already have the photographs. Put them into
  a free theme before paying to find out the free theme was enough.

Paid themes named for the niche (Ella, Symmetry, Debutify) are legitimate and are on the official
Theme Store — but they buy layout variety and built-in conversion features, not Indian-ness.

> **Recommendation: launch free. Give the ₹15,000 to the drape-shot reshoot if the photo audit shows
> gaps** → [catalog/PHOTOS.md](catalog/PHOTOS.md). Better photographs beat a better theme, every time,
> on the same budget.

---

# Part 2 — Step-by-step build

**🟡 Estimated time: 12–20 hours of actual work**, spread across 1–2 weeks, most of it photos and
product data — not setup.

**Prerequisites, all from [../01-START-HERE.md](../01-START-HERE.md) Stages 1–3. Do not start without them:**

- [ ] GSTIN in hand (or a written CA position saying registration is not required)
- [ ] Business current account open, with the account details to hand
- [ ] Razorpay or Cashfree account live, settling to the **business** account
- [ ] Domain bought, registrar 2FA on
- [ ] Zoho Books Free set up
- [ ] Catalog spreadsheet filled for the products you will launch with
- [ ] Legal pages reviewed by the CA → [legal-pages/](legal-pages/)

---

## Stage A — Account and plan · ~30 min

1. **Sign up at shopify.com.** Take the free trial. Do not pay yet.
2. **Set store country India, currency INR, timezone IST.** 🔴 **Currency is painful to change after
   orders exist.** Get it right now.
3. **Do not pick a plan yet.** Build the whole store on the trial. Pay on the day you go live.
4. When you do pay: **Basic, billed annually** — ₹1,499/month equivalent. Monthly billing costs more
   for the same thing. Confirm the current figure on Shopify's own India pricing page that day.

> ⚠️ Basic carries Shopify's **2% per-order fee** because Shopify Payments does not operate in India.
> Full arithmetic: [../02-COSTS-AND-TRADEOFFS.md](../02-COSTS-AND-TRADEOFFS.md). The crossover to Grow is
> around 🟡 ₹4.1 lakh monthly GMV — far above launch volume. Basic is correct now.

## Stage B — Theme · ~1–2 hours

**Choosing one that looks Indian is Part 1b above — read it before this stage.** Short version:
photography and words carry the identity; the theme just has to stay out of the way.

1. **Online Store → Themes → browse free themes.** Start with **Heritage**, **Fabric** or **Dawn**.
   Preview each with three of your own saree photos before deciding.
2. **Judge every theme on a phone.** Over 🟡 70% of Indian retail traffic is mobile. A theme that
   looks good on a laptop and cramped on a phone is the wrong theme.
3. **Check these four things before committing** — swapping later means redoing Stage E:
   - Product image zoom, at fabric-weave level
   - A gallery layout that handles 6–8 portrait photos without scroll fatigue
   - A visible, unambiguous sold-out state
   - Fast on a mid-range Android on mobile data, not office wifi
4. **Theme editor → set brand colours, fonts, logo.** No code. → [brand.md](brand.md)
5. **Duplicate the theme before any change you are unsure about.** Themes → Actions → Duplicate.
   This is your undo button, and it is free.

## Stage C — Legal, policy and trust pages · ~1 hour

**Already drafted in this repo. This stage is copy-paste, then link.**

| Shopify location | Source file |
|---|---|
| Settings → Policies → Refund policy | [legal-pages/return-refund-cancellation.md](legal-pages/return-refund-cancellation.md) |
| Settings → Policies → Privacy policy | [legal-pages/](legal-pages/) |
| Settings → Policies → Terms of service | [legal-pages/](legal-pages/) |
| Settings → Policies → Shipping policy | [legal-pages/](legal-pages/) |
| Pages → Add page → About | [content/about.md](content/about.md) |
| Pages → Add page → Contact | Real address, real phone, real email |

**🔴 Have the CA confirm these before launch.** A published return policy is a contract with every
buyer. Do not launch on unreviewed text.

Trust microcopy — the short reassurance lines near the buy button — is drafted in
[content/trust-microcopy.md](content/trust-microcopy.md).

## Stage D — Payments, shipping, tax · ~2 hours · **the part that must be exactly right**

### Payments

1. **Settings → Payments → third-party provider → Razorpay** (or Cashfree).
2. 🚨 **Verify the settlement account is the business current account, not personal.** Removing
   personal-account payments is the reason this entire project exists. Check it twice.
3. **Test mode first.** Place a real test order end to end before you take a rupee from anyone.
4. **COD: off.** Deliberate, on the evidence, and documented in [../01-START-HERE.md](../01-START-HERE.md).
5. Enable UPI in the gateway dashboard. It will be the dominant method.

### Tax

> **🔴 No GST rate, HSN code, or threshold in this repository or from any AI goes into your store.**
> Every tax figure comes from the CA in writing. This is the single most expensive thing to get
> wrong, because it is wrong on every invoice retroactively.

1. Settings → Taxes and duties → India. Enter your GSTIN.
2. Enter rates and HSN codes exactly as the CA gave them → [gst-position.md](gst-position.md).
3. Decide with the CA whether prices shown are tax-inclusive. Set it before the first order.

### Shipping

1. Settings → Shipping → zones and rates, using real weights → [shipping.md](shipping.md).
2. **Weigh 10 packed sarees before setting rates.** Underpriced shipping is a loss on every order,
   invisible until you reconcile.
3. Install Shiprocket from the Shopify App Store. Free signup, pay per shipment.

## Stage E — Products · the longest stage · 🟡 15–30 min per saree

This is where the hours go. Photos and copy, not clicks.

1. **Import the catalog spreadsheet** via Products → Import, CSV. The 21-column header in
   [catalog/catalog.csv](catalog/catalog.csv) was frozen for exactly this.
2. **Do three products by hand first**, fully, before importing everything. The three will teach you
   what the import needs to contain.
3. Per product:
   - **6–8 photos.** Drape shot first — it is non-optional per PRD §3.3. Then fabric close-up,
     border, pallu, full flat lay.
   - **Alt text on every image.** Accessibility, and it is how Google Images finds you.
   - **Description with the facts buyers actually ask:** fabric, weave, length, blouse piece
     included or not, care instructions, colour honesty ("photographed in daylight; screens vary").
   - **Inventory: 1** for unique pieces. Shopify then marks it sold automatically.
   - **"Continue selling when out of stock": OFF.** 🚨 On, this sells the same unique saree twice —
     the failure the PRD calls the fastest way to lose a customer permanently.
4. **Collections** — by fabric, by occasion, by colour. Buyers browse by occasion far more than by
   fabric. Use automated collections driven by tags; hand-curated collections rot.

## Stage F — Domain, launch checks, go live · ~2 hours plus DNS wait

1. Settings → Domains → connect your existing domain. 🟡 DNS propagation up to 48 hours.
2. **Pre-launch checklist — all of it, on a phone:**
   - [ ] Real order placed with a real card or UPI, and then refunded
   - [ ] Order confirmation email arrives, and reads correctly
   - [ ] Invoice raised in Zoho Books against that order
   - [ ] Every policy page opens and is the CA-approved text
   - [ ] Contact details are real and monitored
   - [ ] A sold-out product displays as sold out, and cannot be bought
   - [ ] Shipping cost is right for a real saree weight to a far state
   - [ ] Store loads acceptably on mobile data, not wifi
3. **Remove the password page.** Online Store → Preferences.
4. Submit the sitemap in Google Search Console. Free, and the only thing standing between you and
   search traffic.

## Stage G — Handover to the operator · ~2 hours

The store will be run day-to-day by a non-technical person → [operator.md](operator.md).

- Install the **Shopify mobile app**. The whole day-to-day job — orders, stock, fulfilment — runs
  from a phone.
- Create a **Staff account with limited permissions.** 🚨 The operator does not get the owner login.
- **2FA on the owner account.** Non-negotiable.
- Write the daily loop on one page: order arrives → pack → Shiprocket label → mark fulfilled →
  invoice in Zoho.
- Agree what the operator must *not* do alone: refunds above a limit, price changes, policy edits.

---

# Part 3 — Apps: what to install, and what not to

**Every app is a monthly cost and a thing that can break your theme.** The default answer is no.

| App | Verdict |
|---|---|
| **Shiprocket** | ✅ Install. Courier, labels, tracking |
| **Razorpay / Cashfree** | ✅ Install. It is the payment method |
| Google & YouTube channel | 🟡 Free, worth it for Shopping listings once stable |
| Meta / Instagram channel | 🟡 Free. Useful — Instagram is already a live channel |
| **GST invoicing apps** | ❌ **Skip.** 🟡 ₹400–1,500/month for what Zoho Books does free |
| Reviews | ⏸️ Later. Zero reviews looks worse than no review section |
| Popups, urgency timers, "X people viewing" | ❌ Never. Fake scarcity is on the never list |
| Page builders | ❌ Skip. Sections already do this, without the monthly fee or the lock-in |
| Abandoned-cart apps | ⏸️ Later. Shopify has basic recovery built in |

---

# Part 4 — If you do decide on a custom design later

The safe order, cheapest first. Stop as soon as the look is right.

1. **Theme settings.** Free, upgrade-safe, no code.
2. **Custom CSS** in the theme editor. Targeted overrides, no file editing.
3. **A custom section.** A new `.liquid` file in `sections/` with a schema block; it then appears in
   the theme editor for the operator to place and configure. This is the right level for a
   saree-specific component — a weave-detail block, a drape-story block.
4. **Duplicate the theme, then edit files.** Never edit a live theme. Themes → Actions → Duplicate,
   edit the copy, preview, publish when right.
5. **A full custom theme.** Only with measured revenue and a measured problem.

**Two rules that prevent the expensive mistake:**

- **Edits to a theme's own files are lost when that theme updates.** Custom sections and custom CSS
  largely survive; direct edits to core theme files do not. This is how stores end up frozen on an
  old theme version.
- **Use the theme editor for anything it already exposes.** Code only where the editor genuinely
  cannot reach. Every line of Liquid you write is a line you maintain, and the operator cannot.

---

# Part 5 — Writing theme code in this repository

**The question asked (2026-09-16): can the store be designed here in code and then imported into
Shopify?**

## Yes, and it is a supported, first-class workflow

A Shopify theme is a folder of files. Shopify ships a CLI and a GitHub integration specifically so
themes can live in version control.

**But the order is the other way round from what "design here, then import" suggests.**

## Why you cannot design it here first

**Liquid renders on Shopify's servers, not on yours.** There is no meaningful local preview without a
store attached — `shopify theme dev` works by connecting to a real store and proxying it. Writing a
theme before the store exists means writing blind, against no products, no collections, and no
settings.

**The store comes first. The code comes out of the store, not into it.**

## The workflow, in order

1. Store exists, free theme chosen in the admin (Stage B).
2. `shopify theme pull` — the theme arrives in this repository as files.
3. Edit here. Branches, commits, review — all normal git.
4. `shopify theme dev` — live preview against the real store, hot reload.
5. `shopify theme push` to an **unpublished** theme. Preview it.
6. Publish only when it is right.

**Optionally, the GitHub integration.** It syncs a branch to a theme *both ways*: your pushes update
the theme, and changes the operator makes in the theme editor come back as commits. That two-way sync
is what makes code and a non-technical operator coexist safely — otherwise one silently overwrites
the other.

🟡 Branching convention if it gets that far: `main` = the live theme · `staging` = an unpublished QA
theme · `feature/*` for work in progress.

## Why not at launch

**This is Level 3–4 of the ladder in Part 1, and it re-imports the ongoing maintenance cost that
Path B was rejected for** — through a side door, in a smaller size.

Three specific reasons:

- **Everything in Part 1b is theme-editor work with zero code.** One accent colour, serif headings,
  restrained layout, occasion collections. Writing it as Liquid produces the same store plus a file
  to maintain.
- **🚨 Every setting replaced with hardcoded markup is a setting the operator loses.** She can change
  a heading in the theme editor. She cannot change it in Liquid, and she is the one running the store
  day to day → [operator.md](operator.md). That trade needs a reason better than preference.
- **Theme updates.** Custom sections and custom CSS largely survive; edits to core theme files do
  not. This is how a store ends up frozen on an old theme version.

## When it becomes the right move

After the store is live and taking real orders, and a **specific** thing shows up that the theme
editor genuinely cannot reach — a weave-detail block, a drape-story layout.

Then: `shopify theme pull`, add it as a **custom section with a schema block**, push it back. A
custom section appears in the theme editor afterwards, so the operator can still place and configure
it, and it survives theme updates. That is the shape to aim for — code that hands control back,
not code that takes it away.

**Set up the git workflow at that moment. Not before.**

---

*Workflow references: [Shopify CLI for themes](https://shopify.dev/docs/storefronts/themes/tools/cli) ·
[Shopify GitHub integration for themes](https://shopify.dev/docs/storefronts/themes/tools/github)*

---

*Decision context: [../04-implementations.md](../04-implementations.md) §7 · Costs:
[../02-COSTS-AND-TRADEOFFS.md](../02-COSTS-AND-TRADEOFFS.md) · Gate:
[../01-START-HERE.md](../01-START-HERE.md) Stage 4*

**Sources for the design-freedom findings (Part 1), verified 2026-09-16:**
[Shopify — Customizing your store theme](https://www.shopify.com/blog/customizing-store-theme) ·
[Shopify 2.0 Themes: Complete Developer Guide](https://www.braincuber.com/blog/shopify-2-0-themes-complete-developer-guide) ·
[How to Add Custom Liquid Code to Shopify](https://liquidfy.app/blog/how-to-add-custom-liquid-code-to-shopify) ·
[Shopify Theme Development Guide 2026](https://amasty.com/blog/how-to-build-a-custom-shopify-theme/)

**Sources for the theme findings (Part 1b), verified 2026-09-16:**
[26 Best Shopify Themes](https://www.shopify.com/blog/shopify-themes) ·
[Best Shopify Themes for Clothing Stores](https://www.omnisend.com/blog/best-shopify-themes-for-clothing/) ·
[Best Free Shopify Themes 2026](https://ed.codes/blog/best-free-shopify-themes-2026) ·
[Choosing a theme for an ethnic clothing store](https://cartcoders.com/blog/shopify-store/shopify-theme-ethnic-clothing-store/) ·
[Top Shopify themes for Indian ethnic wear and saree stores](https://www.purplechalk.in/post/top-shopify-themes-for-indian-ethnic-wear-and-saree-stores)

🟡 Pricing, plan names and checkout-customisation rules change. Verify on Shopify's own India pages
the day you commit.
