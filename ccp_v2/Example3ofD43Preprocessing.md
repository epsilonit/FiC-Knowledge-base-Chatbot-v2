Example 3 of D4.3 - Pre-processing
### Use case

common

### Name of resource

JPEG images to numpy array transformation

### ID

JPEG_to_numpy_transformation

### Description

Building dataset as numpy array. In machine learning, Python uses image data in the format of a NumPy array, i.e., [Height, Width, Channel] format. Therefore, the images must be transformed in this format. In this case, the images are in JPEG format and, through pillow, NumPy and OpenCV functions, the transformation is performed. The cv2 package (OpenCV) has the method imread() which is used to load the image and it also reads the given image (PIL image) in the NumPy array format. Because the images within the dataset (i.e., the NumPy arrays) must all be the same size to be used, and as a matter of efficiency and calculation power, using cv2's resize() the images are resized from 350x350 pixels into 100x100 (this dimension can be easily changed). The channel is three because the images are RGB. This method then returns a dataset containing the images in the format of NumPy arrays and their respective class labels.

### Main category

Pre-processing

### Other category

_No response_

### Publication date

2023-08-05

### Objective

data-transformation

### Platform

Google Colab

### Framework

OpenCV

### Architecture

None

### Approach

None

### Algorithm

custom-method

### Processor

cpu

### OS

linux

### Keyword

numpy array, data transformation, jpeg

### Reference link

_No response_

### Example

https://github.com/cozzolinoac11/wildfire_prediction/blob/main/img_to_NPY_transformation.ipynb

### Input data used

1. https://open.canada.ca/data/en/dataset/9d8f219c-4df0-4481-926f-8a2a532ca003

### Characteristics of input data

1. Refer to Canada's website for the original wildfires data. The dataset is composed by satellite images (shape is 350x350).

### Biases and ethical aspects

_No response_

### Output data obtained

1. https://public.epsilon-italia.it/FAIRiCUBE/wildfire-classification/data_numpy.zip

### Characteristics of output data

1. Dataset in format Numpy arrays. The images are resized in 100x100.

### Performance

_No response_

### Conditions for access and use

cc-by-4.0

### Constraints

_No response_
Similar to the comment on #11 I think a bit more detail may be useful for non-expert users

On the description, could you provide a bit more detail on how the transformation is performed, what's available in the numpy array (how do you split the JPEG RGB to the array)

On "Input data used", the page you link to provides diverse datasets, it's unclear which are being used. In "Characteristics of input data", there's no link, only way of finding the information is the input data link.

On sizes, you don't provide a UoM. I'm assuming meters, but would be nice to add. 
The same comment made in issue https://github.com/FAIRiCUBE/resource-metadata/issues/11