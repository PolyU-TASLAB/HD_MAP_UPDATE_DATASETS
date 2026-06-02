# HD Map Update Datasets

<p align="center">
  <img src="https://github.com/user-attachments/assets/66d83fe7-4e06-4b6f-a090-41814a341dd3" alt="Framework Overview" width="800"/>
</p>

This repository provides the CARLA simulation dataset used in the following paper:

> **VLM-Enhanced Vehicle-Infrastructure Collaborative Framework for Incremental HD Map Updating**  
> Shaoting Qiu, Runzhi Hu, Feng Huang, Weisong Wen, Dongzhe Su, Tacitus Hui, Stella Zhu

---

## Overview

HD maps are essential for autonomous driving but frequently become outdated due to urban construction and infrastructure changes. This dataset supports research on **incremental HD map updating** using vehicle-infrastructure collaborative perception.

The dataset contains two components:

| Component | Source | Description |
|-----------|--------|-------------|
| **CARLA Simulation** | CARLA Simulator + HKSTP map | Two construction change scenarios (this repo) |
| **Real-World** | [UrbanV2X](https://polyu-taslab.github.io/UrbanV2X/) | HKSTP construction site, Feb–Dec 2025 |

---

## Repository Structure

```
HD_MAP_UPDATE_DATASETS/
├── carla/
│   ├── scene1_construction/        # Scene 1: Construction work added
│   │   ├── infrastructure/         # Roadside LiDAR & camera data
│   │   ├── vehicle/                # Onboard LiDAR & camera data
│   │   ├── baseline_map/           # Pre-built HD vector map
│   │   └── ground_truth/           # Ground truth updated map
│   └── scene2_restoration/         # Scene 2: Post-construction restoration
│       ├── infrastructure/
│       ├── vehicle/
│       ├── baseline_map/
│       └── ground_truth/
└── README.md
```

---

## CARLA Sensor Configuration

The simulation replicates the **UrbanV2X** sensor platform deployed at the Hong Kong Science and Technology Park (HKSTP).

<p align="center">
  <img src="https://github.com/user-attachments/assets/efa7e28d-c1fb-438a-baf2-351154e52f5a" alt="Sensor Setup" width="600"/>
</p>

### Infrastructure Side (Roadside Unit)
| Sensor | Specification |
|--------|--------------|
| LiDAR  | 360° spinning, mounted at **6 m** height |
| Camera | RGB surround-view cameras |
| GNSS   | Fixed reference position |

### Vehicle Side (Mobile Mapping Platform)
| Sensor | Specification |
|--------|--------------|
| LiDAR  | 64-beam spinning LiDAR |
| Camera | Surround-view RGB cameras |
| GNSS   | RTK-GPS |

---

## Scenarios

### Scene 1: Construction Work

<p align="center">
  <img src="https://github.com/user-attachments/assets/4b4b7502-6ff5-4c1c-aaa4-344f2646ba96" alt="Scene 1" width="700"/>
</p>

- **Change type:** Additions only
- **Description:** Construction barriers and equipment are introduced into a road segment at HKSTP
- **Task:** Detect newly added obstacles and modified lane boundaries
- **Result:** 3D mean Euclidean distance of **4.07 cm**

### Scene 2: Post-Construction Restoration

<p align="center">
  <img src="figures/scene2.png" alt="Scene 2" width="700"/>
</p>

- **Change type:** Bidirectional (additions + deletions)
- **Description:** Construction elements are removed and original road markings are restored
- **Task:** Simultaneously detect deleted construction-period boundaries and newly restored markings

---

## Real-World Data

Real-world experiments in this paper use the publicly available **UrbanV2X** dataset collected at HKSTP, Hong Kong.

- **Baseline map:** Captured in **February 2025** (construction barriers present)
- **New scan:** Collected in **December 2025** (construction completed, original road restored)

Please refer to the official UrbanV2X repository for download and usage instructions:

> Qin, Q., Zhang, Z., Zhong, Y., Huang, F., Liu, X., Hu, R., Chen, H., Hu, W., Su, D., Zhang, J., Ng, H.-F., & Wen, W. (2025).  
> **UrbanV2X: A Multisensory Vehicle-Infrastructure Dataset for Cooperative Navigation in Urban Areas.**  
> *Accepted by IEEE ITSC 2025.*  
> 🔗 [https://polyu-taslab.github.io/UrbanV2X/](https://polyu-taslab.github.io/UrbanV2X/)

---


---

## License

This dataset is released under the **GPLv3 License**.  
For commercial inquiries, please contact: welson.wen@polyu.edu.hk

---

## Acknowledgement

This work was supported by the Innovation and Technology Fund under projects "Safety-Certified Multi-Source Fusion Positioning for Autonomous Vehicles in Complex Scenarios (ZPE8)" and "Advanced Smart Mobility Road-Side and Edge System (ART/369CP)".

We thank Ziqi Zhang and Qijun Qin for their generous support in providing experimental data and guidance.
