# Connected Rural Mobility: Local-Intercity Coordination in Rockingham County, NC

### 🚌 [Live Dashboard → komalgulati1.github.io/rockingham-transit-gap-analysis](https://komalgulati1.github.io/rockingham-transit-gap-analysis/)

---

**CR2C2 Southeast Data Competition 2026 · NC A&T State University**

> *Sponsored by **Honda** · Organized by the **Center for Regional and Rural Connected Communities (CR2C2)***
>
> *Competition Chair: **Dr. Venktesh Pandey**, Civil & Environmental Engineering, NC A&T State University*

---

## Overview

This project analyzes transit coordination gaps in Rockingham County, NC — a rural county of 91,585 residents served by three disconnected transit systems: **SKAT** (local fixed-route), **RCATS** (demand-response), and the **Triad-Danville Connector** (intercity). The work identifies three structural pain points — a last-mile intercity gap, scheduling rigidity with no coordination protocol, and spatial mismatch — and proposes three data-driven solutions to address them.

The interactive dashboard at the link above allows planners, commissioners, and community partners to explore transit accessibility across all 70 block groups in Rockingham County without requiring GIS software.

---

## Team

| Name | Role |
|---|---|
| **Komal Gulati** | Team Lead |
| **David Quansah** | Technical Analyst |
| **Akilah Jones** | Stakeholder Engager |
| **Meray Abdelmalak** | Designer / Planner |


---

## The Problem

Rockingham County has transit service on paper. In practice, riders face three compounding barriers:

### Pain Point 1 — Last-Mile Intercity Gap
- Last SKAT departure: **16:30**
- Connector arrives back in Reidsville: **19:55**
- Unserved evening window: **205 minutes**
- No transfer point, no on-call vehicle, no backup option
- Result: riders who commute to Greensboro via the Connector cannot return home by transit

### Pain Point 2 — Scheduling Rigidity & No Coordination Protocol
- RCATS requires **3-day advance booking** — incompatible with next-day Connector trips
- Morning window for demand-response pickup to catch the Connector: only **35 minutes**
- Zero data sharing or timed-transfer agreements between ADTS, Sunway, and private employers
- A 3-minute missed connection (06:32 vs 06:35) is unrecoverable with no coordination in place

### Pain Point 3 — Spatial Mismatch
- **42.2%** of residents (≈38,400) face a walk exceeding **45 minutes** to the nearest SKAT stop
- **71%** of block groups have less than 10% walkshed coverage
- **1,009 km²** of HRSA-designated Medically Underserved Area is unreachable on foot
- SKAT/RCATS/Local Link do not operate on weekends; the Connector runs 7 days/week
- Weekend service gap: **280 minutes** (08:05 → 16:50) with no local feeder available

---

## The Solutions

### Solution 1 — RockinghamConnect: Multimodal Trip Planner
A unified mobile app providing centralized booking across RCATS, Local Link, and the Connector; a unified payment wallet (RockinghamPay); healthcare appointment sync; and a personalized rider incentives program. The backend uses MILP + ACS + GTFS data and is scalable to all 9 counties in the CR2C2 region.

**"One app. One fare. One trip."**

### Solution 2 — Employer Coordination with ADTS
An employer-transit partnership framework — beginning with the Gildan corridor — that introduces monthly service contracts with ADTS, an on-demand evening vehicle tied to Connector arrivals, and a coordination dashboard visualizing SKAT schedules against Connector windows. Closes the 205-minute evening gap without new infrastructure.

### Optimization Derived — MILP-Optimized Weekend Transfer Hub
A Mixed-Integer Linear Program (PuLP/CBC solver) selecting the optimal weekend transfer hub from all 67 SKAT stop candidates, scoring each on:

```
s*_j = α·vuln_j + β·norm_visits_j − γ·penalty_j
```

where `vuln_j` is a composite vulnerability index (population, poverty rate, elderly share — ACS 2023), `norm_visits_j` is a ridership proxy from real-world POI visit density (Safegraph/Veraset 2025), and `penalty_j` is network walk time to the Connector boarding point (r5py + OSM).

**Result: Meadow Greens Shopping Center, Eden** (s\* = 0.363, vuln = 0.509) — optimal under baseline weights (α=0.4, β=0.3, γ=0.3) and robust across 4 of 7 weight combinations tested.

---

## Implementation Scenarios

