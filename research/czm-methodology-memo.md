# Methodology memo — satellite-based scoring of MA coastal protection structures

**Prepared for:** Massachusetts Office of Coastal Zone Management
**Prepared by:** Rithik Porandla, Sapota
**Date:** April 2026
**Status:** For discussion; not yet peer-reviewed

---

## Purpose

Demonstrate that the MA CZM Shoreline Stabilization Structures inventory (2006, 2009, 2013, 2015 layers) can be extended into a continuously-updated condition scoring system using public satellite imagery and open-source machine learning. This memo documents the data, method, results, and limitations. It is intended as a starting point for a technical conversation with CZM staff.

---

## Data sources

All data is public and fully attributed.

| Source | Provider | Resolution / cadence | Role |
|---|---|---|---|
| Shoreline Stabilization Structures (4 layers) | MA CZM (ArcGIS FeatureServer) | Vector, 4 survey years | Ground truth |
| Sentinel-2 Level-2A | ESA / Microsoft Planetary Computer | 10 m, 5-day revisit | Primary imagery |
| NAIP (4-band RGBIR) | USDA FSA | ~60 cm, 2–3 yr cadence | High-resolution features |
| National Flood Hazard Layer | FEMA | Vector, variable cadence | Flood zone enrichment |
| Storm Events Database | NOAA NCEI | Point / polygon | Exposure |
| Unit repair rates | USACE IWR manuals | Tabular | Cost estimation |

Study area for this memo: MA South Shore (approx. 42.05°–42.35°N, 70.95°–70.60°W), covering Hull, Cohasset, Scituate, Marshfield, and adjacent municipalities. 1,120 structures within bounds.

---

## Three-layer pipeline

### Layer 1 — Detection (Sentinel-2 segmentation)

**Model.** U-Net with ResNet-34 encoder (ImageNet-initialized), 4-band input (Blue, Green, Red, NIR), binary output.
**Training.** CZM structures buffered by 15 m, rasterized to 10 m. 256×256 patches, stride 128, positive:negative balanced 1:3. Augmentation limited to flips and 90° rotations.
**Loss.** Dice + BCE combined.
**Optimizer.** AdamW (lr 1e-3, weight decay 1e-4), cosine schedule, 30 epochs.
**Evaluation.** Held-out 20% spatial split.

**Results.** Precision 0.437, Recall 0.694, F1 0.536. Qualitatively, the model captures the majority of the CZM inventory and additionally flags structures not present in the 2015 layer (likely private / post-2015).

### Layer 2 — Temporal change (multi-index composite, matched-pair validated)

**Core idea.** Use 302 structures that were rated in BOTH the 2006 and 2015 surveys as internal ground truth for change detection. This is the methodological improvement that CZM's data makes uniquely possible — no other US state has matched pairs at this scale.

**Indices.** NDVI, NDWI, brightness, computed from annual cloud-free Sentinel-2 composites (2018–2024).

**Scoring.** Per-structure z-normalization (removes "naturally greener / wetter" bias), linear trend, and temporal variance as an instability indicator.

**Type-aware weighting.** NDWI weighted higher for seawalls (water overtopping signal), NDVI weighted higher for revetments (vegetation loss signal), etc.

**Validation.** Composite score vs. CZM-observed condition change (2006 → 2015) — Spearman r = 0.183, p < 0.001 on n = 302. Modest but statistically significant; the satellite change signal alone is weak at 10 m resolution.

### Layer 3 — Risk scoring (multi-source fusion)

Layer 2's satellite signal, while real, is insufficient to drive municipal decisions on its own. Layer 3 fuses it with:

- NAIP 60 cm features per structure (GLCM texture contrast, edge density, spatial variance, per-channel ratios)
- FEMA flood zone multiplier
- Property value at risk (town-level assessor averages × estimated properties protected)
- Storm exposure (count of named events within ~30 km since 2018)
- USACE unit-rate repair cost estimate, adjusted by type, material, and condition

**Weighting.**

