# Sapota

**Satellite-based coastal infrastructure intelligence.** We tell coastal decision-makers which seawall, revetment, jetty, or breakwater to fix first — using free satellite imagery, CZM historical ground truth, and machine learning. No drones. No field crews. No hardware.

---

## The wedge

Massachusetts inventoried 9,543 coastal protection structures between 2006 and 2015. ~60% of publicly-owned ones were rated Poor or worse. No systematic reassessment has happened since — while Sentinel-2 has been imaging the coast every 5 days, free, the entire time.

We turn that latent feed into a living, ranked repair queue with dollar value at risk, repair cost estimate, and FEMA/storm exposure for every structure.

**First customer:** Scituate, MA. **Next eight:** Hull, Hingham, Marshfield, Cohasset, Duxbury, Weymouth, Boston, Quincy (already ranked in pipeline). **Then horizontal:** NJ, NC, FL, CA — same playbook, different state inventory.

---

## The product

Three-layer AI pipeline (built, validated):

| Layer | What | Result |
|---|---|---|
| L1 Detection | U-Net + ResNet34 on Sentinel-2 | P=0.437, R=0.694, F1=0.536 |
| L2 Temporal | 7-yr multi-index composite, CZM matched pairs | Spearman r=0.183 |
| L3 Risk | NAIP 60cm + FEMA + property value + storm + USACE costs | Spearman r=0.443 (142% lift over L2) |

Delivered to the customer as:
- **Interactive dashboard** (Leaflet, priority queue, budget allocator)
- **Pilot report** (PDF/HTML with per-structure detail, time-series, cost estimates)
- **GeoJSON / CSV export** for the customer's GIS

Working files: `SapotaLayer3Risk.ipynb`, `sapota-dashboard.html`, `sapota-pilot-report.html`, `agents.py` (SENTINEL, ANALYST, SCOUT, BUILDER).

---

## Why this is defensible

Not any single component. The combination:
- CZM historical ground truth (9,543 structures, 2006 + 2015 both) — nobody else has matched pairs across a 9-year gap in a US coastal state.
- Free Sentinel-2 cadence (5-day, 10m, forever).
- Municipal budget decision framing — "which to fix first, given $X" — not academic change detection.
- Solo operator economics. No competitor at this wedge has unit economics that work on a $5k–$15k pilot.

---

## Repo map

- `GTM.md` — who, how, for how much
- `90-day-plan.md` — weekly operating plan
- `site/index.html` — sapota.ai landing page
- `sales/one-pager.html` — leave-behind for municipal meetings
- `sales/roi-calculator.html` — interactive pilot-value tool
- `research/` — customer interview notes, funding paths, competitive scan (populate as you go)
