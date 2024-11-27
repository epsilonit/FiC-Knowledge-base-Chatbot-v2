Valid stac json metadata file in the catalog appearing as invalid , 
I have noticed that in the [stac-fastapi catalog](https://catalog.eoxhub.fairicube.eu/), when you click to get details of an item's source, the associated stac json metadata file is always declared invalid  (this happens for all items in the catalog, no matter if they're datasets or a/p metadata) - 
However, when I validate these same files with Oxygen or with JSON validators (I tried https://jsonchecker.com/ and https://jsonchecker.com/) they are fully valid.
Can you explain this behaviour? What kind of validation is performed and against what requirements?
See below screenshots for an example
![image](https://github.com/FAIRiCUBE/FAIRiCUBE-Hub-issue-tracker/assets/13329248/f4af8f1d-0865-467e-a48a-72b1dd67df58)
![image](https://github.com/FAIRiCUBE/FAIRiCUBE-Hub-issue-tracker/assets/13329248/2ae18388-ba4d-4289-81ee-f0a7690e60ab)


The stac browser sends a validation/linting request to  [https://staclint.com/](https://staclint.com/). And in the case of `lenet_classifier` item, even though it is a valid json item, it does not follow the [https://schemas.stacspec.org/v1.0.0/item-spec/json-schema/item.json](https://schemas.stacspec.org/v1.0.0/item-spec/json-schema/item.json) schema.
"bbox" is specifically missing, adding a "bbox" list ( that matches the geometry extent) would resolve this issue  : 

```json
{
  "id": "lenet_classifier",
  "type": "Feature",
  "links": [
    {
      "rel": "collection",
      "type": "application/json",
      "href": "https://stacapi-write.eoxhub.fairicube.eu/collections/ML collection"
    },
    {
      "rel": "parent",
      "type": "application/json",
      "href": "https://stacapi-write.eoxhub.fairicube.eu/collections/ML collection"
    },
    {
      "rel": "root",
      "type": "application/json",
      "href": "https://stacapi-write.eoxhub.fairicube.eu/"
    },
    {
      "rel": "self",
      "type": "application/geo+json",
      "href": "https://stacapi-write.eoxhub.fairicube.eu/collections/ML collection/items/lenet_classifier"
    },
    {
      "rel": "about",
      "href": "https://en.wikipedia.org/wiki/LeNet",
      "type": "text/html",
      "title": "Reference link"
    },
    {
      "rel": "about",
      "href": "https://github.com/cozzolinoac11/wildfire_prediction/blob/main/ann.ipynb",
      "type": "text/html",
      "title": "Example"
    }
  ],
  "assets": {
    "input-data-used": {
      "href": "https://public.epsilon-italia.it/FAIRiCUBE/wildfire-classification/data_numpy.zip",
      "type": "application/json",
      "roles": [
        "data"
      ],
      "title": "Input data used",
      "description": "Numpy arrays. (Perfectly) balanced classes.",
      "biases-and-ethical-aspects": ""
    },
    "model-checkpoint": {
      "href": "http://www.epsilon-italia.it/public/model.zip",
      "type": "application/octet-stream",
      "roles": [
        "ml-model:checkpoint"
      ],
      "title": "Model",
      "description": "Keras model for wildfire or nowildfire classification. The model gets in input a dataset as numpy arrays (dimension 100x100x3) and returns the predicted labels.",
      "performance": "Accuracy score: 0.9505 (validation). Running time: 2 min for 23 training epochs with early stopping (total number of epochs: 50) on a gpu Nvidia a100. Modified hyperparameters: Input shape: (100,100,3); Optimizer: 'adam'; batch size: 128. Train-test-valid split: 70-15-15. Loss function: sparse_categorical_crossentropy."
    }
  },
  "geometry": {
    "type": "Polygon",
    "coordinates": [
      [
        [
          -180,
          -90
        ],
        [
          -180,
          90
        ],
        [
          180,
          90
        ],
        [
          180,
          -90
        ],
        [
          -180,
          -90
        ]
      ]
    ]
  },
  "bbox":[-180, -90, 180, 90],
  "collection": "ML collection",
  "properties": {
    "title": "LeNet Classifier",
    "license": "CC-BY-4.0",
    "datetime": "2023-04-04T00:00:00Z",
    "keywords": [
      "classification",
      "CNN",
      "LeNet"
    ],
    "platform": "Google Colab",
    "provider": "epsit",
    "use-case": "common",
    "algorithm": "LeNet",
    "framework": "Keras",
    "description": "Multi-layer Convolutional Neural Network for image classification",
    "main-category": "Deep Learning",
    "ml-model:type": "ml-model",
    "use-constraints": "",
    "model-configuration": "",
    "ml-model:training-os": "linux",
    "ml-model:architecture": "CNN - Convolutional-Neural-Network",
    "ml-model:prediction_type": "classification",
    "ml-model:learning_approach": "supervised",
    "ml-model:training-processor-type": "gpu"
  },
  "stac_extensions": [
    "https://stac-extensions.github.io/ml-model/v1.0.0/schema.json"
  ],
  "stac_version": "1.0.0"
}
```
Thank you for explanation! I am now closing the issue.