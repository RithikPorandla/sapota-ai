# Sapota 90-Day Operating Plan

Solo founder, full-time. Everything is land-Scituate-and-one-more. If day 90 ends with one paid pilot + one signed LOI + SeaAhead acceptance, the quarter is a win.

## North-star sequence
1. Ship the Scituate report.
2. Get MA CZM to warm-intro three more towns.
3. Submit one MVP grant co-application.
4. Apply to NOAA SBIR Phase I.

Everything else is secondary.

---

## Weeks 1–2: Foundations & Scituate close

**Product**
- Polish `sapota-pilot-report.html` with Scituate-specific data, town seal, contact.
- Print 5 hard copies for in-person meetings.
- Fix FEMA data ingestion (NFHL REST API) — kills the "using default multiplier" bug. 15% of the risk weight is currently dead.

**Sales**
- Email Scituate DPW Director requesting 30-min in-person meeting. Subject: "Free pilot: AI-ranked repair priority for Scituate seawalls."
- If no reply in 5 days: call. Then show up at the DPW counter.
- Prepare one-pager (`sales/one-pager.html`) and the ROI calculator as leave-behinds.

**Content**
- LinkedIn post #7: "How we detected 186 coastal structures from space (and why Scituate is first)."
- Personal connection requests to 30 coastal-MA municipal employees.

**Ops**
- Form LLC if not done (MA SOC, $500). EIN. Business checking (Mercury or Relay).
- Register sapota.ai (or confirm) — deploy `site/index.html`.
- Stripe for pilot invoicing.

**Win condition for these 2 weeks:** Scituate pilot meeting on the calendar.

---

## Weeks 3–4: Scituate pilot delivery

**Product**
- Run full L3 pipeline on Scituate with latest Sentinel-2 + NAIP.
- Generate personalized pilot report.
- Host dashboard at sapota.ai/scituate (private link, basic auth).

**Sales**
- Deliver the Scituate report in person. Walk DPW through the top 10.
- Get verbal commitment for paid Year 1 subscription ($18k) OR reference right.
- Ask for intros to 3 peer DPW directors.

**Fundraising prep**
- Draft NOAA SBIR Phase I solicitation response (coastal resilience topic).
- Identify MVP Action Grant opportunity for next town (Hull or Marshfield).

**Content**
- LinkedIn post #8: "We delivered Scituate's coastal asset priority list today. Here's what we found." (with DPW permission; else blur).

**Win condition:** Scituate signed paid subscription OR formal reference letter.

---

## Weeks 5–6: Second town & CZM warm intro

**Sales**
- Request 30-min call with MA CZM. Pitch: "You trained us. We're giving state back the quarterly re-scoring." Ask for 3 intros.
- Target town #2: Hull (largest structure count, highest value at risk). Pitch paid pilot at $7,500.
- Target town #3: Marshfield (active MVP town — pitch co-grant).

**Product**
- Add multi-town comparison view to dashboard.
- Build CSV export aligned to MA CZM schema (wins CZM meeting).

**Content**
- LinkedIn post #9: "What Massachusetts CZM taught us — and what we can give back."
- Apply to speak at NERCZMA spring meeting.

**Win condition:** 1 new paid pilot signed OR 2 CZM-referred town discovery calls booked.

---

## Weeks 7–8: Grant applications & SeaAhead

**Fundraising**
- Submit NOAA SBIR Phase I (target: $175k non-dilutive).
- Submit SeaAhead Pilot 2026 final application with Scituate as anchor.
- Ghost-write MVP Action Grant application for town #2 with Sapota as deliverable.

**Sales**
- Cold-email Woods Hole Group practice lead. Offer partnership conversation OR clearly price against them.
- Email two engineering firms (GEI, Tighe & Bond) pitching $36k seat license.

**Content**
- Long-form blog post / LinkedIn article: "The 9,543 forgotten coastal structures (and how Massachusetts is going to find them again)."

**Win condition:** SBIR submitted. SeaAhead submitted. One MVP grant in motion.

---

## Weeks 9–10: Scale the motion

**Sales**
- Follow-up cadence on every contact touched in weeks 1–8.
- Book discovery calls with 2 insurance/reinsurance contacts (start the slow sale early — Chubb or Munich Re).
- Run a "Coastal Resilience Roundtable" (virtual, invite 10 DPW directors + CZM). Low-cost demand gen.

**Product**
- Storm-triggered rapid reassessment feature (the differentiator for annual subscription upsell).
- API v0: authenticated GeoJSON endpoint for one engineering firm.

**Win condition:** 2 signed annual subscriptions OR $50k+ in booked pipeline.

---

## Weeks 11–12: Close & report

**Sales**
- Push all outstanding proposals to close before end of quarter.
- Publish Q1 customer case study (Scituate results — with permission).
- Update investor/advisor list; schedule 5 informal advisor calls.

**Content**
- Year-one roadmap publicly on sapota.ai — builds credibility for next cohort of towns.

**Win condition:** End Q with $25k+ ARR booked, 3 named customers, SBIR filed, SeaAhead decision pending, pipeline of 10+ qualified municipal opportunities.

---

## What NOT to do in the first 90 days

- Don't build a mobile app.
- Don't chase insurance or USACE before you have 3 municipal references.
- Don't hire (even part-time) unless a customer is blocked on it.
- Don't expand outside MA/NE until you have 5 paid MA towns.
- Don't raise a friends-and-family round unless SBIR gets denied.
- Don't rebuild the product. The pipeline works. Ship.

---

## Daily discipline (the only three things)

1. **Mornings (2 hrs):** product — ship something a customer can see.
2. **Midday (3 hrs):** sales — 10 touches minimum (call, email, LinkedIn DM).
3. **Evening (1 hr):** content — one post or one paragraph toward a long-form piece.

If a day has none of these three, the day did not happen.
