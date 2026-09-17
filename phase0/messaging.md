# Tasks 15 & 16 — DLT Registration and WhatsApp Business API

**Added 2026-09-15.** Surfaced while assessing Path B, but **stack-independent — a hosted platform
does not exempt you from DLT.**

**Consumes:** Task 2 (legal entity), Task 6 (brand, for WhatsApp display name).
**Produces:** the ability to send the six notification triggers in PRD §4.5 automatically.

⚠️ **Bureaucratic delay, not engineering difficulty.** Same shape as the CA dependency: it can be
started today and it cannot be compressed later. Start both in parallel with Track L.

---

## Task 15 — DLT registration (TRAI-mandated)

No transactional SMS can be sent in India until this clears. Three separate registrations, each
approved individually.

### Step 1 — Register the entity

- **Registered on:** `<<FILL: DLT portal — your telecom operator's, e.g. Jio/Airtel/VI>>`
- **Entity ID:** `<<FILL>>` · **Date:** `<<FILL>>`
- Requires the legal entity documents from [legal-structure.md](legal-structure.md)

### Step 2 — Register the sender ID (header)

Six characters, and it should read as the brand.

- **Sender ID requested:** `<<FILL>>` · **Approved:** `<<FILL: date>>`

### Step 3 — Register every template

**Templates are rejected on wording.** Variable placeholders, spacing and punctuation all matter.
Register all of them at once rather than discovering the rejection pattern one at a time.

| # | Trigger (PRD §4.5) | Template text | Submitted | Approved |
|---|---|---|---|---|
| 1 | Payment success | `<<FILL>>` | | |
| 2 | Order packed | `<<FILL>>` | | |
| 3 | Shipped + tracking | `<<FILL>>` | | |
| 4 | Delivered | `<<FILL>>` | | |
| 5 | Payment failed + retry | `<<FILL>>` | | |
| 6 | Refund initiated | `<<FILL>>` | | |

**Verify:** every template shows Approved, with its template ID recorded.

**Expected elapsed time: 2–4 weeks, mostly waiting.**

---

## Task 16 — WhatsApp Business API via a BSP

WhatsApp is the existing relationship channel and PRD §8.2 calls the WhatsApp contact button "the
single highest-trust element available to this specific business." Order updates belong there.

### Step 1 — Choose a BSP

Candidates: Gupshup, Interakt, Twilio, and others. Compare on: per-message pricing, whether they
integrate with the platform chosen at the gate, template-approval turnaround, and support.

| BSP | Per-message cost | Integrates with our stack | Monthly minimum |
|---|---|---|---|
| `<<FILL>>` | | | |
| `<<FILL>>` | | | |

- **Chosen:** `<<FILL>>` · **Reason:** `<<FILL>>`

### Step 2 — Facebook Business verification

- ☐ Business Manager account created
- ☐ Business verified · **Date:** `<<FILL>>`
- Requires the legal entity documents and a verifiable business phone number

### Step 3 — Number selection

🚨 **Decide deliberately: the number used for the WhatsApp Business API cannot also be used in the
normal WhatsApp app.**

The business currently runs on WhatsApp groups by word of mouth. **Migrating the existing number to
the API would break the manual conversations that are today's entire sales channel.**

- **Number for the API:** `<<FILL: strongly prefer a NEW number>>`
- **Existing relationship number stays on normal WhatsApp:** ☐ confirmed

### Step 4 — Template approval

Same six triggers as Task 15. WhatsApp's approval is separate from DLT's.

**Verify:** all six approved, template names recorded.

**Expected elapsed time: 1–3 weeks.**

---

## Launch impact

**A store can launch before either completes**, falling back to email plus manual WhatsApp messages
from the owner.

**But the ≤5-minutes-per-order target in PRD §1.5 does not hold until automation is live** — manual
messaging is precisely the labour this project exists to remove (PRD §1.2 problem #3).

Treat as: not a launch blocker, but a metric blocker. Launching without it means launching knowingly
into the old manual loop for as long as approvals take.
