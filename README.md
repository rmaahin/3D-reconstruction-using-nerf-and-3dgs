# 3D-reconstruction-using-nerf-and-3dgs
An Empirical Analysis of State-of-the-Art 3D Reconstruction Methods

You can find the link to our custom dataset captures: https://drive.google.com/drive/folders/1WiHibzQylWT7y7yOPCqeMWnLMw52DWpi?usp=sharing

This project presents a comparative analysis of Neural
Radiance Fields (NeRF) and 3D Gaussian Splatting
(3DGS) using the nerfstudio framework across Mip-NeRF
360 and custom real-world datasets. We evaluate the tradeoffs
in rendering quality, computational efficiency, and robustness
to data sparsity. Our empirical results demonstrate
that while 3DGS achieves superior visual fidelity (6–10 dB
improvement) and real-time rendering speeds (>100 FPS),
it suffers from severe low-frequency structural degradation
under sparse view conditions (24 views). In contrast, NeRF
exhibits lower-quality but more stable degradation due to
implicit regularization. We conclude by providing a practical
decision framework to help researchers and practitioners
select the optimal reconstruction model based on their
specific needs and constraints including image/view count,
hardware availability, and scene-type.
