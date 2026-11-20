# Camera system

Repository: `kathmandu-traffic-vision`.

## Pipeline

1. YOLO detector: `motorcycle`, `car` (incl. vans, jeeps, microbuses), `bus`, `truck`.
2. ByteTrack tracker: one id per vehicle across frames.
3. Stop-line counting: per minute, class and direction, in vehicles and PCU.
4. Signal state: from a box around the signal head, a fixed timing cycle, or (planned) the controller.
5. Red-light rule: entering the junction, signal red (debounced over 3 frames) for at least the
   grace period (1 s). Amber and an unread signal are never flagged.
6. Plate detector on flagged vehicles, then the plate reader: find each character, name it, put the
   characters in reading order (one or two rows), and check the result against the plate layout.
   Implausible or unsure readings are marked for a person to review.

## Models and training

| Model | Base | Training data | Status |
|---|---|---|---|
| Vehicles (4 classes) | YOLO11s, COCO-pretrained | Indian Driving Dataset sample, Indian traffic video frames, Nepali bike-and-car photos | run 1 done: mAP50 0.67-0.73 on dense street scenes |
| Plates (1 class) | YOLO11s, COCO-pretrained | Kathmandu-valley plate images, Nepali photos, distant plates from traffic video | run 1 done: mAP50 0.89-0.99, mAP50-95 0.44-0.93 |
| Plate reader: characters | YOLO11s, COCO-pretrained | 2,503 cropped Nepali plates with character boxes | run 1 done: 98.3% of characters found (mAP50 0.993) |
| Plate reader: naming | YOLO11s-cls | Devanagari character crops; embossed digit and letter crops | run 1 done: 99.0% (Devanagari), 99.9% (embossed) top-1 |

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

Plate reader: the confusions are look-alikes (६ and ३, बा and ना). These scores are upper bounds (random
splits of crops from the same plates), and errors multiply over a plate: at 99% per character an
8-character plate is fully right about 92% of the time. Whole-plate accuracy is not measured, because
no public dataset has full plate text.

Each run's settings, notebook and per-source results: `docs/training.md` in the vision repository.
Datasets, licences and class mappings: `docs/datasets.md` there.

## Limits

- No open Kathmandu junction footage exists; the models have not been tested on a Kathmandu junction
  camera yet. A held-out set of hand-checked Kathmandu frames is needed before any accuracy claim.
- Plates are personal data: read only for flagged violations, never published; enforcement use
  belongs with the traffic authorities.
