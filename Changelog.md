# Changelog — Decision Log

Purpose: stop decisions from drifting. Every entry records **what was decided, when, why, what was
rejected, and what would reverse it.** If a decision gets re-litigated later, the answer is here or
it gets a new dated entry — never a silent change.

**Format**

```
## [YYYY-MM-DD] Title
**Decision:** one sentence
**Context:** what forced the decision
**Alternatives rejected:** with the reason for each
**Consequences:** what this commits us to
**Reverses if:** the specific condition that reopens it
**Status:** Active | Superseded by [date] | Reversed [date]
```

Related: [PRD.md](PRD.md) · [implementations.md](implementations.md)

---

## [2026-09-11] Project initiated — saree D2C website

**Decision:** Build a direct-to-consumer saree website to replace manual WhatsApp + personal-UPI
selling.
**Context:** Sarees are already selling through WhatsApp groups by word of mouth, with payment
collected as individual PhonePe transfers to a personal account. Professional product photography
already exists. No catalog, no order record, no invoice trail.
**Alternatives rejected:**
- *Stay on WhatsApp only* — caps sales at the owner's available hours, no discovery, no order
  record, and leaves the personal-account risk unaddressed.
- *Sell on a marketplace (Amazon/Myntra/Meesho)* — not evaluated in depth; high commission, no
  brand ownership, no customer relationship. Can coexist later as a channel, not as the base.
**Consequences:** Commits to running a real merchant operation: business banking, GST position,
invoicing, shipping, returns.
**Reverses if:** Owner decides the operational overhead of formal commerce outweighs the growth.
**Status:** Active

---

## [2026-09-11] Persona adopted for this project — "Rukh, Principal Web Architect"

**Decision:** Work under the operating principle in `website_builder.md` — business outcome first,
boring technology unless a requirement earns complexity, assumptions flagged explicitly, no
promises about SEO rankings or conversion rates.
**Consequences:** Stack recommendations come with one reason and one named trade-off. Anything
guessed is marked as guessed (PRD §13).
**Status:** Active

---

## [2026-09-11] Market scope — India only, INR only, for v1

**Decision:** Domestic India only. INR only. No international shipping, no multi-currency.
**Context:** Existing buyers are domestic and already pay by UPI.
**Alternatives rejected:** *India + NRI export* and *export-first* — both add customs
documentation, international courier rates, USD payment handling, and higher fraud exposure,
for a buyer base that does not exist yet.
**Consequences:** Payment stack, tax model, and shipping design are all India-specific.
**Reverses if:** Sustained international enquiries appear post-launch. Revisit in Phase 4.
**Status:** Active

---

## [2026-09-11] Inventory model — mixed unique and restockable

**Decision:** Support both one-of-a-kind pieces (`is_unique = true`, qty 1) and restockable designs
(`is_unique = false`, qty N) via a single flag on the product.
**Context:** Owner confirmed the catalog contains both.
**Alternatives rejected:** *Unique-only* (simpler, but wrong — restockable designs exist);
*treat everything as multi-stock* (would allow a unique saree to be oversold).
**Consequences:** Sold-out behaviour differs by type (PRD §3.5). The concurrency guarantee in PRD
§3.6 becomes a hard correctness requirement, not a nicety.
**Reverses if:** Catalog becomes entirely one or the other.
**Status:** Active

---

## [2026-09-11] Operator model — build now, hand off later

**Decision:** Jaya builds and seeds the catalog; a non-technical person must be able to run
day-to-day operations afterwards, from a phone, without developer help.
**Consequences:** **Eliminates any design where listing a product requires editing a file, running
a command, or triggering a deploy.** A real admin UI is mandatory in every path. This single
constraint removes flat-file/markdown-CMS/static-site approaches entirely.
**Reverses if:** Owner confirms Jaya will remain the sole operator indefinitely — which would be a
bus-factor problem of its own.
**Status:** Active

---

## [2026-09-11] Cash on Delivery excluded at launch

