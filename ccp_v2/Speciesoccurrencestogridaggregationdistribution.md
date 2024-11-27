Species occurrences to grid aggregation/distribution
How could be species occurence data converted to grid?
- NDFF attributes interpretation
- Cells allignment to common grid
Bringing in some of the aspects from the original mail thread:

**Grids**
The EEA reference grid, based on the Lambert azimuthal equal-area projection (LAEA), has all sorts of resolutions, going up and down in powers of 10. So, in addition to the 1km grid deemed too coarse, there's a 100m and 10m versions (think also 1m), as well as 10.000m, 100.000m...

While you could theoretically use any grid, if you want to integrate the NL data with other datasets coming from Copernicus, you'd be well advised to use the LAEA grid (what Copernicus datasets are provided in), as resampling these grids to a different CRS can be an issue (Manuel and myself will do a presentation on the issues dependent on the content of the grid in the June Data Science Seminar (June 20th)

However, if the NL data is being provided via an NL specific grid, it may be more advantageous to use this (see reprojection issues)

**Aggregation metric/target**
I'm assuming that the goal is to have a numeric value in each cell representing the number of individuals. To my view, the problem is that especially birds don't stay in one place, the locations of the observations correlate more with the locations of the birdwatchers. In addition, the uncertainty provided with the datasets (reason you have a polygon in place of a point) should be taken into account. Some thoughts:
- I'd merge the uncertainty polygon with the movement range polygon, basically extend the uncertainty area by the distance once can expect the species to move around in normal life (not covering things like migration)
- I'd base the assignment of individual observations to grid cells on the percentage of the polygon. If the polygon is split 50:50 across 2 grid cells, each cell would have 0.5 individuals added. While this could appear strange as you'd have non-integer values, I believe this could more accurately indicate the actual location of the species in question.
- There have been suggestions to utilize Kriging here, I'm still a bit wary. To my understanding this approach is good for determining the potential area, but I worry that this approach will unduly reduce the number of individuals per cell.
> **Grids** The EEA reference grid, based on the Lambert azimuthal equal-area projection (LAEA), has all sorts of resolutions, going up and down in powers of 10. So, in addition to the 1km grid deemed too coarse, there's a 100m and 10m versions (think also 1m), as well as 10.000m, 100.000m... Do you have a link where I can download the 10m and 100m versions? Thanks
> **Aggregation metric/target** I'm assuming that the goal is to have a numeric value in each cell representing the number of individuals. To my view, the problem is that especially birds don't stay in one place, the locations of the observations correlate more with the locations of the birdwatchers. In addition, the uncertainty provided with the datasets (reason you have a polygon in place of a point) should be taken into account. Some thoughts:
> 
> * I'd merge the uncertainty polygon with the movement range polygon, basically extend the uncertainty area by the distance once can expect the species to move around in normal life (not covering things like migration) Do we have such movement range polygon per species?
> * I'd base the assignment of individual observations to grid cells on the percentage of the polygon. If the polygon is split 50:50 across 2 grid cells, each cell would have 0.5 individuals added. While this could appear strange as you'd have non-integer values, I believe this could more accurately indicate the actual location of the species in question.

The NDFF observations have a GPS accuracy polygon and a radius. Adding those usually will create a region much larger than a 10m grid cell. So an abundance of 1 will then be divided over many cells. Fine to do so but then we are not aggregating to grid cells, but distributing the occurrences. In the occurrence cubes paper this was done with a probability distribution, but then assigning all to 1 cell.

Either way, we need to decide on this before I can implement it :-) 


Adding info from Marian about the attributes in the NDFF data:

> ‘countsubject’ attribute refers to what is observed. It seems then that only “living animal” refers to animal observation. Though, when it’s ‘nest’ for example likely there will be more individuals in that year. We might assume/estimate how many it will be in average per different species. And still, we don’t know if that specific individual which belongs to nest was not observed again and is recorded elsewhere in the database. It’s actually the same for other observations as well. We could make similar assumptions or not, or simply taking values in abundance field.
>  
> Radius attribute refers to accuracy of measurement. We could use it and/or extend it and link it to abundance probability.
>  
> Full description of attributes is at https://woordenboek.ndff.nl/
>  We also still need to select which species data from NDFF to use. So far we have only the 3 datasets from 2016, of which 1 is plant species. Are we adding results from all three, or only use 1 file? on the **finer grids**, it's all just powers of 10, e.g. the 1km grid gets split into 10x10 100m cells, the 100m cells again get split into 10x10 10m cells.

