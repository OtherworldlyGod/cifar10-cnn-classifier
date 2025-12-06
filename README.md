# cifar10-cnn-classifier
A custom Convolutional Neural Network (CNN) built from scratch using TensorFlow/Keras to classify images on the CIFAR-10 dataset. Achieved 80% accuracy.

# Image Classification using CNN (CIFAR-10)

## Project Overview
This project involves building and training a Convolutional Neural Network (CNN) **from scratch** to classify images into 10 distinct categories. The goal was to demonstrate a deep understanding of CNN architecture design, training dynamics, and model evaluation without relying on pre-trained models (like ResNet or VGG).

**Dataset Used:** [CIFAR-10](https://www.cs.toronto.edu/~kriz/cifar.html)
**Framework:** TensorFlow / Keras

---

## Dataset Details
* **Source:** CIFAR-10 Public Dataset
* **Input Shape:** 32x32 pixels (RGB Color)
* **Classes:** 10 (Airplane, Automobile, Bird, Cat, Deer, Dog, Frog, Horse, Ship, Truck)
* **Training Size:** 50,000 images
* **Test Size:** 10,000 images
* **Preprocessing:** Pixel values were normalized to the range `[0, 1]`.

---

## Model Architecture
The model follows a custom VGG-style architecture designed to balance performance and computational efficiency. It consists of **3 Convolutional Blocks** followed by a Dense Classifier.

### Layer Breakdown:
1.  **Input Layer:** `(32, 32, 3)`
2.  **Convolutional Block 1:**
    * `Conv2D` (32 filters) + `BatchNormalization` + `ReLU`
    * `Conv2D` (32 filters) + `BatchNormalization` + `ReLU`
    * `MaxPooling2D`
    * `Dropout` (0.2)
3.  **Convolutional Block 2:**
    * `Conv2D` (64 filters) + `BatchNormalization` + `ReLU`
    * `Conv2D` (64 filters) + `BatchNormalization` + `ReLU`
    * `MaxPooling2D`
    * `Dropout` (0.3)
4.  **Convolutional Block 3:**
    * `Conv2D` (128 filters) + `BatchNormalization` + `ReLU`
    * `MaxPooling2D`
    * `Dropout` (0.4)
5.  **Classifier Head:**
    * `Flatten`
    * `Dense` (128 units) + `BatchNormalization` + `ReLU`
    * `Dropout` (0.5)
    * `Dense` (Output 10 units) + `Softmax`

### Key Design Decisions:
* **Batch Normalization:** Applied after convolutions to stabilize learning and allow for faster convergence.
* **Dropout:** progressively increased (0.2 -> 0.5) in deeper layers to prevent overfitting.
* **No Pre-trained Weights:** The model weights were initialized randomly and trained entirely from scratch.

---

## Training Configuration
* **Optimizer:** Adam
* **Loss Function:** Sparse Categorical Crossentropy
* **Metrics:** Accuracy
* **Epochs:** 20
* **Batch Size:** 64

---

## Results & Observations

### Performance Metrics
* **Peak Validation Accuracy:** ~82.86% (Epoch 17)
* **Final Test Accuracy:** ~80%
* **Final Test Loss:** ~0.58

### Key Observations
1.  **Vehicles vs. Animals:**
    The model performs significantly better on man-made objects compared to animals.
    * **High Performance:** *Automobile* (92%), *Ship* (91%), and *Truck* (89%) show high F1-scores due to distinct, rigid edges.
    * **Lower Performance:** *Cat* (64%) and *Bird* (68%) are frequently misclassified, likely due to similar textures and fuzzy boundaries.

2.  **Precision/Recall Nuance:**
    For classes like **Dog** and **Frog**, the model exhibits **High Recall** but **Low Precision**.
    * *Example:* It successfully identifies 94% of all Frogs (Recall), but it often mistakes other animals for Frogs (Precision 0.67). This suggests the model has learned "broad" features (e.g., green blobs) but lacks fine-grained discrimination for these classes.

3.  **Training Dynamics:**
    Validation accuracy peaked around Epoch 17. By Epoch 20, the training accuracy (83%) began to diverge slightly from validation accuracy (79%), indicating the onset of mild overfitting.

---

## Improvements & Experiments
During the development process, several iterations were tested:
1.  **Baseline:** Initial simple CNN (no Batch Norm) achieved only ~60% accuracy.
2.  **Added Batch Normalization:** This single change improved accuracy by ~15% and stabilized the loss curve.
3.  **Tuning Dropout:** Increasing Dropout in the dense layer from 0.3 to 0.5 helped reduce the gap between training and validation accuracy.


---

## 🛠️ How to Run
1.  Open the `.ipynb` file in Google Colab or Jupyter Notebook.
2.  Ensure a GPU runtime is selected for faster training.
3.  Run all cells sequentially.
4.  The notebook will download the dataset automatically via the Keras API.
