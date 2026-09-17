# Tasks 12 & 13 — Weights, Shipping Rate, Courier

---

## Task 12, Step 1 — Weigh ten sarees, packed as they would ship

**Packed** means with the mailer, the packing slip, and any tissue or inner wrap. Bare weight
produces wrong shipping charges and quiet margin loss on every single order — courier billing uses
the packed weight, and the gap is typically 100-200g that nobody notices until the monthly invoice.

| # | SKU | Fabric | Packed weight (g) |
|---|-----|--------|-------------------|
| 1 | | | |
| 2 | | | |
| 3 | | | |
| 4 | | | |
| 5 | | | |
| 6 | | | |
| 7 | | | |
| 8 | | | |
| 9 | | | |
| 10 | | | |

- **Minimum:** `<<FILL>>` g · **Median:** `<<FILL>>` g · **Maximum:** `<<FILL>>` g

Pick the ten to span the range — the heaviest silk and the lightest cotton included. Ten of the same
kind measures one thing ten times.

## Step 2 — Zone-wise courier rates for that weight band

Rate card from `<<FILL: aggregator or courier>>`, read on `<<FILL: date>>`, for the median weight:

| Zone | Rate | Volumetric rule |
|------|------|-----------------|
| Local | `<<FILL>>` | |
| Regional | `<<FILL>>` | |
| National | `<<FILL>>` | |
| Remote / special | `<<FILL>>` | |

Ask explicitly about the **volumetric weight rule**. A saree in a bulky box can bill on dimensions
rather than mass, which quietly moves every parcel into the next slab.

## Step 3 — Rate model

PRD §6.1 prefers flat-rate or free-over-threshold at launch. Live weight-based calculation is an
integration; a flat rate is a number.

- **Chosen model:** `<<FILL: flat ₹N | free over ₹N, else ₹M>>`
- **Worst-case zone cost it must absorb:** `<<FILL>>`
- **Margin impact per order at worst case:** `<<FILL>>`

Set the free-shipping threshold, if used, above average order value — otherwise it is a discount on
every order rather than an incentive to add one.

## Step 4 — Delivery-estimate zone table

Coarse is fine at launch, but **it must be achievable** (PRD §8.4).

| Zone | Estimate from dispatch |
|------|-----------------------|
| Metro | `<<FILL: e.g. 2-4 working days>>` |
| Tier 2 | `<<FILL: e.g. 3-6 working days>>` |
| Rest of India | `<<FILL: e.g. 5-9 working days>>` |

These figures go into [legal-pages/shipping-policy.md](legal-pages/shipping-policy.md) and must
match it exactly.

## Step 5 — Backfill weights into the catalog

☐ No `weight_grams` cell in `catalog.csv` is empty.

---

## Task 13 — Courier / aggregator account ⏳

### Step 1 — Aggregator or direct courier

- **Chosen:** `<<FILL>>` · **Reason:** `<<FILL>>`

An aggregator reduces setup work and means one courier's outage is not fatal — worth the margin for
a business with no shipping staff.

### Step 2 — Onboarding and KYC

- ☐ Account active · **Date:** `<<FILL>>`
- ☐ **Settlement set to the Task 5 business current account, not the personal account.** Check
  explicitly. This is the failure mode the entire track exists to prevent.

### Step 3 — Pickup serviceability

- ☐ Pickup confirmed for the **actual pickup address**, not assumed from a coverage map
- ☐ Pickup slot confirmed · **Address:** `<<FILL>>`

### Step 4 — Ship one real test parcel

Ship something to a friend in another state. This tests the label flow, the pickup, the tracking
link, and the real transit time in one go — before a paying customer does it for you.

- **Sent:** `<<FILL: date>>` · **To zone:** `<<FILL>>` · **Delivered:** `<<FILL: date>>`
- **Actual transit time:** `<<FILL>>` vs. Step 4 table estimate of `<<FILL>>`
- ☐ Zone table adjusted if reality disagreed. **Publish reality, not the rate card.**

### Step 5 — Log

☐ `05-Changelog.md` entry.
