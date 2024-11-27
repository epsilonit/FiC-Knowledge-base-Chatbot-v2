Example 1 of D4.3 - Deep Learning
### Use case

common

### Name of resource

LeNet Classifier

### ID

lenet_classifier

### Description

Multi-layer Convolutional Neural Network for image classification

### Main category

Deep Learning

### Other category

_No response_

### Publication date

2023-04-04

### Objective

classification

### Platform

Google Colab

### Framework

Keras

### Architecture

CNN - Convolutional-Neural-Network

### Approach

supervised

### Algorithm

LeNet

### Processor

gpu

### OS

linux

### Keyword

classification, CNN, LeNet 

### Reference link

https://en.wikipedia.org/wiki/LeNet

### Example

https://github.com/cozzolinoac11/wildfire_prediction/blob/main/ann.ipynb

### Input data used

1. https://public.epsilon-italia.it/FAIRiCUBE/wildfire-classification/data_numpy.zip

### Characteristics of input data

1. Numpy arrays. (Perfectly) balanced classes. 

### Biases and ethical aspects

_No response_

### Output data obtained

1. http://www.epsilon-italia.it/public/model.zip

### Characteristics of output data

1. Keras model for wildfire or nowildfire classification. The model gets in input a dataset as numpy arrays (dimension 100x100x3) and returns the predicted labels.

### Performance

Accuracy score: 0.9505 (validation). Running time: 2 min for 23 training epochs with early stopping (total number of epochs: 50) on a gpu Nvidia a100. Modified hyperparameters: Input shape: (100,100,3); Optimizer: 'adam'; batch size: 128.  Train-test-valid split: 70-15-15. Loss function: sparse_categorical_crossentropy. 

### Conditions for access and use

cc-by-4.0

### Constraints

_No response_