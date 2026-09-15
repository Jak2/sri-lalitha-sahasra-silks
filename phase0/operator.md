# Task 5, Step 5 — The Operator (resolves PRD Q7)

PRD §7.1 makes non-technical operability a hard constraint. The three implementation paths differ
sharply on it, so this is a **decision-gate input**, not an afterthought.

---

## The four questions

1. **Who lists products and processes orders after handoff?**
   `<<FILL: name and relationship>>`

2. **What device will they do it on?**
   `<<FILL: phone / laptop / both>>` — if the answer is *phone only*, any admin that is not
   genuinely mobile-usable is disqualified, regardless of how good it looks on a desktop.

3. **How comfortable are they with software?**
   `<<FILL>>` — concretely: can they use a spreadsheet unaided? have they used any admin panel
   before? do they read English comfortably, or is a regional-language interface needed?

4. **Could anyone other than Jaya repair the system if it broke?**
   `<<FILL: name, or "no one">>`

## Why question 4 decides more than the other three

If the answer is **"no one"**, that fact weighs heavily toward **Path A (Shopify)** — a hosted
platform means an outage is someone else's pager, and a broken self-hosted Medusa stack with no one
able to fix it is a business outage with no end date.

Record that reasoning at the gate rather than discovering it after eight weeks of building Path C.

- **Assessment:** `<<FILL: what this implies for the path choice>>`
- **Recorded at gate:** ☐

## Handoff implications to carry into Phase 1

Once answered, these become concrete requirements on the chosen path:

- Admin must work on the operator's actual device, tested on that device, not on a simulator.
- Everyday actions — add a product, mark shipped, issue a refund, adjust stock — must each be
  achievable without a written manual. Where a manual is needed, it is written in Phase 1, not left
  implied.
- If the bus factor is 1, a documented recovery path is mandatory: where the data lives, who the
  vendors are, how to reach support, and which credentials exist and where they are stored (the
  credentials themselves never in this repository).
