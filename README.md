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

<img width="1848" height="410" alt="image" src="https://github.com/user-attachments/assets/d98ab02a-eb68-4806-9f52-12edaf04f403" />


> **✅ Verified:** This guide has been tested successfully on the configuration above.

---

---

# 📊 Performance Benchmark

The following benchmark was performed using the tested configuration listed above.

## Model Architecture

A U-Net–style convolutional neural network with:

- 4 encoder blocks
- Bottleneck with 512 feature maps
- 4 decoder blocks
- Skip connections
- 59-class semantic segmentation output

### Input

```text
Input Shape: (input_w, input_h, input_ch)
```

### Model Statistics

| Metric | Value |
|--------|------:|
| Trainable Parameters | **7,724,475** |
| Non-trainable Parameters | **0** |
| Total Parameters | **7,724,475** |

---

## Inference Benchmark

The benchmark measures the average inference time after a warm-up run.

| Device | Average Time / Image | Images / Second |
|--------|---------------------:|----------------:|
| CPU | *(measure)* | *(measure)* |
| NVIDIA RTX 5060 | *(measure)* | *(measure)* |

---

## Training Benchmark

Batch Size: **8**

| Device | Time / Epoch |
|--------|-------------:|
| CPU | *(185 s)* |
| NVIDIA RTX 5060 | *(9 s)* |

---

## TensorFlow Model Summary (Abbreviated)

```text
Input
 ├── Encoder
 │    ├── Conv2D (32)
 │    ├── Conv2D (32)
 │    ├── MaxPool
 │
 │    ├── Conv2D (64)
 │    ├── Conv2D (64)
 │    ├── MaxPool
 │
 │    ├── Conv2D (128)
 │    ├── Conv2D (128)
 │    ├── MaxPool
 │
 │    ├── Conv2D (256)
 │    ├── Conv2D (256)
 │    ├── MaxPool
 │
 │    └── Conv2D (512)
 │         Conv2D (512)
 │
 ├── Decoder
 │    ├── Conv2DTranspose
 │    ├── Skip Connection
 │    ├── Conv2D (256)
 │    ├── Conv2D (256)
 │
 │    ├── Conv2DTranspose
 │    ├── Skip Connection
 │    ├── Conv2D (128)
 │    ├── Conv2D (128)
 │
 │    ├── Conv2DTranspose
 │    ├── Skip Connection
 │    ├── Conv2D (64)
 │    ├── Conv2D (64)
 │
 │    ├── Conv2DTranspose
 │    ├── Skip Connection
 │    ├── Conv2D (32)
 │    └── Conv2D (32)
 │
 └── Output
      Conv2D (59 classes, Softmax)

Total Parameters: 7,724,475
```
CPU Train Process

Epoch 1/25
90/90 - 491s - 5s/step - loss: 1.6541 - val_loss: 1.0522
Epoch 2/25
90/90 - 568s - 6s/step - loss: 0.9832 - val_loss: 0.9462
Epoch 3/25
90/90 - 535s - 6s/step - loss: 0.9029 - val_loss: 0.8887
Epoch 4/25
90/90 - 242s - 3s/step - loss: 0.8521 - val_loss: 0.8180
Epoch 5/25
90/90 - 168s - 2s/step - loss: 0.7931 - val_loss: 0.7748
Epoch 6/25
90/90 - 169s - 2s/step - loss: 0.7424 - val_loss: 0.7363
Epoch 7/25
90/90 - 179s - 2s/step - loss: 0.7152 - val_loss: 0.7139
Epoch 8/25
90/90 - 186s - 2s/step - loss: 0.6912 - val_loss: 0.6994
Epoch 9/25
90/90 - 185s - 2s/step - loss: 0.6613 - val_loss: 0.6544
Epoch 10/25
90/90 - 185s - 2s/step - loss: 0.6365 - val_loss: 0.6312
Epoch 11/25
90/90 - 184s - 2s/step - loss: 0.6121 - val_loss: 0.6104
Epoch 12/25
90/90 - 184s - 2s/step - loss: 0.5939 - val_loss: 0.6071
Epoch 13/25
90/90 - 193s - 2s/step - loss: 0.5864 - val_loss: 0.5793
Epoch 14/25
90/90 - 201s - 2s/step - loss: 0.5737 - val_loss: 0.5964
Epoch 15/25
90/90 - 199s - 2s/step - loss: 0.5539 - val_loss: 0.5545
Epoch 16/25
90/90 - 197s - 2s/step - loss: 0.5365 - val_loss: 0.5601
Epoch 17/25
90/90 - 200s - 2s/step - loss: 0.5251 - val_loss: 0.5518
Epoch 18/25
90/90 - 192s - 2s/step - loss: 0.5136 - val_loss: 0.5334
Epoch 19/25
90/90 - 193s - 2s/step - loss: 0.4930 - val_loss: 0.5283
Epoch 20/25
90/90 - 192s - 2s/step - loss: 0.4825 - val_loss: 0.5213
Epoch 21/25
90/90 - 186s - 2s/step - loss: 0.4694 - val_loss: 0.5201
Epoch 22/25
90/90 - 186s - 2s/step - loss: 0.4606 - val_loss: 0.5104
Epoch 23/25
90/90 - 186s - 2s/step - loss: 0.4477 - val_loss: 0.5222
Epoch 24/25
90/90 - 185s - 2s/step - loss: 0.4324 - val_loss: 0.5102
Epoch 25/25
90/90 - 185s - 2s/step - loss: 0.4235 - val_loss: 0.5025

