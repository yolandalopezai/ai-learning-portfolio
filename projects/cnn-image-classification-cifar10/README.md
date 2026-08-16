# CNN Image Classification with CIFAR-10

A deep learning image classification project comparing a baseline Convolutional Neural Network with a more regularised architecture using TensorFlow and Keras.

The project focuses not only on overall accuracy, but also on generalisation, class-level performance, confusion patterns and prediction errors.

## Project Objective

The objective is to classify CIFAR-10 images into ten object categories and evaluate how architectural design and regularisation affect model performance.

Rather than stopping after training a single CNN, the project follows an iterative modelling approach:

1. Build a simple baseline CNN.
2. Analyse training behaviour and class-level errors.
3. Identify overfitting and recurring confusion patterns.
4. Build a more robust CNN using regularisation and data augmentation.
5. Compare both models on the untouched test set.
6. Analyse where the improved model succeeds and where limitations remain.

## Dataset

The project uses the **CIFAR-10** dataset available through the TensorFlow/Keras dataset API.

CIFAR-10 contains **60,000 RGB images** across ten balanced classes:

- Airplane
- Automobile
- Bird
- Cat
- Deer
- Dog
- Frog
- Horse
- Ship
- Truck

Each image has a resolution of **32 × 32 pixels** with three colour channels.

The original dataset contains:

- 50,000 training images
- 10,000 test images
- 5,000 training images per class
- 1,000 test images per class

The original training data was further divided into:

- 40,000 training images
- 10,000 validation images

A stratified split preserved the balanced class distribution.

The official CIFAR-10 test set remained untouched until final model evaluation.

## Data Preparation

Pixel values were originally stored as unsigned 8-bit integers ranging from 0 to 255.

Images were normalised to the **0–1 range** before training.

Labels were converted from `(n, 1)` arrays into one-dimensional vectors for easier evaluation.

The improved model also incorporated moderate data augmentation during training:

- horizontal flipping
- small rotations
- light zoom transformations

Validation and test images were not augmented.

## Baseline CNN

The baseline architecture consisted of three convolutional blocks:

- Conv2D — 32 filters
- MaxPooling
- Conv2D — 64 filters
- MaxPooling
- Conv2D — 128 filters
- MaxPooling
- Flatten
- Dense — 128 units
- Softmax output — 10 classes

The model contained **356,810 trainable parameters**.

Training used:

- Adam optimiser
- Sparse categorical cross-entropy
- Batch size: 64
- Early stopping based on validation loss
- Maximum: 30 epochs

### Baseline Results

The best validation loss occurred at **epoch 6**.

| Metric | Result |
|---|---:|
| Best validation accuracy | 72.69% |
| Test accuracy | 72.57% |
| Test loss | 0.8175 |
| Macro F1-score | 0.7246 |
| Trainable parameters | 356,810 |

Training accuracy continued increasing after epoch 6 while validation loss deteriorated, showing clear evidence of **overfitting**.

## Baseline Error Analysis

Performance varied substantially across classes.

Strongest baseline F1-scores included:

- Automobile: **0.8467**
- Ship: **0.8310**
- Truck: **0.8087**

The weakest class was:

- Cat: **0.5274**

Important confusion patterns included:

- 183 cats classified as dogs
- 110 dogs classified as cats
- 108 cats classified as birds
- 93 deer classified as frogs
- 83 horses classified as deer
- 82 trucks classified as automobiles

These results motivated the development of a more strongly regularised model.

## Improved CNN

The second architecture was designed to improve feature extraction while reducing overfitting.

It introduced:

- additional convolutional layers
- Batch Normalisation
- progressive Dropout
- data augmentation
- Global Average Pooling
- Early Stopping
- learning-rate reduction when validation loss plateaued

The improved model contained:

- **141,674 total parameters**
- **141,034 trainable parameters**
- **640 non-trainable parameters**

Despite using deeper convolutional feature extraction, the model was substantially smaller than the baseline because Global Average Pooling replaced the large flattened dense representation.

