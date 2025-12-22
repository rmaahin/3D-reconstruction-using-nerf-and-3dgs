# An Empirical Study of State-of-the-Art 3D Reconstruction Methods

**Authors:** Harsha Rajendra & Maahin Rathinagiriswaran
**Context:** COS 529 (Computer Vision), Princeton University

![Framework](https://img.shields.io/badge/Framework-Nerfstudio-orange) ![Comparison](https://img.shields.io/badge/NeRF-vs_3DGS-blue) ![License](https://img.shields.io/badge/License-MIT-green)

## Project Overview

We benchmark the performance trade-offs between **Neural Radiance Fields (NeRF) [1]** and **3D Gaussian Splatting (3DGS) [2]** using the `nerfstudio` framework. Our study systematically evaluates implicit vs. explicit scene representations, focusing on rendering fidelity, inference latency, and—crucially—robustness to data sparsity and spectral error distribution.

## Methodology

We designed a three-pronged evaluation strategy to stress-test these architectures beyond standard benchmarks.

### 1. Models & Architectures
* **Implicit (NeRF):** We evaluated **`nerfacto`** [3], a hybrid architecture utilizing hash encoding [4] and lightweight MLPs.
* **Explicit (3DGS):** We evaluated **`splatfacto`** [2], representing scenes via discrete, optimizable Gaussian primitives.

### 2. Experimental Protocols
We introduced two specific analytical frameworks to better understand failure modes:

* **Sparse View Analysis:**
  We simulated data-constrained environments by systematically subsampling the training views. We trained both models at **100%**, **25%**, and **8% (approx. 24 views)** data density. This allowed us to plot degradation curves and identify the "breaking point" where geometry collapses.

* **Frequency Domain Analysis:**
  To go beyond simple PSNR metrics, we applied **Fast Fourier Transforms (FFT)** to the error residuals of rendered images. This enabled us to decompose errors into:
  * **Low-Frequency:** Structural/geometric inaccuracies (e.g., shape distortion).
  * **High-Frequency:** Texture details and noise (e.g., blurry leaves or carpet).

## Datasets

We utilized a mix of standard benchmarks and custom stress-test footage:

### Standard Benchmark (Mip-NeRF 360 [5])
* **Outdoor:** *Garden*, *Bicycle* (Complex vegetation, unbounded backgrounds).
* **Indoor:** *Bonsai*, *Kitchen* (Fine geometric details).

### Custom Real-World Data (4K/30fps)
* **Scarlet Knight Statue:** Evaluating performance on non-Lambertian (metallic) surfaces.
* **Water Bottle:** Testing transparency and refraction (a traditional failure case for photogrammetry).
* **Medicine Ball:** Assessing preservation of high-frequency surface textures.

## Key Findings

### 1. Visual Fidelity & Performance
3DGS is the clear winner in full-data regimes. It achieves a **6–10 dB PSNR improvement** over NeRF and renders at real-time speeds (**>100 FPS**), whereas NeRF remains at ~3 FPS.

### 2. Sparse View Robustness
Our sparse analysis revealed a critical trade-off:
* **3DGS Fragility:** At 24 views, 3DGS degrades linearly. Without sufficient overlap, the optimization produces severe "floater" artifacts and geometric collapse.
* **NeRF Stability:** NeRF exhibits a "square-root" degradation curve. While the output becomes blurrier, the implicit regularization maintains the global structure of the scene far better than 3DGS.

### 3. Spectral Error Analysis
* **High-Frequency:** 3DGS excels here, recovering sharp textures that NeRF tends to smooth over.
* **Low-Frequency:** In sparse settings, NeRF outperforms 3DGS in the low-frequency domain, indicating better structural consistency despite lower texture fidelity.

## Reproduction

### Environment Setup
```bash
conda create --name cos529 -y python=3.8
conda activate cos529
pip install nerfstudio
