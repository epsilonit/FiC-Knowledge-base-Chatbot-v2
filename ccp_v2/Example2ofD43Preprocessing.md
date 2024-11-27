Example 2 of D4.3 - Pre-processing
### Use case

common

### Name of resource

SMOTE dataset balancing

### ID

SMOTE_dataset_balancing

### Description

Dataset balancing using SMOTE oversampling technique. A balanced dataset is a dataset where each output class (or target class) is represented by the same number of input samples. Imbalanced data is not always a bad thing and there is always some degree of imbalance in real data sets. That said, if the level of imbalance is relatively low, there should not be much impact on the performance of the model but, in some cases, working on unbalanced data could introduce a high error rate. Imbalanced data is one of the potential problems in the field of data mining and machine learning. This problem can be approached by properly analyzing the data. One way to solve this problem is to oversample the examples in the minority class. This can be achieved by simply duplicating examples from the minority class in the training dataset prior to fitting a model. This can balance the class distribution but does not provide any additional information to the model. An improvement on duplicating examples from the minority class is to synthesize new examples from the minority class. SMOTE works by selecting examples that are close in the feature space, drawing a line between the examples in the feature space and drawing a new sample at a point along that line. Specifically, a random example from the minority class is first chosen. Then k of the nearest neighbors for that example are found (typically k=5). A randomly selected neighbor is chosen, and a synthetic example is created at a randomly selected point between the two examples in feature space.

### Main category

Pre-processing

### Other category

_No response_

### Publication date

2023-08-05

### Objective

dataset-balancing

### Platform

Google Colab

### Framework

imblearn

### Architecture

None

### Approach

None

### Algorithm

SMOTE - Synthetic-Minority-Oversampling-TEchnique

### Processor

cpu

### OS

linux

### Keyword

dataset balancing, SMOTE

### Reference link

https://imbalanced-learn.org/stable/references/generated/imblearn.over_sampling.SMOTE.html 

### Example

https://github.com/cozzolinoac11/wildfire_prediction/blob/main/dataset_balancing.ipynb 

### Input data used

1. https://public.epsilon-italia.it/FAIRiCUBE/wildfire-classification/data_numpy.zip  

### Characteristics of input data

1. Numpy arrays with unbalanced classes (47.9% - 52.1%)

### Biases and ethical aspects

1. Initial unbalanced dataset

### Output data obtained

1. https://public.epsilon-italia.it/FAIRiCUBE/wildfire-classification/balanced_data_numpy.zip

### Characteristics of output data

1. Numpy arrays with balanced classes (50% - 50%)

### Performance

_No response_

### Conditions for access and use

cc-by-4.0

### Constraints

_No response_
Nice!

However, I'm wondering if a bit more descriptive text would be valuable. Maybe I'm the only one who gets a bit lost in your description, but I fear I don't quite understand what's meant by:
- SMOTE
- dataset balancing

The Description of "Dataset balancing using SMOTE oversampling technique" doesn't add much more information. Thus, at least to me, it's unclear what this could be used for.

Going back to the FAIRiCUBE core objective, I doubt domain experts like Martin or Heimo will understand more than I do. How can you help us understand???

Clearly this is a fairly simplistic example, so it has not been documented in detail, but it was intended to give an idea of how (and with what) to fill in each field according to the type of resource.
That said, I agree that my description was too short and meant for acknowledged people. I have just updated the ‘Description’ field, adding a few more details, to make the resource more understandable. For sure, in the form, we can recommend providing more explanatory descriptions meant also for less expert people. What do you think?
Agreed! Thanks!

In addition, I'm wondering if we should try and extract relevant keywords (e.g. "SMOTE", "balanced dataset") for inclusion in the Knowledge Base