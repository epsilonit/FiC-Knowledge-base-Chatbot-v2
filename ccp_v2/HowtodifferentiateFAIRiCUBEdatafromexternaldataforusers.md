How to differentiate FAIRiCUBE data from external data for users?
Currently, the catalog lists both local datasets and those form outside (CoperniCUBE, Sentinel Hub, etc.) which have been made available in addition. This begs for a means that users can see what the project's own data are. A further distinction is between data provided by rasdaman vs EOxHub.

Here is a first slate of ideas how this could be implemented:

- separate catalogs
 - con: no integrated view for users
- STAC collections for hierarchically grouping 
 - pro: just 1 catalog, remains easy for users
 - can be exposed to users conveniently, eg, with tick boxes at top of search page to select the desired data pools 

Thoughts? please provide your requirements, based on individual STAC elements, under the issue on [FAIRiCUBE catalogue client functionality](https://github.com/FAIRiCUBE/FAIRiCUBE-Hub-issue-tracker/issues/5)