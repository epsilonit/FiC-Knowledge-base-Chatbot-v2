Presentation of vector data in high-resolution gridded form
Hi!

We are at the stage where we would like to test the possibility of presenting UC4 data in gridded format. We are looking for the best possible practice to transform our data from vector format to gridded format. We are also interested in testing different pixel sizes to find an optimal outcome concerning resolution and data size. 

Best wishes
Babak
I recommend using gdal_rasterize (https://gdal.org/programs/gdal_rasterize.html) for the conversion and then gdal_translate (https://gdal.org/programs/gdal_translate.html) for any further raster conversions. 
you can have a look at an example from ETC DI work : 
https://github.com/jetschny/323_end_noise/blob/master/src/features/load_osm_data_streets_knossos.py could it be that you already have this problem solved?
We have already used to vector-to-raster in some scripts. I will extract the vec-to-raster part of  the script  and make it available.
I set up a subfolder in our common-code repository where we can collect vector to raster codes:

https://github.com/FAIRiCUBE/common-code/tree/main/from_VECTOR_to_RASTER
We have just taken another look at the Vector to Raster tools and created a workfow:
[LINK to workflow png](https://github.com/FAIRiCUBE/common-code/blob/main/from_VECTOR_to_RASTER/workflow/vector_to_raster_work_flow.png)

So far we only have polygons to raster as a tool:
[LINK to vector (polygon) to raster](https://github.com/FAIRiCUBE/common-code/blob/main/from_VECTOR_to_RASTER/Tool_vector_to_raster.ipynb)

--> If you have any additional ideas, please add them to the notbook. 

What will be created next is a **notbook that converts points and lines into raster**. As this is a slightly different approach, we have decided to create a separate notebook for this. 


-->If you have already done something, please post it here.