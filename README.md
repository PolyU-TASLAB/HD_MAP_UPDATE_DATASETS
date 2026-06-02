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

Each scenario uses two recordings — one with clear road conditions and one with construction obstacles — shared across both scenes with swapped roles.

```
HD_MAP_UPDATE_DATASETS/
├── east_clear/                            # Vehicle-side · clear road
│   ├── town03_lidar_test_clear.bag        # ROS bag: vehicle LiDAR + camera, 2.21 GB
│   ├── gt_global.txt                      # Global ground truth trajectory
│   ├── route1_gt_global.txt               # Route-level ground truth
│   ├── town03_gt.m                        # MATLAB ground truth helper
│   ├── route_vehicle_status.csv           # Vehicle status log (per route)
│   └── vehicle_status.csv                 # Vehicle status log (full session)
├── east_construction/                     # Vehicle-side · construction scenario
│   ├── hkstp_east_construction.bag        # ROS bag: vehicle LiDAR + camera, 2.21 GB
│   ├── gt_global.txt
│   ├── town03_gt.m
│   └── vehicle_status.csv
├── roadside_0923/
│   ├── data_clear/                        # Roadside · clear road
│   │   ├── lidar_test_data_with_seg_clear.bag   # ROS bag: roadside LiDAR, 6.48 GB
│   │   └── config/                        # Sensor and world config files
│   └── data_with_obs/                     # Roadside · construction scenario
│       ├── lidar_test_data_with_seg.bag   # ROS bag: roadside LiDAR, 6.49 GB
│       ├── actor_settings_hksp_with_infr.json
│       ├── sensor_config_infrastructure.json
│       ├── sensor_config_template_32line.json
│       └── world_config_town03_revise_hkstp.json
└── README.md
```

How the folders map to each scenario:

| Folder | S1: Construction | S2: Restoration |
|--------|-----------------|-----------------|
| `east_clear` | Vehicle baseline | Vehicle new scan |
| `east_construction` | Vehicle new scan | Vehicle baseline |
| `roadside_0923/data_clear` | Roadside baseline | Roadside new scan |
| `roadside_0923/data_with_obs` | Roadside new scan | Roadside baseline |


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
  <img src="https://github.com/user-attachments/assets/02a93365-0bde-4836-9ddf-46aadbf9cbde" alt="Scene 2" width="700"/>
</p>

- **Change type:** Bidirectional (additions + deletions)
- **Description:** Construction elements are removed and original road markings are restored
- **Task:** Simultaneously detect deleted construction-period boundaries and newly restored markings
- **Result:** 3D mean Euclidean distance of **15.54 cm**

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

## Benchmark Results

| Scenario | \|ΔX\| (cm) | \|ΔY\| (cm) | \|ΔZ\| (cm) | 3D Mean (cm) |
|----------|-------------|-------------|-------------|--------------|
| S1: Construction      | 2.61 ± 2.36   | 2.86 ± 3.69   | 1.51 ± 2.12   | **4.07**  |
| S2: Restoration       | 10.81 ± 7.39  | 5.18 ± 3.62   | 10.07 ± 13.31 | **15.54** |

---

## Getting Started

### Prerequisites

```bash
# Python 3.8+, ROS Noetic recommended
pip install open3d numpy
```

### Download

ROS bag files are hosted on Dropbox. **Scene 1** data is currently available; Scene 2 will be released in a future update.

**Scene 1 — Construction Work**

| Folder | Link |
|--------|------|
| `east_clear` (vehicle baseline) |[Download](https://www.dropbox.com/scl/fo/2yt6phd0kxnytoog3hobb/ADD4xqLr306Vho1kG_TQJWo/HKSTP_data/east_clear?rlkey=k5jiool8uhs1lwjgpp66x4do3&dl=0) |
| `east_construction` (vehicle new scan) |[Download](https://www.dropbox.com/scl/fo/2yt6phd0kxnytoog3hobb/AJ-jhg8O74q09lJT1mzu__g/HKSTP_data/east_construction?dl=0&rlkey=k5jiool8uhs1lwjgpp66x4do3&subfolder_nav_tracking=1) |
| `roadside_0923/data_clear` (roadside baseline) |[Download](https://www.dropbox.com/scl/fo/2yt6phd0kxnytoog3hobb/ANzVOIm-X24Skz8WH8bzKZU/HKSTP_data/roadside_0923/data_clear?rlkey=k5jiool8uhs1lwjgpp66x4do3&dl=0) |
| `roadside_0923/data_with_obs` (roadside new scan) |[Download](https://www.dropbox.com/scl/fo/2yt6phd0kxnytoog3hobb/AHo5j5SzHVYLJHjS4y54CIw/HKSTP_data/roadside_0923/data_with_obs?rlkey=k5jiool8uhs1lwjgpp66x4do3&dl=0) |

**Scene 2 — Post-Construction Restoration:** coming soon.

### Usage

Play back ROS bags:

```bash
# Vehicle-side
rosbag play east_clear/town03_lidar_test_clear.bag
rosbag play east_construction/hkstp_east_construction.bag

# Roadside
rosbag play roadside_0923/data_clear/lidar_test_data_with_seg_clear.bag
rosbag play roadside_0923/data_with_obs/lidar_test_data_with_seg.bag
```

Ground truth trajectories are provided in `gt_global.txt` (format: `timestamp x y z qx qy qz qw`).

---

## License

This dataset is released under the **GPLv3 License**.  
For commercial inquiries, please contact: welson.wen@polyu.edu.hk

---

## Acknowledgement

This work was supported by the Innovation and Technology Fund under projects "Safety-Certified Multi-Source Fusion Positioning for Autonomous Vehicles in Complex Scenarios (ZPE8)" and "Advanced Smart Mobility Road-Side and Edge System (ART/369CP)".

We thank Ziqi Zhang and Qijun Qin for their generous support in providing experimental data and guidance.
