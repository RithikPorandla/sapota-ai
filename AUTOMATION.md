# Sapota automation — how one person runs a platform

The premise: solo operator through $400k ARR. First hire only when the work that remains after automation still exceeds 60 hrs/wk.

Every function below is either *automated* or *semi-automated* (AI agent drafts → human approves in <5 min). Nothing is "done by hand twice." If a task is touched manually twice, it gets automated before the third time.

---

## The four engines

Everything is one of four engines. Each engine has a trigger, a pipeline, and an output destination.

### 1. Product engine — the ML pipeline runs itself
### 2. Customer engine — onboarding, delivery, support
### 3. Growth engine — lead gen, outreach, content
### 4. Ops engine — billing, reporting, grant writing, founder dashboard

---

## Engine 1 · Product (the pipeline that runs itself)

**Today:** you open a Colab notebook and click "Run All." That's the thing to kill.

**Target state:** every customer's dashboard is re-scored every quarter without you touching anything.

### Pipeline topology

```
[schedulers: GitHub Actions + Modal cron]
          │
          ▼
[Sentinel-2 weekly pull] ──► [per-AOI composite] ──┐
                                                     │
[NAIP annual delta]       ──► [feature extraction] ──┤
                                                     │
[FEMA NFHL monthly diff]  ──► [enrichment layer]   ──┤
                                                     ├──► [Layer 3 scoring] ──► [Postgres (Supabase)]
[CZM FeatureServer diff]  ──► [ground-truth update]──┤                             │
                                                     │                             ▼
[NOAA storm webhook]      ──► [triggered re-score] ──┘                    [per-customer static site]
                                                                                   │
                                                                                   ▼
                                                                       [customer dashboards auto-update]
```

### Stack

| Component | Tool | Why |
|---|---|---|
| ML compute | **Modal** | GPU on demand, pay-per-second, no devops. Run L1/L2/L3 as deployed Python functions. |
| Scheduler | **Modal cron** + **GitHub Actions** | Simple. No Airflow. |
| Data warehouse | **Supabase (Postgres + PostGIS)** | Structures, scores, time-series, auth, storage in one place. |
| Object storage | **Cloudflare R2** | Imagery tiles, GeoTIFFs, reports. Egress-free. |
| Tile serving | **Protomaps / PMTiles** on R2 | Static vector tiles for dashboards; no tile server needed. |
| Dashboards | **Next.js on Vercel**, static export per customer | One codebase, customer-specific data slice at build time. |
| Storm trigger | **NOAA NHC CAP feed** → Modal webhook | Auto-fires rapid reassessment. |

### What gets automated

- **Weekly:** pull latest cloud-free Sentinel-2 over each customer AOI; update NDVI/NDWI/brightness composites.
- **Quarterly:** re-run L3 scoring for every active customer; regenerate dashboards and PDF reports; email the customer a "quarterly change summary" automatically.
- **Storm-triggered:** within 72 hours of a named storm impacting a customer AOI, run a rapid Sentinel-2 + (if available) emergency-tasked Planet/Maxar acquisition; re-score; email.
- **Annual:** NAIP refresh (when USDA releases new imagery), CZM FeatureServer diff, model retraining with new matched pairs.
- **Monitoring:** each pipeline run writes a row to a `runs` table with precision/recall/r-value. A Sentry alert fires if any metric drops >10% from rolling median.

### What stays manual (for now)
- Onboarding a new state (requires studying their inventory schema).
- Model architecture changes.
- Customer-requested one-offs that don't generalize.

---

## Engine 2 · Customer (onboarding → delivery → support)

**Today:** pilot means a meeting, a notebook run, a PDF emailed. That's one-off.

**Target state:** self-serve pilot request → contract signed → dashboard provisioned → report emailed, with founder involvement only at the discovery call and the walkthrough.

### Customer-facing automation

| Step | Tool | Automation |
|---|---|---|
| Pilot request | `sapota.ai/pilot` form | Form submit → Attio (CRM) + Slack notification |
| Discovery call | **Cal.com** self-booking | Town info pre-filled from form; auto-pulls OpenStreetMap / CZM count for that town into the briefing doc |
| Contract | **Stripe Checkout** + **Dock** for signed MSA | $7,500 pilot auto-invoiced; signed LOI template |
| Provisioning | Modal-triggered run on receipt of Stripe `checkout.session.completed` webhook | Dashboard URL + credentials auto-emailed within 30 min |
| Delivery | Auto-generated PDF report + hosted dashboard | Re-run quarterly; email sent via Resend |
| Support | **Plain** (or Intercom) | AI triage (Claude): low-urgency answered from docs, high-urgency routed to you |
| Customer health | Metrics dashboard in Supabase | Login frequency, dashboard views, feature usage — flags at-risk accounts |

### Contract templates (all in `research/contracts/`)
- Pilot MSA (1-page)
- Annual subscription addendum
- Reference-right clause
- Data-use + IP terms
- NDA (mutual, for pre-sale)

All maintained as Markdown → rendered to PDF via a script. Never re-typed.

---

## Engine 3 · Growth (lead gen → outreach → content)

**Today:** you manually write LinkedIn posts and cold emails.

