# Lunar South Pole Ice Detection & Rover Traverse Planning 🌕🧊

**Event:** [Bharatiya Antariksh Hackathon 2026](https://hack2skill.com/event/bah2026/) (ISRO)  
**Challenge:** 08 - Detection and Characterization of Subsurface Ice in Lunar South Polar Regions Using Chandrayaan-2 Radar and Imagery Data for Landing Site and Rover Traverse Planning.  
**Team:** Team Deimos  

## 📌 The Core Problem: The Hunt for Lunar Water
Water ice is trapped in the **Permanently Shadowed Regions (PSRs)** of the Lunar South Pole—deep craters that haven't seen sunlight in billions of years. Normal optical cameras can't see inside them, so we must rely on **Synthetic Aperture Radar (SAR)** from orbiters like Chandrayaan-2, which can penetrate the lunar regolith to bounce off subsurface ice. 

This project solves the end-to-end challenge: **How do we take massive, noisy radar data, pinpoint exactly where the ice is, and then figure out how to drive a rover there safely?**

## 🎯 Objectives & Codebase Mapping

To successfully solve this challenge, our pipeline is broken down into three major phases, directly mapping to our repository structure:

### Phase 1: Ice Detection & Characterization (Data Science)
* **The Goal:** Analyze Chandrayaan-2’s DFSAR (Dual-Frequency Synthetic Aperture Radar) data to find high "Circular Polarization Ratio" (CPR) signatures indicative of subsurface ice.
* **Code Implementation:** Handled by `scripts/feature_detect.py` and `scripts/data_loader.py`. This module scans the raw datasets, filters out surface rock noise (false positives), and outputs the exact coordinates of the ice deposits.

### Phase 2: Landing Site Selection (Terrain Mapping)
* **The Goal:** Generate a 3D map of the lunar surface to find a flat, rock-free zone close to the ice but safe enough for a lander touchdown.
* **Code Implementation:** Handled by `scripts/dem_generator.py`. This script takes optical imagery (from Chandrayaan-2's OHRC camera) and generates a **Digital Elevation Model (DEM)**, effectively mapping out steep inclines, boulders, and PSRs.

### Phase 3: Rover Traverse Planning (Robotics & Pathfinding)
* **The Goal:** Calculate an obstacle-free, energy-efficient route from the landing site to the target ice deposit.
* **Code Implementation:** Handled entirely within the `simulation/` ROS 2 workspace. 
    * We convert the DEM into a 3D costmap.
    * A custom URDF rover model is spawned into a Gazebo environment mirroring the south pole terrain (`simulation/worlds/`).
    * The ROS 2 Navigation Stack (Nav2) calculates a traverse path that limits incline grades (keeping under 15-20 degrees) and avoids deep craters.

## 🛠️ Technology Stack
* **Data Processing & ML:** Python, NumPy, OpenCV, Scikit-Learn/PyTorch (for radar feature extraction)
* **Robotics & Simulation:** C++, ROS 2, Gazebo (for lunar environment modeling and rover kinematics)
* **Pathfinding:** A* / D* Lite algorithms, Nav2 (ROS 2 Navigation Stack)

## 📂 Repository Structure
```text
├── data/                   # Raw and processed Chandrayaan-2 datasets (ignored in git)
├── docs/                   # Documentation, mathematical models, and presentation assets
├── scripts/
│   ├── data_loader.py      # Phase 1: Ingests and normalizes SAR/optical data
│   ├── feature_detect.py   # Phase 1: Algorithms identifying subsurface ice CPR signatures
│   └── dem_generator.py    # Phase 2: Topography modeling and landing site generation
├── simulation/             # Phase 3: ROS 2 Workspace
│   ├── launch/             # Launch files to bring up Gazebo and Nav2 nodes
│   ├── models/             # URDF/SDF models of the lunar rover
│   ├── worlds/             # Gazebo worlds built from the generated DEMs
│   └── src/                # C++ traverse planning nodes and custom rover controllers
├── requirements.txt        # Python dependencies for data pipeline
└── README.md
