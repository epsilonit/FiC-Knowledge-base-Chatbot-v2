Semantic segmentation (CNN) model to detect dutch crop classes from Sentinel-2 imagery
### Name of resource

Crop Classification CNN (Example only, not suitable for production use!)

### ID

1

### Description

!! This model is for development purposes only, it is not suitable for production use !!

An example convolutional neural network trained on 7 Sentinel-2 images throughout the Dutch growing season, using bands R, G, B, and NIR of each image, and ground truth data taken from the Dutch agricultural land registration. All data used was from 2018, and the model has been trained to infer 76 different crop types.

### Main category

DL

### Other category

_No response_

### Pubblication date

2022

### Objective

segmentation

### Platform

Rasdaman

### Framework

PyTorch

### Architecture

cnn

### Approach

supervised

### Algorithm

Convolutional-Neural-Network

### Processor

gpu

### OS

linux

### Keyword

Dutch crop types, Sentinel-2, Convolutional Neural Network, Case Study, PyTorch

### Reference link

https://github.com/FAIRiCUBE/uc2-agriculture-biodiversity-nexus/blob/main/rasdaman-ml-udf/proof_of_concept/FAIRICUBE%20Machine%20Learning%20UDF%20Proof%20of%20Concept.ipynb

### Example

https://github.com/FAIRiCUBE/uc2-biodiversity-agriculture/tree/main/rasdaman-ml-udf

### Input data used

In rasdaman

### Characteristics of input data

Feature data:
7 Sentinel-2 images, R,G,B,NIR bands, representative of the Dutch growing season 2018. The data was in UTM projection and only cloud free images have been used. It covered a study area in the North-East of the country.

Label data:
The Dutch agricultural land registration data from 2018 of the study area has been used as ground truth data. It contains the farm parcel boundaries and the planted crops. The full list of crops has been reduced to 76 major types that were at least present in the region and thought to be potentially recognisable from the feature data. Still, the labels are significantly imbalanced. 

### Biases and ethical aspects

The crop data (labels) are significantly imbalanced, particularly towards grasslands. The trained model is merely a proof of concept and not recommended for serious applications or use outside of the study region and/or for years it has not been trained for.

### Output data obtained

In rasdaman?

### Characteristics of output data

The model produces a spatial dataset with the inferred crop type as integer index value for each grid cell. The index is sequential and can be translated into the actual crop type.

### Performance

This model is mostly a technological proof of concept and performance strongly varies per crop type (30% - 80%). Furthermore it achieves only low IoU values and the straight-forward CNN architecture used is not capable of reproducing parcel boundaries very well.

### Conditions for access and use

cc-by-nc-sa-4.0

### Constraints

_No response_