# Dataset

## Source

This project uses the **CIFAR-10** image classification dataset.

CIFAR-10 contains **60,000 colour images** distributed across 10 object classes:

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

Each image has a resolution of **32 × 32 pixels** with three RGB colour channels.

The dataset is divided into:

- 50,000 training images
- 10,000 test images

## Why this dataset

CIFAR-10 is well suited to Convolutional Neural Network experimentation because it provides a balanced multi-class image classification problem with enough complexity to evaluate:

- convolutional feature extraction,
- model architecture,
- training and validation behaviour,
- overfitting and generalisation,
- class-level performance,
- and prediction errors.

The project will go beyond overall accuracy by analysing which classes the network learns well, where confusion occurs and how model behaviour changes during training.

## Data Access

The raw image dataset is not stored in this repository.

The notebook loads CIFAR-10 directly through the TensorFlow/Keras dataset API:

`tensorflow.keras.datasets.cifar10`

This keeps the repository lightweight while making the analysis reproducible.
