# 🧪 Microplastic Segmentation using U-Net

This project focuses on **binary semantic segmentation of microplastics** in microscopic images using a deep learning model based on the U-Net architecture. This repository presents a deep learning approach for detecting microplastics in microscopic imagery using binary semantic segmentation. The model is trained to generate pixel-level masks that distinguish microplastic regions from the background, enabling automated analysis of complex microscopy data.

The goal is simple and precise:
Microplastics are small, irregular particles that are difficult to identify through manual inspection due to noise, variability in shape, and dense backgrounds. This project formulates the task as a binary segmentation problem, where each pixel is classified as either microplastic or background.

Given the strong class imbalance in the dataset, a plastic-aware patch sampling strategy is employed to increase the likelihood of selecting patches that contain microplastic regions.

👉 **Predict a binary mask indicating the presence of microplastics (pixel-wise classification).**

---

## 📌 Problem Statement

Microplastics are tiny plastic particles that are difficult to detect manually in microscopy images. This project automates detection by learning:

* **Input:** Raw microscopic image
* **Output:** Binary mask

  * `0` → Background
  * `1` → Microplastic region


The work focuses on solving real-world challenges such as class imbalance, small object detection, and noisy visual data, which are common in scientific and environmental imaging applications.


---

## 🗂 Dataset Structure

The dataset consists of images and corresponding instance masks stored in `.npz` format.

```
data/
│
├── 0_train/
├── 0_train_instance/
│
├── 1_valid/
├── 1_valid_instance/
│
├── 2_test/
└── 2_test_instance/
```

### Mask Format (`.npz`)

Each mask contains multiple channels:

* Channel 0 → Microplastic presence (used in this project)
* Channel 1 → Type (fiber/fragment) [0, 100] -> 0 means fiber, 100 means fragment
* Channel 2 → Instance IDs

👉 This project **only uses Channel 0** for binary segmentation.

---

## ⚙️ Approach

### 1. Data Preprocessing

* Convert instance masks → binary masks
* Normalize images to `[0,1]`
* Random patch extraction (`256×256`)
* Plastic-aware sampling to handle class imbalance

* The primary challenge in this project is severe class imbalance, where most image regions contain no microplastics. While patch-based sampling mitigates this issue, the model still struggles to achieve high overlap-based scores.

---

### 2. Model Architecture

We use a custom implementation of **U-Net**, a widely used architecture for biomedical segmentation.

**Key components:**

* Encoder (Downsampling path)
* Bottleneck
* Decoder (Upsampling path with skip connections)

---

### 3. Loss Function

To handle class imbalance:

* **Binary Cross Entropy (BCE)**
* **Dice Loss**

Combined loss:

```
Loss = BCE + Dice Loss
```

---

### 4. Training Details

* Optimizer: Adam
* Learning Rate: `1e-4`
* Patch Size: `256×256`
* Training uses random cropped patches

---

## 📊 Evaluation Metrics

The model is evaluated using:

* Dice Score
* Intersection over Union (IoU)
* Accuracy
* Precision
* Recall
* F1 Score

---

## 🧠 Key Challenge

⚠️ **Class Imbalance**

Most patches contain **no microplastics**, causing the model to predict only background.

###  Solution Implemented

* Plastic-aware patch sampling:

  * Prefer patches containing microplastics
  * Retry sampling up to 10 times

---

## 🖼️ Sample Output

<img width="1165" height="695" alt="image" src="https://github.com/user-attachments/assets/ac3fc55d-499e-4412-bb36-d7b74c41c201" />


| Input Image | Ground Truth | Prediction |
| ----------- | ------------ | ---------- |
| ✔️          | ✔️           | ✔️         |

The model outputs a **probability map**, which is thresholded:

```
Prediction = (output > 0.5)
```

---

## 🚀 How to Run

### 1. Install Dependencies

```bash
pip install torch torchvision numpy matplotlib pillow
```

### 2. Train Model

```python
python train.py
```

### 3. Evaluate

```python
python evaluate.py
```

---

## 📈 Future Improvements

* Improve Dice score with better sampling strategies
* Use advanced losses (Focal Loss, Tversky Loss)
* Try pretrained encoders (ResNet, EfficientNet)
* Classification model for Microplastic type (Fiber or Fragment)
---

## 🎯 Conclusion

This project demonstrates a complete pipeline for:

✔️ Loading instance segmentation data
✔️ Converting to binary masks
✔️ Training a U-Net model
✔️ Evaluating segmentation performance

This project demonstrates a complete pipeline for binary segmentation of microplastics using deep learning. While current results indicate moderate performance, the framework provides a solid foundation for further research and improvement in microscopic image analysis.

---
