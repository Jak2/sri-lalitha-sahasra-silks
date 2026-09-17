# Task 5 — Business Current Account — 🚩 DECISION GATE

🔒 **This file records only that an account exists, at which bank, and from what date. Account
numbers, IFSC codes, and KYC scans never enter this repository.**

---

## Step 1 — Bank shortlist

Compare on the four criteria that actually matter. Not on the branch being nearby.

| Bank | Min. average balance | Current-account transaction limits | Works cleanly with Indian gateways | Net-banking usability |
|------|---------------------|-----------------------------------|------------------------------------|----------------------|
| `<<FILL>>` | | | | |
| `<<FILL>>` | | | | |
| `<<FILL>>` | | | | |

**Verify:** three banks compared in writing on those four criteria.

**Why minimum average balance leads:** it is the only one of the four that costs money every month
whether or not the business trades. A high MAB requirement on a young business is a standing charge
for nothing.

## Step 2 — Account opened

- **Bank:** `<<FILL>>` · **Branch:** `<<FILL>>` · **Account type:** `<<FILL>>`
- **Opening date:** `<<FILL>>`

## Step 3 — Tested

- ☐ ₹1 transferred **in**, confirmed in the statement
- ☐ ₹1 transferred **out**, confirmed in the statement

An account that exists but is restricted is not an account. Gateway settlement will fail silently
against a restricted account.

## Step 4 — 🚨 Personal-account cutover

**The single most important action in Phase 0.** This is PRD §1.2 problem #1 — the highest-severity
risk in the entire project. Business sales landing in a personal savings account carry freeze risk,
no reconciliation, no invoice trail, and tax exposure.

- **Hard cutover date:** `<<FILL: a date, not "soon">>`
- **From that date, no business payment goes to the personal account.** Until the site is live,
  buyers are directed to the current account.
- **Owner has confirmed this in writing:** ☐ — record where and when: `<<FILL>>`

This does not wait for the website. It is complete the day the current account is tested.

## Step 5 — Operator

See [operator.md](operator.md). Required as a decision-gate input, not an afterthought.

## Step 6 — 🚩 Close the stack decision gate

**Stop here. Do not proceed past this step without choosing a path.**

Re-read [04-implementations.md](../04-implementations.md) §7. Gather the §7.2 inputs — in particular:

| Input | Value | Read from | Date read |
|-------|-------|-----------|-----------|
| Shopify India plan price (today's published figure) | `<<FILL>>` | Shopify's own pricing page | |
| Shopify **additional** third-party-gateway transaction fee | `<<FILL>>` | Shopify's own pricing page | |
| Chosen gateway fee — UPI | `<<FILL>>` | gateway's own pricing page | |
| Chosen gateway fee — cards | `<<FILL>>` | gateway's own pricing page | |
| Projected monthly GMV | `<<FILL>>` | owner | |
| Operator's technical comfort (Step 5) | `<<FILL>>` | operator.md | |
| Bus factor — anyone but Jaya who can repair it | `<<FILL>>` | operator.md | |

⚠️ Shopify Payments does not operate in India, so a third-party gateway is mandatory and Shopify
levies its own per-order fee **on top of** the gateway's fee. That stacked fee against projected GMV
is the decisive Path A vs. Path C number. Read both figures on the day of the decision — not from
this document, not from the PRD, not from an AI.

Then apply the decision rule in 04-implementations.md §7.3.

**Verify:** a `05-Changelog.md` entry records the chosen path, the actual fee numbers that drove it,
what was rejected and why, and — required — **what would reverse the decision**.
