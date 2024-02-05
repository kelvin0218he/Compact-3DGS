# Compact 3D Gaussian Representation for Radiance Field
### Joo Chan Lee, Daniel Rho, Xiangyu Sun, Jong Hwan Ko, and Eunbyung Park

### [[Project Page](https://maincold2.github.io/c3dgs/)] [[Paper(arxiv)](https://arxiv.org/abs/2311.13681)]

Our code is based on [3D Gaussian Splatting](https://github.com/graphdeco-inria/gaussian-splatting).

## Method Overview
<img src="https://github.com/maincold2/maincold2.github.io/blob/master/c3dgs/images/fig_demo.jpg?raw=true" />

We place a specific emphasis on two key objectives: reducing the number of Gaussian points without sacrificing performance and compressing the Gaussian attributes. To this end, we propose a learnable mask strategy that significantly reduces the number of Gaussians while preserving high performance. In addition, we propose a compact but effective representation of view-dependent color by employing a grid-based neural field rather than relying on spherical harmonics. Finally, we learn codebooks to compactly represent the geometric attributes of Gaussian by vector quantization.

## Setup

For installation:
```shell
conda env create --file environment.yml
conda activate c3dgs
git submodule update --init --recursive
```
We used Mip-NeRF 360, Tanks & Temples, Deep Blending, and NeRF synthetic datasets.

## Running

### Real-world scenes (e.g., 360, T&T, and DB)


```shell
python train.py -s <path to COLMAP> --eval
```

<details>
<summary><span style="font-weight: bold;">Command Line Arguments for train.py</span></summary>

  #### --lambda_mask
  Weight of masking loss to control ma the number of Gaussians masking control factor, 0.01 by default
  #### --mask_lr
  Learning rate of masking parameter, 0.01 by default
  #### --net_lr 
  Learning rate for the neural field, 0.01 by default
  #### --net_lr_step
  Step schedule for training the neural field, [5000, 15000, 25000] by default
  #### --max_hashmap
  Maximum hashmap size (log) of the neural field, 19 by default
  #### --rvq_size
  Codebook size in each R-VQ stage, 64 by default 
  #### --rvq_num
  The number of R-VQ stages, 6 by default

  #### Refer to other arguments of [3DGS](https://github.com/graphdeco-inria/gaussian-splatting).


</details>
<br>

### NeRF-synthetic scenes

Some different hyper-parameters are required for synthetic scenes.
```shell
python train.py -s <path to NeRF Synthetic dataset> --eval --max_hashmap 16 --lambda_mask 4e-3 --mask_lr 1e-3 --net_lr 1e-3 --net_lr_step 25000
```

## Evaluation
```shell
python render.py -m <path to trained model> --max_hashmap <max hash size of the model>
python metrics.py -m <path to trained model> 
```

## 3DGS Viewer
The original SIBR interactive viewer of 3DGS can not support neural fields for view-dependent color. We would like to support and update this shortly if possible. 

Currently, to use the viewer, you have two options: either bypass the neural field for view-dependent color by only applying masking and the geometry codebook, or train neural fields to represent spherical harmonics without inputting view direction (slightly lower performance). After this, you can save the output in a PLY format, similar to 3DGS.

## BibTeX
```
@article{lee2023compact,
title={Compact 3D Gaussian Representation for Radiance Field},
author={Lee, Joo Chan and Rho, Daniel and Sun, Xiangyu and Ko, Jong Hwan and Park, Eunbyung},
journal={arXiv preprint arXiv:2311.13681},
year={2023}
}
```