GPU Train Process

90/90 - 99s - 1s/step - loss: 2.0392 - val_loss: 1.0779
Epoch 2/25
90/90 - 9s - 99ms/step - loss: 0.9732 - val_loss: 0.8938
Epoch 3/25
90/90 - 9s - 98ms/step - loss: 0.8790 - val_loss: 0.8433
Epoch 4/25
90/90 - 9s - 98ms/step - loss: 0.8144 - val_loss: 0.7698
Epoch 5/25
90/90 - 9s - 98ms/step - loss: 0.7626 - val_loss: 0.7240
Epoch 6/25
90/90 - 9s - 98ms/step - loss: 0.7158 - val_loss: 0.6753
Epoch 7/25
90/90 - 9s - 99ms/step - loss: 0.6750 - val_loss: 0.6510
Epoch 8/25
90/90 - 9s - 100ms/step - loss: 0.6422 - val_loss: 0.6288
Epoch 9/25
90/90 - 9s - 99ms/step - loss: 0.6225 - val_loss: 0.6063
Epoch 10/25
90/90 - 9s - 99ms/step - loss: 0.5928 - val_loss: 0.5797
Epoch 11/25
90/90 - 9s - 102ms/step - loss: 0.5715 - val_loss: 0.5501
Epoch 12/25
90/90 - 9s - 101ms/step - loss: 0.5600 - val_loss: 0.5531
Epoch 13/25
90/90 - 9s - 102ms/step - loss: 0.5368 - val_loss: 0.5526
Epoch 14/25
90/90 - 9s - 101ms/step - loss: 0.5267 - val_loss: 0.5306
Epoch 15/25
90/90 - 9s - 99ms/step - loss: 0.5068 - val_loss: 0.5397
Epoch 16/25
90/90 - 9s - 103ms/step - loss: 0.4963 - val_loss: 0.5094
Epoch 17/25
90/90 - 9s - 100ms/step - loss: 0.4839 - val_loss: 0.5078
Epoch 18/25
90/90 - 10s - 113ms/step - loss: 0.4737 - val_loss: 0.5498
Epoch 19/25
90/90 - 9s - 100ms/step - loss: 0.4624 - val_loss: 0.4999
Epoch 20/25
90/90 - 10s - 113ms/step - loss: 0.4537 - val_loss: 0.4778
Epoch 21/25
90/90 - 9s - 104ms/step - loss: 0.4322 - val_loss: 0.5137
Epoch 22/25
90/90 - 10s - 106ms/step - loss: 0.4392 - val_loss: 0.4889
Epoch 23/25
90/90 - 9s - 103ms/step - loss: 0.4153 - val_loss: 0.4926
Epoch 24/25
90/90 - 9s - 100ms/step - loss: 0.4052 - val_loss: 0.4773
Epoch 25/25
90/90 - 9s - 100ms/step - loss: 0.3990 - val_loss: 0.4894
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
