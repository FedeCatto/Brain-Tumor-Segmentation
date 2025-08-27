# Brain Tumor Segmentation with Deep Learning

This repository contains the implementation of a Deep Learning project for **brain tumor segmentation** using the [BraTS dataset](https://www.med.upenn.edu/cbica/brats2020/data.html).  
The goal of the project is to automatically segment brain tumor regions from multi-modal MRI scans by applying convolutional neural networks, with a focus on the **U-Net architecture** and its variants.

---

## Project Overview
Gliomas are aggressive brain tumors, and accurate segmentation of tumor regions is crucial for diagnosis, treatment planning, and patient monitoring.  
Manual segmentation is time-consuming, subjective, and prone to variability across radiologists. This project explores AI-based methods to provide **automated, consistent, and efficient** segmentation.

We developed and tested different models:
- **Baseline U-Net (2D)**
- **Attention U-Net**
- **Residual U-Net**

---

## Dataset
- **Dataset**: BraTS (Brain Tumor Segmentation Challenge)  
- **Training cases**: 484 labeled volumes  
- **Test cases**: 300 unlabeled volumes  
- **Modalities**: FLAIR, T1w, T1gd, T2w  
- **Segmentation labels**:
  - Label 1: Non-enhancing Tumor Core (NET)  
  - Label 2: Peritumoral Edema  
  - Label 4: Enhancing Tumor (ET)  

These labels are grouped into three clinical subregions:
- **ET** (Enhancing Tumor)  
- **TC** (Tumor Core = Labels 1 + 4)  
- **WT** (Whole Tumor = Labels 1 + 2 + 4)  

---

## Preprocessing
- **Slice extraction**: selected only slices containing tumor tissue  
- **Mask recoding**: relabeled original masks (0,1,2,4 → 0,1,2,3)  
- **Normalization**: z-score applied per modality  
- **Cropping**: removed background to focus on the brain region  
- **Data augmentation**: applied non-spatial transformations (noise, brightness, contrast)  

---

## Model Architecture
The segmentation task is based on a **2D U-Net**:
- Input: 2D slices ($240 \times 240$), 4 channels (MRI modalities)  
- Encoder: 4 convolutional blocks with increasing filters (64 → 128 → 256 → 512)  
- Bottleneck: 2×Conv2D with 1024 filters  
- Decoder: symmetric path with upsampling + skip connections  
- Output: $1 \times 1$ Conv2D with softmax (4 classes)  

Additional experiments were carried out with:
- **Attention U-Net** (added attention gates to skip connections)  
- **Residual U-Net** (residual connections and dropout for regularization)  

---

## Training and Evaluation
- **Loss function**: Dice Loss (to handle class imbalance)  
- **Optimizer**: Adam  
- **Metric**: Dice Score  
- **Validation strategy**: patient-wise split to prevent data leakage  

---

## Results
- **Baseline U-Net**: Dice Score ≈ 0.53  
- **Residual U-Net**: Dice Score ≈ 0.58  
- **Attention U-Net**: Dice Score ≈ 0.64 (best performing model)  

Visual inspection confirmed that the Attention U-Net produced the most accurate tumor boundaries, especially for irregular tumor regions.  

---

## Discussion & Future Work
- Preprocessing improved consistency but had limited impact on accuracy.  
- Attention U-Net showed clear benefits over the baseline.  
- Limitations included computational resources and long training times.  

**Future directions**:
- Validation on external clinical datasets  
- Advanced augmentation and class balancing strategies  
- Exploration of full 3D architectures  
- Integration of transformer-based models  

---

