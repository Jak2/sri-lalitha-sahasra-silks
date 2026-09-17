# Phase 0 — Status Board

Working folder for every Phase 0 artefact. Plan: [phase0 plan](../../../docs/superpowers/plans/2026-09-11-saree-site-phase0.md).

**Placeholder convention:** `<<FILL: what is needed | who provides it>>`. A file containing any
`<<FILL:` marker is a draft and must not be published or treated as decided.

## Status

| # | Task | Track | Artefact | Status | Blocked on |
|---|------|-------|----------|--------|-----------|
| 1 | Engage a CA | L | [ca-engagement.md](ca-engagement.md) | Enquiry drafted, not sent | Owner must send |
| 2 | Legal structure | L | [legal-structure.md](legal-structure.md) | Template ready | Task 1 |
| 3 | GST position | L | [gst-position.md](gst-position.md) | Template ready | Task 1 |
| 4 | GST registration | L | gst-position.md | Not started | Task 3 |
| 5 | Current account + **DECISION GATE** | L | [banking.md](banking.md), [operator.md](operator.md) | Criteria set | Tasks 2, 4 |
| 6 | Brand + domain | B | [brand.md](brand.md) | Checklist ready | Owner |
| 7 | Five legal pages | B | [legal-pages/](legal-pages/) | **All five drafted** | Owner confirm + review |
| 8 | About + trust content | B | [content/](content/) | Skeleton + microcopy drafted | Owner's story, photo |
| 9 | Payment gateway | C | [payments.md](payments.md) | Comparison frame ready | Tasks 2, 4, 5, 7 |
| 10 | Catalog spreadsheet | C | [catalog/](catalog/) | **Contract + header done** | Owner enters rows |
| 11 | Photo audit | C | [catalog/photo-audit.csv](catalog/photo-audit.csv) | Header done | Task 10 |
| 12 | Weights + shipping rate | C | [shipping.md](shipping.md) | Method ready | Physical weighing |
| 13 | Courier account | C | shipping.md | Not started | Tasks 2, 5, 12 |
| 14 | Image normalisation | C | catalog/images/ | Scheme defined | Task 10 |
| 15 | DLT registration (TRAI) | L | [messaging.md](messaging.md) | **Deprioritised 2026-09-15** — manual WhatsApp is free | Task 2 |
| 16 | WhatsApp Business API via BSP | L | [messaging.md](messaging.md) | **Deprioritised 2026-09-15** | Tasks 2, 6 |

## Research (decision-gate inputs, not tasks)

| Artefact | Holds |
|---|---|
| [stack-research.md](stack-research.md) | Shopify's India fee (Basic 2%), gateway fees, GST capability per platform, mobile-admin comparison — with per-figure confidence marking |
| [competitor-research.md](competitor-research.md) | What Indian saree D2C brands actually run on. Suta on Shopify at $4M+; the headless regret; why Shopify is the India default |
| [nextjs-risk-annex.md](nextjs-risk-annex.md) | What Path B makes you own: invoice numbering, inventory locking on serverless, webhook traps, the admin as a second app |
| **[zero-budget-plan.md](zero-budget-plan.md)** | **Read this first.** Near-zero budget changes the sequence: fix the payment rail and invoice trail now with free tools, build the site when volume justifies it |

## What is genuinely blocked vs. what is waiting on the owner

**Blocked on third parties (unavoidable wait):** GST filing, bank account opening, gateway KYC,
courier onboarding, test parcel transit, **DLT approval, WhatsApp template approval**.

**Waiting on the owner only — these can move today:** sending the CA enquiries (Task 1), brand
candidates (Task 6), confirming the return policy clauses (Task 7 Step 1), the About story and
photo (Task 8), entering catalog rows (Task 10), weighing ten sarees (Task 12).

Six of sixteen tasks need nothing but an hour of the owner's time.

**And one number that blocks the stack decision:** real monthly GMV from the last 3 months of
WhatsApp sales. Every cost figure in `stack-research.md` is modelled on an assumed ₹3,000 AOV. Until
the real number exists, the fee comparison is arithmetic on a guess.

## Security rules for this folder

- No account numbers, IFSC codes, or scanned KYC documents in git. See [.gitignore](.gitignore).
- No API keys or gateway secrets, ever, not even in a file named `.example`.
- No regulatory figure (GST rate, HSN code, threshold) is written here from memory or from a web
  search. Every one is attributed to the CA in writing, with a date.
