FAIRiCUBE catalogue client  functionality
Minimum requirements: 

- Filter by spatial window 

- Advanced search with more than one field  (e.g. give me data from publishing year 2020 and/or that has 10 m resolution )

- Anything else`? 


Current FiC catalogue was readily available without any efforts but does not seem to have more than one search field 

Test STAC browser client deployed by radiant earth  https://radiantearth.github.io/stac-browser/#/external/catalog.fairicube.eu/stac/index.json?.language=en 

- Also has only one search field 

- Can this be configured to provide more? 

- Do we have another STAC browser example that shows what is possible (that we can take as reference)? 

- Has a filter for spatial windows 


[ open] Future Catalogue = STAC Browser?! 

- Has been agreed on  several times 

- Does it fulfill all the minimum requirements? 
Once we harvest the static STAC files and add STAC API support we can enable more advanced search in STAC Browser. There are plenty of examples at https://radiantearth.github.io/stac-browser/#/ like for example https://radiantearth.github.io/stac-browser/#/search/external/earth-search.aws.element84.com/v1/ or 
https://radiantearth.github.io/stac-browser/#/search/external/api.terrabyte.eox.at/public/stac/
see also: https://github.com/FAIRiCUBE/FAIRiCUBE-Hub-issue-tracker/issues/10#
- please check out this instantiation of STAC-Browser, it has TOI, AOI searches and extra fileds (which, I assume for now, are configurable): 
   - https://radiantearth.github.io/stac-browser/#/search/external/api.terrabyte.eox.at/public/stac/
any update on the stac browser installation and this issue ticket?
FAIRiCUBE stac-fastapi Catalog, is now available with the long awaited extended Search functionality. At the moment still limited to the Title.
(Add filter -> Title -> (=)button -> matches (case-insensitive) /matches (case-sensitive)
https://catalog.eoxhub.fairicube.eu/ thanks! Works! :)
Fear I'm still running on 1-2 neurons ;) When will it be possible to search by additional fields? At present, still limited to the basis bbox and ID, with additional fields we can add the title and acquisition date.
Where have we defined which fields will be searchable?
Maybe a topic for the planned workshop with UC partners on MD requirements? When can we expect search on additional fields? This deficit is causing ever more problems across the project

In addition, making this a bit more user friendly would be most appreciated, seems I'm not the only one who had to learn to use % as a wildcard here any updates here? Just checked the catalog, didn't see any progress.
Btw - from what I'm seeing in the EOX-Dashboard presentation, seems like far more catalog search options are available there, any way to leverage for FAIRiCUBE? Clarification regarding eodash search:   

- eodash loads the high level catalog and the search is performed locally. 
- it therefore provides summarized information at a collection level  and is not very good to go to item level (too slow)
- so its not an ready-to-use replacement for the `query-extension` or the future `full-text-search` extension