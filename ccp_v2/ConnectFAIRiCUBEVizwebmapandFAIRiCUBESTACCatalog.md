Connect FAIRiCUBEViz web map and FAIRiCUBE STAC Catalog 
I propose the following changes to the Catalog Editor, so that the FAIRiCUBEViz Web Map can use the STAC Catalog to discover and display Assets tagged for visualization:

- [ ] Enable using the keywords `overview` and `metadata` in the `roles` property of an Asset. At the moment, only `data` and `thumbnail` are used. It can be implemented in the Catalog Editor as a dropdown or checkboxes field named "Role" in the Asset section.
- [ ] ~Add metadata fields for band statistics, i.e. `raster:histogram`, as specified by the raster extension https://github.com/stac-extensions/raster.~
- [ ] Change the API Section title from "API" to "Additional resources" to generalize the use for external links (e.g. link to the web map), and to be consistent with the STAC Browser.


### Background

FAIRiCUBEViz is a Web Map to display gridded and vector data ingested in FAIRiCUBE (hence registered in the Catalog). The scope is to show progress and results of a data analysis pipeline. The data creator (data scientist) has control on which data is displayed, and how (e.g. color palette). We have a working prototype here: https://fairicube-tiled-map-test-dmor-f3c68074eac69722d13ff0724adbba03a.pages.nilu.no/ol/. It can display COG files and vector data from an object storage.

Currently, additional information and files needed for the rendering are stored alongside the main file. Also, the available datasets are simply hardcoded in the UI. 

This auxiliary information can (and indeed should!) be part of the STAC Item, and auxiliary files registered as Assets, to allow FAIRiCUBEViz to leverage the STAC APIs to discover the data to display and get the relevant information.


Some changes in the Catalog Editor are necessary. The most important one is to allow more keywords in the "roles" property of an Asset. At the moment, only "data" and "thumbnail" are used. Having a dedicated role to tag the dataset Asset for visualization allows FAIRiCUBEViz to dynamically discover and display the relevant data. Looking at the STAC Best Practices https://github.com/radiantearth/stac-spec/blob/master/best-practices.md#asset-roles, the role "overview" is the closest to our use case.


Another aspect is how to include auxiliary information used for rendering, namely the color palette information, an extra .txt file, and the band statistics, a PAM *.aux.xml file (the output of gdalinfo). The quickest solution I see is to make use of the Asset "roles" property: Add any additional file as an Asset with the role "metadata". The most STAC-like way is to use the exploit the existing stac extensions: For the band statistics, the raster stac-extension https://github.com/stac-extensions/raster already provides the relevant fields. I haven't found anything suitable for the color palette.

   
Finally, we need a way to link the STAC Browser to the FAIRiCUBEViz: Mussab created the API section in the Catalog Editor to add links under the section "Additional resources" of the Browser. I think that this already perfect for this use! Therefore I suggest to simply change the naming from "API" to "Additional resources" in the Editor.
Updated todo list:

- [ ] Enable using the keywords `overview` and `metadata` in the `roles` property of an Asset as a dropdown field named "Role" in the Asset section.
- [ ] Change the API Section title from "API" to "Additional resources"
- [ ] Automatically generate link to data visualization `https://vis.fairicube.eu/=data?<overview_url>` as Additional Resource link