STAC Catalog vs. Collection
At present, we have 2 types of STAC Metadata within FAIRiCUBE:
- Dataset Metadata
- A/P Resource Metadata

Based on discussions with we're seeing 2 additional metadata types emerging, that we'd like to utilize STAC for:
- Knowledge Base Concepts
- Insights on external datasets (datasets UC partners have investigated but then not used due to deficits identified. As this information could be valuable to other UC requiring similar data, we're proposing)

Based on these different types, I'm wondering of the STAC Collection (or Catalog) concept wouldn't be valuable to group these 4 different metadata types.
Absolutely, it seems we'll have 4 STAC catalogs at the first level merged together by a single FiC STAC catalog to be searchable in the FiC catalog client via STAC API in STAC Browser. I don't think STAC collection would be the right concept on this level as we'd have to add common metadata.

FYI, for dataset metadata below the initial STAC catalog mostly STAC collections are used. I suppose that will be the same for the other three concepts.
Do I understand correctly that collections are only used when a dataset is only being expanded along one dimension (or similar concept. I know that a series of satellite images varies across 2 dimensions), catalogs for more general grouping?
Yes, that is also my understanding, Catalogs are for general grouping and Collections for Items or further Collections that share some commonality.
there seem to be general agreement, can the ticket be closed or are there any follow up action?
Closing with the agreement of the following catalogs for FAIRiCUBE:
- [Dataset Metadata](https://catalog.eoxhub.fairicube.eu/collections/index)
- [A/P ML Resource Metadata](https://catalog.eoxhub.fairicube.eu/collections/ML%20collection)
- [A/P non-ML Resource Metadata](https://catalog.eoxhub.fairicube.eu/collections/no-ML%20collection)

Potentially we will add additional catalogs for:
- Knowledge Base Concepts
- Insights on external datasets (datasets UC partners have investigated but then not used due to deficits identified. As this information could be valuable to other UC requiring similar data, we're proposing)