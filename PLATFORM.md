# Sapota platform vision

From a Scituate seawall priority list to the system of record for every coastal protection asset on Earth, and the connective tissue between the three sides of coastal resilience that never talk to each other today.

---

## The three sides that don't talk

| Side | Who | What they need | What they can't get today |
|---|---|---|---|
| **Asset owners** | Municipal DPWs, ports, utilities, state CZMs, USACE, private seawall owners | A ranked repair queue and grant narrative | Standardized, continuously updated condition data |
| **Capital providers** | Insurance, reinsurance, muni bond underwriters, climate adaptation funds, FEMA BRIC | Per-asset exposure, condition, and defensibility | Structured structural data at scale — they model properties, not protection assets |
| **Executors** | Engineering consultancies, contractors | Qualified leads with pre-assessment data | A feed of condition-ranked opportunities |

Sapota becomes the shared ledger. Every side pays for a different view of the same underlying asset graph.

---

## Ring structure (wedge → platform)

Each ring has an **unlock condition** — you don't move to the next ring until the prior is defensible. Skipping a ring burns years of optionality.

### Ring 0 · Wedge · MA municipal seawall priority *(NOW — Q1 2026)*
- **Product:** Layer 1/2/3 pipeline, dashboard, pilot report
- **Customer:** MA coastal DPWs + Coastal Resilience Officers
- **Revenue target:** $100k ARR by end Q3 2026
- **Unlock:** 5 paid MA town references, 1 state (CZM/DER) warm relationship

### Ring 1 · MA system of record *(Q4 2026 — Q2 2027)*
- **Product:** Statewide dashboard + API; MA CZM-branded "Living Inventory" successor to the 2015 update
- **Customer:** MA CZM, DER, EEA as anchor; all 78 coastal MA municipalities as sub-logins
- **Revenue target:** $250k–$400k ARR (state + 10 municipal subs)
- **Unlock:** MA CZM signs statewide; Woods Hole Group either partners or has been competed with on 2 RFPs

### Ring 2 · Northeast multi-state *(Q3 2027 — Q2 2028)*
- **Product:** Same product, trained on each state's inventory data. RI CRMC, NH DES, CT DEEP, NJ DEP, ME BEP, NY DEC
- **Customer:** State CZM-equivalent agencies; their municipalities inherit
- **Revenue target:** $1M–$2M ARR (5 states at $95k, municipal revenue on top)
- **Unlock:** automated retraining pipeline per state; one non-MA case study with comparable correlation (r > 0.35)

### Ring 3 · Insurance & reinsurance data license *(Q3 2028 — Q1 2029)*
- **Product:** Per-asset condition + exposure feed, with methodology white-paper. Likely sold through Verisk/RMS/Moody's or direct.
- **Customer:** Munich Re, Swiss Re, Chubb, AIG, Lloyd's coastal peril desks, parametric insurers, ILS funds
- **Revenue target:** $2M–$5M ARR (2–5 contracts at $500k–$1.5M)
- **Unlock:** 10k+ indexed structures across 3+ states with continuous update cadence; actuarial/catastrophe-modeling reference customer

### Ring 4 · Federal *(2029)*
- **Product:** USACE asset integration (they own a parallel but disjoint inventory system called Periodic Inspections and Engineered Structures); FEMA Risk MAP input; NOAA Climate Adaptation
- **Customer:** USACE HQ + 10 coastal districts, FEMA, NOAA Coastal Resilience
- **Revenue target:** $3M–$10M ARR (enterprise contracts)
- **Unlock:** SBIR Phase II completed, one district pilot, security compliance (FedRAMP Moderate or CMMC L2)

### Ring 5 · International *(2030+)*
- **Product:** Same product, country-specific data feeds. Start with UK (Environment Agency has structured coastal data), Netherlands (Rijkswaterstaat — most mature coastal asset system globally), Australia (state-level), Japan
- **Revenue target:** $10M+ ARR
- **Unlock:** $5M ARR US baseline, multilingual product, international data partnerships

### Ring 6 · Adjacent hazards *(opportunistic)*
Once you own "coastal protection asset," you extend to:
- **Fluvial levees** (USACE has 30,000+ miles with poor condition data — same playbook)
- **Shoreline itself** (erosion / accretion without protection structures)
- **Coastal stormwater outfalls**
- **Dams** (MA DER already cares; adjacent buyer)
- **Subsidence-exposed infrastructure** (Norfolk, New Orleans, Bay Area, Jakarta)

Each is a new AI model, same customer, same platform.

---

## The defensibility stack (why this is a platform, not a tool)

A tool gets replaced. A platform compounds. The things that make Sapota compound:

1. **Structure ontology.** The canonical taxonomy and identifier of every coastal protection asset. Once insurance writes policies referencing Sapota IDs, and USACE references Sapota IDs in appropriations language, we become plumbing.
2. **Temporal ground truth.** Every re-score adds a dated observation to the asset's history. Five years in, we're the only place with a continuous time-series of US coastal structure condition.
3. **Match-pair training data.** MA 2006↔2015 was the ignition source. Each new state we onboard that has historical inventories extends the training set; models improve non-linearly.
4. **Decision-network effect.** When asset owners, capital, and executors all look at the same record, leaving the platform becomes painful. This is the real lock-in, and it only kicks in at Ring 3+.
5. **Regulatory integration.** State CZM offices that cite Sapota in their grant guidelines (Ring 1–2) make us the de facto standard. Subsequent competitors have to unseat written policy, not just a vendor.

---

## Revenue model across rings

| Ring | Primary model | Secondary |
|---|---|---|
| 0 | Per-town pilot + annual subscription | Grant-write fee |
| 1 | State SaaS + municipal subscriptions | Engineering-firm seat licenses |
| 2 | Multi-state SaaS | API per-call for firms |
| 3 | Data licensing (annual) | Per-contract modeling services |
| 4 | Enterprise / fed contracts | SBIR/DOD non-dilutive |
| 5 | International SaaS | Dev-aid / World Bank |
| 6 | New-hazard SaaS | Cross-sell to existing customers |

**ARR arc (plan, not promise):** $100k → $400k → $2M → $6M → $15M → $30M over 5 years. Solo-operator through $400k. First hire at $2M (technical co-founder or head of customer). Team of 6 by $15M.

---

## Unlock conditions summarized

Do not move to the next ring until each of these is true for the current ring:

1. Current ring's flagship customer is live, paid, and a public reference.
2. Gross margin on the current ring is > 70%.
3. At least one incumbent / would-be competitor has tried and failed to displace us on a deal we won.
4. The automation layer (see `AUTOMATION.md`) lets the current ring run without your daily attention.

If all four are true, open the next ring. If not, do not get distracted.

---

## The ten-year story

*"Sapota is the living record of every structure that holds back the sea. Municipalities use it to decide what to fix. States use it to allocate funding. Insurers use it to price risk. The Corps uses it to plan appropriations. In ten years, there is no coastal capital decision in the developed world made without consulting it."*

That's the story investors can underwrite. The ring structure is how you prove you can execute it without burning capital.
