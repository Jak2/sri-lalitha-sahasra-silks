# Task 9 — Payment Gateway

🔒 **API keys and secrets never enter this repository, not even in a file named `.example`.** They go
into the chosen platform's secret storage at build time. **If a key is ever pasted anywhere by
accident, rotate it immediately rather than deleting the message** — a deleted message is still in
logs, backups, and someone's scrollback.

**Consumes:** Tasks 2, 4, 5, and 7 (gateways check for the legal pages during onboarding — missing
pages delay go-live). **Produces:** a live gateway account. No real order can be taken without it.

---

## Step 1 — Compare on today's published fees

⚠️ Read every figure from **each provider's own current pricing page**, on the day of comparison.
Record the date. Do not take a fee from this document, from 04-implementations.md, or from an AI —
Indian gateway pricing changes, and MDR regulation changes with it.

| | Gateway A: `<<FILL>>` | Gateway B: `<<FILL>>` |
|---|---|---|
| **UPI fee** | `<<FILL>>` | `<<FILL>>` |
| Card fee (domestic) | `<<FILL>>` | `<<FILL>>` |
| Net banking fee | `<<FILL>>` | `<<FILL>>` |
| Wallet fee | `<<FILL>>` | `<<FILL>>` |
| Setup / annual fee | `<<FILL>>` | `<<FILL>>` |
| Settlement cycle | `<<FILL>>` | `<<FILL>>` |
| Refund handling; refund fee charged? | `<<FILL>>` | `<<FILL>>` |
| Webhook reliability + retry policy | `<<FILL>>` | `<<FILL>>` |
| Documentation quality | `<<FILL>>` | `<<FILL>>` |
| Support responsiveness | `<<FILL>>` | `<<FILL>>` |
| **Pricing page read on** | `<<FILL: date>>` | `<<FILL: date>>` |

**Weight the UPI fee heaviest.** UPI will be the dominant method here — the existing customers
already pay by UPI on PhonePe. A gateway that is cheapest on cards and expensive on UPI is the wrong
gateway for this business.

**Weight the refund fee second.** A gateway that keeps its fee on refunds turns every return into a
direct loss, on a product category where returns are normal.

**Settlement cycle is working capital.** T+2 versus T+7 on a young business is the difference between
restocking this week and next.

## Step 2 — Apply and complete KYC

- **Chosen gateway:** `<<FILL>>` · **Reason:** `<<FILL>>`
- **Applied:** `<<FILL: date>>` · **Approved:** `<<FILL: date>>`
- ☐ Account switched **out of test mode**

Onboarding checks for the five legal pages (Task 7) and a working contact number. Have them live
first or the application bounces.

## Step 3 — Confirm the settlement account

- ☐ Settlement account is the **Task 5 business current account**
- ☐ **Confirmed it is NOT the personal account**

Check this explicitly, by reading the account in the gateway dashboard. It is the exact failure this
entire track exists to prevent, and it is silent — money arrives either way, and the problem only
surfaces at tax time or at an account freeze.

## Step 4 — Log

☐ `05-Changelog.md` entry with the gateway chosen, **the fee schedule as quoted on that date**, and the
alternatives rejected.

---

## Non-negotiable requirements for whichever gateway wins

Carried from PRD §5.4 and the Changelog's money-and-inventory-correctness decision. These constrain
the Phase 1 build regardless of the chosen stack:

- **No card data ever touches our servers or our database.** Payment is collected on the gateway's
  hosted page or its SDK. This keeps PCI-DSS scope minimal and is non-negotiable.
- **Order state is driven by a signature-verified, idempotent, server-to-server webhook** — never by
  the browser redirect. A buyer closing the tab after paying must still get their order.
- **Stock decrements on confirmed payment**, not on cart-add. Two concurrent buyers of a unique
  saree must yield exactly one success and one automatic full refund.
- **Money is stored as integer paise.** Never a float.
