# Read This First — what to open, in what order

One screen. Everything else in this repository is linked from here.

**Files are numbered in reading order.** `00` is this. `01` is what to do today. The numbers are the
order, not a priority ranking.

---

## If you are Jaya — the owner

Read two files. Do not read the rest yet.

| # | File | Why |
|---|---|---|
| **1** | **[01-START-HERE.md](01-START-HERE.md)** | **The working checklist.** Four stages, in order, starting with about four hours of ₹0 work this week |
| **2** | [02-COSTS-AND-TRADEOFFS.md](02-COSTS-AND-TRADEOFFS.md) | What ₹0 actually costs you, what it does not, and what each skipped thing costs to buy back |

**If you read only one thing: [01-START-HERE.md](01-START-HERE.md) Stage 1, item 1.** Total three
months of WhatsApp payments. Every rupee figure in this repository rests on a guessed ₹3,000 average
order value, and you are the only person who has the real number.

Everything else here is reference. Open it when a decision needs it, not before.

---

## If you are an agent picking this project up

Read in this order. Stop at 4 unless the task needs more.

| # | File | Holds |
|---|---|---|
| **1** | [README.md](README.md) | What the project is, the full v1 feature scope, the conventions, the current state |
| **2** | [01-START-HERE.md](01-START-HERE.md) | The active plan. The website is deliberately **not** the next step |
| **3** | [05-Changelog.md](05-Changelog.md) | Every decision, dated, with what was rejected and what would reverse it. **Read before proposing anything** — most obvious ideas have already been considered and recorded |
| **4** | [06-website_builder.md](06-website_builder.md) | The operating persona this project runs under |
| 5 | [03-PRD.md](03-PRD.md) | What the system must do. Stack-agnostic. **The source of truth** — where it and the README disagree, the PRD wins |
| 6 | [04-implementations.md](04-implementations.md) | The stack paths A–F, their trade-offs, and the decision gate |
| 7 | [02-COSTS-AND-TRADEOFFS.md](02-COSTS-AND-TRADEOFFS.md) | The cost ledger |
| 8 | [phase0/README.md](phase0/README.md) | Status board for the 16 Phase 0 tasks and their artefacts |

**Three rules that override any assumption:**

- **No regulatory figure** — GST rate, HSN code, threshold — is taken from these documents, a web
  search, or an AI. Each comes from the CA in writing.
- **No secrets in this repository.** No API keys, no account numbers, no IFSC codes, no KYC scans.
- **A decision not appended to [05-Changelog.md](05-Changelog.md) the same day is not decided.**

---

## The whole repository

### Root — numbered, reading order

| File | What it is |
|---|---|
| [00-READ-THIS-FIRST.md](00-READ-THIS-FIRST.md) | This file |
| [01-START-HERE.md](01-START-HERE.md) | The action checklist. Stages 1–4 |
| [02-COSTS-AND-TRADEOFFS.md](02-COSTS-AND-TRADEOFFS.md) | The cost ledger |
| [03-PRD.md](03-PRD.md) | The requirements. Source of truth |
| [04-implementations.md](04-implementations.md) | Stack paths and the decision gate |
| [05-Changelog.md](05-Changelog.md) | Dated decision log |
| [06-website_builder.md](06-website_builder.md) | Operating persona |
| [README.md](README.md) | Project overview and full v1 feature scope. Unnumbered because GitHub and most tools open it automatically |

### `phase0/` — business prerequisites, the real critical path

**Not numbered, deliberately — these are not read in sequence.** They are opened by task from the
status board in [phase0/README.md](phase0/README.md), which numbers all 16 tasks and shows what is
blocked and what is waiting on the owner.

| Group | Files |
|---|---|
| Legal and money | [ca-engagement.md](phase0/ca-engagement.md) · [legal-structure.md](phase0/legal-structure.md) · [gst-position.md](phase0/gst-position.md) · [banking.md](phase0/banking.md) · [payments.md](phase0/payments.md) |
| Brand and content | [brand.md](phase0/brand.md) · [content/](phase0/content/) · [legal-pages/](phase0/legal-pages/) |
| Product and fulfilment | [catalog/](phase0/catalog/) · [shipping.md](phase0/shipping.md) · [messaging.md](phase0/messaging.md) · [operator.md](phase0/operator.md) |
| Research — decision-gate inputs | [zero-budget-plan.md](phase0/zero-budget-plan.md) · [stack-research.md](phase0/stack-research.md) · [competitor-research.md](phase0/competitor-research.md) · [nextjs-risk-annex.md](phase0/nextjs-risk-annex.md) · [shopify-build-guide.md](phase0/shopify-build-guide.md) |

---

## Where things are, by question

| Question | File |
|---|---|
| What do I do this week? | [01-START-HERE.md](01-START-HERE.md) |
| What will this cost me? | [02-COSTS-AND-TRADEOFFS.md](02-COSTS-AND-TRADEOFFS.md) |
| Why was X decided, or rejected? | [05-Changelog.md](05-Changelog.md) |
| Which platform are we using? | [04-implementations.md](04-implementations.md) §7 — **still open**, gated on the current account opening |
| What will the site actually do? | [README.md](README.md), Feature Scope |
| How do I build the Shopify store? | [phase0/shopify-build-guide.md](phase0/shopify-build-guide.md) — **after** the Stage 4 gate |
| How do I make it look Indian? | [phase0/shopify-build-guide.md](phase0/shopify-build-guide.md) Part 1b |
| What is blocking Phase 0? | [phase0/README.md](phase0/README.md) |
| What do I tell the CA? | [phase0/ca-engagement.md](phase0/ca-engagement.md) — drafted, ready to send |
