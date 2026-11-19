# Camera system

Repository: `kathmandu-traffic-vision`.

## Pipeline

1. YOLO detector: `motorcycle`, `car` (incl. vans, jeeps, microbuses), `bus`, `truck`.
2. ByteTrack tracker: one id per vehicle across frames.
3. Stop-line counting: per minute, class and direction, in vehicles and PCU.
4. Signal state: from a box around the signal head, a fixed timing cycle, or (planned) the controller.
5. Red-light rule: entering the junction, signal red (debounced over 3 frames) for at least the
   grace period (1 s). Amber and an unread signal are never flagged.
6. Plate detector on flagged vehicles; plate reading next.

## Models and training

| Model | Base | Training data | Status |
|---|---|---|---|
| Vehicles (4 classes) | YOLO11s, COCO-pretrained | Indian Driving Dataset sample, Indian traffic video frames, Nepali bike-and-car photos | run 1 done: mAP50 0.67-0.73 on dense street scenes |
| Plates (1 class) | YOLO11s, COCO-pretrained | Kathmandu-valley plate images, Nepali photos, distant plates from traffic video | run 1 done: mAP50 0.89-0.99, mAP50-95 0.44-0.93 |
| Plate reader | to choose | Nepali Devanagari character set, Nepali embossed character sets | not started |

### Run 1 results (per validation source)

| Validation source | Vehicles mAP50 | Plates mAP50 / mAP50-95 |
|---|---|---|
| Indian Driving Dataset (dense street scenes) | 0.667 | – |
| Indian traffic video | 0.727 | 0.890 / 0.436 (24 distant plates) |
| Nepali bike-and-car photos | 0.910 (close-ups, no buses or trucks) | 0.970 / 0.560 |
| Kathmandu-valley plate images | – | 0.993 / 0.934 (likely optimistic: random split of the same sessions) |

Read: about two-thirds of vehicles in crowded scenes are found well; trucks are the weakest class
(0.64-0.67); plates are found reliably, but tight boxes on small, distant plates are weak, and that
is what a junction camera sees.

Each run's settings, notebook and per-source results: `docs/training.md` in the vision repository.
Datasets, licences and class mappings: `docs/datasets.md` there.

## Limits

- No open Kathmandu junction footage exists; the models have not been tested on a Kathmandu junction
  camera yet. A held-out set of hand-checked Kathmandu frames is needed before any accuracy claim.
- Plates are personal data: read only for flagged violations, never published; enforcement use
  belongs with the traffic authorities.