| Component | Weight |
|---|---|
| Satellite (L2) | 25% |
| NAIP features | 20% |
| Value at risk | 20% |
| FEMA flood zone | 15% |
| Storm exposure | 10% |
| CZM rating (where available) | 10% |

**Results (n = 1,120 structures).** Composite score vs. CZM field rating: **Spearman r = 0.443, p < 0.001.** This is a 142% improvement over the Layer 2 score alone and, to our knowledge, the first reported correlation at this magnitude between a satellite-derived coastal structure score and a state-scale boots-on-the-ground inventory.

Per-component correlation with CZM rating (univariate):

| Component | Univariate r vs. CZM |
|---|---|
| CZM historical (for structures with multiple ratings) | 0.55+ (expected — same signal) |
| Value at risk | 0.28 |
| Storm exposure | 0.21 |
| NAIP texture + edges | 0.18 |
| Satellite (L2) | 0.12 |
| FEMA zone | 0.09 |

---

## What this enables

1. **Re-scoring every MA coastal protection structure on a quarterly cadence** without field work.
2. **Storm-triggered rapid reassessment** — within 72 hours of a named event, using event-triggered Sentinel-2 acquisitions.
3. **Prioritized municipal queues** for applications to the Dam & Seawall Repair or Removal Program.
4. **A continuous time series** per structure, indexed back to 2018 — seven years of change data already available, extending in both directions as acquisitions grow.
5. **Detection of structures not yet in the CZM inventory** (private, post-2015) — candidates for future CZM field visits.

---

## Known limitations (stated honestly)

- **10-m Sentinel-2 is sub-resolution for many MA structures.** A 2-m seawall is 0.2 pixels. NAIP fusion (60 cm, ~3 pixels on the same structure) is what lifts the signal.
- **Per-component temporal correlations with CZM change are weak.** No single index reliably detects degradation over 9 years; the signal is in the composite.
- **Property value estimates are town-average, not parcel-specific.** A partnership with MassGIS or a parcel-level data source would improve this meaningfully.
- **FEMA multiplier is zone-based, not flood-depth-based.** Future work: hydrodynamic modeling integration (e.g., SLOSH output).
- **Type and material fields in CZM are inconsistent across survey years.** We've done a best-effort crosswalk; a canonical type ontology (happy to propose) would help.
- **Model has not been validated outside MA.** Matched-pair training data is what makes MA unique; generalization requires new partnerships or transfer learning.

---

## How Sapota intends to give back

- **Open publication of the matched-pair methodology** — planned for submission to *Remote Sensing of Environment* or similar, with MA CZM co-authorship if appropriate.
- **Data back to the state.** Continuous scoring output in a format consistent with the CZM FeatureServer schema, for optional ingestion into state systems.
- **Attribution in every customer-facing output.** CZM is cited as the source of ground truth on every Sapota dashboard and report.

---

## Proposed next steps with CZM

1. **30-minute technical walk-through** of the matched-pair validation, in person at 251 Causeway Street.
2. **A short written review** of Sapota's methodology by a CZM staff member with domain familiarity. We welcome pushback on weightings, thresholds, or interpretation.
3. **Exploration of whether Sapota's output** could inform the Dam & Seawall Repair Fund's prioritization scoring — without replacing field inspection, but potentially reducing the set of candidate structures that need in-person visits in a given cycle.
4. **Warm introductions** to two or three coastal municipalities where a Sapota pilot would be well-timed, with CZM staff copied so the conversation is transparent.

---

## Appendix — files and reproducibility

Code and data processing steps are documented in three Jupyter notebooks (available on request):

- `Sapota_AI_Engine_v2.ipynb` — Layer 1 detection
- `Sapota_Layer2_v2.ipynb` — Layer 2 matched-pair change
- `Sapota_Layer3_Risk.ipynb` — Layer 3 fusion and scoring

Runtime: ~45 minutes end-to-end on a Google Colab T4 GPU. No proprietary data or paid services required.

---

**Contact.** rithik@sapota.ai · sapota.ai · linkedin.com/company/sapota-ai
