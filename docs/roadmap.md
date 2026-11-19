# Roadmap

## Done

- Simulation of the valley network under JICA 2011 demand; seven interventions on 15 paired seeds;
  demand projection to 2030; paper with every number traced to a public notebook.
- Six rounds of calibration against junction counts (best 2 of 6); a lane-count bug in the network
  import found and fixed.
- Future-demand test: the plan at 2026 and 2030 demand (helps, does not solve).
- Training run 1: vehicle detector (motorcycle, car, bus, truck) and plate detector.
- Camera pipeline: detection, tracking, stop-line counting, signal reading, red-light rule, evidence
  frames; self-checks.
- Dataset survey and inspection for vehicles, plates and plate characters.

## Next

1. **Kathmandu junction footage** (rooftop or footbridge, fixed, signal and stop line in view), to
   test the camera system and build a hand-checked Kathmandu test set.
2. Plate reader for Devanagari and embossed plates.
3. Restricted-vehicle rule: trucks on inner roads during restricted hours.
4. Camera counts by approach at the junctions where the simulation is furthest off (Kalimati,
   New and Old Baneshwor), to finish calibration; then the held-out junction test.
5. Rerun the interventions on the corrected network if calibration holds on the held-out junctions.
6. With the traffic authorities: signal controller feed, and a pilot at one junction.
