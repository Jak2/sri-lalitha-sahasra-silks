# Catalog Column Contract

`catalog.csv` is **the single artefact every implementation path imports.** It is the highest-leverage
work in Phase 0 and the reason the stack decision can be deferred safely — the data is portable to
Shopify, Medusa, or a custom build without rework.

The header row in `catalog.csv` is **fixed, including column order**. Every downstream import and
every manual mapping depends on it. Do not reorder, rename, or insert columns.

---

## Columns

```
sku              Format <FABRIC3>-<YYMM>-<NNN>, e.g. KAN-2609-014. Assigned once, never changed.
title            Buyer-facing. Fabric + distinguishing feature. Never "Saree 14".
price_paise      INTEGER paise. ₹2,499.00 is 249900. No symbols, no decimals, no commas.
mrp_paise        INTEGER paise. Leave EMPTY unless it is a genuine previous price.
stock_qty        Integer. 1 for unique pieces.
is_unique        TRUE / FALSE. TRUE means quantity can never exceed 1.
fabric           One of the permitted fabrics below. No other value permitted.
weave_technique  One of the permitted weaves below.
primary_color    One of the 14 colour families below. Free text breaks filtering.
secondary_colors Semicolon-separated, same vocabulary.
saree_length_m   Decimal metres. Typically 5.5 without blouse, 6.3 with.
blouse_piece     One of: attached_unstitched, separate_unstitched, none.
                 Largest single source of post-sale complaints when left blank.
care_instructions Free text. E.g. "Dry clean only".
weight_grams     Integer. Measured PACKED, not bare. Shipping is billed on weight.
hsn_code         From the CA (Task 3). Required on the invoice.
gst_rate         From the CA (Task 3). Per product. Not a global constant.
occasion_tags    Semicolon-separated from: Wedding, Festive, Office, Daily, Party.
description      80-150 words for a human. Occasion, pairing, feel. Not keyword soup.
image_folder     Relative path to this SKU's image folder, e.g. images/KAN-2609-014
alt_hero         Alt text for the hero image. Describes the saree. Required.
status           draft | active | sold_out | archived
```

## Controlled vocabularies

**fabric** (10) — `Silk` · `Cotton` · `Cotton-Silk` · `Linen` · `Georgette` · `Chiffon` · `Crepe` ·
`Organza` · `Tussar` · `Blended`

**weave_technique** (11) — `Kanjivaram` · `Banarasi` · `Ikat` · `Jamdani` · `Chanderi` ·
`Maheshwari` · `Sambalpuri` · `Pochampally` · `Printed` · `Plain` · `Other`

**primary_color — the 14 colour families** — `Red` · `Maroon` · `Pink` · `Orange` · `Yellow` ·
`Gold` · `Green` · `Blue` · `Navy` · `Purple` · `Black` · `White` · `Grey` · `Beige`

A colour family is not the colour name you would use in conversation. "Rani pink", "magenta" and
"fuchsia" all file under `Pink`. The family is what a filter offers; the poetic name belongs in
`title` and `description`. Free text here produces a filter with 300 options and no users.

**blouse_piece** (3) — `attached_unstitched` · `separate_unstitched` · `none`

**occasion_tags** (5) — `Wedding` · `Festive` · `Office` · `Daily` · `Party`

**status** (4) — `draft` · `active` · `sold_out` · `archived`

## SKU format

`<FABRIC3>-<YYMM>-<NNN>` — three-letter fabric or weave prefix, year-month of intake, sequence.

```
KAN-2609-014   Kanjivaram, intake Sept 2026, 14th
COT-2610-003   Cotton, intake Oct 2026, 3rd
```

**The SKU never changes.** It is the permanent link between a physical saree, its image folder, its
catalog row, every order line, and every invoice. Renaming one after an order exists breaks the
audit trail.

## Money

**Integer paise only.** ₹2,499.00 is `249900`.

No floats anywhere — `24.99 * 100` does not reliably equal `2499` in binary floating point, and a
single rounded paise in a GST split is a reconciliation failure. No `₹`, no `,`, no `.` in a price
cell.

`mrp_paise` stays **empty** unless it is a genuine previous price. A fabricated MRP to manufacture a
discount is prohibited by PRD §8.4 — no fake MRPs, no fake scarcity.

## Validation checks (Task 10, Step 4)

Run all six before the catalog is considered complete:

- [ ] No duplicate `sku`
- [ ] Every `is_unique = TRUE` row has `stock_qty = 1`
- [ ] No `price_paise` or `mrp_paise` cell contains `.` or `,` or `₹`
- [ ] Every row has a non-empty `weight_grams`, `hsn_code`, `gst_rate`, `blouse_piece`, `alt_hero`
- [ ] Every `fabric`, `weave_technique`, `primary_color`, `blouse_piece`, `status` value is in the
      permitted vocabulary above
- [ ] Row count matches the physical saree count

## Working method

**Enter three products completely first, as a pilot.** Three, not thirty. Entering three surfaces
every ambiguity in this contract before it is repeated three hundred times.

If a value cannot be determined for a pilot row, **the contract is wrong — fix the contract, not the
row.**

Two people can work in a shared spreadsheet simultaneously. That is the reason the catalog is built
here and not in an admin panel that does not exist yet.
