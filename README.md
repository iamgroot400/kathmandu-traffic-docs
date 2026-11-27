# Kathmandu traffic: simulation-tested signal reform, measured by junction cameras

Kathmandu's peak-hour congestion is mostly a junction problem, and changing signals on the street is
slow and risky. This project has two parts that work together:

- a **simulation** of the valley's road network, to test which changes cut delay before anything is
  changed on the street;
- a **camera system** for junctions that counts traffic, reads the signal, catches red-light running
  and reads number plates, to give the simulation real counts and measure the effect of a change.

```
junction cameras -> vehicle counts, PCU flow ---------> calibrate / validate the simulation
       |                                                         |
       +-> red-light violations + evidence cards      test signal timings and other changes
                                                                 |
                                    deploy the best plan -> cameras measure the effect
```

## How we got here

1. **The question.** Can Kathmandu's peak-hour congestion be cut without building roads? Most of the
   delay forms at junctions, and most signals run fixed timings or are handled by hand.
2. **Pick levers a city can actually use.** A metro or new roads cannot be built, or even piloted,
   within this challenge. So we chose changes a city can make with what it has: signal timing, road
   pricing, and later tidal lanes, parking enforcement, staggered work hours and moving offices out of
   the core.
3. **Test before touching the street.** Retiming real signals is slow, and a bad plan makes traffic
   worse for everyone. A simulation lets us compare every option with numbers first. We chose a
   mesoscopic simulator (UXsim), fast enough to run the whole valley 150 times, so each result is
   measured over 15 random seeds with confidence intervals instead of one run that could be luck.
4. **Build the model on real survey data.** Demand comes from the JICA 2011 household travel survey,
   the most recent full survey of the valley, loaded onto 472 junctions and 1,242 road links from
   OpenStreetMap. Model speeds come out close to the surveyed speeds.
5. **Check the ideas against the real world.** We looked for cities like Kathmandu where congestion
   pricing has worked, and found no well-documented case (Jakarta's plan never launched, and its
   car-sharing rule was beaten by people paid to sit in cars). So we lead with signals and staggered
   work hours, which need no payment system and cannot be gamed.
6. **Make every number checkable.** Every simulated result comes from public Kaggle notebooks that
   anyone can open and rerun, and is tested seed by seed against the same baseline.
7. **Test against the future.** Traffic has grown since 2011, so we reran the plan at projected 2026
   and 2030 demand: it still cuts time in the network by about half in 2026.
8. **Measure today's traffic with cameras.** A camera at a junction gives current counts to keep the
   model up to date and, after a change, measures whether it worked. The same camera catches red-light
   running and reads the plate, with evidence for a person to review.

## Start here

