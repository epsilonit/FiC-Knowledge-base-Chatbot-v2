Access ingested datasets from EOX
Once the datasets have been ingested, how do I access them? 
I need to get the Copernicus DEM, but the steps are not clear to me. 
Thanks when posing such questions, please also provide a link to the metadata record. To my understanding you're trying to access the following record: 
- [Copernicus DEM](https://catalog.eoxhub.fairicube.eu/collections/index/items/COPERNICUS_30)
There I only find a link to the EuroDataCube metadata entry:
- [EuroDataCube Copernicus DEM](https://collections.eurodatacube.com/copernicus-dem/) True, sorry I forgot about the link. Thank you for adding them. 
Hi Susanna, if you have configured SentinelHub access in your EOX Hub workspace, you can access Copernicus DEM via SentinelHub API. See this notebook in uc1 repository: [notebooks/demo/demo_processing.ipynb](https://github.com/FAIRiCUBE/uc1-urban-climate/blob/master/notebooks/demo/demo_processing.ipynb) (skip the part about data ingestion).
Admittedly, it is not very beginner-friendly :/
If you need further help let me know and we can set up a call :) has this been clarified? If so, please close the issue