## Improved Model Results

The best validation result occurred at **epoch 29**.

| Metric | Baseline CNN | Improved CNN |
|---|---:|---:|
| Test accuracy | 72.57% | **76.58%** |
| Macro F1-score | 0.7246 | **0.7636** |
| Best validation accuracy | 72.69% | **76.97%** |
| Test loss | 0.8175 | **0.6768** |
| Total parameters | 356,810 | **141,674** |

The improved CNN increased test accuracy by **4.01 percentage points**.

Validation accuracy and test accuracy were also closely aligned, indicating substantially better generalisation than the baseline.

## Class-Level Comparison

F1-score improved in **nine of the ten classes**.

| Class | Baseline F1 | Improved F1 | Change |
|---|---:|---:|---:|
| Airplane | 0.7766 | 0.7784 | +0.0018 |
| Automobile | 0.8467 | 0.9018 | +0.0551 |
| Bird | 0.6201 | 0.6621 | +0.0420 |
| Cat | 0.5274 | 0.5988 | +0.0714 |
| Deer | 0.6607 | 0.7205 | +0.0598 |
| Dog | 0.6457 | 0.6819 | +0.0362 |
| Frog | 0.7674 | 0.7263 | -0.0410 |
| Horse | 0.7613 | 0.8206 | +0.0593 |
| Ship | 0.8310 | 0.8736 | +0.0425 |
| Truck | 0.8087 | 0.8720 | +0.0633 |

The strongest improvement occurred for the **cat** class.

## Improved Model Error Analysis

Several important baseline confusion patterns were reduced:

- Cat → Dog: **183 → 125**
- Cat → Bird: **108 → 47**
- Horse → Deer: **83 → 59**
- Truck → Automobile: **82 → 47**

The main exception was the **frog** class.

Its recall increased to **90.50%**, but precision decreased to **60.66%**.

The model became more likely to predict the frog label for visually similar animal images, including:

- 145 birds classified as frogs
- 159 cats classified as frogs
- 142 deer classified as frogs

This demonstrates why aggregate accuracy alone is insufficient for evaluating multi-class models.

## Key Findings

- A simple CNN achieved respectable performance but showed clear overfitting.
- Data augmentation and regularisation substantially improved generalisation.
- The improved architecture achieved higher accuracy with far fewer parameters.
- Test accuracy increased by **4.01 percentage points**.
- Macro F1 increased from **0.7246 to 0.7636**.
- Nine of ten classes improved in F1-score.
- Error analysis revealed performance trade-offs that would not be visible from accuracy alone.
- Model complexity should be evaluated by architecture and behaviour, not simply by parameter count.

## Technologies

- Python
- TensorFlow
- Keras
- NumPy
- Pandas
- Matplotlib
- scikit-learn
- Google Colab

## Repository Structure

```text
cnn-image-classification-cifar10/
│
├── README.md
├── cnn_image_classification_cifar10.ipynb
└── data/
    └── README.md
```

The raw CIFAR-10 dataset is not stored in the repository. It is loaded directly through:

`tensorflow.keras.datasets.cifar10`

## Limitations

CIFAR-10 images have a very low resolution of 32 × 32 pixels, which limits the visual detail available to the model.

Several object categories contain similar visual characteristics, particularly among animal classes.

The improved CNN reduces many baseline errors but does not eliminate class-specific trade-offs. The frog class demonstrates that increased recall can be accompanied by reduced precision.

Further experimentation could explore alternative architectures, stronger augmentation strategies, transfer learning or additional hyperparameter optimisation.

## Conclusion

This project demonstrates how systematic model evaluation can guide architectural improvement.

The improved CNN is both **smaller and more accurate** than the baseline, increasing test accuracy from **72.57% to 76.58%** while reducing the total parameter count from **356,810 to 141,674**.

More importantly, the analysis shows that meaningful model evaluation extends beyond a single accuracy score. Learning curves, class-level metrics, confusion matrices and individual prediction errors provide essential information about how a model behaves and where further improvement is needed.
