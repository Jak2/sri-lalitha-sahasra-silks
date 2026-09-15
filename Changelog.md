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
| — | **Stack choice: A, B, or C** | Everything after Phase 0 |

---

*Append new entries at the bottom of the dated section. Never edit a past entry — supersede it with
a new dated one and mark the old entry's status.*
