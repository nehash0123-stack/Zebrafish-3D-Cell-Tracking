# Zebrafish-3D-Cell-Tracking
An optimized, memory-safe 3D+Time cell tracking and lineage reconstruction pipeline built with Python and SciPy for processing large-scale anisotropic zebrafish embryo microscopy data.

# 🧬 Zebrafish Embryo 3D+Time Cell Tracking Pipeline

An optimized, high-performance classical machine learning and computer vision pipeline built for robust cell detection, motion-aware tracking, and lineage reconstruction in large-scale volumetric 3D microscopy datasets.

---

## 🗺️ System Architecture Pipeline

The data flows systematically through every metric-aware stage using physical micron adjustments to preserve strict volumetric scalability:

* **[Input Zarr Movie]**
  
  * └── **Block XY-Downsampling (x4):** Resolves voxel anisotropy (~Isotropic Grid)
    * └── **Gaussian Smoothing Filter:** Denoises volumetric imaging artifacts
      * └── **Adaptive Otsu Threshold:** Drops fluctuating background floors
        * └── **3D Local-Maxima Detection:** Extracts sub-pixel cellular candidate seeds
          * └── **Center-of-Mass Refinement:** Calibrates exact sub-pixel coordinates
            * └── **Physical Metric NMS:** Suppresses duplicate nodes within 4.0 µm radius
              * ├── **Pass-1: Tight Hungarian Gate:** Connects confident links within 7.0 µm
              * ├── **Pass-2: Relaxed Boundary Gate:** Recovers fast cells within 11.0 µm
                * └── **Isolated Node Graph Pruning:** Removes unlinked false-positives
                  * └── **[Output submission.csv Schema]**

---

## 🚀 Key Technical Highlights

* **Sparse Ground Truth Handling:** Applied strict isolated-node pruning to avoid flooding the evaluation metric with False Positives (FP).
* **Anisotropic Voxel Resolution:** Engineered a 4× XY-block mean pooling layer to force distance tracking values into an isotropic 3D frame.
* **Bipartite Assignment Steals:** Developed a two-pass Hungarian linking engine (Tight → Full gate) to secure near neighbors first.
* **Metric Penalties Mitigation:** Enforced integer voxel coordinate round-off algorithms and fixed terminal edge indexes exactly to `-1`.

---

## 🛠️ Core Engineering Toolkit

The pipeline is completely self-contained and operates without GPU dependencies or active internet connection queries:
* **Matrix Utilities:** Python, NumPy, Pandas
* **Mathematical Operations:** SciPy (`linear_sum_assignment`, `cKDTree`, `maximum_filter`, `gaussian_filter`)
* **Computer Vision Triggers:** Scikit-Image (`threshold_otsu`, `peak_local_max`)

---

## 📈 Scalability and Performance Metrics

* **Execution Velocity:** Processed 100 frames of dense 3D space-time arrays in **under 23 seconds**.
* **Database Volume:** Mapped and structurally validated **70,903 precise Node instances** and **64,405 valid Edge tracking links** error-free under restricted offline infrastructure.