**Aggregation metric/target - movement range polygon per species**: do we even have a list of species being targeted?

**‘countsubject’ attribute**: cannot parse the Dutch documentation, we'd need a readable description of the relevant fields. 
> > **Grids** The EEA reference grid, based on the Lambert azimuthal equal-area projection (LAEA), has all sorts of resolutions, going up and down in powers of 10. So, in addition to the 1km grid deemed too coarse, there's a 100m and 10m versions (think also 1m), as well as 10.000m, 100.000m...
> 
> Do you have a link where I can download the 10m and 100m versions? Thanks Here you could download it per country: https://www.eea.europa.eu/data-and-maps/data/eea-reference-grids-2
> > > **Grids** The EEA reference grid, based on the Lambert azimuthal equal-area projection (LAEA), has all sorts of resolutions, going up and down in powers of 10. So, in addition to the 1km grid deemed too coarse, there's a 100m and 10m versions (think also 1m), as well as 10.000m, 100.000m...
> > 
> > 
> > Do you have a link where I can download the 10m and 100m versions? Thanks
> 
> Here you could download it per country: https://www.eea.europa.eu/data-and-maps/data/eea-reference-grids-2

Thanks. That's the link I used to get the 1km version, which is the most detailed. So then we have to create our own 10m grid in the same CRS and assign our own grid cell codes I guess.





> Bringing in some of the aspects from the original mail thread:
> 
> **Grids** The EEA reference grid, based on the Lambert azimuthal equal-area projection (LAEA), has all sorts of resolutions, going up and down in powers of 10. So, in addition to the 1km grid deemed too coarse, there's a 100m and 10m versions (think also 1m), as well as 10.000m, 100.000m...
> 
> While you could theoretically use any grid, if you want to integrate the NL data with other datasets coming from Copernicus, you'd be well advised to use the LAEA grid (what Copernicus datasets are provided in), as resampling these grids to a different CRS can be an issue (Manuel and myself will do a presentation on the issues dependent on the content of the grid in the June Data Science Seminar (June 20th)
> 
> However, if the NL data is being provided via an NL specific grid, it may be more advantageous to use this (see reprojection issues)
> 
> **Aggregation metric/target** I'm assuming that the goal is to have a numeric value in each cell representing the number of individuals. To my view, the problem is that especially birds don't stay in one place, the locations of the observations correlate more with the locations of the birdwatchers. In addition, the uncertainty provided with the datasets (reason you have a polygon in place of a point) should be taken into account. Some thoughts:
> 
>     * I'd merge the uncertainty polygon with the movement range polygon, basically extend the uncertainty area by the distance once can expect the species to move around in normal life (not covering things like migration)
> 
>     * I'd base the assignment of individual observations to grid cells on the percentage of the polygon. If the polygon is split 50:50 across 2 grid cells, each cell would have 0.5 individuals added. While this could appear strange as you'd have non-integer values, I believe this could more accurately indicate the actual location of the species in question.
> 
>     * There have been suggestions to utilize Kriging here, I'm still a bit wary. To my understanding this approach is good for determining the potential area, but I worry that this approach will unduly reduce the number of individuals per cell.

In my view establishing EEA grid which is based on LAEA projection is important in poisitional point of view i.e. where are cell boundaries located (how are shifted), so certain studies or datasets could refer to these cells which has unique IDs.

LAEA is most suitable for mapping in continental scale. Countries then established their national grids for better representation of surface in small scale (to have lowest distortions as possible). It might make sense maybe to do any data operations  on national grid (dutch RD) since most available data using it and then reffer to EEA grid with final product e.g. biodiversity index.
> > > > **Grids** The EEA reference grid, based on the Lambert azimuthal equal-area projection (LAEA), has all sorts of resolutions, going up and down in powers of 10. So, in addition to the 1km grid deemed too coarse, there's a 100m and 10m versions (think also 1m), as well as 10.000m, 100.000m...
> > > 
> > > 
> > > Do you have a link where I can download the 10m and 100m versions? Thanks
> > 
> > 
> > Here you could download it per country: https://www.eea.europa.eu/data-and-maps/data/eea-reference-grids-2
> 
> Thanks. That's the link I used to get the 1km version, which is the most detailed. So then we have to create our own 10m grid in the same CRS and assign our own grid cell codes I guess.

