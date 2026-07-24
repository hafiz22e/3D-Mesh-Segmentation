# 3D Dental Mesh Segmentation using Geometric Descriptors

This repository contains an implementation of a hybrid segmentation pipeline for high-density **STL dental meshes**. The project focuses on the precise partitioning of individual tooth crowns from gingival tissue (gum) using localized geometric features and region-growing heuristics.

## Project Overview
Automated dental segmentation is a critical step in digital orthodontics and CAD/CAM workflows. This project addresses the challenge of identifying complex boundaries in 3D space by leveraging mesh normals and surface curvature rather than standard voxel-based processing.

## Key Methodologies

### 1. Normal Angle-Based Region Growing
To identify smooth surfaces and detect abrupt changes in mesh topology, the algorithm evaluates the angular distance between the normals of adjacent triangular faces ($n_i$ and $n_j$). 

* **Boundary Criterion:** The growth of a region is terminated when the angular threshold satisfies:
    $$\theta = \cos^{-1}(n_i \cdot n_j) > T$$
    where $T$ is a predefined sensitivity constant.

### 2. Curvature-Guided Segmentation
To distinguish the **tooth ridge** (high relief) from the **gum** (flatter or concave regions), a curvature index $C(i)$ is computed for each face based on its neighborhood $N(i)$:

$$C(i) = \sum_{j \in N(i)} \cos^{-1}(n_i \cdot n_j)$$

* **High $C(i)$:** Identified as the dental ridge or incisal edge.
* **Low $C(i)$:** Classified as the gingival margin or flat enamel surfaces.

### 3. Crown Seed-Guided Hybrid Approach
To handle cases of dental crowding where boundaries are ambiguous, a hybrid strategy is used. Initial seeds are placed on the occlusal surfaces (crowns), and the region-growing process is constrained by both the normal angles and the curvature indices to prevent "bleeding" into the gingival mesh.

## Technical Stack
* **Language:** Python
* **Key Skills:** Computer Vision, Medical Image Analysis, 3D Mesh Segmentation
* **Libraries:** NumPy (Vectorized math), Open3D / Trimesh (Mesh processing), Matplotlib (Visualization)
* **Data Format:** `.stl` (Stereolithography)

## Results
The pipeline effectively generates a clean partition between the anatomy of the tooth and the supporting structure, providing a foundation for automated tooth numbering and alignment analysis.
