# An Empirical Study of State-of-the-Art 3D Reconstruction Methods

**Authors:** Harsha Rajendra & Maahin Rathinagiriswaran
**Context:** COS 529 (Computer Vision), Princeton University

![Framework](https://img.shields.io/badge/Framework-Nerfstudio-orange) ![Comparison](https://img.shields.io/badge/NeRF-vs_3DGS-blue) ![Dataset](https://img.shields.io/badge/Dataset-Available-green)

### [**🔗 Access Our Custom Dataset Captures (Google Drive)**](https://drive.google.com/drive/folders/1WiHibzQylWT7y7yOPCqeMWnLMw52DWpi?usp=sharing)

## Project Overview

We benchmark the performance trade-offs between **Neural Radiance Fields (NeRF) [1]** and **3D Gaussian Splatting (3DGS) [2]** using the `nerfstudio` framework. Our study systematically evaluates implicit vs. explicit scene representations, focusing on rendering fidelity, inference latency, and robustness to data sparsity.

## Methodology

### 1. Architectures Evaluated
* **Implicit (NeRF):** `nerfacto` [3] — Uses hash encoding with a lightweight MLP.
* **Explicit (3DGS):** `splatfacto` [2] — Uses discrete, optimizable Gaussian primitives.

### 2. Datasets
We standardized processing by downscaling all input frames by a factor of 4 to mitigate memory constraints.

* **Standard (Mip-NeRF 360):** *Garden*, *Bicycle*, *Bonsai*, *Kitchen*.
* **Custom (4K/30fps):**
    * *Scarlet Knight:* Metallic/non-Lambertian surface.
    * *Medicine Ball:* High-frequency texture.
    * *Water Bottle:* Transparency and refraction.

### 3. Experimental Controls
* **Sparse View Analysis:** Trained on **24 views** (approx. 8% of data) vs **100 views** (Full) to determine degradation curves.
* **Frequency Domain Analysis:** Applied FFT to error residuals to separate low-frequency (structural) errors from high-frequency (texture) errors.

## Key Findings & Results

### 1. Visual Fidelity (Full Dataset)
3DGS significantly outperforms NeRF in full-data regimes. The gap is most pronounced in the **Bonsai** scene, where 3DGS achieves a +10 dB improvement.

[cite_start]**Table: Average Metrics (All 4 Mip-NeRF Scenes) [cite: 315]**
| Metric | NeRF (`nerfacto`) | 3DGS (`splatfacto`) | Delta |
| :--- | :--- | :--- | :--- |
| **PSNR (Avg)** | 21.78 dB | **28.48 dB** | +6.70 dB |
| **SSIM (Avg)** | 0.560 | **0.851** | +0.291 |
| **LPIPS (Avg)** | 0.183 | **0.103** | -0.080 |
| **FPS (Avg)** | ~2.9 | **~127.3** | ~43x Faster |

### 2. Custom Dataset Performance
On our custom object-centric captures, the performance gap widened further, showcasing 3DGS's ability to model complex real-world objects.

[cite_start]**Table: Custom Dataset Results [cite: 196]**
| Scene | NeRF PSNR | 3DGS PSNR | Delta |
| :--- | :--- | :--- | :--- |
| **Medicine Ball** | 27.65 dB | **38.10 dB** | +10.45 dB |
| **Scarlet Knight** | 28.27 dB | **36.28 dB** | +8.01 dB |
| **Bottle** | 23.70 dB | **32.20 dB** | +8.50 dB |

### 3. Sparse View Analysis (24 Views)
We observed distinct degradation patterns when reducing data to 24 views:
* **3DGS:** Degrades **linearly**. [cite_start]It suffers from "floaters" and severe background collapse, dropping an average of **~12.6 dB**[cite: 315].
* **NeRF:** Degrades with a **square-root** pattern. [cite_start]It maintains better structural consistency (implicit regularization) despite blurriness, dropping an average of **~7.9 dB**[cite: 315].

### 4. Frequency Domain Analysis
* **Low Frequency:** In full data, 3DGS dominates (+11 dB on Bonsai). [cite_start]However, in sparse settings, **NeRF outperforms 3DGS** in preserving low-frequency structural components[cite: 146, 155].
* **High Frequency:** 3DGS consistently recovers sharper textures across all tests.

## Reproduction

### Environment Setup
```bash
conda create --name cos529 -y python=3.8
pip install nerfstudio