**Decision:** Prepaid only (UPI, cards, netbanking, wallets). No COD in v1.
**Context:** Apparel COD in India carries high return-to-origin rates. Each RTO costs forward
shipping plus return shipping plus handling on an order that earned nothing, delays cash, and
requires manual reconciliation — which is exactly the manual labour this project exists to remove.
**Alternatives rejected:** *COD from day one* — risks negative unit economics before any data
exists to manage it. *COD with a deposit* — added friction, still requires reconciliation.
**Consequences:** Some sales will be lost. Mitigation: a prepaid incentive, and the WhatsApp
channel stays open for buyers who will not pay online.
**Reverses if:** After 90 days, measured abandonment specifically attributable to COD absence
justifies a limited pilot — restricted to pincodes with good delivery history and to orders under
a value cap.
**Status:** Active — **explicitly revisit 90 days post-launch**

---

## [2026-09-11] Trust layer treated as a launch requirement, not polish

**Decision:** Real About page with owner photo and story, visible return policy, WhatsApp contact
button, secure-payment mark, GSTIN in footer, one-of-a-kind badge where true — all required at
launch and weighted equally with checkout in the PRD.
**Context:** The business runs entirely on relationships today. A referred stranger decides in
seconds whether the site is real. New saree sites usually fail on belief, not on technology.
**Consequences:** Legal pages and the About page become launch blockers, not backlog items. They
are also required for payment gateway onboarding.
**Related:** No fabricated reviews, no fake MRPs, no fake scarcity (PRD §8.4).
**Status:** Active

---

## [2026-09-11] Guest checkout mandatory; accounts optional and post-purchase only

**Decision:** No account required to buy. Account creation offered only after payment.
**Context:** The dominant launch journey is an existing WhatsApp buyer tapping a shared link. If
the site is more work than sending a message, it will not be used.
**Consequences:** Mobile number is the primary order identifier. Order-status lookup must work by
order number + mobile with no login.
**Reverses if:** Never, for v1.
**Status:** Active

---

## [2026-09-11] Money and inventory correctness named as the top technical risk

**Decision:** Three requirements are non-negotiable in every implementation path: (a) stock
decrements only on confirmed payment, with a guarantee that concurrent purchases of a unique item
yield exactly one success; (b) order state is driven by a signature-verified, idempotent
server-side webhook, never by the browser redirect; (c) no card data ever touches our servers.
**Context:** These are the failures that cost real money and permanent trust, and they are silent
until they happen.
**Consequences:** Each of (a), (b), (c) has an explicit test in the launch acceptance criteria
(PRD §14). Paths that build these by hand carry a correspondingly higher risk weighting in the
stack decision.
**Status:** Active

---

## [2026-09-11] Documentation set established

**Decision:** Three living documents — `PRD.md` (what and why, stack-agnostic), `implementations.md`
(how, with all paths and the decision gate), `Changelog.md` (this file).
**Context:** Owner asked explicitly for documents that prevent drift from agreed decisions.
**Consequences:** PRD must stay stack-agnostic; if a requirement can only be met by one platform,
that is a bug in the PRD. Any decision changed in conversation must be reflected here the same day
or it does not count as decided.
**Status:** Active

---

## [2026-09-11] Stack decision deferred; Phase 0 started immediately

**Decision:** Document all three paths (A: Shopify, C: Medusa self-hosted, B: fully custom) and
defer the choice, while starting Phase 0 — the business prerequisites — now.
**Context:** Owner wants revenue soon *and* wants all options documented. These pull in opposite
directions, but resolve: Phase 0 is stack-independent and is the true critical path (CA → GST
position → business current account → gateway KYC ≈ 2–6 weeks of waiting on third parties). No
build can launch before it completes, so the stack decision has that long to sit open — and no
longer.
**Alternatives rejected:** *Pick Shopify now and start building* — faster, but the owner asked for
the comparison. *Defer everything until the stack is chosen* — wastes the 2–6 weeks of external
waiting, which is the actual delay.
**Consequences:** Decision gate deadline is the completion of P0-5 (business current account open).
Gateway selection depends on the path, and gateway KYC is on the critical path.
**Reverses if:** Phase 0 completes faster than expected — then the gate closes earlier, not later.
**Status:** Active — **open decision, deadline P0-5**

