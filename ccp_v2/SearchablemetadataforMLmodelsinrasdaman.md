Searchable metadata for ML models in rasdaman
- What metadata would allow to find ML models stored in rasdaman? 
- How would these be provided / harvested into the catalog?
- How can users search for models?
- How can users invoke a model found?

linking in , , , 
For the catalogue there is already an initial form to provide ML model information, good to try that and see if it needs changes. I would also think that a user should be able to query rasdaman to get a list of stored models, which maybe returns less extensive metadata, but with appropriate info how to invoke the model. And then both sides should be linked up somehow.
Regarding the use of the form,  in case of ML and DL resources:

- the 'Output data obtained' field is intended to provide information about where the model is stored i.e., it is a pointer to the model via a link
- also replying to the question 'How can users invoke a model found?', the 'Characteristics of output data' field is intended to provide a textual description of how the model should be used and/or invoked.
Regarding 'How would these be provided / harvested into the catalog?', when the form is filled in, the yaml files are automatically created and are stored in the 'yaml-file' folder of the 'resource-metadata' repository. Then these files can be automatically published (with a Python procedure) in the a/p resources catalog. So when EOX creates the catalog (@Schpidi) the files will be automatically published in it.
Concerning 'How can users search for models?' this is something that should also be discussed with EOX (which is responsible for the catalogs).
Regarding the search on GitHub, using the label 'a/p resource metadata' from the issue list, it is possible to obtain all the issues concerning a/p resource metadata.
However, regarding the catalogue search, looking at the catalog that EOX has made for the data, at the moment one can search by tag and keyword (only one label at a time), but (our) idea is to extend this possibility through the use of queries.
> * the 'Output data obtained' field is intended to provide information about where the model is stored i.e., it is a pointer to the model via a link Would such a (deep) link (URI?) to a model stored in rasdaman be possible? What would be a proper reference to use in the form?

On searchability, we definitely need to update the [FAIRiCUBE Catalog](https://catalog.fairicube.eu/) to enable more complex search. Details under the [catalog repo, issue 5](https://github.com/FAIRiCUBE/FAIRiCUBE-Hub-issue-tracker/issues/10). 

On links to the model, good question of how to provide this link. Is it safe to say for the present that the links refer to UDFs? Probably needs to be rethought a bit due to adding JupyterHub to rasdaman. : How would this be done with EOX? 
> On links to the model, good question of how to provide this link.

Depends on what should be the answer - the pure identifier (possibly embedded in the corresponding query and URL) would give back the model = byte string.

What would you want to get back when cklicking such a URL?

> > * the 'Output data obtained' field is intended to provide information about where the model is stored i.e., it is a pointer to the model via a link
> 
> Would such a (deep) link (URI?) to a model stored in rasdaman be possible? What would be a proper reference to use in the form?

yes, but is it what you want? You would get back the byte string comprising the model. 

That's a good question. With rasdaman specifically currently we would have the option to indeed (A) return the TorchScript data of the model (and someone could load that directly into PyTorch), or (B) refer the user to a template WCPS request with the referenced model already filled in (not sure if that is doable), or (C) provide a webpage describing the model and its intended usage and so on (but that would be kind of a duplicate of the catalogue entry I guess). And maybe there are more alternatives?
> Concerning 'How can users search for models?' this is something that should also be discussed with EOX (which is responsible for the catalogs). Regarding the search on GitHub, using the label 'a/p resource metadata' from the issue list, it is possible to obtain all the issues concerning a/p resource metadata. However, regarding the catalogue search, looking at the catalog that EOX has made for the data, at the moment one can search by tag and keyword (only one label at a time), but (our) idea is to extend this possibility through the use of queries.

Agreed, the current catalog is fairly limited as it is a static catalog and the search is a pure client side one. Our idea is to harvest the static catalog into a tool providing API support e.g. PyCSW or STAC-FastAPI that can be used in clients.
> On links to the model, good question of how to provide this link. Is it safe to say for the present that the links refer to UDFs? Probably needs to be rethought a bit due to adding JupyterHub to rasdaman.
> 
> : How would this be done with EOX?

If I understand the question correctly we'd suggest to use the model registry provided by MLflow (https://mlflow.org/docs/latest/model-registry.html).
linking in  how does this new proposal for mlflow fit with the original proposal from EOX to use STAC for metadata?

Also, is there a better overview of the [MLFlow concepts](https://mlflow.org/docs/latest/model-registry.html#id1)? Before we change anything, we should assure that it aligns with the requirements in [D4.3 processing resource metadata](https://nilu365.sharepoint.com/:w:/r/sites/Horizon2021_CUBE/Shared%20Documents/General/deliverables_milestones_archive/2023/D4_3%20Public%20Listing%20(Catalogue)%20of%20FAIRiCUBE%20processing-analysis%20resources_V1.2.docx?d=wafb6e9f6bc4f4bc5a2cde884acbc8470&csf=1&web=1&e=KMjJkX)
sorry MLflow is just some tooling that is offered. there is no change in using STAC speaking of tooling, what's the outlook on human-readable catalog?
 what do you mean by human-readable catalog? Related to [Catalog Search Options FAIRiCUBE/FAIRiCUBE-Hub-issue-tracker#10](https://github.com/FAIRiCUBE/FAIRiCUBE-Hub-issue-tracker/issues/10), in addition to exposing the STAC metadata via an API, we need a catalog enabling human users to search the catalog.

As STAC is for Spatio-Temporal Assets, and Spatio-Temporal query tends to require provision of intervals, we need more than the current mini-catalog with one search field.