# Camera-informed traffic surveillance and signal timing for Kathmandu

Kathmandu's junctions are congested and its signals are mostly fixed-time or manually controlled. This
project combines two parts:

- a **road-network simulation** of the valley, used to test signal timing and other interventions
  before anything is changed on the street, and
- a **camera system** that counts traffic and detects red-light violations at junctions, whose counts
  can calibrate the simulation and measure the effect of a change.

```
junction cameras ─► vehicle counts, PCU flow, congestion ──► calibrate / validate the simulation
       │                                                              │
       └─► red-light violations (evidence frames)       test signal timings and other interventions
                                                                      │
                                         deploy the best timing ─► cameras measure the effect
```

## Repositories

| Repository | Contents | Key documents |
|---|---|---|
| **`kathmandu-traffic-docs`** (this one) | overview, data sources, technology, results summary, roadmap | [docs/](docs) |
| [`kathmandu-traffic`](https://github.com/iamgroot400/kathmandu-traffic) | simulation: UXsim model of the valley, interventions, validation, paper | `README.md`, `docs/pipeline.md`, `docs/count-calibration.md`, `paper/` |
| [`kathmandu-traffic-vision`](https://github.com/iamgroot400/kathmandu-traffic-vision) | camera system: detection, tracking, counting, red-light violations, plate detection, training | `README.md`, `docs/datasets.md`, `docs/training.md` |

## Documents

| Page | Contents |
|---|---|
| [docs/simulation.md](docs/simulation.md) | the simulation, its results, and how they were validated |
| [docs/vision.md](docs/vision.md) | the camera system: pipeline, models, violation rule, training |
| [docs/data-sources.md](docs/data-sources.md) | every dataset used by either part, with licences |
| [docs/technology.md](docs/technology.md) | software, models, compute |
| [docs/roadmap.md](docs/roadmap.md) | what is done, in progress and next |

## Headline results so far

- **Simulation** (15 paired seeds, 95% intervals): adaptive signals cut mean time in system by
  23.2% +/- 2.0%; with congestion pricing, 28.9% +/- 1.7%; staggered work hours, 26.7% +/- 1.3%. At projected 2026 and 2030 demand the combined plan cuts time
  in system by 51% and 37% but does not restore 2011 conditions.
  Details and caveats in [docs/simulation.md](docs/simulation.md).
- **Camera system**: counting and the red-light rule are implemented and self-checked; detectors
  fine-tuned for South Asian traffic are trained (vehicles mAP50 0.67-0.73 on dense street scenes;
  plates 0.89-0.99; plate reader 98% of characters found, 99% named on known sources but 64-94% on unseen ones; whole plates not yet measured). No test on real Kathmandu junction footage yet.
