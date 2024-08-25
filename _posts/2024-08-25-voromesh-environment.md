---
layout: post
title:  Set up Environment for VoroMesh
date:   2024-08-25 11:17:00
description: Resolve issues of building MinkowskiEngine on WSL
tags: cuda
categories: programming
---

I was trying to run [this code](https://github.com/nissmar/VoroMesh), but encountered some issues with the installation of `MinkowskiEngine` and PyTorch, GPU version on WSL in my isolated `conda` environment.

- Operating system: Windows Subsystem for Linux (Windows 11, Ubuntu 22.04.3 LTS)
- System CUDA version: 12.4

Here are the commands to successfully set up the environment. I use `mamba` instead of naive `conda`.

```bash
mamba create -n voromesh python=3.9
mamba activate voromesh

mamba install -c "nvidia/label/cuda-12.1.0" cuda-toolkit cuda # cuda 12.4 seems to have issues with PyTorch
mamba install pytorch torchvision torchaudio pytorch-cuda=12.1 -c pytorch -c nvidia
nvcc --version # Verify that the cuda version is what we installed in this isolated environment.
python -c "import torch; print(torch.version.cuda)" # Verify that the pytorch-cuda version (GPU) is what we installed in this isolated environment. If it is None it means that you installed the CPU version. 

mamba install ninja
# WARNING: Do NOT install openblas-devel using conda as listed in the README.md of the MinkowskiEngine repository, since it will downgrade the PyTorch to CPU version. Instead, we just use MKL
mamba install mkl-include
git clone https://github.com/NVIDIA/MinkowskiEngine.git
cd MinkowskiEngine
# For cuda 12 please first modify the code refer to https://github.com/NVIDIA/MinkowskiEngine/issues/543#issuecomment-1773458776
export MAX_JOBS=1;  # For WSL
python setup.py install --blas_include_dirs=${CONDA_PREFIX}/include --blas=mkl

# Install PyTroch3D
mamba install -c iopath iopath
mamba install jupyter
mamba install scikit-image matplotlib imageio plotly opencv
mamba install -c fvcore -c conda-forge fvcore
mamba install black usort flake8 flake8-bugbear flake8-comprehensions
mamba install pytorch3d -c pytorch3d

# Install other needed libraries
mamba install trimesh igl tqdm -c conda-forge
mamba install yaml scikit-learn -c anaconda
```