| | Scenario A — RCATS Feeder | Scenario B — Eden Connector Stop |
|---|---|---|
| **Path** | Meadow Greens SC → RCATS (~25 min) → Walmart Reidsville → Board Connector | Connector rerouted directly through Eden (Meadow Greens SC) |
| **Journey Time (NB)** | ~65 min | ~25 min |
| **New infrastructure needed** | None | Requires NCDOT route amendment |
| **Block groups newly served** | 8–12 | 15–21 (high-vulnerability) |
| **Status** | **Operable today** | Advocacy goal |

---

## Dashboard

🔗 **[komalgulati1.github.io/rockingham-transit-gap-analysis](https://komalgulati1.github.io/rockingham-transit-gap-analysis/)**

The Transit Access Coverage Dashboard visualizes walk-time accessibility across Rockingham County using **2,195 H3 hexagonal zones** (resolution 8, ~0.74 km² each) with population areal-weighted from ACS 2023 block group estimates.

**Key statistics displayed:**

| Coverage Category | Residents | Share | Block Groups |
|---|---|---|---|
| Zero coverage | 39,483 | 43% | 29 |
| Partial coverage | 25,814 | 28% | 21 |
| Adequately served | 26,288 | 29% | 20 |
| **Total** | **91,585** | — | **70** |

**Dashboard features:**
- Walk time bands (0–5 min through 45+ min) across 2,195 H3 hexagons
- Stop vulnerability index (0–100) for all 67 SKAT stops
- SKAT ¼-mile walkshed (FTA 402m standard)
- Connector route + Scenario A (RCATS Feeder) + Scenario B (Eden Direct)
- Per-stop filtering by city or name
- Four color modes: walk time, population size, poverty rate, elderly share

---

## Financial Viability

The Connector is financially viable at low ridership:

- Total annual operating cost: **$386K**
- NCDOT covers 80%: **$309K**
- Farebox gap from ticket sales: **$77K**
- Break-even: **~8 passengers per trip** at $7 average fare × 1,460 trips/year

**"The gap is coordination, not cost."**

*Sources: FTA NTD 2023 · American Bus Association Motorcoach Benchmarks · Connector Schedule & Fare Table (rockinghammoves.org, Aug 2025)*

---

## Data & Methods

| Layer | Source |
|---|---|
| Population estimates | ACS 2023 (B01003, C17002, B01001) |
| Transit stop locations | SKAT GTFS — ITRE/NC State, v1 (Jan 2023) |
| Connector schedule | NCDOT/Sunway Charters Timetable, Aug 2025 |
| Block group shapefiles | Census TIGER 2023 |
| H3 hexagonal grid | Uber H3, resolution 8 (~0.74 km²) |
| Walk time computation | Haversine distance at 1.4 m/s (FTA standard) |
| Network walk time | r5py + OpenStreetMap; haversine fallback |
| Ridership proxy | Safegraph/Veraset Work Location Panel, Feb–May 2025 |
| Optimization solver | PuLP / CBC (MILP) |
| Analysis tools | Python · GeoPandas · Shapely · H3-py |
| Visualization | Leaflet.js · CARTO basemap |

---

## Acknowledgments

This project was made possible through the support of:

- **Honda** — Competition Sponsor
- **CR2C2 (Center for Regional and Rural Connected Communities)** — Competition Organizer, NC A&T State University
- **PART (Piedmont Authority for Regional Transportation)** — Community Partner
- **ADTS (Aging, Disability & Transit Services of Rockingham County)** — Stakeholder; special thanks to **Meggan Odell** for stakeholder engagement and schedule confirmation
- **Rockingham County** — Stakeholder
- **Sunway Charters / NCDOT** — Triad-Danville Connector operator
- **SKAT** and **RCATS** — Local transit data

---

## Repository Contents

| File | Description |
|---|---|
| `index.html` | Interactive Transit Access Coverage Dashboard (self-contained, no build step) |
| `Data_Competition_2026_Presentation.pdf` | Full competition presentation (27 slides + appendices) — [View PDF](https://github.com/komalgulati1/rockingham-transit-gap-analysis/blob/main/Data_Competition_2026_Presentation.pdf) |

---

## Citation

If referencing this work, please cite:

> Gulati, K., Quansah, D., Jones, A., & Abdelmalak, M. (2026). *Connected Rural Mobility: Local-Intercity Coordination in Rockingham County, North Carolina.* CR2C2 Southeast Data Competition 2026, NC A&T State University. Advisor: Dr. Venktesh Pandey.

---

*CR2C2 Southeast Data Competition 2026 · NC A&T State University · Data: ACS 2023 · SKAT GTFS (ITRE 2023) · Census TIGER 2023*
