# Simulation

Repository: `kathmandu-traffic`. Every simulated number comes from public Kaggle notebooks; the
repository's `paper/claims.md` traces each one to its notebook, file and computation.

## Model

- **Simulator:** UXsim 1.14.2, a mesoscopic link-based kinematic wave model; drivers reroute to
  current travel times every 600 s.
- **Network:** OpenStreetMap trunk, primary, secondary and tertiary roads in a 12 x 12.5 km box
  around the Ring Road: 472 nodes, 1,242 directional links. Main roads 60 km/h; side streets one
  lane at 30 km/h with 0.353 of a main lane's capacity; fixed-time signals on a 90 s cycle.
- **Demand:** JICA 2011 vehicle origin-destination survey, 840,775 PCU per day over 51 zones; peak
  factor 0.10 gives 58,525 PCU/h, held for two hours.
- **Measure:** mean time in system over all vehicles (unfinished trips counted up to the horizon),
  paired by seed, 95% intervals, Bonferroni correction over 8 tests.

## Results (15 paired seeds)

| Test | Change in time in system | Verdict |
|---|---|---|
| H1 adaptive signals | -23.2% +/- 2.0% | reduces |
| H2 congestion pricing | -5.9% +/- 1.9% | reduces |
| H3 adaptive signals + pricing | -28.9% +/- 1.7% | reduces (effects add, interaction +0.2% +/- 3.1%) |
| H4 tidal lanes | -0.2% +/- 0.4% | no reliable effect |
| H5 parking enforcement proxy | -1.3% +/- 1.9% | no reliable effect |
| H6 staggered departures | -26.7% +/- 1.3% | reduces |
| H7 decentralised attraction | -9.1% +/- 1.3% | reduces, but no better than a uniform demand cut of the same size |

Demand growth: trips completed within 6 hours fall from 98.1% at 2011 demand to 33.6% at 2026
demand and 24.1% at 2030 demand.

## Does the plan hold at future demand? (6 paired seeds per arm)

Test fixed before the runs: an arm "solves" a year if, within 6 hours, it completes at least as many
trips as 2011 does with no intervention (98.3%) and its mean time in system is no worse (3,068 s).

| Arm | 2026: completed | 2026: time vs doing nothing | 2030: completed | 2030: time vs doing nothing |
|---|---|---|---|---|
| Do nothing | 33.5% | – | 23.2% | – |
| Adaptive signals | 36.2% | -4.8% +/- 2.8% | 25.8% | -4.0% +/- 1.8% |
| Signals + pricing | 38.5% | -7.5% +/- 2.8% | 25.9% | -4.1% +/- 1.7% |
| Staggered departures | 57.4% | -34.6% +/- 2.1% | 37.4% | -27.7% +/- 1.5% |
| All three | 78.9% | -51.0% +/- 7.3% | 45.5% | -37.2% +/- 2.7% |

**No arm solves either year.** The combined plan halves time in system at 2026 demand, but its effect
shrinks as demand grows. In this model, signal timing and demand management alone cannot absorb the
projected growth; fewer car trips (public transport) or more capacity are also needed. Caveats: the
growth rate (4.52% a year) is assumed, the toll schedule is the 2011 one, and the model is the
published one, not a calibrated one.

## Validation and its limits

- Speeds: Ring Road 18.9 +/- 1.2 km/h (survey 24.0), inner roads 13.8 +/- 0.3 km/h (survey 15.3).
- Junction volumes: the model carries 64% of the counted volume at 9 JICA junctions, correlation 0.31.
  The model gets total traffic and speeds roughly right but not where traffic goes.

## Calibrating to the junction counts (stopped at round 5)

Goal: GEH < 5 at 6 tuning junctions, with 3 junctions held out (drawn at random before any
tuning) to test the final setting once. Six rounds, each a set of public Kaggle notebooks:

| Round | Idea | Best result (of 6) |
|---|---|---|
| 1 | peak demand, entry roads, side streets, loading points | 1 |
| 2 | junction capacity (lanes, saturation flow, signal cycle) | 2, but roads far too fast |
| 3 | capacity x demand | 2 |
| 4 | measure junctions as 100 m clusters; load trips mid-block; fit OD flows to counts | 1; OD fitting diverged |
| 5 | fix a lane-count bug (two-way OpenStreetMap lanes read per direction); roadside friction | **2, mean GEH 20.0, speeds 22.3 / 15.1 km/h** |
| 6 | lane widths for untagged roads; pedestrian-street friction; OD fitting with a fixed total | 0; too much capacity removed, network far slower than the survey |

Round 5 found that the OpenStreetMap importer doubled the width of every tagged two-way road, which
explained why the Ring Road junctions were always too high. The remaining gap (Kalimati too high,
Baneshwor too low) is where camera counts by approach would help most; more parameter search is
unlikely to close it. The held-out junctions stay unrevealed because no setting reached 6 of 6. Full
record: `docs/count-calibration.md` and `lab/decisions.md` (D6-D12) in the simulation repository.
