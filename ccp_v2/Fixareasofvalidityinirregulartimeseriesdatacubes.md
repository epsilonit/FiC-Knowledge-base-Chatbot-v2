Fix areas of validity in irregular timeseries datacubes
The areas/periods of validity of irregular time slices are incorrect on some of the datacubes.

- `corine_land_cover` see table under Technical Summary [here](https://land.copernicus.eu/en/products/corine-land-cover): 1986-1998, 2000 +/- 1 year (1999-2001), 2006 +/- 1 year (2005-2007), 2011-2012, 2017-2018
  - I assume that the intervals include both the start and end year, e.g. 2017-2018 = 2017-01-01 - 2018-12-31.
 
- These should have **one year validity** for each time slice: `LGN, dominant_leaf_type, eu_demography, forest_type, european_settlement_map, grassland_status, imperviousness, small_woody_features, tree_cover_density, water_and_wetness`
`dominant_leaf_type` has +/- 1 year temporal extents:
- [2012](https://sdi.eea.europa.eu/catalogue/copernicus/api/records/5afeffa4-ccda-4ef9-a7ef-637cb7310f58?language=all)
- [2015](https://sdi.eea.europa.eu/catalogue/copernicus/api/records/47e32c1d-f025-4622-934a-f1b63572609f?language=all)
- [2018](https://sdi.eea.europa.eu/catalogue/copernicus/api/records/7b28d3c1-b363-4579-9141-bdd09d073fd8?language=all)
Correction: 2018 has a different temporal validity:

- Begin date: 2018-03-01 
- End date: 2018-10-31 

In the user manual it's listed as "Reference year: 2018 (March to October)". On the [Copernicus Land page](https://land.copernicus.eu/en/products/high-resolution-layer-dominant-leaf-type/dominant-leaf-type-2018) it is just 2018.

I think it may be best to define it as 2018 (full year) in the service to avoid confusion when subsetting?
- `forest_type` and `tree_cover_density` - same as `dominant_leaf_type`
- `imperviousness` has +/- 1 year for all years ([2006](https://land.copernicus.eu/en/products/high-resolution-layer-imperviousness/imperviousness-density-2006), [2009](https://land.copernicus.eu/en/products/high-resolution-layer-imperviousness/imperviousness-density-2009), [2012](https://land.copernicus.eu/en/products/high-resolution-layer-imperviousness/imperviousness-density-2012), [2015](https://land.copernicus.eu/en/products/high-resolution-layer-imperviousness/imperviousness-density-2015), [2018](https://land.copernicus.eu/en/products/high-resolution-layer-imperviousness/imperviousness-density-2018))
- `grassland_status` 2014-2016 for [2015](https://land.copernicus.eu/en/products/high-resolution-layer-grassland/grassland-2015), 2018-03 - 2018-10 for [2018](https://land.copernicus.eu/en/products/high-resolution-layer-grassland/grassland-2018)
- `small_woody_features` has +/- 1 year for all years ([2015](https://land.copernicus.eu/en/products/high-resolution-layer-small-woody-features/small-woody-features-2015), [2018](https://land.copernicus.eu/en/products/high-resolution-layer-small-woody-features/small-woody-features-2018))
- `water_and_wetness` - 2012-2018 for [2018](https://land.copernicus.eu/en/products/high-resolution-layer-water-and-wetness/water-and-wetness-status-2018), 2009-2015 for [2015](https://land.copernicus.eu/en/products/high-resolution-layer-water-and-wetness/water-and-wetness-status-2015)
  - ~~this is an interesting case because the temporal extents overlap. I think the correct way to model it is with two separate datacubes. For a single datacube it would be necessary to remove the overlap.~~
  - edit: it has to be separate datacubes anyway because the thematic classes are different.
- `european_settlement_map` - 2014-2016 for [2015](https://www.eea.europa.eu/en/datahub/datahubitem-view/42783031-34d5-4b38-b9ba-a0783723053c)
- `LGN`, `eu_demography` - could not find explicit specification = yearly validity MANY THANKS for going into the details of the dataset descriptions!

On the temporal overlap on `water_and_wetness`, I wonder if it would make sense to get in touch with Copernicus to clarify this, I'm wondering if the validity 2012-2018 for 2018 is correct or a typo

I noticed that on dominant_leaf_type, there are now additional datasets _10m and _20m - how do these relate to the rest? _20m=2012&2015, _10m=2018?

I wanted to take a closer look at the source for Demography, but noticed that the link is no longer in the [MD record](https://github.com/FAIRiCUBE/data-requests/pull/276/files). Did notice that there seems to be a 2024 update.
For `water_and_wetness` I doubt it could be a typo, but feel free to contact them if you think it could be, or dig around the publications. In any case I think it's not our task to validate whether the data publishers made mistakes.

The _10m and _20m are new datacubes with the proposed scheme where datacubes are temporally grouped by same resolution or other properties. They are meant to replace the previous datacubes. If we agree this scheme makes sense, I'd announce that there are new datacubes and give people a month or so to migrate to using the new ones.

I already spent a ton of time trying to track down the download location for eu_demography. Besides [the issue you linked](https://github.com/FAIRiCUBE/data-requests/pull/276/files), there's also [this one](https://github.com/FAIRiCUBE/data-requests/pull/260/files) with a bit more details. I've no idea what's going on with the reverts, maybe can shed some light. Ideally I'd like to see the original request for this data, as it is it is impossible to verify if the [download page](https://ec.europa.eu/eurostat/web/gisco/geodata/population-distribution/geostat) I found is correct.
> I already spent a ton of time trying to track down the download location for eu_demography. Besides [the issue you linked](https://github.com/FAIRiCUBE/data-requests/pull/276/files), there's also [this one](https://github.com/FAIRiCUBE/data-requests/pull/260/files) with a bit more details. I've no idea what's going on with the reverts, maybe can shed some light. Ideally I'd like to see the original request for this data, as it is it is impossible to verify if the [download page](https://ec.europa.eu/eurostat/web/gisco/geodata/population-distribution/geostat) I found is correct.

Meanwhile I figured out the eu_demography data and updated the catalog data request for review.

This issue can be closed in my opinion, please reopen if you think there's still problems.