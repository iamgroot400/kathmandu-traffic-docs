# Technology

| Area | Tool | Notes |
|---|---|---|
| Traffic simulation | UXsim 1.14.2 (Python) | mesoscopic kinematic wave model with dynamic route choice |
| Network import | UXsim OpenStreetMap importer, Overpass API | lane tags corrected for two-way roads (round 5) |
| Demand | JICA 2011 OD tables, extracted from the survey report | 51 zones, PCU |
| Statistics | paired seeds, 95% t-intervals, Bonferroni correction | 15 seeds per configuration |
| Object detection | Ultralytics YOLO11 (COCO-pretrained, fine-tuned) | any Ultralytics model works, e.g. YOLO12 |
| Tracking | ByteTrack (in Ultralytics) | one id per vehicle |
| Image processing | OpenCV | signal colour from HSV, annotation, video I/O |
| Compute | Kaggle notebooks (CPU for simulation, GPU for training); a CPU laptop runs the camera pipeline on recorded video | simulation notebooks public, training notebooks private |
| Paper | LaTeX (natbib), generated number macros | every number traced to a notebook output |
