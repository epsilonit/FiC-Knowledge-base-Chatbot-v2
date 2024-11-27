Describing external datasets
When describing external datasets used within FAIRiCUBE, but not ingested to FAIRiCUBE infrastructure as accessed via APIs, we have slightly different requirements to the cardinalities.

Two examples:
[ERA5_global_climate_data](https://github.com/FAIRiCUBE/data-requests/pull/272/files): simplified approach, no info in Bands, relevant variables provided as keywords
[SPARTACUS_v2.1](https://github.com/FAIRiCUBE/data-requests/pull/273/files): basic information provided in bands, temporal resolution further described in assets

From this experience, for external datasets, we need to relax the requirement for provision of information on bands. However, we need to make sure that for internal data, band information is always provided (otherwise it's just an empty grid)
related to #13 
Add to documentation that users can simplify this when describing an external resource