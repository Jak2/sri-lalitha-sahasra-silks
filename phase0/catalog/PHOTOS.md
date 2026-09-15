# Tasks 11 & 14 — Photo Audit and Image Normalisation

⚠️ PRD §3.3 names the **drape shot** as non-optional. A saree product page without one converts
materially worse — a flat-lay tells a buyer the colour, a drape shot tells them how it will fall on
a body, which is the actual purchase question.

**PRD §13.1 assumption A6 — that the existing professional photos include drape shots — is
UNVERIFIED.** Task 11 exists to verify it. If drape shots are widely missing, **a reshoot becomes the
launch critical path, ahead of any website work.**

---

## Task 11 — Coverage audit

Fill [photo-audit.csv](photo-audit.csv), one row per SKU from `catalog.csv`, `Y`/`N` per shot:

```
sku,hero,drape,pallu,border,fabric_macro,blouse,notes
```

**Verify:** every SKU from Task 10 appears exactly once.

### Gap totals

| Gap | Count |
|-----|-------|
| SKUs missing a hero | `<<FILL>>` |
| SKUs missing a **drape** shot | `<<FILL>>` |
| SKUs missing a fabric macro | `<<FILL>>` |
| SKUs missing a blouse shot (where `blouse_piece` ≠ `none`) | `<<FILL>>` |

### Decision on the gaps

Choose one, explicitly, and log it:

- ☐ Reshoot all
- ☐ Reshoot the top N sellers only — N = `<<FILL>>`
- ☐ Launch with a subset of the catalog — only SKUs with full coverage go `active`, the rest stay
  `draft`

Say it explicitly rather than launching without drape shots and wondering later why the conversion
rate is poor. **Recorded in `Changelog.md`:** ☐

---

## Task 14 — Image normalisation

### Folder and filename scheme — fixed

```
catalog/images/KAN-2609-014/
  01-hero.jpg
  02-drape.jpg
  03-pallu.jpg
  04-border.jpg
  05-fabric.jpg
  06-blouse.jpg
```

Folder name matches `sku` **exactly**. Filenames match this scheme **exactly**. Consistent naming is
what makes bulk import possible on every one of the three paths — inconsistent naming means three
hundred manual uploads, and that cost lands whichever stack wins.

`image_folder` in `catalog.csv` points here: `images/KAN-2609-014`.

> Note: `catalog/images/` is git-ignored — masters are large binaries and do not belong in git
> history. Back them up outside the repository. The naming contract is what is version-controlled;
> the pixels are not.

### Steps

- [ ] **Crop every hero to one aspect ratio.** Recommended **4:5 portrait** — matches how a saree
      hangs and matches Instagram, which is the Phase 2 channel. A grid of mixed ratios reads as
      broken before a buyer can name why.
- [ ] **Colour-correct for consistency across the catalog.** Twenty heroes side by side on one
      screen must look like one shop, not twenty photographers. A judgement call — make it
      deliberately.
- [ ] **Export masters at 2000px+ on the long edge**, quality ~85. Masters only. Per-device resizing
      is the platform's job, never a manual one. Product-page zoom depends on the long edge.
- [ ] **Write `alt_hero` for every SKU into `catalog.csv`.** No empty cell. Required for
      accessibility (PRD §10.2) and the only image-SEO signal that actually matters.

### Alt text that works

Describe the saree, not the file. A screen-reader user is deciding whether to buy.

- ✅ `Deep maroon Kanjivaram silk saree with a gold zari border and peacock-motif pallu`
- ❌ `saree image` · `KAN-2609-014` · `beautiful traditional saree buy online india`

The third is keyword stuffing. It helps nobody, and it reads as spam to both a person and a
crawler.
