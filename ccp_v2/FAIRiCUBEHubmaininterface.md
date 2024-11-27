FAIRiCUBE Hub main interface
One of the main deliverables of FiC is the FAIRiCUBE Hub and currently there is no real interface from where the user can start to access FiC Hub services. There is however a general listing available on the FiC website (https://fairicube.nilu.no/fairicube-hub/) which outlines the FiC Hub services. 

1) Now we need a [simple but] attractive and user friendly website that provides access to all the FiC Hub services. It needs to be determined later which of the services can directly been included in the main framework of the FAIRiCUBE Hub landing page (introduction and documentation to all services as a starting point) and which will only open a new window (rasdaman dashboard, EOX lab, ...). 

2) faircube-hub.eu (to only suggest a name) can be hosted by a FiC partner but should be available and maintained (by the consortium) for at least 3 years after project end.

3) The landing page can build on other websites designs and technology (e.g. from FiC partner) but shall have a unique FAIRiCUBE design. Fairicube-hub.eu shall link be independent but connected to fairicube.eu (and vice versa).

4) The FAIRiCUBE Hub page shall be the main entry point for users to
- find all relevant FAIRiCUBE service documentation and information
- get complete guidance on 
- - access to services
- - search for / bring data 
- - data storage / analysis / processing resources
- - ML applications
- learn from the FiC UCs
- search for FiC resources (documentation, data, meta data, publications, resource collection, github)

An early illustration/vision on how the FAIRiCUBE Hub interface could look like is available here: https://shorturl.at/BxP8B
If I understand well, this will be a mostly static page that provides pointers to FAIRiCUBE services and resources, and potentially integrate some documentation.

1. What about a design with cards, e.g. scroll down to "Use Case A" [here](https://testbed19.rasdaman.com)?
2. We can host the website on the rasdaman VM.
3. Is there any FAIRiCUBE-specific design guidelines?

yes, most (if not all) content is static but we should have a CMS in the back so that several users can contribute.  most services might be pointers but it would be good if we can integrate services, e.g. data catalog, data ingestion gui, documentation, search queries ..

1) card design looks good, can work on several levels
- service overview
-  UC overview
- detailed & structured overview of UCs
..

2) can the VM operation be guaranteed for some years?

3) we have some guidelines, but this is mostly addressing ppt/word templates. as a starting point the website theme should work with the colors present in our FAIRiCUBE logo 

https://nilu365.sharepoint.com/:i:/r/sites/Horizon2021_CUBE/Shared%20Documents/General/templates_design/logo%20FAIRiCUBE/[fairicube_logo.jpg](https://nilu365.sharepoint.com/:i:/r/sites/Horizon2021_CUBE/Shared%20Documents/General/templates_design/logo%20FAIRiCUBE/fairicube_logo.jpg?csf=1&web=1&e=ccsqIS)?csf=1&web=1&e=ccsqIS

and that have been adapted in the ppt layout 
https://nilu365.sharepoint.com/:p:/r/sites/Horizon2021_CUBE/Shared%20Documents/General/templates_design/FAIRiCUBE_presentation_template_new%20logos.pptx?d=w506a8ee5cc0041a1b7bc9234f6e4cbc7&csf=1&web=1&e=jHgqgT
We have no experience with any CMS unfortunately, the best we could do is plain HTML that exists in a github repo. Others can contribute to it, and it would automatically deploy within 10 minutes or so. If a CMS is a requirement then maybe another partner with previous experience can take over.

Guaranteeing several years from our side is no problem.
if we have a predefined layout and can transfer plain html pieces from github, that could be an elegant solution and avoids "yet another" service. do you use that internally already at rasdaman? would be good to see an example how that looks like in practice..

We don't have something like that in use currently at rasdaman; we mainly have experience with plain HTML websites of JavaScript apps like our dashboard/wcs-client.

Btw, the [FAIRICUBE Hub doc](https://fairicube.readthedocs.io/en/latest/guide/fic_hub/) is confusing: the text sounds like the Hub already exists, and is based on EOxHub? Or is this about different type of FAIRiCUBE Hub?

> The FAIRiCUBE Hub is based on the existing software named EOxHub, which is extended in two ways to provide the FAIRiCUBE Collaborative Development Tools:
> - Extended support for teams as part of the multi-user plan. Allow for easier sharing of versioned notebooks and other artifacts within the team but not necessarily the public.
> - Integrated support for Machine Learning (ML) workflows. This includes the versioning, sharing, and collaborative using of all ML artifacts like code, data, models, results, etc. Based on user feedback, the readily available Open-Source tools like Data Version Control (DVC), MLflow, Kubeflow, or similar, will be integrated during the project.


the text is outdated, whereas the FAIRiCUBE Architecture image at the bottom of the page https://fairicube.readthedocs.io/en/latest/guide/fic_hub/ is more or less up to date. there you see that FAIRiCUBE Hub consists of more than just the EOxHub....