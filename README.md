# LightSeg: Lightweight and Interpretable Multiclass Fetal Ultrasound Segmentation

LightSeg is a lightweight, interpretable, and clinically oriented deep learning framework for **multiclass fetal ultrasound segmentation**, designed to segment:

* Brain
* Cavum Septum Pellucidum (CSP)
* Lateral Ventricles (LV)

The framework combines **ensemble learning**, **black-box knowledge distillation**, and a **DLCE hybrid loss** to achieve state-of-the-art performance while maintaining **99.69% fewer parameters** than high-capacity teacher models. It is optimized for **real-time inference** in resource-constrained clinical settings such as low- and middle-income countries. 

---

## 📘 Key Features

### 📝 Notebook-Only Workflow

LightSeg is fully implemented in Jupyter Notebooks for transparent, accessible experimentation:

* Data preprocessing
* Offline + online augmentation
* Teacher model training
* Ensemble optimization
* Knowledge distillation
* Evaluation and explainability

---

### 🧩 Multiple Segmentation Architectures

Four high-capacity teacher models are reproduced from the study:

```
models/
│── unet++/
│── attention_unet/
│── deeplabv3+/
│── segformer/
```

Each is trained with multiple backbones (MobileNetV3, Xception, InceptionResNetV2, EfficientNetB4, MobileViT, EfficientViT) and evaluated using a **custom Lovász-Softmax + Dice** objective.
These teachers form the basis of the ensemble used during distillation. 

---

### ⚙️ DLCE Hybrid Loss 

The paper proposes **DLCE**, a unified loss combining:

* **Dice Loss** → optimizes region overlap
* **Lovász-Softmax** → improves boundary accuracy and IoU
* **Categorical Cross-Entropy (CCE)** → stabilizes multiclass probability calibration

This hybrid objective outperforms traditional combinations and yielded the **best student model DSC (92.49%)** in ablation studies. 

---

### 🔄 Ensemble-Guided Black-Box Knowledge Distillation

LightSeg uses a **weighted soft-voting ensemble** of four top-performing teacher models:

* UNet++ + InceptionResNetV2
* DeepLabV3+ + Xception
* AttentionUNet + EfficientNetB4
* SegNet

The ensemble achieves:

* **93.09% mean DSC**
* Strong robustness under blur/brightness variation
* Optimal weights learned via Bayesian optimization (Optuna) 

---

### 🔍 Explainability & Robustness

* GradCAM++ 
---

## 📂 Repository Structure

```
LightSeg/
│── AttentionUNet/
│── DeepLabV3+/
│── SegFormer/
│── U-Net++/
│── LightSeg/
│── README.md
```

---

## 🗂️ Dataset

LightSeg uses the **3,832-image fetal head ultrasound dataset** by Alzubaidi et al.
Images contain annotations for Brain, CSP, and LV across:

* Trans-thalamic (40.8%)
* Trans-ventricular
* Trans-cerebellar
* Diverse fetal head views

Dataset link:
[https://zenodo.org/records/8265464](https://zenodo.org/records/8265464)

---

## 🧪 Data Preprocessing & Augmentation

### Offline Augmentation

* Shift-Scale-Rotate
* Gaussian Blur
* Grid Distortion
* Elastic Transform
* Random brightness/contrast
* Horizontal/Vertical flips

---

## 📊 Performance Summary

| Model (Best Configuration)             | Params (M) | Mean DSC (%) | Mean IoU (%) |
| -------------------------------------- | ---------: | -----------: | -----------: |
| **UNet++ + InceptionResNetV2**         |   **69.3** |    **92.07** |    **83.34** |
| **AttentionUNet + EfficientNetB4**     |   **29.7** |    **91.56** |    **89.08** |
| **DeepLabV3+ + Xception**              |   **37.8** |    **91.62** |    **83.25** |
| **SegFormer + EfficientNetB4**         |   **26.9** |    **86.23** |    **78.85** |
| **CNN SegNet**                         |   **31.4** |    **91.34** |    **83.32** |
| **Proposed Model: LightSeg (Student)** |   **1.33** |    **92.49** |    **80.39** |

LightSeg matches teacher-level accuracy while enabling real-time deployment on edge devices. 

---

## 🧠 Explainability

GradCAM++ analyses reveal:

* Strong localization around fetal brain boundaries
* Accurate CSP and LV attention zones
* Smooth, anatomically constrained activation patterns

These patterns align with clinical expectations and improve interpretability. 

---

## 📘 Final Notes

LightSeg provides a **scalable, resource-efficient**, and **clinically aligned** segmentation pipeline for fetal ultrasound.
Based on extensive experiments (25 model variants, six backbones, multiple loss functions, robustness testing, ablations), the framework demonstrates that:

* Multiclass fetal segmentation **can be accurate without heavy models**
* Ensemble-guided distillation dramatically reduces computational cost
* DLCE loss addresses class imbalance and boundary precision effectively
* Explainability and robustness evaluations are essential for clinical readiness

This framework is well-positioned for portable ultrasound systems and AI-augmented prenatal care in low-resource environments.

---
