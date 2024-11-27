Unifying and completing our FAIRiCUBE data catalog
we have several (federated, external) data resources:

https://fairicube.rasdaman.com/
and
https://www.sentinel-hub.com/
https://eurodatacube.com/

that only partially or don't show up in our FAIRiCUBE data catalogue
https://catalog.eoxhub.fairicube.eu/collections/index

Instead I find some confusing Catalogs titles, 2 of the Catalogs are empty.

From a user perspective, I would like to find all the catalogs (even if the metadata is unverified) under Catalogs items and then the data data FiC had ingested, following our ingestion pipeline through the WebGUI https://catalog-editor.eoxhub.fairicube.eu/

Once we have transferred our data inventory items to GitHub PR and following the reviewer approval, they will automatically show up in our catalog under Data Request catalog. 

How can we integrate the other [external] data resources, what are the technical requirements to make that possible?

It would be interesting to understand what descriptive information is available in these additional catalogs, is there information beyond the basic spatio-temporal bbox that could be queried? To my view, this reduction of searchable metadata to spatio-temporal bbox is one of the deficits of current EO metadata, one of the aspects FAIRiCUBE is trying to fix.

Alternatively we can integrate all the metadata stubs stemming from these external systems, then use this to illustrate how search by keywords makes finding data far easier than inspecting all datasets fitting to the bbox.
I like the approach of : quickly build first catalog records, and then find out, as per how to enhance that. From our side, the normative coverage description document is available, it can be parsed.
I'm concerned at the current "process" for data and metadata ingestion, as it seems the possibility for the requesting UC partners to validate the provided data has been removed from the loop. My understanding was that the requesting UC partners would first review and confirm the PR before this is merged to the catalog. At present, I've seen cases where the entire process was performed by the technical partner, leading to incorrect data within FAIRiCUBE.

Example EU Demography: 
- [Metadata Record](https://catalog.eoxhub.fairicube.eu/collections/index/items/eu_demography)
- [DescribeCoverage](https://fairicube.rasdaman.com/rasdaman/ows?&SERVICE=WCS&VERSION=2.1.0&REQUEST=DescribeCoverage&COVERAGEID=eu_demography&outputType=GeneralGridCoverage) _Note: the link to DescribeCoverage is still missing in the Metadata Record_
- Issue: Demography provides counts, is described as a categorization where have you documented this process, maybe we need to revise
let me second that: we should reinspect the procedures again now that we have executed them a few times. Let me suggest to have a brief (30min?) meeting with all relevant WP at the table. Once we agree on the WPs to participate I can offer to set up a doodle.
what is the status here? Should we clarify this before more and more data is requested by the UCs? UC leads and WP4 & WP5 leads should be invited together with coordination team...
status is: no response to my proposal above, so no action. Should I...? looking through the deliverables, the steps followed during the ingestion process (including confirmation by the requesting partner before closing the ingestion process) should be described in a bit more detail in D5.2. There I currently find the following fuzzy statement:

`Once all metadata and data requirements are fulfilled and confirmed by the data requester, the ingestion handling partners will perform the merge and the issue will be closed.`

Also pertains to Figure 2, no indication of a requestor check before finalization, leading to errors as cited above in the [demography dataset](https://github.com/FAIRiCUBE/FAIRiCUBE-Hub-issue-tracker/issues/41#issuecomment-2094108957).