---

## [2026-09-11] Recommendation on record (not yet a decision)

**Recommendation:** Path A (Shopify) for this business at this moment. Path C (Medusa) as the
correct second choice and the correct first choice if the platform take rate is unacceptable at
projected GMV or if building the system is itself a primary goal. Path B not recommended for this
business.
**Reasoning:** Sales are already running on a personal UPI account; every week without a proper
merchant setup accumulates risk. Path A removes that in 2–3 weeks, gives a non-technical operator
a tool they can actually use, and is reversible — data exports, and a headless storefront can be
added later without a data migration.
**Stated trade-off:** a percentage of every order, forever, plus a ceiling on design flexibility.
**Status:** Recommendation only — awaiting owner's decision at the gate.

---

## [2026-09-11] WhatsApp Business Catalog and Instagram Shop — complements, not replacements

**Decision:** Keep building the website. Adopt WhatsApp Business Catalog and Instagram Shop as
**channels feeding it**, not as substitutes for it.
**Context:** Raised directly — plenty of Indian saree businesses run on WhatsApp/Instagram catalogs
alone. The question deserved a real answer rather than a dismissal.
**Analysis:** Against the five problems in PRD §1.2, a platform catalog solves only #4 (discovery),
and only via Instagram. It does not touch the personal-UPI-account risk (#1), the absence of an
order record (#2), the manual per-order labour (#3), or the double-sale risk on unique pieces (#5).
Catalogs solve *browsing*; the actual pain is *transacting*.
Specifically: WhatsApp Catalog ends in a DM — payment, address, and records stay manual. Instagram
native checkout has not been available in India (US-only rollout — **verify Meta's current India
availability**), so Instagram Shop links out to a destination site; it is a traffic layer on top of
a website, not a replacement for one.
**Alternatives rejected:**
- *WhatsApp Catalog only* — no payment automation, no order record, no invoice, no inventory truth,
  no search discovery. Caps at the owner's hours, exactly as today.
- *Instagram Shop only* — needs a link destination in India; audience and reach are owned by Meta;
  an account ban ends the business with no appeal path.
**Consequences:** PRD §8.2's WhatsApp CTA and §4.1's shared-link journey stay as the dominant launch
path. Instagram/Meta catalog sync stays in Phase 2 (implementations.md §8), sourced from the site's
catalog rather than maintained separately.
**Low-cost validation available:** Set up WhatsApp Business Catalog now (free, ~2 hours). If buyers
browse it and still message to buy, that confirms the checkout gap. If volume stays low enough that
manual handling is tolerable, the website scope can be reconsidered. **Phase 0 runs either way** —
the current account and the GST position are required with or without a website.
**Reverses if:** Order volume stays under roughly 5/week indefinitely AND the personal-account
exposure is resolved by other means. At that point a website is not earning its maintenance.
**Status:** Active

---

## [2026-09-15] Phase 0 execution started — all owner-independent work completed

**Decision:** Execute every Phase 0 artefact that does not require the owner or a third party, now,
rather than waiting for the stack decision or for the CA.
**Context:** Four days elapsed since the plan was written with zero tasks started, while the
critical path (CA → GST → current account → gateway KYC) is 2–6 weeks of external waiting. Every day
not spent waiting is a day added to launch — and every day the personal-account exposure (problem #1)
stands.
**Delivered:** `phase0/` scaffolded with 18 artefacts — the CA enquiry drafted verbatim and ready to
send, the bank shortlist criteria, the operator questionnaire, the stack decision-gate input table,
the brand/domain checklist, **all five legal pages drafted in full**, the About frame and the five
trust microcopy strings, the catalog CSV header and complete column contract, the photo-audit header
and image-naming scheme, the weighing method and shipping rate model, and the gateway comparison
frame.
**Convention adopted:** `<<FILL: what is needed | who provides it>>` marks every value that must come
from the owner, the CA, or a provider. **A file containing any `<<FILL:` marker is a draft and must
not be published or treated as decided.** This makes the boundary between drafted and decided
mechanically checkable rather than a matter of memory.
**Deliberately not done:** no regulatory figure, no fee, no rate, and no delivery timeline has been
filled in. Every one is marked as sourced from the CA or from a provider's own current pricing page,
with the date of reading recorded. PRD §13 and the plan's global constraints both forbid taking these
from an AI; drafting them "provisionally" would have them quietly harden into fact.
**Consequences:** Six of fourteen tasks now need nothing but an hour of the owner's time — sending
the CA enquiries, brand candidates, confirming the return-policy clauses, the About story and photo,
entering catalog rows, weighing ten sarees. The remaining blockers are genuinely external.
**Security:** `phase0/.gitignore` excludes KYC scans, statements, any file matching account/IFSC
patterns, image masters, and every secret-file form. Banking details and API keys are excluded by
rule as well as by pattern.
**Status:** Active

---

## [2026-09-15] Catalog column contract frozen

**Decision:** The 21-column `catalog.csv` header is fixed, including column order, and the controlled
vocabularies for `fabric` (10), `weave_technique` (11), `primary_color` (14 colour families),
`blouse_piece` (3), `occasion_tags` (5) and `status` (4) are closed sets.
**Context:** This spreadsheet is the single artefact every implementation path imports, and the
reason the stack decision can be deferred safely. Its portability depends on it not drifting.
**Alternatives rejected:** *Free-text colour* — produces a filter with 300 options and no users;
"rani pink", "magenta" and "fuchsia" must all file under `Pink`, with the poetic name living in
`title`. *Free-text fabric* — breaks every facet and every import mapping.
**Consequences:** A value that cannot be expressed in the vocabulary means **the contract is wrong —
fix the contract, not the row.** Enter three products as a pilot before three hundred, so that
ambiguity surfaces once rather than repeatedly.
**Reverses if:** The pilot rows expose a genuinely missing category. Amend here with a new dated
entry, never silently in the CSV.
**Status:** Active

---

## [2026-09-15] Shopify's India fee established — 2% on Basic

**Decision:** Record 2% (Basic), 1% (Grow), 0.6% (Advanced) as the additional Shopify fee levied on
top of the payment gateway's own fee, because Shopify Payments does not operate in India.
**Context:** implementations.md §7.2 named this the decisive open input in the Path A vs. Path C
comparison. It was unknown until now.
**Derived:** Basic (₹1,499 + 2%) and Grow (₹5,599 + 1%) break even at **~₹4.1 lakh monthly GMV**.
Below that, Basic; above it, Grow is cheaper despite the higher subscription.
**Consequences:** The fee scales with GMV — ₹2,000/mo at ₹1L, ₹7,200/mo at ₹3.6L, ₹20,000/mo at
₹10L. Cheap insurance now, expensive later. **The Phase 4 migration trigger should therefore be a
specific GMV number decided in advance**, not a feeling that the fee has become annoying.
**Caveat:** researched, not read from Shopify's page directly. **Re-read on the day of the decision.**
Gateway fees (Razorpay ~2.36% effective, Cashfree ~2.30%) came from provider blogs rather than
pricing pages and remain unverified. Cashfree's promotional 0% claim is explicitly excluded from the
margin model — a business whose unit economics only work during a promo window does not have working
unit economics.
**Status:** Active — see [phase0/stack-research.md](phase0/stack-research.md)

---

## [2026-09-15] DLT and WhatsApp BSP registration added to Phase 0 as P0-15 and P0-16

**Decision:** Treat TRAI DLT registration and WhatsApp Business API onboarding as Phase 0 critical-path
tasks, started immediately, in parallel with the CA track.
**Context:** Surfaced while assessing Path B, but they are **stack-independent — a hosted platform
does not exempt you from DLT.** PRD §4.5 defines six notification triggers; each is a template
requiring individual approval, and templates are rejected on wording. No transactional SMS can be
sent at all until DLT clears.
**Why this was missed:** Phase 0 was built around money and catalog. Messaging was treated as an
implementation detail, when in fact it is the same shape as the CA dependency — **bureaucratic delay
that can be started today and cannot be compressed later.**
**Consequences:** Two new tasks, 2–4 and 1–3 weeks respectively, mostly waiting. A store **can**
launch before they complete, falling back to email plus manual WhatsApp — but the ≤5-minutes-per-order
target in PRD §1.5 does not hold until automation is live.
**Status:** Active

---

## [2026-09-15] Paths D (Zoho Commerce) and E (Odoo) added to the decision gate

**Decision:** Add both as genuine contenders at the gate. **The Path A recommendation is unchanged.**
**Context:** Both beat Shopify on native Indian GST invoicing and, more importantly, **charge no
platform percentage per order** — which is the entire structural objection to Path A.
**Against Zoho:** 🔴 no dedicated Commerce mobile admin app, browser only. This collides directly with
PRD §7.3 — "photos are on the phone; requiring a laptop to list a product means products do not get
listed." It partially cancels the cost advantage.
**Against Odoo:** it is an ERP, so the storefront is a feature of an accounting system. Heavy asset
bundles make the PRD §10.1 target of ≤2.5s LCP a genuine fight, and the design ceiling matters for a
product bought on visual desire. **Odoo becomes the right answer** if a physical shop, POS, or weaver
purchase orders should share one system — then it goes to the top of the list.
**Open:** whether Odoo's full Indian GST filing and e-invoicing needs Enterprise. The claim came from
a partner blog, and Odoo Online's paid plans are already Enterprise-tier. **Ask Odoo directly.**
**Signal worth noting:** almost nobody writes about running Odoo or Zoho Commerce for Indian D2C
fashion. That is itself information about how much independent help exists when something breaks.
**Resolution method:** not more reading. **An hour-long hands-on trial per platform — list one saree
with six photos from a phone.** PRD §7.3 is a hard constraint and the trial settles it.
**Status:** Active — open until the gate

---

## [2026-09-15] Competitor evidence supports the operator model and the no-COD decision

**Finding 1 — the operator argument is not theoretical.** Suta, a saree brand at $4M+ revenue
supporting ~14,000 weavers, runs on Shopify. Its founders' stated reason: they "don't even need a big
team to maintain" it. A brand at many multiples of our projected revenue chose not to own the
platform. Our bus factor is 1.
*Caveat: Suta sells cotton and linen at volume — mostly restockable stock. The operational lesson
transfers; the unique-piece inventory lesson does not. **No brand selling one-of-a-kind handloom at
our scale has been found yet** — that comparison is still missing and would be worth more than any of
this.*
**Finding 2 — the most common headless regret is exactly our forbidden failure mode.** Merchants who
went headless most often report that "every content change is a developer deploy." That is the
operator-model entry of 2026-09-11 almost word for word, independently confirmed by people who
learned it expensively.
**Finding 3 — why Shopify is the Indian default.** Not the platform; the ecosystem. Razorpay Magic
Checkout, GoKwik, Shiprocket and the GST apps are all deepest on Shopify, because every Indian
commerce vendor ships a Shopify integration first. This qualifies the "Shopify has no native GST"
objection — it has the largest population of GST apps precisely because it has the largest Indian
merchant base.
**Finding 4 — the no-COD decision is supported by the size of the industry built around COD.**
GoKwik, Razorpay Magic Checkout and Shiprocket Checkout exist largely to manage COD and RTO. We avoid
the problem rather than buying tools to manage it. **Corollary: introducing COD in Phase 3 means
adopting an RTO-scoring vendor with its own fee and integration — a project, not a setting.**
**Status:** Active — see [phase0/competitor-research.md](phase0/competitor-research.md)

---

## [2026-09-15] Budget constraint revised to near-zero — build second, not first

**Decision:** Keep Path A as the eventual choice, but **change the sequence**. Fix the payment rail
and the invoice trail now, at near-zero cost, and let measured order volume decide when the website
earns its subscription.
**Context:** The owner stated investment is **near zero**. This supersedes the earlier brief ("budget
is not a problem, or else low budget but scaling and features should not suffer"), which the Path A
recommendation was built on.
**The reframe:** PRD §1.2's two highest-severity problems — the personal-UPI-account risk (#1) and
the absence of an order/invoice record (#2) — **can be solved this week with no website at all**:
- **Razorpay/Cashfree Payment Links** — ₹0 setup, ₹0 monthly, per-transaction only. Money lands in a
  business account instead of personal PhonePe.
- **Zoho Books Free** — genuinely free under **₹25 lakh turnover** (1 user + 1 accountant, 1,000
  transactions/year). GST invoices, e-invoicing, and GSTR-1/GSTR-3B filing directly, as Zoho is a
  registered GST Suvidha Provider.

**Revises the 2026-09-11 WhatsApp/Instagram entry.** That entry held that platform catalogs "solve
browsing, not transacting." Payment Links plus Zoho Books **do solve transacting** without a website.
What remains unsolved is problem #3, the manual per-order labour — and that is what eventually
justifies the build, on evidence rather than assumption.
**What does NOT change:** Path A stays the correct destination. Zero budget does not make a VPS
cheaper — it makes it dearer, because time becomes the scarce resource and the on-call burden lands
on the one person who cannot be replaced. Free hosting tiers (Vercel, Supabase, Cloudflare) are real
and adequate at this volume; **the cost was never the hosting, it was the 10–14 weeks and the
permanent maintenance.**
**Alternatives rejected:** *Free store builders* — the ₹0-forever tiers cap around 10 products
(catalog is 50–300) and fail PRD §7.3 and the unique-stock requirement. *Amazon SmartBiz* —
promotional fee waiver with marketplace lock-in.
**Added for evaluation:** **ONDC.** Free onboarding, no central network commission, seller apps at
~1–3% versus 20–30% on marketplaces. Attacks problem #4 (reach) at near-zero cost. 🟡 Verify seller-app
fees and unique-stock handling.
**Unavoidable cash acknowledged:** ~₹4,000–12,000 to start — almost entirely the CA and the domain.
**The CA is not the place to economise.** A wrong GST position costs more than every other line
combined and cannot be fixed later without correcting every past invoice.
**Deprioritised, not cancelled:** **P0-15 (DLT) and P0-16 (WhatsApp BSP).** Still stack-independent,
still weeks of approval — but manual WhatsApp messages are free and already being sent. Automate when
volume justifies the per-message cost.
**The decision rule for building:** if ₹1,499/month is unaffordable, the order volume does not yet
justify a website. That is information, not failure.
**Reverses if:** manual-handling pain grows faster than expected (build sooner), or the free loop
proves workable for longer than expected (build later). Both measurable.
**Status:** Active — see [phase0/zero-budget-plan.md](phase0/zero-budget-plan.md)

---

## [2026-09-15] Recommendation split into two — sequence and stack

**Decision:** State the recommendation as **two separate decisions**, made at different times on
different evidence, and stop presenting them as one.

**(a) Sequence — decided now.** Do not build the website yet. Fix the payment rail and the invoice
trail first. Of the five problems in PRD §1.2, the two most dangerous do not need a website: business
money in a personal account needs a current account and a payment link; the missing invoice trail
needs Zoho Books Free. Building first would mean spending weeks on problem #3 while problem #1 keeps
running.

**(b) Stack — decided at the §7 gate.** Path A (Shopify), unchanged.

**Context:** The owner said the recommendation was not understood. On review the fault was in the
presentation, not the reasoning — sequence and stack had been argued together across five documents,
each with its own caveats, so neither came through plainly.

**The stack argument, reduced to one line:** *you are not paying Shopify for features, you are paying
them to not be on call.* The alternatives cost less cash and more of Jaya. With a bus factor of 1, an
outage has no end date.

**Honesty about the uncertainty, now stated in the documents rather than only in conversation:**
"Path A" currently means *"Path A, assuming monthly GMV of roughly ₹1–4 lakh."* That range was
assumed, not measured. At ₹1L/month Shopify costs ~₹42,000/year and is clearly worth 7–12 hours a
month; at ₹10L/month it costs ~₹2.58L/year and a custom build starts to make sense. **Same
recommendation, opposite conclusions, and the deciding number (Q10) is not known.**

**Reversal conditions now recorded explicitly** in implementations.md §6.1 and README: 50+
orders/month reverses (a); ~₹8L/month GMV, a second technical person, a physical shop, or learning
being the explicit goal each reverse (b).

**Consequences:** README now opens with the plain-language version before any requirement detail.
implementations.md §6.1 carries the split and the reversal table.

**Status:** Active

---

## [2026-09-16] Path F rejected — Odoo back office behind a Next.js storefront

**Decision:** Do not run Odoo headless with a custom Next.js storefront. Recorded as Path F in
[implementations.md §7.2b](implementations.md) so it is not re-proposed.
**Context:** Asked directly whether Odoo-plus-Next.js resolves the objections raised separately
against Path A (Shopify's 2%) and Path B (custom build effort). It does not — it inherits both.
**Why it fails:**
- Odoo replaces **Zoho Books**, not Shopify. Zoho Books Free already files GST returns as a
  registered GSP at ₹0 under ₹25L turnover. Odoo Online's free tier is one app; the apps needed here
  are paid, 🟡 ~₹31,000/user/year.
- The Next.js half stays **Path B in full** — webhook signature verification, idempotency,
  unique-stock locking, ISR-vs-live-stock, paise arithmetic. Odoo removes admin screens. Admin
  screens were never the risk.
- Two systems with no shared transaction. An order must land in both; Odoo unavailable at checkout
  means a payment taken with no order recorded.
- Odoo already ships eCommerce, so the module is either paid-for-unused or makes Next.js redundant.

**Alternatives rejected alongside it:** none new — this compares against existing Paths A, B and E.
**Consequences:** Path F is the only shape that is simultaneously the most expensive in cash
(🟡 ~₹31,000+/year) **and** the slowest to launch (12–16 weeks). Path E — Odoo alone, using its own
storefront — dominates it on both and remains the live option at the gate.
**Reverses if:** a physical shop or POS shares the inventory · multiple warehouses or wholesale
price lists appear · weaver purchase orders / manufacturing enter scope · monthly GMV passes
🟡 ~₹13 lakh, where Shopify's 2% exceeds Odoo's licence.
**Status:** Active

**Also recorded:** "2% of GMV" is now defined in plain terms in
[COSTS-AND-TRADEOFFS.md](COSTS-AND-TRADEOFFS.md) — total rupees sold before costs, not profit;
₹60 on a ₹3,000 saree; charged *in addition* to the gateway's own ~2.36%.

---

## [2026-09-16] Visual identity — restrained, not ornate

**Decision:** the store's Indian character comes from photography, one accent colour sampled from a
real saree, one serif heading face, and the language used for weaves, regions and occasions — not
from ornamental theme design. Recorded in [phase0/brand.md](phase0/brand.md) Step 4 with fill slots,
reasoning and theme candidates in
[phase0/shopify-build-guide.md](phase0/shopify-build-guide.md) Part 1b.
**Context:** asked for a theme that "looks Indian". A theme is structure; identity is photography,
palette, typography and copy. Two stores on the same free theme selling shoes and Kanjivarams look
nothing alike.
**Rejected:** the ornate default — gold filigree, maroon ground, paisley backgrounds, decorative
Devanagari-style display font. It reads as low-trust to exactly the referred stranger the PRD calls
Persona B, dates quickly, and loads badly on mobile data. Premium Indian textile brands present
restrained.
**Consequences:** applies to Instagram and the WhatsApp Business profile immediately, not only to a
future website. Free Theme Store candidates recorded (Heritage, Fabric, Atelier, Dawn, Studio),
chosen by previewing real saree photographs on a phone. Any theme spend of 🟡 ~₹15,000 is better
directed at the drape-shot reshoot if [phase0/catalog/PHOTOS.md](phase0/catalog/PHOTOS.md) shows
gaps.
**Reverses if:** photography and copy are in place and a measured conversion problem still points at
the visual design.
**Status:** Active

---

## [2026-09-16] Launch on a stock Shopify theme, not a custom one

**Decision:** when the Stage 4 gate clears and Path A is taken, build on a free Shopify theme
(Dawn or Horizon) configured through the theme editor. No custom theme, no premium theme, no page
builder app at launch. Build steps written up in
[phase0/shopify-build-guide.md](phase0/shopify-build-guide.md).
**Context:** asked whether Shopify allows a custom design or only its templates. It allows a fully
custom theme — theme code is fully editable Liquid/HTML/CSS/JS, and since Online Store 2.0 sections
work on every page type. Checkout is the one hard boundary below Plus, which is desirable here:
it keeps card data off our systems and PCI scope minimal.
**Why stock anyway:** a custom theme re-imports the ongoing maintenance cost that Path B was
rejected for · design decided before any real buyer behaviour is observed is a guess · the three
saree-specific needs (photo-first product pages, honest unique-stock display, stranger trust) are
all reachable through theme settings and sections.
**Consequences:** build estimated at 🟡 12–20 hours, most of it product photos and copy, not setup.
Premium theme (🟡 ₹15,000–25,000 one-time) deferred, not rejected.
**Reverses if:** a specific measured problem is attributable to the theme — page speed, mobile
drop-off, an abandoned flow — with revenue to justify maintaining the code. "It looks generic" does
not qualify.
**Status:** Active

---

## Open Items Carried Forward

These are unresolved and tracked in PRD §13.2. They are not decisions yet.

| # | Open question | Blocks |
|---|---|---|
| Q1 | GST registration status and per-category rates | Gateway onboarding, invoicing |
| Q2 | Legal structure — proprietorship vs. private limited | Current account, KYC |
| Q3 | Business current account — open, which bank | All online payment |
| Q4 | Legal business name, address, phone to publish | Legal pages, invoices |
| Q5 | Exact catalog size and photo coverage per SKU | Launch scope and timeline |
| Q6 | Return policy — confirm or amend the PRD §5.6 recommendation | Legal pages |
| Q7 | Who is the non-technical operator, on what device | Admin design, path choice |
| Q8 | Brand name, domain, logo | Everything visible |
| Q9 | Is a CA engaged | Q1, Q2, tax model |
| — | **Stack choice: A, B, C, D or E** (F rejected 2026-09-16) | Everything after Phase 0 |
| Q10 | **Real monthly GMV, last 3 months of WhatsApp sales** | **The fee comparison — every stack cost figure is currently modelled on a guess** |
| Q11 | Does Odoo's Indian GST filing / e-invoicing need Enterprise? | Whether Path E is affordable |
| Q12 | Phone trial: list a saree with 6 photos on Shopify, Zoho, Odoo | PRD §7.3, path choice |
| Q13 | Which system would the CA rather receive records in? | Path choice |

---

*Append new entries at the bottom of the dated section. Never edit a past entry — supersede it with
a new dated one and mark the old entry's status.*