| | Link |
|---|---|
| Project brief (2 pages, PDF) | [Kathmandu_traffic_project_brief.pdf](Kathmandu_traffic_project_brief.pdf) |
| Simulation paper (PDF) | [kathmandu_traffic_paper.pdf](https://github.com/iamgroot400/kathmandu-traffic-simulation/blob/main/paper/kathmandu_traffic_paper.pdf) |
| Simulation code | [github.com/iamgroot400/kathmandu-traffic-simulation](https://github.com/iamgroot400/kathmandu-traffic-simulation) |
| Camera system code | [github.com/iamgroot400/kathmandu-traffic-vision](https://github.com/iamgroot400/kathmandu-traffic-vision) |
| Simulation inputs on Kaggle | [dataset kathmandu-traffic-sim](https://www.kaggle.com/datasets/aashikkhatri/kathmandu-traffic-sim) |
| Every simulated number, rerunnable | [Kaggle notebook 5: results](https://www.kaggle.com/code/aashikkhatri/kathmandu-traffic-5-results) |

## Results

**Simulation** (15 paired random seeds, 95% intervals, Bonferroni correction for 8 tests; 2011 demand,
117,050 vehicles per morning peak):

| Change | Time in network | Vehicle-hours saved per peak | Minutes saved per vehicle |
|---|---|---|---|
| Adaptive (queue-actuated) signals | -23.2% +/- 2.0% | 21,915 +/- 2,033 | 11.2 +/- 1.0 |
| Adaptive signals + congestion pricing | -28.9% +/- 1.7% | 27,232 +/- 1,890 | 14.0 +/- 1.0 |
| Staggered work hours | -26.7% +/- 1.3% | 25,151 +/- 1,408 | 12.9 +/- 0.7 |
| Congestion pricing alone | -5.9% +/- 1.9% | 5,617 +/- 1,843 | 2.9 +/- 0.9 |

- At projected 2026 and 2030 demand the full plan cuts time in the network by 51% and 37%.
- Model speeds are close to the survey (Ring Road 18.9 vs 24.0 km/h, inner roads 13.8 vs 15.3).

**Camera system:**

| Part | Result |
|---|---|
| Vehicle detector (motorcycle, car, bus, truck) | mAP50 0.67-0.73 on crowded South Asian street scenes |
| Plate detector | mAP50 0.89-0.99 |
| Plate reader, whole plate | 43.8% exact; 88.7% exact when it passes its own format and zone check (112 real Kathmandu plates) |
| Red-light rule | entering only, red confirmed over 3 frames, 1 s grace, amber never flagged; self-checked |
| Evidence per violation | scene and close-ups at the crossing and from the second before, plate read from the best frame, one evidence card, a review page |

## All public notebooks (Kaggle)

**Main run** (the paper's numbers):

| Step | Notebook | What it does |
|---|---|---|
| 1 | [build demand](https://www.kaggle.com/code/aashikkhatri/kathmandu-traffic-1-build-demand) | peak-hour trip tables from the JICA 2011 survey |
| 2 | [baseline and validation](https://www.kaggle.com/code/aashikkhatri/kathmandu-traffic-2-baseline-and-validation) | baseline runs, speeds and counts checked against the survey |
| 3a, 3b | [seeds 1-8](https://www.kaggle.com/code/aashikkhatri/kathmandu-traffic-3a-interventions-seeds-1-8), [seeds 9-15](https://www.kaggle.com/code/aashikkhatri/kathmandu-traffic-3b-interventions-seeds-9-15) | the seven interventions, 150 runs |
| 4a, 4b | [seeds 1-8](https://www.kaggle.com/code/aashikkhatri/kathmandu-traffic-4a-projection-seeds-1-8), [seeds 9-15](https://www.kaggle.com/code/aashikkhatri/kathmandu-traffic-4b-projection-seeds-9-15) | demand growth to 2026 and 2030 |
| 5 | [results](https://www.kaggle.com/code/aashikkhatri/kathmandu-traffic-5-results) | every results table, computed from steps 2-4 |

**Calibration rounds** (tuning the model to junction counts):

| Round | Notebooks |
|---|---|
| 1 | [peak demand](https://www.kaggle.com/code/aashikkhatri/kathmandu-traffic-6a-tuning-peak-demand), [valley entry roads](https://www.kaggle.com/code/aashikkhatri/kathmandu-traffic-6b-tuning-valley-entry-roads), [side streets](https://www.kaggle.com/code/aashikkhatri/kathmandu-traffic-6c-tuning-side-streets), [loading points](https://www.kaggle.com/code/aashikkhatri/kathmandu-traffic-6d-tuning-loading-points) |
| 2 | [main-road lanes](https://www.kaggle.com/code/aashikkhatri/kathmandu-traffic-7a-tuning-main-road-lanes), [saturation flow](https://www.kaggle.com/code/aashikkhatri/kathmandu-traffic-7b-tuning-saturation-flow), [signal cycle](https://www.kaggle.com/code/aashikkhatri/kathmandu-traffic-7c-tuning-signal-cycle), [combined](https://www.kaggle.com/code/aashikkhatri/kathmandu-traffic-7d-tuning-combined-levers) |
| 3 | [0.8 s + demand](https://www.kaggle.com/code/aashikkhatri/kathmandu-traffic-8a-tuning-0-8s-demand), [0.67 s + demand](https://www.kaggle.com/code/aashikkhatri/kathmandu-traffic-8b-tuning-0-67s-demand), [3 lanes + demand](https://www.kaggle.com/code/aashikkhatri/kathmandu-traffic-8c-tuning-3-lanes-demand), [0.5 s, 2 lanes + demand](https://www.kaggle.com/code/aashikkhatri/kathmandu-traffic-8d-tuning-0-5s-2-lanes-demand) |
| 4 | [mid-block loading](https://www.kaggle.com/code/aashikkhatri/kathmandu-traffic-9a-tuning-mid-block-loading), [fit trips to counts, 0.67 s](https://www.kaggle.com/code/aashikkhatri/kathmandu-traffic-9b-fit-od-to-counts-0-67s), [fit trips to counts, 3 lanes](https://www.kaggle.com/code/aashikkhatri/kathmandu-traffic-9c-fit-od-to-counts-3-lanes) |
| 5 | [two-way lane fix](https://www.kaggle.com/code/aashikkhatri/kathmandu-traffic-10-tuning-two-way-lane-fix), [lane fix + roadside friction](https://www.kaggle.com/code/aashikkhatri/kathmandu-traffic-10-tuning-lanes-friction) |
| 6 | [lanes + pedestrian streets](https://www.kaggle.com/code/aashikkhatri/kathmandu-traffic-11-tuning-lanes-pedestrians), [fit trips, round-6 network](https://www.kaggle.com/code/aashikkhatri/kathmandu-traffic-11-fit-od-round-6-network), [fit trips, round-5 network](https://www.kaggle.com/code/aashikkhatri/kathmandu-traffic-11-fit-od-round-5-network) |

**Future demand:** [the plan at 2026](https://www.kaggle.com/code/aashikkhatri/kathmandu-traffic-12a-interventions-at-2026), [the plan at 2030](https://www.kaggle.com/code/aashikkhatri/kathmandu-traffic-12b-interventions-at-2030).

The camera system's training notebooks are private, because they use real number-plate photos. Their
settings and results are in [training.md](https://github.com/iamgroot400/kathmandu-traffic-vision/blob/main/docs/training.md).

## Documentation

| Page | Contents |
|---|---|
| [docs/simulation.md](docs/simulation.md) | the simulation, its results and validation |
| [docs/vision.md](docs/vision.md) | the camera system: pipeline, models, violation rule, evidence |
| [docs/data-sources.md](docs/data-sources.md) | every dataset used, with licences |
| [docs/technology.md](docs/technology.md) | software, models, compute |
| [docs/roadmap.md](docs/roadmap.md) | done, in progress, next |
| [Simulation pipeline](https://github.com/iamgroot400/kathmandu-traffic-simulation/blob/main/docs/pipeline.md) | how each number goes from Kaggle into the paper |
| [Calibration](https://github.com/iamgroot400/kathmandu-traffic-simulation/blob/main/docs/count-calibration.md) | the six calibration rounds |
| [Datasets for the cameras](https://github.com/iamgroot400/kathmandu-traffic-vision/blob/main/docs/datasets.md) | what each model was trained on |
| [Violation rules](https://github.com/iamgroot400/kathmandu-traffic-vision/blob/main/docs/violations.md) | implemented and planned |
| [Privacy](https://github.com/iamgroot400/kathmandu-traffic-vision/blob/main/docs/privacy.md) | how plates and footage are handled |
