# MRI Sorter

## Brain MRI Image Classification

An experimental **computer vision and machine learning project** developed to classify brain Magnetic Resonance Imaging (MRI) scans into two categories:

- `yes`: brain tumor detected
- `no`: no brain tumor detected

This project was developed as a personal side project during my free time, with the goal of exploring and comparing two different image classification approaches using **Convolutional Neural Networks (CNNs)** and **transfer learning** with TensorFlow.

> **Disclaimer:** This project is intended for educational and experimental purposes only. The models are not designed for medical diagnosis and should not be used to make clinical decisions.

---

## Dataset

The training process used **662 brain MRI images**, distributed as follows:

| Class | Images |
|---|---:|
| No tumor (`no`) | 334 |
| Tumor (`yes`) | 328 |
| **Total** | **662** |

The images were collected from two publicly available datasets on **Kaggle**:

- *Brain MRI Images for Brain Tumor Detection* — Navoneel
- *Brain Tumor MRI Dataset* — Masoud Nickparvar

An additional small independent dataset containing **20 images** was used to visually evaluate the models by generating predictions on images that were not part of the training process.

---

## Approaches

The project contains two notebooks, each implementing a different classification approach.

### 1. Custom CNN Classifier

The first approach consists of a convolutional neural network developed from scratch.

The architecture includes:

- 4 convolutional (`Conv2D`) layers
- `MaxPooling2D` layers for progressive dimensionality reduction
- A `Flatten` layer
- A fully connected (`Dense`) layer with 1,000 neurons
- `Dropout` with a rate of 0.5 to reduce overfitting
- A final `Dense` layer with 2 neurons using `softmax` activation

**Data augmentation** was applied to increase the variety of the training data, using transformations such as:

- Rotation
- Horizontal and vertical shifts
- Shearing
- Zoom
- Horizontal flipping

The main hyperparameters were:

| Parameter | Value |
|---|---:|
| Image size | 250 × 250 |
| Batch size | 32 |
| Learning rate | 0.001 |
| Epochs | 30 |
| Optimizer | Adam |
| Loss function | Categorical Crossentropy |
| Train/validation split | 70% / 30% |

### Results

After training, the model achieved:

```text
accuracy:     0.8064
loss:         0.4243
val_accuracy: 0.7071
val_loss:     0.4985
```

<img width="1577" height="989" alt="first method" src="https://github.com/user-attachments/assets/0d760a5f-95a3-4889-9b47-79a82bf1acce" />

Although the model successfully learned relevant features from the MRI images, training was relatively expensive in terms of time and computational resources. The difference between training and validation accuracy also indicated room for improvement in the model's ability to generalize to unseen data.

This motivated the exploration of a second approach based on **transfer learning**.

---

### 2. Transfer Learning Classifier

The second approach uses **MobileNetV2** as a pretrained feature extractor.

Instead of training a complete convolutional neural network from scratch, a pretrained MobileNetV2 model was used with its weights frozen:

```text
MobileNetV2
     ↓
Feature extraction
     ↓
Dense (2 neurons)
     ↓
Softmax
```

The input images were resized to **224 × 224 pixels** and normalized before being passed to the model.

The pretrained feature extractor was obtained from:

**MobileNetV2 Feature Vector — TensorFlow Hub**

The main parameters were:

| Parameter | Value |
|---|---:|
| Image size | 224 × 224 |
| Batch size | Default `model.fit` value |
| Epochs | 20 |
| Optimizer | Adam |
| Loss function | Sparse Categorical Crossentropy |
| Train/validation split | 70% / 30% |
| MobileNetV2 weights | Frozen |

`tf_keras` was used for this implementation due to compatibility issues encountered with `tf.keras` and TensorFlow Hub.

### Results

After training, the model achieved:

```text
accuracy:     1.0000
loss:         0.0365
val_accuracy: 0.9397
val_loss:     0.1533
```
<img width="1577" height="989" alt="second method" src="https://github.com/user-attachments/assets/ec40e010-34b8-491a-9b2a-817d0d46edbe" />


The transfer learning approach achieved higher validation accuracy than the custom CNN while also significantly reducing the complexity of the training process.

---

## Comparison

The two approaches provide an opportunity to explore different strategies for image classification:

| Feature | Custom CNN | Transfer Learning |
|---|---|---|
| Architecture | Built from scratch | Pretrained MobileNetV2 |
| Training | All model parameters | Classification layer only |
| Data augmentation | Yes | No |
| Input size | 250 × 250 | 224 × 224 |
| Epochs | 30 | 20 |
| Validation accuracy | 70.71% | 93.97% |
| Computational complexity | Higher | Lower |

The results illustrate the potential benefits of **transfer learning** when working with relatively small datasets, as pretrained models can provide useful visual representations learned from much larger datasets.

---

## Project Structure

```text
MRI-sorter/
│
├── README.md
│
├── clasificador_propio.ipynb
│
├── clasificador_con_transfer_learning.ipynb
│
└── testing_dataset/
    ├── no/
    │   ├── ...
    │
    └── yes/
        ├── ...
```

### Notebooks

- **`clasificador_propio.ipynb`**  
  Implementation and training of the custom CNN classifier.

- **`clasificador_con_transfer_learning.ipynb`**  
  Implementation of the MobileNetV2 transfer learning classifier using TensorFlow Hub.

---

## Technologies

- Python
- TensorFlow
- Keras / `tf_keras`
- TensorFlow Hub
- NumPy
- OpenCV
- Matplotlib
- Scikit-learn
- Jupyter Notebook

---

## Results Visualization

Both notebooks include a visual evaluation stage in which test images are displayed together with:

- The predicted class (`yes` / `no`)
- The actual class
- The confidence associated with the prediction

This provides a qualitative view of how each model behaves on individual MRI images.

---

## Limitations

This project should be considered an **educational experiment**, not a medical diagnostic tool.

Some of its main limitations include:

- The dataset is relatively small for a medical image classification problem.
- The images come from different public datasets, which may introduce differences in acquisition methods, formats, and data distributions.
- The validation set is relatively small.
- The reported metrics do not demonstrate that the models would generalize reliably to real-world clinical data.
- No clinical validation or comparison against assessments performed by medical professionals was conducted.
- Accuracy alone is not sufficient to evaluate a model for medical applications. Metrics such as sensitivity, specificity, precision, recall, and AUC would be necessary for a more comprehensive evaluation.

For these reasons, the model's predictions **should not be interpreted as medical diagnoses**.

---

## Project Goal

The primary goal of this project was not to develop a clinical tool, but to experiment with different **deep learning techniques for image analysis**, compare a CNN built from scratch with a transfer learning approach, and explore the advantages and limitations of both strategies.

The project provided hands-on experience with concepts including:

- Convolutional Neural Networks
- Data augmentation
- Dropout regularization
- Adam optimization
- Image classification
- Transfer learning
- Feature extraction
- Computer vision model evaluation
