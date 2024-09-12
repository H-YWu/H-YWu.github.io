---
layout: post
title:  Setup Environment for VoroMesh and PoNQ
date:   2024-09-12 11:24:00
description: Resolve issues of building MinkowskiEngine on WSL
tags: cuda
categories: environment 
---

I was trying to run [code of VoroMesh](https://github.com/nissmar/VoroMesh), but encountered some issues with the installation of `MinkowskiEngine` and PyTorch, GPU version on WSL in my isolated `conda` environment.


- Operating system: Windows Subsystem for Linux (Windows 11, Ubuntu 22.04.3 LTS)
- System CUDA version: 12.4

Here are the commands to successfully set up the environment. I use `mamba` instead of naive `conda`.

For PoNQ simply remove the part of compiling MinkowskEngine.

```bash
mamba create -n your_env python=3.9
mamba activate your_env

# WSL2 Ubuntu22
export LD_LIBRARY_PATH=/usr/lib/wsl/lib:$LD_LIBRARY_PATH

# CUDA and PyTorch
# cuda 12.4 seems to have issues with PyTorch, and MinkowskiEngine seems to cannot work with cuda 12
mamba install -c "nvidia/label/cuda-11.6.0" cuda-toolkit cuda -y
mamba install pytorch==1.12.1 torchvision==0.13.1 torchaudio==0.12.1 cudatoolkit=11.6 -c pytorch -c conda-forge -y
# Downgrade to use deprecated features in PyTorch
mamba install mkl==2024.0 -y
# Verify that the cuda version is what we installed in this isolated environment.
nvcc --version
# Verify that the pytorch-cuda version (GPU) is what we installed in this isolated environment.
python -c "import torch; print(torch.version.cuda)"

# MinkowskiEngine
mamba env config vars set CUDA_HOME="path/to/your/env"
mamba install ninja
# WARNING: Do NOT install openblas-devel using conda as listed in the README.md of the MinkowskiEngine repository, since it will downgrade the PyTorch to CPU version. Instead, we just use MKL
mamba install mkl-include
# Missing cusparse.h
mamba install -c conda-forge cudatoolkit-dev
git clone https://github.com/NVIDIA/MinkowskiEngine.git
cd MinkowskiEngine
# NOTE: If you use cuda 12, please first modify the code refer to https://github.com/NVIDIA/MinkowskiEngine/issues/543#issuecomment-1773458776
# Work on WSL
export MAX_JOBS=1;
python setup.py install --blas_include_dirs=${CONDA_PREFIX}/include --blas=mkl

# PyTorch3D
mamba install -c iopath iopath -y
mamba install jupyter -y
pip install scikit-image matplotlib imageio plotly opencv-python
mamba install -c fvcore -c conda-forge fvcore -y
pip install black usort flake8 flake8-bugbear flake8-comprehensions
mamba install pytorch3d=0.7.0 -c pytorch3d -y

# Other required packages
mamba install trimesh igl tqdm -c conda-forge -y
mamba install yaml scikit-learn -c anaconda -y
mamba install pytorch-scatter -c pyg -y
mamba install conda-forge::meshplot -y
mamba install anaconda::h5py -y 
mamba install open3d-admin::open3d -y
```