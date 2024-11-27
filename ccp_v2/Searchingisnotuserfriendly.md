Searching is not user-friendly
The [search interface](https://catalog.eoxhub.fairicube.eu/search) could definitely be improved with more user-friendly defaults:

1. Do partial matching by default, as it is you need to wrap the search term with % in order to find things (and this only works for Title which is hidden in Additional filters). Exact match is usually done explicitly with double quoting the search terms
3. Case-insensitive by default
4. Support for full-text search that matches everything in the catalog entries; this could be put on top as the first field
5. Temporal/spatial filtering should be lower priority, maybe down in the Additional filters

cc 
Agreed with all points above, aligned with what we've been requesting from EOX. See Minutes of the meeting [2024_06_11 : general overview and discussion on data ingestion and provisioning](https://nilu365.sharepoint.com/sites/Horizon2021_CUBE/SiteAssets/FAIRiCUBE%20Notebook/meetings_MoM.one#2024_06_11%20%20general%20overview%20and%20discussion%20on%20data&section-id={795821E2-7BFB-497F-A691-0625BBF2E405}&page-id={C2112C03-E712-4BC5-B344-7B43261D22B0}&end)

Additional search functionality I'm just seeing in a different project on soil data, they allow for search based on data source, e.g. INSPIRE, Copernicus, etc. This seems like a useful functionality. Wondering if an additional metadata field for this purpose would be useful. am I correct that there is no formal specification on catalog functionality?

the search had indeed been identified as a real bottleneck. a first "easy" solution is to change the layout of the search so that "filters" come up first, afterwards time and space extend appear. further, searchable filter items should go beyond the 3 current items (id, title, acquired). it might not cover all meta data fields but an agreed subset of the most important ones. finally, if we cannot have partial search but rely on "%" characters, we should give an example "%no2%" ...