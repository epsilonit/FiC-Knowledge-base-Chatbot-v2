Need more information on ML models
A core topic for CU is to support ML model integration in rasdaman. A first implementation has been done (via UDFs) and is available. However, for more comprehensive and convenient support substantial information is missing:

- we need more models to compare and generalize
- we need use cases on intended application: applicable regions (and times), expected output, etc.
- we need descriptors of the models so that users can search and select them
- more details will be needed as development progresses

Currently we have received only have one model from WER which, howeer, according to WER was not created by them so detail information is not available.

More information has been requested various times but not obtained, therefore this issue is created now for further discussion. We are kindly asking (WER or anybody interested in contributing) to elucidate on the questions above, and further ones arising.

PS: While this repo is maybe not exactly the best for this location I could not find one on the WER Use Case, and as it is about advancing support in the Hub it is also not entirely wrong. Please someone advise if the issue should be moved elsewhere.

Hi ,

Part of this request is actually a duplicate of the single issue in the WUR UC2 repository, also by you:
https://github.com/FAIRiCUBE/uc2-agriculture-biodiversity-nexus/issues/6
Can you please close that one as duplicate then, or shall I do it?

To avoid mistakes: The model that I provided (for WER) was created and trained by me, and (as far as I recall) I have provided complete information about it (to Otoniel, and also as example ML resource for the catalog (it might still be in there if it has survived the development cycles)). Please let me know which details you are still missing and I will try to fill them in. But remember it was a prototype model for a different project, and came with its own training and input data that was added to rasdaman. So accuracy and transferability (beyond the original training dataset) are rather limited (it was not an aim for the model training). sorry, I stand corrected:

> To avoid mistakes: The model that I provided (for WER) was created and trained by me 

> Please let me know which details you are still missing and I will try to fill them in

This will be more of a dialog as we are investigating how to best offer models.

Definitely it starts with:

- what metadata would a user like to see to select the best suitable model for a given purpose?
- description of the data type the model works on (SAR, optical, DEM, whatever...)
- description of any preprocessing the vanilla data (such as Sentinel-2) need prior to model application
- indication of the patch extent the model expects


Seems to overlap a lot with previous discussions about metadata needed for the a/p resources (that include ML models). However I think you might be targeting other types of users as well (that need more dummy proof options that protect them a bit against running the wrong model)?
As agreed to earlier, Jivitesh will take the coordination role to provide ML models for the integration. Every other data scientist is encouraged to pack up models from the FAIRiCUBE UC work with example data and submit to CU. CU will handle the internal implementation details. I assume that there is a requirement that the data is already or shall be ingested on the rasdman stack? Regarding the meta data I suggest to create an own issue and align with EPSIT efforts on the a/p resources if indeed this goes beyond that meta data concept. have you created an a/p resource metadata record for your UDF? It would be interesting to see to what extent the a/p resource metadata fulfills CU's information requirements I have done that for the initial C++ UDF based version, but it seems like that has not survived. For the new Python UDF based version I don’t think anyone has filled in an a/p resource form. do you mean the entry on  [Semantic segmentation (CNN) model to detect dutch crop classes from Sentinel-2 imagery #7 ](https://github.com/FAIRiCUBE/resource-metadata/issues/7)? how can we get this to the a/p resource catalog? Yes, that is the original one I think.
> do you mean the entry on [Semantic segmentation (CNN) model to detect dutch crop classes from Sentinel-2 imagery #7 ](https://github.com/FAIRiCUBE/resource-metadata/issues/7)?
> 
> how can we get this to the a/p resource catalog?

Hi, I have just added the resource [Crop Classification CNN](https://catalog.eoxhub.fairicube.eu/collections/ML%20collection/items/WNBSWQ0F0Q) to the catalog.  To edit it, you can access the [metadata form](https://fairicube-md.dev.epsilon-italia.it/) using your WUR credentials.