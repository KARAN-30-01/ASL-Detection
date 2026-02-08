# 🤟 American Sign Language Recognition

This repository implements **American Sign Language (ASL) alphabet recognition** using deep learning.  
Two approaches are explored and compared:

1. **Baseline Custom CNN** trained from scratch  
2. **ResNet18 with Transfer Learning and Fine-Tuning**

The focus of this project is not only high accuracy, but **correct evaluation methodology, training efficiency, and real-world readiness**.

---

## 📊 Dataset

- **ASL Alphabet Dataset**
- 87,000 RGB images
- 29 classes:
  - Letters A–Z
  - SPACE, DELETE, NOTHING
- Perfectly balanced (3,000 images per class)

> Dataset is not included in this repository due to size.  
> Download from Kaggle: https://www.kaggle.com/datasets/grassknoted/asl-alphabet

---

## 🧠 Models

## 1️⃣ Baseline Model: Custom CNN

### Architecture
- Input: **64×64 RGB images**
- 4 convolutional blocks with progressive channel expansion:
  - 64 → 128 → 256 → 512
- Each block:
  - Convolution
  - Batch Normalization
  - ReLU
  - Max Pooling
- Fully connected head:
  - 8192 → 1024 → 512 → 29
  - Dropout (0.5, 0.3)

**Total parameters:** ~13.6 million

### Training Strategy
- Optimizer: Adam
- Loss: Cross Entropy
- Learning rate scheduling: ReduceLROnPlateau
- Early stopping to prevent overfitting
- Proper **stratified train / validation / test split (70 / 15 / 15)**

### Results
- Validation Accuracy: **99.90%**
- Test Accuracy: **99.94%**
- Kaggle Test Accuracy: **100% (28 samples)**

### Key Observation
The model demonstrates that ASL classification can be learned from first principles, but requires longer convergence time and higher computational cost.

---

## 2️⃣ ResNet18 Model: Transfer Learning with Fine-Tuning

### Motivation
Instead of learning low-level features from scratch, this model reuses **ImageNet-pretrained visual features** (edges, textures, shapes), which transfer well to hand gesture recognition.

### Architecture
- Backbone: **ResNet18 (ImageNet pretrained)**
- Custom classification head:
  - 512 → hidden layer → 29
  - Dropout: 0.4

**Total parameters:** ~11.4 million

### Training Strategy
- Backbone frozen for first **10 epochs**
- Classifier-only training initially
- Late fine-tuning for last **2 epochs** with very small learning rate
- Cosine learning rate scheduling
- Automatic Mixed Precision (AMP) for faster and memory-efficient training

### Results
- Best Validation Accuracy: **99.90%**
- Test Accuracy: **100%**
- Faster convergence with fewer trainable parameters during early training

### Why It Works
- ImageNet features generalize well to ASL hand shapes
- Late unfreezing prevents catastrophic forgetting
- Controlled fine-tuning enables domain-specific adaptation

---

## 🔍 Evaluation Philosophy

Although both models achieve near-perfect accuracy, the dataset is **balanced and captured under controlled conditions**, making classification relatively straightforward.

Therefore:
- **Raw accuracy alone is not a sufficient metric**
- Training efficiency, convergence behavior, and scalability are more meaningful indicators

Transfer learning provides clear advantages in terms of **efficiency and production readiness**, despite similar accuracy.

---

## 🚀 Future Work

- Real-time webcam-based ASL recognition
- Video-based ASL recognition using CNN–LSTM or Transformer architectures
- Multimodal fusion with MediaPipe hand landmarks
- Robust training on diverse backgrounds, lighting conditions, and motion blur

---

## 🛠️ Tech Stack

- Python
- PyTorch
- Torchvision
- Scikit-learn
- NumPy
- Matplotlib