**Target state:** you *approve* drafts; you don't *write* drafts. Approval takes <10 min/day.

### The four agents (your `agents.py` — evolved)

Already in seed form. Each becomes a scheduled Modal function with inputs/outputs in Postgres.

| Agent | Input | Output | Human step |
|---|---|---|---|
| **SCOUT** | New public data (RFPs from COMMBUYS, MA central register; grant announcements; storm events; new LinkedIn hires at target orgs) | Ranked lead list in CRM | Review weekly, approve outreach |
| **ANALYST** | Signed customer AOI | Risk report, grant-narrative draft, MVP application draft | Edit + send |
| **BUILDER** | Weekly product deltas + news events | Draft LinkedIn post, newsletter, blog section | Approve + post (or auto-post at set threshold) |
| **SENTINEL** | Storm feeds, satellite availability, model performance | Operational alerts + customer-facing incident updates | Acknowledge |

All run on Claude or open-weights (Llama 3.1 70B via Modal) for cost control on volume tasks.

### Outbound stack

| Layer | Tool |
|---|---|
| Prospect DB | **Apollo.io** API → enriched into Attio |
| Email sequencer | **Instantly.ai** (or Smartlead) — 3-step sequence, auto-pauses on reply |
| LinkedIn outreach | **Dripify** or manual (safer) |
| Meeting booking | **Cal.com** |
| Content | **Typefully** for LinkedIn scheduling |

### Content automation loop

1. BUILDER watches the `runs` table. When a model run surfaces something newsworthy (e.g., "Hull seawall cluster now in rapidly_degrading bucket"), BUILDER drafts a post.
2. You approve or reject (one click).
3. Approved → Typefully schedule → LinkedIn.
4. Same deltas feed a monthly newsletter (Beehiiv or Resend broadcast).

Target: 3 LinkedIn posts/week, 1 long-form/month, 1 newsletter/month — author effort < 30 min/week total.

---

## Engine 4 · Ops (billing, reporting, grants, founder cockpit)

### Billing
- **Stripe** subscriptions + invoicing
- Grant-funded customers: Stripe custom invoice + NET 30 terms; Sapota pays MA sales tax where applicable (no sales tax on SaaS in MA currently, but check)

### Grant-writing automation
The MVP / CZM / BRIC / SBIR narrative templates live in `research/grants/` as prompt-and-template pairs.
- Agent: fills in town-specific data (structure count, value at risk, top-10 priorities) into the narrative.
- Human review: 20 min per application, not 3 days.
- Target volume: 2 grant narratives ghost-written per month.

### Founder cockpit
One page (Supabase + simple Next.js view) that shows, every morning:
- Pipeline runs last 24h (success/fail)
- New pilot requests
- CRM stage movement
- Cash balance (Stripe + Mercury API)
- Weekly metrics progress vs target (see `GTM.md` scoreboard)

Automated Monday morning email summarizing the week. Quarterly investor/advisor update auto-drafted from the same data.

### Compliance & records
- **Rewind** or **BorgBase** daily backups of Supabase
- **Vanta** when you start selling to insurance or federal (SOC 2 readiness) — not before
- Every contract, every run, every email logged; auditable from day one

---

## The stack in one view

| Layer | Tool | Monthly cost solo |
|---|---|---|
| Compute (GPU + functions) | Modal | $50–$300 depending on re-score volume |
| Database + auth + storage | Supabase Pro | $25 |
| Object storage | Cloudflare R2 | $5–$20 |
| Frontend hosting | Vercel Pro | $20 |
| Email (transactional) | Resend | $20 |
| Scheduling | Cal.com self-hosted or Pro | $15 |
| CRM | Attio free / starter | $0–$34 |
| Outbound sequencer | Instantly.ai | $37 |
| Billing | Stripe | 2.9% + 30¢ |
| Monitoring | Sentry free + UptimeRobot | $0 |
| AI API | Anthropic (Claude) | $50–$300 agents + content |
| Document signing | Dock or Docusign (1 seat) | $15 |
| Accounting | Relay + Rillet or Xero | $30 |
| Legal | Clerky / Stripe Atlas one-time | $500 one-time |
| **Total run rate** | | **~$300–$850/mo** |

Fully loaded monthly opex < $1k until the first hire. Gross margin on pilot revenue ≥ 90% at this stack.

---

## What gets built when (automation sequencing)

Don't try to automate everything in month one. Build each engine as you hit the pain.

| Month | Automation milestone |
|---|---|
| 1 | Stripe + Cal.com + simple pipeline trigger (manual re-run is fine at 1 customer) |
| 2 | Supabase schema + agent scaffolding; SCOUT + BUILDER live |
| 3 | Modal-scheduled quarterly re-scoring for all active customers |
| 4 | Dashboard auto-provisioning on Stripe webhook |
| 5 | Storm-triggered rapid reassessment live |
| 6 | Grant-narrative generator (MVP template first) |
| 9 | ANALYST drafting full pilot reports end-to-end (you review in 20 min) |
| 12 | Full state onboarding automation — add a state in <1 week of founder time |

If any of these slip, the platform rings slip.

---

## The rule

If you do it twice and it wasn't fun the second time, it gets automated before the third. That's how solo reaches $2M ARR.
