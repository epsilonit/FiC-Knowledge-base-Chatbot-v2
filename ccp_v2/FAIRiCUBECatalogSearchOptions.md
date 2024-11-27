FAIRiCUBE Catalog Search Options
At present, the [FAIRiCUBE Catalog](https://catalog.fairicube.eu/) only provides one search field. This has various drawbacks:
- as the user isn't aware of the concepts available in the metadata, they may not even know what type of keywords should be entered
- unclear how multiple terms in the search field are dealt with
- impossible to query for spatio-temporal intervals

While I appreciate the usefulness of such a simplified search for some use cases, I believe we'll also need an expert search, clearly exposing the individual metadata fields, ideally providing the user with available content for each field (e.g., a list of the types of license provided under the license field)

This requirement is for both dataset and processing resource metadata 
Agreed, the current catalog is fairly limited as it is a static catalog and the search is a pure client side one. Our idea is to harvest the static catalog into a tool providing API support e.g. PyCSW or STAC-FastAPI that can be used in clients. while an API would be most welcome for FAIRiCUBE metadata, thinking back to our core objective of making all this data cube magic more graspable for mere mortal humans, I do believe something like a GUI would also be required
sure, once we have the API following the STAP API spec we can use any STAC client in front of it have you identified a STAC Client we'll use within FAIRiCUBE Hub?
we made good experience with STAC Browser (https://github.com/radiantearth/stac-browser) and will explore usage for FAIRiCUBE. The current static FAIRiCUBE catalog can be explored in STAC Browser at https://radiantearth.github.io/stac-browser/#/external//catalog.fairicube.eu/stac/?.language=en on the radiantearth STAC Browser, can this also do search on specific fields, or just the search for title? Especially on the spatiotemporal aspects, we'd need to be able to provide intervals. I'd really like to see an example with more than one generic search field (actually, I'd really like to see this deployed for FAIRiCUBE! M7: FAIRiCUBE HUB in operation is due 30.06.2023)

In addition, I'm now totally confused as to the datasets described by the FAIRiCUBE catalog:
- [FAIRiCUBE Catalog](https://catalog.fairicube.eu/): 89 Datasets
- [Radiant Earth FAIRiCUBE](https://radiantearth.github.io/stac-browser/#/external//catalog.fairicube.eu/stac/?.language=en): 62 Datasets
- [Requested via Inventory](https://github.com/orgs/FAIRiCUBE/projects/1/views/1): 54 Datasets, whereby only a few have been imported to either rasdaman or EOX

As both catalog versions you point me to have more datasets than requested, I'm pretty sure that the current content have more to do with what EOX has available on EuroDataCube, less with what the UC have requested
As for now, the important question for me is: will the current https://catalog.fairicube.eu/ stay or will it go! 

this depends on what requirements this catalogue has (can only read yaml files is a no-go) and how much we can configure it to provide us with a GUI that we need (with more then one search field). : when will this decision been made? I read that radiant STAC Browser is an alternative (?) under consideration? our current plan is to deploy STAC Browser connected to STAC API which holds the static STAC JSONs harvested from https://fairicube.github.io/resource-metadata/ and https://fairicube.github.io/data-requests/. STAC API allows us to do more advanced searches in the client (see https://github.com/FAIRiCUBE/FAIRiCUBE-Hub-issue-tracker/issues/5#issuecomment-1580835504). Which URL we use for this deployment can still be discussed, we can either replace the existing one or add another one in addition.

Cc 
see also: https://github.com/FAIRiCUBE/FAIRiCUBE-Hub-issue-tracker/issues/5
- please check out this instantiation of STAC-Browser, it has TOI, AOI searches and extra fileds (which, I assume for now, are configurable): 
   - https://radiantearth.github.io/stac-browser/#/search/external/api.terrabyte.eox.at/public/stac/
Thanks for the example

Question on the configuration options of the radiantearth browser - can one explicitly expose individual concepts, or can they only be offered under "additional filters"?
FAIRiCUBE stac-fastapi Catalog, is now available with the long awaited extended Search functionality. At the moment still limited to the Title. 
(Add filter -> Title -> (=)button ->  matches (case-insensitive) /matches (case-sensitive) 
[https://catalog.eoxhub.fairicube.eu/](https://catalog.eoxhub.fairicube.eu/)  cool!
Just tried, but seem to have failed. First checked the existing datasets, see that there's one with the title "Water Bodies". Did a case insensitive search for "water", got 0 results
:?

![grafik](https://github.com/FAIRiCUBE/FAIRiCUBE-Hub-issue-tracker/assets/11915304/3858156d-fdfb-4700-8028-45794166b19f)

Please also read the instructions given below the search field!