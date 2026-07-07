# 🚀 TensorFlow-NVIDIA-GPU-WSL-Guide

# How to Get TensorFlow Working with an NVIDIA GPU on Windows Using WSL (2026 Guide)

Getting TensorFlow to recognize your NVIDIA GPU on Windows via WSL2 *should* be simple based on the official TensorFlow instructions — but for many people, it isn't. After digging through countless unhelpful tutorials and AI-generated suggestions, the real culprit turned out to be simple: **TensorFlow couldn't find the NVIDIA libraries it needed.**

---

## 💻 Tested Configuration

| Component | Specification |
|-----------|---------------|
| 🖥️ CPU | 13th Gen Intel® Core™ i9-13900H (2.60 GHz) |
| 🧠 Memory | 16 GB RAM (15.7 GB usable) |
| 🎮 NVIDIA GPU | GeForce RTX 5060 Laptop GPU (8 GB VRAM) |
| 📺 Integrated GPU | Intel UHD Graphics |
| 🪟 Operating System | Windows 11 Pro 25H2 |
| 🐧 WSL | Ubuntu 24.04 (WSL2) |

> **✅ Verified:** This guide has been tested successfully on the configuration above.

---

> **Note:** While these instructions should work on most modern NVIDIA GPUs supported by TensorFlow, they have been specifically verified on the configuration listed above.

This guide documents the exact, working fix — step by step.

## Prerequisites

Before you start, make sure:

1. You have an NVIDIA GPU that is [supported by TensorFlow](https://www.tensorflow.org/install/source#gpu).
2. You have correctly installed the NVIDIA driver on Windows.
3. You have WSL working on Windows 11.

> **Note:** Newer versions of WSL ship with a more recent Ubuntu release that includes Python 3.14, which can cause compatibility issues. To avoid problems, explicitly install **Ubuntu 24.04** rather than the latest available version.

## Step-by-Step Instructions

### 0. Confirm the NVIDIA driver is working on Windows

```bash
nvidia-smi
```

### 1. Install a fresh Ubuntu 24.04 in WSL2

```bash
wsl --install Ubuntu-24.04
```

Then confirm the NVIDIA driver is also visible inside WSL:

```bash
nvidia-smi
```

### 2. Install Python venv and pip

```bash
sudo apt update
sudo apt install python3-venv python3-pip
```

### 3. Create a virtual environment in your Linux home directory

Important: create this in your Linux home directory (`~`), **not** in `/mnt/c/`.

```bash
cd ~
python3 -m venv tf-gpu
```

(You can name the environment anything — `tf-gpu` is just an example.)

### 4. Activate the virtual environment

```bash
cd tf-gpu
source bin/activate
```

### 5. Install TensorFlow with CUDA support

```bash
pip install --upgrade pip
pip install tensorflow[and-cuda]
```

Verify the installation:

```bash
python -c "import tensorflow as tf; print(tf.config.list_physical_devices('GPU'))"
```

### 6. Create symbolic links to the NVIDIA libraries

This is the step that fixes the "GPU not found" problem for most people:

```bash
cd $(dirname $(python -c 'print(__import__("tensorflow").__file__)'))
ln -svf ../nvidia/*/lib/*.so* .
cd -
ln -sf $VIRTUAL_ENV/lib/python3.12/site-packages/nvidia/cuda_nvcc/bin/ptxas $VIRTUAL_ENV/bin/ptxas
```

### 7. Add the library path to `LD_LIBRARY_PATH`

```bash
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:$VIRTUAL_ENV/lib/python3.12/site-packages/tensorflow
```

### 8. Make it permanent

Add the `LD_LIBRARY_PATH` export above to your virtual environment's `activate` script so it's applied automatically every time you activate `tf-gpu`.

---

If this guide helped you, consider starring the repo so others can find it too.
