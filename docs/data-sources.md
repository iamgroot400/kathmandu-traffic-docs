# Data sources

## Simulation

| Source | Used for | Licence |
|---|---|---|
| OpenStreetMap (trunk to tertiary roads, lane and direction tags, shops / amenities / offices) | road network, lane counts, roadside-friction rule | ODbL, © OpenStreetMap contributors |
| JICA and Department of Roads, Nepal (2012), *Data Collection Survey on Traffic Improvement in Kathmandu Valley* | 2011 origin-destination demand, travel-speed survey, junction counts (2011-12) | survey report |

## Camera system: vehicle detection

| Kaggle dataset | Images | Licence |
|---|---|---|
| `redzapdos123/indian-driving-dataset-detections-yolov11` (Indian Driving Dataset, IIIT Hyderabad) | 41,962 | research use only |
| `daudshah/vehicle-detection-dataset` (Roboflow "ai-traffic-system") | 9,440 | CC BY 4.0 |
| `kshitizgajurel042/nepali-bike-and-car-images-with-annotations` | 1,052 | as listed on Kaggle |

## Camera system: number plates

| Kaggle dataset | Images | Use |
|---|---|---|
| `ishworsubedii/vehicle-number-plate-datasetnepal` | 8,078 | plate detection (Kathmandu valley) |
| `kshitizgajurel042/nepali-bike-and-car-images-with-annotations` | 1,052 | plate detection |
| `deepakat002/indian-vehicle-number-plate-yolo-annotation` | 160 | plate detection (distant plates) |
| `inspiring-lab/nepali-number-plate-characters-dataset` | 26,537 | plate reading (Devanagari), next step |
| `theciceerguy/embossed-number-plate-detection-nepal-dataset`, `utsavbaral007/nepal-embossed-number-plate-dataset` | 10,158 / 3,592 | plate reading (embossed), next step |

Full inspection results and class mappings: `docs/datasets.md` in the vision repository. No
dataset is redistributed in any of the three repositories.

## Missing

- Video of Kathmandu junctions from a raised, fixed camera: needed to test the camera system and
  to collect counts by approach for the simulation.
- Current (2026) junction counts. The newest public valley-wide counts are from 2011-12. One recent
  single-junction count was found: Thapathali, June 2022 (IOE Thapathali thesis T09/075), about
  5,600 PCU in the weekday AM peak with Kathmandu Valley Integrated Transport Strategic Plan (KVITSP 2022)
  factors (motorcycle 0.25 PCU). Its counting definitions differ from JICA's, so it was not used to
  check or calibrate the model.