Yes, we could resample it to 10m. If it's needed, we can assign ID codes to cells. But also, once we have 10 m grid based on EEA, we could reproject it to dutch RD and make other data processing.
> > **Aggregation metric/target** I'm assuming that the goal is to have a numeric value in each cell representing the number of individuals. To my view, the problem is that especially birds don't stay in one place, the locations of the observations correlate more with the locations of the birdwatchers. In addition, the uncertainty provided with the datasets (reason you have a polygon in place of a point) should be taken into account. Some thoughts:
> > 
> > * I'd merge the uncertainty polygon with the movement range polygon, basically extend the uncertainty area by the distance once can expect the species to move around in normal life (not covering things like migration)
> 
> Do we have such movement range polygon per species?

I'm not aware about any, but it might be anyway linked to probability distribution maps which exists.
> We also still need to select which species data from NDFF to use. So far we have only the 3 datasets from 2016, of which 1 is plant species. Are we adding results from all three, or only use 1 file? These are 3 datastes we have first to experiment to see how is database organized and if tere are many data gaps in spatial context. Then we can get more data to analyse further. 
Careful on reprojecting! While this seems quite easy, just call the required function in GDAL or GIS tool, you really don't know how it's reprojecting. As your data deals with the count of individuals in a cell, I'm pretty sure that most out-of-the-box reprojections will provide wrong values.

This is why I'm pushing you to decide on the grid early on, as merging different grids is quite an issue.

Counter-question: how much of your NL data is only available on a national grid?
I am inclined to think that for FAIRiCUBE we need to select a common grid for *all* data cubes (including the existing satellite data etc.), so that the data is at least analysis ready with respect to CRS transformations. Per use case (and data source) a cell size then has to be selected (within that grid) that best represents the characteristics of the data. Or a good way to distribute said data over cells (keeping the distribution info). Ultimately (after aggregation or disaggregation) these will be the features and labels for the machine learning, and should thus be suitable for that, without too much user effort or chance of introducing errors (e.g. because of cell misalignment).
Reason we'd agreed last fall to focus on the LAEA EPSG 3035 European Grid :)

To my view, the only reason for a UC to select an alternative grid would be if some of the source data is already provided in this alternative grid.
Yes, more likely that datasets have a best CRS for storage (probably the 'native' CRS), a best one for computation/analysis (likely an Equal Area one matching the scale of the data), and good one for appealing data visualisation. And then it becomes a trick for the user to keep data properly aligned and not accumulate transformation errors.
My impression is that the different grids (and CRS) come from the source communities providing the data. Examples:
- Things like elevation try to provide very exact geo-coordinates, thus use a geodetic grid based on WGS84 or more exact local CRS
- Generated European datasets (e.g. Copernicus, Eurostat) aim to provide a consistent European perspective, thus utilize the projected LAEA EPSG 3035 European Grid
- Local/National datasets use either geodetic or projected grids based on local CRS

Transforming between these grids is tricky as one needs to understand the nature of the grid content before one can begin to consider how to reproject. The danger is that the data is reprojected regardless, leading to serious errors (details at the June Data Science Seminar)

