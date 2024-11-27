generated stac objects type
The current supported structure of stac api is a catalog(root) that contains collections, then each collection contains its items. ( Catalog (root) -> Collections -> Items) 


Which means if we have a `resource-metadata` collection, no sub-collection would be enabled in the stac api.


I can see that the  `resource-metadata` stac objects are generated as collections "`ml_collection` and `no_ml_collection`" which means  they cannot be ingested as sub-collections, I would propose to generate a one `resource-metadata` collection and then add the rest of objects to it as items. 
Do I understand correctly that while STAC can formally nest catalogs and collections in any way, the api only supports the simplified hierarchy of Catalog (root) -> Collections -> Items? No Collections of Collections?

To my memory, most of the datasets available on the existing FAIRiCUBE catalogue (taken from EDC) were described as Collections already (wasn't quite sure why, working assumption was that as they were mostly time series, a single time slice was seen as an Item, the time series as a Collection), we will not be able to create a group of dataset metadata separate from a/p resource metadata?
exactly, you are correct regarding the api structure.
As for the EDC collections, I assume ingesting the collection's items instead would be a valid workaround.
dear 
while I am not that happy with this structure, I am afraid we have no choice. 
Let's agree on the following:
we have one FiC catalog with 3 collections: dataset, ML resources and non-ML resources . 
Probably you could just take the related jsons (https://fairicube.github.io/resource-metadata/ml_collection.json and https://fairicube.github.io/resource-metadata/no_ml_collection.json)  and include as child in the root FiC catalog?  
Hi 
having 3 collections can be a workaround for now.
I will try and adjust `ML resources` and `non-ML resources` jsons and create their collections (not sure if this will work),
However, it will be  better if both jsons are created as root catalogs "with the correct links and relations".
e.g ml-collection.josn (note that `root` links and `self` links are the same): 
```json
{
    "type": "Catalog",
    "stac_version": "1.0.0",
    "stac_extensions": [],
    "title": "ML collection",
    "id": "ML collection",
    "description": "An example of ML collection.",
    "license": "various",
    "keywords": [
        "Machine Learning",
        "Deep Learning",
        "Artificial Intelligence"
    ],
    "links": [
        {
            "href": "https://fairicube.github.io/resource-metadata/ml_collection.json",
            "rel": "self",
            "type": "application/json"
        },
        {
            "href": "https://fairicube.github.io/resource-metadata/ml_collection.json",
            "rel": "root",
            "type": "application/json"
        },
        {
            "href": "https://fairicube.github.io/resource-metadata/Example 1 of D4.3 - Deep Learning.json",
            "rel": "item",
            "type": "application/geo+json"
        }
    ]
}
```

I also noticed that the links in the generated stac items are relative (e.g: `"href": "./ml_collection.json"`).
I don't know if the deployed stac-fastapi resolves the links or not, if it does then that's not a problem, otherwise absulote hrefs are needed.

Hi
we have seen that two collections ("[ML collection](https://catalog.eoxhub.fairicube.eu/collections/ML%20collection)" and "[no-ML collection](https://catalog.eoxhub.fairicube.eu/collections/no-ML%20collection)") have been created, but that the items we had uploaded in the [resource-metadata/stac](https://github.com/FAIRiCUBE/resource-metadata/tree/main/stac) folder are no longer visible using [the browser](https://catalog.eoxhub.fairicube.eu/). This probably happens because different collections are used than the ones in the GitHub folder.
Can you please have a look and fix the issue? 
Hi 
Yes I manually created the two collections, but regarding the items, I tried to ingest them, but they are invalid items, since they are missing some required fileds (geometry & bbox). stac-fastapi validates ingested items against [items-specs](https://github.com/radiantearth/stac-spec/blob/master/item-spec/item-spec.md) before ingesting them.

I have edited `Example 3 of D4.3 - Pre-processing.json` ( I additionally added datacube extention object to it ) and you can see it now in the browser [here](https://catalog.eoxhub.fairicube.eu/collections/no-ML%20collection/items/JPEG_to_numpy_transformation).

the edited ingested json:
```json
{
    "type": "Feature",
    "stac_version": "1.0.0",
    "id": "JPEG_to_numpy_transformation",
    "collection": "no-ML collection",
    "geometry": {
    "type": "Polygon",
    "coordinates": [
      [
        [
          -180.0,
          -90.0
        ],
        [
          -180.0,
          90.0
        ],
        [
          180.0,
          90.0
        ],
        [
          180.0,
          -90.0
        ],
        [
          -180.0,
          -90.0
        ]
      ]
    ]
  },
    "properties": {
        "cube:dimensions": {
            "x": {
                "axis": "x",
                "extent": [
                -180,
                180
                ],
                "reference_system": "ESPG:4326",
                "type": "spatial"
            },
            "y": {
                "axis": "y",
                "extent": [
                -90,
                90
                ],
                "reference_system": "ESPG:4326",
                "type": "spatial"
            },
            "time": {
                "values": [
                "2000-01-01T00:00:00"
                ],
                "type": "temporal"
            }
        },
        "title": "JPEG images to numpy array transformation",
        "description": "Building dataset as numpy array. In machine learning, Python uses image data in the format of a NumPy array, i.e., [Height, Width, Channel] format. Therefore, the images must be transformed in this format. In this case, the images are in JPEG format and, through pillow, NumPy and OpenCV functions, the transformation is performed. The cv2 package (OpenCV) has the method imread() which is used to load the image and it also reads the given image (PIL image) in the NumPy array format. Because the images within the dataset (i.e., the NumPy arrays) must all be the same size to be used, and as a matter of efficiency and calculation power, using cv2s resize() the images are resized from 350x350 pixels into 100x100 (this dimension can be easily changed). The channel is three because the images are RGB. This method then returns a dataset containing the images in the format of NumPy arrays and their respective class labels.",
        "main-category": "Pre-processing",
        "objective": "data-transformation",
        "datetime": "2023-08-05",
        "keywords": [
            "numpy array",
            "data transformation",
            "jpeg"
        ],
        "platform": "Google Colab",
        "framework": "OpenCV",
        "algorithm": "custom-method",
        "license": "CC-BY-4.0",
        "processor-used": "cpu",
        "operating-system-used": "linux",
        "use-constraints": "no Constraint of Use"
    },
    "links": [
        {
            "rel": "root",
            "href": "./index.json",
            "type": "application/json",
            "title": "Root Catalog"
        },
        {
            "rel": "parent",
            "href": "./no_ml_collection.json",
            "type": "application/json",
            "title": "no_ml_collection"
        },
        {
            "rel": "collection",
            "href": "./no_ml_collection.json",
            "type": "application/json",
            "title": "no_ml_collection"
        },
        {
            "href": "https://github.com/cozzolinoac11/wildfire_prediction/blob/main/img_to_NPY_transformation.ipynb",
            "rel": "about",
            "type": "text/html",
            "title": "Example-1"
        }
    ],
    "assets": {
        "input-data-used": {
            "href": "https://open.canada.ca/data/en/dataset/9d8f219c-4df0-4481-926f-8a2a532ca003",
            "type": "application/json",
            "title": "Input data used",
            "description": "Refer to Canadas website for the original wildfires data. The dataset is composed by satellite images (shape is 350x350).",
            "biases-and-ethical-aspects": "",
            "roles": [
                "data"
            ]
        },
        "output-data-obtained": {
            "href": "https://public.epsilon-italia.it/FAIRiCUBE/wildfire-classification/data_numpy.zip",
            "type": "application/json",
            "title": "Output data obtained",
            "description": "Dataset in format Numpy arrays. The images are resized in 100x100.",
            "roles": [
                "data"
            ]
        }
    },
    "bbox": [
    -180.0,
    -90.0,
    180.0,
    90.0
  ],
  "stac_extensions": [
    "https://stac-extensions.github.io/datacube/v2.0.0/schema.json"
  ]
}
```

also I had to correct the collection id 

Hi 

- please remove the datacube-extention from these resources because it is not relevant to what is being metadated and does not add significant information
- most importantly, we are assuming that we continue to upload the STAC JSON files in the [resource-metadata/stac](https://github.com/FAIRiCUBE/resource-metadata/tree/main/stac) folder (with the correct links) and then, as before, they will automatically be made public in the catalogue. Do you confirm this is still the correct procedure?
yes, eventually that will be the procedure.

Hello ,
we have a problem with the browser link https://catalog.eoxhub.fairicube.eu/.
Specifically, I can access a resource navigating the browser, but when I try to access it directly using the URL, I receive error 404.
For example, the resource _"JPEG images to numpy array transformation"_ under _no-ML collection_ can be seen navigating the browser, but https://catalog.eoxhub.fairicube.eu/collections/no-ML%20collection/items/JPEG_to_numpy_transformation returns 404 error. 
This also happens with other resources.
Can you help? as FAIRiCUBE Hub is now formally operational, I'd appreciate components such as the catalog to be available and functioning!!!

Btw - what's status on dataset metadata? From what I see, the catalog is still restricted to what's already available via EDC  the issue was casued from the main stac-browser repo, it is fixed now.
could you please try accessing the links again and confirm?

Hi 
Yes, it works correctly now.
Can we then add the item stac-json files to the  [resource-metadata/stac](https://github.com/FAIRiCUBE/resource-metadata/tree/main/stac) folder so that we can see them automatically in the catalog? 
I'm running a script in github action to inject `data-access` items, do you want me to do the same here ?
If so, maybe you can open an issue and assign me :) 

resolved