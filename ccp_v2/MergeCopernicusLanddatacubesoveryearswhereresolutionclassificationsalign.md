Merge Copernicus Land datacubes over years where resolution/classifications align
Currently the Copernicus Land data is ingested as individual datacubes per year + virtual coverages providing an aggregated view suitable for map display via WMS.

For the years where the data spatially and in classification aligns, they should be merged into single 3D time-series datacube.

First step is to determine and report in this issue which years for which data can be merged. The datacubes to be checked:

- dominant_leaf_type
- eu_demography
- forest_type
- grassland_status
- imperviousness
- small_woody_features
- tree_cover_density
- water_and_wetness
For [dominant_leaf_type](https://land.copernicus.eu/en/products/high-resolution-layer-dominant-leaf-type) there is
- Status layer for the 2012 and 2015 reference years at 20 m resolution
- Status layer for the 2018 reference year at 10 m resolution

Currently we have datacubes: `dominant_leaf_type_2012`, `dominant_leaf_type_2015`, `dominant_leaf_type_2018`.

According to this issue we should have two datacubes. How should they be named?

1. `dominant_leaf_type_2012_2015` and `dominant_leaf_type_2018`, or
2. `dominant_leaf_type_20m` and `dominant_leaf_type_10m`

The first is immediately clear about the temporal extent (not a big point given the catalog), while the second allows to extend `dominant_leaf_type_10m` with more data when it's published (probably in same resolution). any thoughts on this? to my view, the temporal information should be available in the catalog and coverage object - if there are issues here, please highlight!!!

As for the options, think you provide the solution in your last statement `the second allows to extend dominant_leaf_type_10m with more data when it's published (probably in same resolution).` To my understanding, the next Copernicus datasets will be at 10m, so think the titles with the resolution gives us more flexibility moving forward!
The temporal information is of course in the coverage object metadata, just not in the coverage ID.

So here are the changes in coverage IDs:

| Old datacube(s)                                                                                                                                    | New datacube(s)                                                                                                                     |
| -------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------- |
| `dominant_leaf_type`,<br>`dominant_leaf_type_2012`,<br>`dominant_leaf_type_2015`,<br>`dominant_leaf_type_2018`                                     | `dominant_leaf_type_10m` (2018),<br>`dominant_leaf_type_20m` (2012, 2015)                                                           |
| `forest_type`,<br>`forest_type_2012`,<br>`forest_type_2015`,<br>`forest_type_2018`                                                                 | `forest_type_10m` (2018),<br>`forest_type_20m` (2012, 2015)                                                                         |
| `grassland_status`, <br>`grassland_status_2015`, <br>`grassland_status_2018`                                                                       | `grassland_10m` (2018),<br>`grassland_20m` (2015)                                                                                   |
| `imperviousness`,<br>`imperviousness_2006`,<br>`imperviousness_2009`,<br>`imperviousness_2012`,<br>`imperviousness_2015`,<br>`imperviousness_2018` | `imperviousness_10m` (2018),<br>`imperviousness_20m` (2006, 2009, 2012, 2015)                                                       |
| `small_woody_features_2015`,<br>`small_woody_features_2018`                                                                                        | `small_woody_features_2015`,<br>`small_woody_features_2018`<br>(no change, same resolution 5m but different pixel value categories) |
| `tree_cover_density`,<br>`tree_cover_density_2012`,<br>`tree_cover_density_2015`,<br>`tree_cover_density_2018`                                     | `tree_cover_density_10m` (2018),<br>`tree_cover_density_20m` (2012, 2015)                                                           |
| `water_and_wetness`,<br>`water_and_wetness_2018`                                                                                                   | `water_and_wetness_10m` (2018),<br>`water_and_wetness_20m` (2015)                                                                   |
| `european_settlement_map` (2015)                                                                                                                   | `european_settlement_map_2015`,<br>`european_settlement_map_2018`<br>(same resolution 2m but different pixel value categories)      |
| `corine_land_cover`                                                                                                                                | (no change)                                                                                                                         |
| `eu_demography`,<br>`eu_demography_2018`,<br>`eu_demography_2021`                                                                                  | `eu_demography`  (2006, 2011, 2018, 2021)                                                                                                             |
| `LGN`                                                                                                                                              | (no change)                                                                                                                         |

The proposed changes have been implemented, and I sent out a notification to [fairicube_all] about it announcing the grace period for migrating to the new datacubes.