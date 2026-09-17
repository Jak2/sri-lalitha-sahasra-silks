# Task 3 — Written GST Position

⚠️ **Every figure in this file comes from the CA in writing, attributed and dated. Nothing here is
filled from memory, from a web search, or from an AI.** Textile GST in India varies by material and
has varied by price threshold, and the slabs have been revised more than once. A wrong rate on
issued invoices is a correction exercise across every past order.

**Source:** `<<FILL: CA name>>` · **Received:** `<<FILL: date>>` · **Medium:** `<<FILL: email / letter>>`

---

## Step 1 — Rates and HSN codes per category

| Category | HSN code | GST rate | Price threshold conditions (if any) |
|----------|----------|----------|-------------------------------------|
| Silk saree | `<<FILL: CA>>` | | |
| Cotton saree | `<<FILL: CA>>` | | |
| Blended / man-made fabric saree | `<<FILL: CA>>` | | |
| Blouse piece, if billed separately | `<<FILL: CA>>` | | |

These values populate the `hsn_code` and `gst_rate` columns of
[catalog/catalog.csv](catalog/catalog.csv). `gst_rate` is **per product, not a global constant** —
if the CA reports a price threshold, rows on either side of it carry different rates.

## Step 2 — Is registration required?

Question asked verbatim: *"Given inter-state online sales, am I required to register for GST
regardless of turnover threshold?"*

- **Answer:** `<<FILL: yes / no | CA>>`
- **Reasoning given:** `<<FILL | CA>>`
- **If no — the threshold at which it becomes required:** `<<FILL | CA>>`

This answer materially changes the timeline: "yes" puts GST registration on the critical path ahead
of the current account.

## Step 3 — Invoice requirements

- **Required fields on a tax invoice:** `<<FILL | CA>>`
- **Sequential-numbering rule:** `<<FILL | CA>>` — note the PRD requires gapless sequential
  numbering; confirm the CA's rule matches.
- **Place of supply, intra-state:** `<<FILL: CGST + SGST split | CA>>`
- **Place of supply, inter-state:** `<<FILL: IGST | CA>>`
- **How place of supply is determined for an online sale:** `<<FILL | CA>>`
- **Retention period for invoices:** `<<FILL | CA>>`

## Step 4 — Registration outcome (Task 4)

- **Application reference number:** `<<FILL>>` · **Filed:** `<<FILL: date>>`
- **GSTIN:** `<<FILL>>` · **Issued:** `<<FILL: date>>`

🔒 This exact GSTIN string goes in the website footer and on every invoice. **Copy it, never retype
it.**

**If registration is NOT required:** record that conclusion and its reasoning above, mark Task 4
skipped with today's date, and add a `05-Changelog.md` note that this must be re-checked when turnover
approaches the threshold the CA named.