Our problem is that we're using data across various sources.
Maybe we have to provide some kind of guidelines about map projections and what are sensible (and non-sensible) transformations. Something along the lines of this overview from ESRI, but then more specific to gridded data cubes:
[Quick_Notes_on_Map_Projections_in_ArcGIS_nov2019.pdf](https://github.com/FAIRiCUBE/uc2-agriculture-biodiversity-nexus/files/11535735/Quick_Notes_on_Map_Projections_in_ArcGIS_nov2019.pdf)

I've now found information (unfortunately only in German) on floristic grids, the following papers are on TEAMS:

- [Bericht über die Kartierung der Flora Mitteleuropas](https://nilu365.sharepoint.com/:b:/r/sites/Horizon2021_CUBE/Shared%20Documents/WP2%20-%20use/uc_phytosociological_methods/literature/niklfeld%201971%20floristische%20kartierung%20taxon%2020%20545-571.pdf?csf=1&web=1&e=D3l58p): 1971, explanation of the central EU grid, I believe based on WGS84, uses either 3'x5' or 6'x10' cells
- [Zum Stand der Kartierung der Gefäßpflanzen-Flora Mitteleuropas](https://nilu365.sharepoint.com/:b:/r/sites/Horizon2021_CUBE/Shared%20Documents/WP2%20-%20use/uc_phytosociological_methods/literature/Niklfeld+Wittig%202012.pdf?csf=1&web=1&e=mgAkW1): 2012, overview of the various floristic grids in Europe. From what I see, while central EU uses the 3'x5' or 6'x10' cells, northwestern EU works on a projection, grid cell sizes given in km.

Not sure this makes our lives easier, but we definitely should be aware. For the NHM Plant Community UC, the decision will probably depend on which grid is used to provide distribution of these plant communities. and I had a good brainstorming session on Occurrence Cubes, [current approach on TEAMs](https://nilu365.sharepoint.com/:p:/r/sites/Horizon2021_CUBE/Shared%20Documents/WP2%20-%20use/uc_phytosociological_methods/[OccurranceCubes.pptx](https://nilu365.sharepoint.com/:p:/r/sites/Horizon2021_CUBE/Shared%20Documents/WP2%20-%20use/uc_phytosociological_methods/OccurranceCubes.pptx?d=wac48820052f64cf3acb695d8028fb1d5&csf=1&web=1&e=NnLcOz)?d=wac48820052f64cf3acb695d8028fb1d5&csf=1&web=1&e=NnLcOz)

Happy to discuss! Did you reach any conclusions? The slides are titled 'thoughts in progress', any ideas on next steps to move forward with this? I am still trying to figure out the characteristics of biodiversity (species distribution) data and how to properly process it to get sensible features for machine learning. sorry, had forgotten to add some of the updates, now done!
For context, there's additional information in the UC description, [Validation of Phytosociological Methods through Occurrence Cubes](https://nilu365.sharepoint.com/:w:/r/sites/Horizon2021_CUBE/Shared%20Documents/WP2%20-%20use/uc_phytosociological_methods/Validation%20of%20Phytosociological%20Methods%20through%20Occurrence%20Cubes.docx?d=w3f0fea355db74a51bdfa5202cb8802b8&csf=1&web=1&e=IYTG5w)

I'll move both docs to GitHub shortly, update the links here. Can you have a look as well and check for synergies between use cases? Thanks, so it seems similar. Do we have any argumentation (or can we write it down) why the proportional attribution of species to grid cells is 'better' than the probabilistic assignment to a single grid cell? Or in which cases it is a more appropriate solution? initial thoughts were triggered by an unease that the probabilistic approach can lead to an individual being assigned to the wrong cell (and conversely be missing in the correct cell, causing issues with further processing/ML stuff). As we don't know in which of the cells the individual lies, seems fairer to assume all potential cells. Adding an entire individual per cell would lead to wrong aggregate counts; providing the proportion of the disc overlapping the cell keeps aggregates clean.

I'd first thought about plant species (due to long association with Heimo), but regarding animals, adding the usual range for the species provides a far better picture of where they really are (see the example with the birds on the water. While they're more often spotted there as they're easier to see, they surely nest on land in the vicinity of the water)

Where I'm still a bit worried are protected species. Even if we take a larger radius ("Protection radius" in the ppt), if there's only one individual close to an edge, one can pinpoint it by the percentages in the individual grid cells overlapped by the disc. One could consider utilizing a more irregular geometry for this.

I agree that something is needed for protected species, however I think we also need a solution that keeps the data suitable for machine learning. If we fuzzy up the occurrences too much, the data will become too noisy for ML to make sense of. 
So lets leave the protected species issue to the side for the moment, agree on the rest of the cubing.

You have a far better grasp of data and ML, requirements to the nature of the data being utilized. How do you see the meaningfulness of the 2 approaches?
Ultimately it depends on the specific (ML) task the data is used for.

However, the data always needs to be a good representation of reality (I leave that to the domain experts). And since the created occurrence cubes will be the input for feature engineering or feature learning the approach that keeps the most information from the original (occurrences) data should be preferred I think.

The proportional assignment to cells seems more realistic wrt species distribution, and keeps more information that can be used to engineer the features (including labels) needed for the ML task at hand.
For completeness, here the link to the minutes of the Occ Cube meeting in Vienna August 10-11. Covers several of the topics above:
https://docs.google.com/document/d/10Lr0tgb0QIFRu24z_y6RN8smYfgZnBAIseUq02TNpoo/edit#heading=h.yhiaa55j05g2