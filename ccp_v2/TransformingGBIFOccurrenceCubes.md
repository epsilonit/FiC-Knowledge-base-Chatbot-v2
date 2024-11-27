Transforming GBIF Occurrence Cubes
As discussed in today's UC synergy meeting, we need to transform GBIF Occurrence Cubes to a grid format, e.g. TIF.

Basic information and a first example dataset are available from the [GBIF issue under data requests](https://github.com/FAIRiCUBE/data-requests-issue-archive/issues/35), [dataset here](https://github.com/FAIRiCUBE/data-requests/blob/main/encoding-examples/datacube_nl_farmland_birds_1.csv).

Initially we'd hoped to be able to create a dedicated taxa dimension, but as a first step we'll have to split this dataset by taxon, still need to see if we can provide the 2 bands count and uncertainty as bands or as separate TIFs
Some information on the lat lon information inside the CSV file:

The csv already contains all the information for grid creation: The GRIDNUM (id) specifies the grid size (10m) and the coordinate of the bottom left corner. 

For example: the coordinate in EPSG3035 projection for **10mE401832N328626** is: 
 East 401832x10m = 4018320 and North 328626x10m = 3286260
--> N (3286260 meter ) & E (4018320 meter)   


You can then build a polygon from the coordinates and convert it into any raster format. In this case, I think the resolution of 10m is a bit too high. The use of a 100m or 1km GRID should be sufficient.
The csv files wiht all species in QGIS:
![original_10_coordinates](https://github.com/FAIRiCUBE/FAIRiCUBE-Hub-issue-tracker/assets/117716295/d377083f-64ff-4924-9647-25c702f82bd4)

The selected species Vellus vanellus as 1km GRID:
![bird_Vanellus_vanellus _1km_GRIDZELL](https://github.com/FAIRiCUBE/FAIRiCUBE-Hub-issue-tracker/assets/117716295/298535cc-0430-4a3e-af97-dfaeb1063df0)


Thanks!
Resolution and species are up to the UC partner, in this case we requested 10m resolution on the species listed. 
- do you really need 10m or would 100m also work?

What we'd need is step-by-step guidance on how to grid these CSVs, ideally so simple that the UC2 & UC5 partners can do this on their own
We're working on farm field level, therefore resolution of 10m would be relevant.
Indeed knowing that coordinate indicates bottom left corner of cell is crucial for gridding.
The dataset is an observation dataset and not a distribution dataset - if my understand correctly.
 This means that the coordinates listed in the csv only indicate that a certain species was seen at this position.
 Experts later use this to produce the distribution map. --> If there are no observations between two observation points, this does not mean that the bird does not occur there, but has not been sighted. With the distribution map, this area would be filled - if the habitat is suitable for the bird.


I have started to set up a notebook for translating the csv into coordinates:
(https://github.com/FAIRiCUBE/uc1-urban-climate/blob/master/notebooks/dev/f06_mixed/Specied_distribution_grid.ipynb)




 
For a description of the logic including a paper by B-Cubed partners, see both the [BIODIV OCCURRENCE CUBES](https://nilu365.sharepoint.com/:p:/r/sites/Horizon2021_CUBE/Shared%20Documents/WP2%20-%20use/uc_phytosociological_methods/OccurranceCubes.pptx?d=wac48820052f64cf3acb695d8028fb1d5&csf=1&web=1&e=cFh2VH) presentation as well as maybe the minutes of last summers [Occurrence Cube Meeting](https://docs.google.com/document/d/10Lr0tgb0QIFRu24z_y6RN8smYfgZnBAIseUq02TNpoo/edit#heading=h.yhiaa55j05g2)

And yes, the eternal problem with biodiv records as provided by GBIF is that while you have presence, you have no information on absence. Cubing doesn't help us (until the experts then generate full distribution)​


Yes. What comes from GBIF are observations. We already started to make distribution maps based on Dutch NDFF data. Thought we still need to work on improvements including more data and better documentation.
Isn't the dataset that GBIF provided one where they already processed the species observations into a grid using their cubing algorithm? Then it would be occupancy per grid cel, derived by a statistical method. I would then think that it is best to ingest it using the grid cells (10m) provided. We can later use this occupancy data for species distribution modelling in our use case.
Ok... let's stick with the 10m data for now. These can then simply be accumulate to a different grid sizes later. I have adapted the notebook so that it now writes 10m vector and raster data sets from the table data. However, I have noticed a small shift in the raster and therefore have to revise the script again.  have you had time to test this NB?
I was kinda waiting for Manual to signal that he finished the revision of the script first.
Sorry, closed it by accident …
No, problem.. please check the script and give me your feedback. 
the script is updated and can be tested here:
[https://github.com/FAIRiCUBE/uc1-urban-climate/blob/master/notebooks/dev/f06_mixed/GBIF_occurence_eea_grid.ipynb](url)
Thanks!!!
Here the URI in a clean form, the one above has the wrong URI underneath:
[https://github.com/FAIRiCUBE/uc1-urban-climate/blob/master/notebooks/dev/f06_mixed/GBIF_occurence_eea_grid.ipynb](https://github.com/FAIRiCUBE/uc1-urban-climate/blob/master/notebooks/dev/f06_mixed/GBIF_occurence_eea_grid.ipynb)
is this issue thereby resolved?