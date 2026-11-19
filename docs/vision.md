# Camera system

Repository: `kathmandu-traffic-vision`.

## Pipeline

1. YOLO detector: `motorcycle`, `car` (incl. vans, jeeps, microbuses), `bus`, `truck`.
2. ByteTrack tracker: one id per vehicle across frames.
3. Stop-line counting: per minute, class and direction, in vehicles and PCU.
4. Signal state: from a box around the signal head, a fixed timing cycle, or (planned) the controller.
5. Red-light rule: entering the junction, signal red (debounced over 3 frames) for at least the
   grace period (1 s). Amber and an unread signal are never flagged.
6. Plate detector on flagged vehicles (training); plate reading next.

## Models and training

| Model | Base | Training data | Status |
|---|---|---|---|
| Vehicles (4 classes) | YOLO11s, COCO-pretrained | Indian Driving Dataset sample, Indian traffic video frames, Nepali bike-and-car photos | run 1 training |
| Plates (1 class) | YOLO11s, COCO-pretrained | Kathmandu-valley plate images, Nepali photos, distant plates from traffic video | run 1 training |
| Plate reader | to choose | Nepali Devanagari character set, Nepali embossed character sets | not started |

Each run's settings, notebook and per-source results: `docs/training.md` in the vision repository.
Datasets, licences and class mappings: `docs/datasets.md` there.

## Limits

- No open Kathmandu junction footage exists; the models have not been tested on a Kathmandu junction
  camera yet. A held-out set of hand-checked Kathmandu frames is needed before any accuracy claim.
- Plates are personal data: read only for flagged violations, never published; enforcement use
  belongs with the traffic authorities.
