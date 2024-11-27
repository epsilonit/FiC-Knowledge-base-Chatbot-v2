Transform GeoTIFF file to COG. 
 
The folder contains three files: two Python scripts and one Jupyter Notebook. 
1. tif_to_COG_translate.py transforms a GeoTIFF to a Cloud-Optimized-GeoTIFF (COG). 
2. tif_to_COG_translate_notebook.ipynb is the Jupyter Notebook version of the script. 
3.  validate_cloud_optimized_geotiff.py is a Python script for check if a .tif file is or not a COG. 
REQUIREMENTS: 
1. GDAL - https://gdal.org/index.html 
2. PROJ - https://proj.org/about.html 
 
The script tif_to_COG_translate.py transforms a GeoTIFF to COG using GDAL translator library and OS 
system call. 
Parameters of function are: 
1. Path of input file (e.g., C:/User/……/input_file.tif) 
2. Path of output file (e.g., C:/User/……/output_file.tif) 
3. Include internal overviews parameter. 
4. Compression method. Typically, no compression, DEFLATE or LZW can be used for lossless, or 
JPEG for lossy. (Note that DEFLATE while more efficient than LZW can cause compatibility 
issues with some software packages) 
 
tif_to_COG_translate.py can be use in this way: 
 
Figure 1 - Example of use 

STEP BY STEP TRASFORMATION 
This example was done using a GeoTIFF file downloaded from CLMS.  
The steps performed are: 
1. Viewing the initial .tif file in QGIS, in order to make a comparison with the respective COG 
obtained. 
2. Checking whether the initial file is COG or not. 
3. Transformation of the initial file into COG. 
4. Displaying the COG .tif file in QGIS in order to compare it with the respective initial file. 
5. Verify whether the resulting file is indeed COG. 
 
 
 
Figure 2 - NO-COG file in QGIS 

 
Figure 3 - NO-COG information 
 
Figure 4 - NO-COG CRS 

Now, using validate_cloud_optimized_geotiff.py, we can see if the GeoTIFF file is really NO-COG.  
 
 
The file is not COG, let us now proceed with the transfo rmation, using the script 
tif_to_COG_translate.py. 
Parameters used are include_internal_overivews = True, compress="LZW".  
We obtained a new file which, when viewed in QGIS, appears: 
 
Figure 5 – COG file in QGIS 

 
Figure 6 - COG file information 
 
Figure 7 - COG file CRS 

Now, using the script using validate_cloud_optimized_geotiff.py we can check whether the resulting 
file is really COG. 
 
The file we obtained is COG! 

