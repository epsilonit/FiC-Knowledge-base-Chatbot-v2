Create binary layers from data layer. 
This example has the objective of creating a binary layer from data layers in QGIS. 
There are step-by-step screenshots showing the stages that have been performed. 
1- load raster file in QGIS (drag and drop) 
 
2- Open “Raster Calculator” in “Raster”. 
 

3- In this window ("Raster Calculator"), all parameters for creating the binary layer are entered. 
 
 
4- At this point I entered the expression (in the 'Raster Calculator Expression' field) and the 
path in which to save the layer (in the 'Output layer' field). Regarding CRS, format and 
others, I left the default values, but these can be changed. 
 
 

The expression basically consists of multiplications by 0 or 1 depending on the thresholds. 
 
A template of the expression follows: 
(("raster_data@1" < threshold_1)*1 + ("raster_data@1" >= threshold_1)*0) *  
(("raster_data@2" < threshold_2)*1 + ("raster_data@2" >= threshold_2)*0) *  
(("raster_data@3" < threshold_3)*1 + ("raster_data@3" >= threshold_3)*0)  
 
Where:  
• "raster_data@1" means band 1 of the raster_data file and so on. 
• Thresholds are given different names because they can take on different values between one 
band and another. 
Of course, different combinations of thresholds can offer very different results. 
 
5- Using the following expression, I obtained the below binary layer. 
(("horring_fields_2022@1" < 150)*1 + ("horring_fields_2022@1" >= 150)*0) *  
(("horring_fields_2022@2" < 150)*1 + ("horring_fields_2022@2" >= 150)*0) *  
(("horring_fields_2022@3" < 150)*1 + ("horring_fields_2022@3" >= 150)*0)  
 

 

