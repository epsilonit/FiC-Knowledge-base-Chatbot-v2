Collaboration of EPS and EOX to handover data from GitHub issue to catalogue 
Where does EPS work stops, where does EOX start 

copied from mail chain
subject : Re: publication of a/p resource metadata
TO Stefania Morrone <s.morrone@epsilon-italia.it>
CC Stefan Jetschny <sjet@nilu.no>; fairicube@epsilon-italia.it; stephan.meissl <stephan.meissl@eox.at>; Kathi Schleidt <kathi@datacove.eu>

Q_EPS : Question by Stefania
R_EOX : Response from Christian

Q_EPS: EPSIT will derive STAC JSON files from the issues in the https://github.com/FAIRiCUBE/resource-metadata
R_EOX : YES,
please use the naming according in D4.3 Table 8, Column: "STAC Mapping".
If you have Tag-Names in the metadata which do not have a corresponding STAC element in the Table, please also check if you can find it it in any of the STAC-Extensions.
If there is nothing in those either, then just use the eg. "Original Tag-Name", we will then have to create a new STAC-Extension later on (TBC).


Q_EPS:   EPSIT will put these JSON files in a dedicated folder e.g.,  https://github.com/FAIRiCUBE/resource-metadata/stac
R_EOX :   YES, and we have already setup & configured the pages deployment and placed an EXAMPLE file there

Q_EPS :   EOX will “take over “ & harvest these JSON files in a STAC catalog /deploy in GitHub io
R_EOX :    YES, we will take over and harvest.

Q_EPS : Is this your vision? I am asking because your last task ”Harvested in pyCSW to provide STAC API (EOX) “ is referring to yaml files in the resource metadata repository : https://github.com/FAIRiCUBE/resource-metadata/tree/main/yaml-files .
> 
> Is this a typo and you meant the folder containing STAC JSON files e.g., https://github.com/FAIRiCUBE/resource-metadata/stac ?
R_EOX : No, it is now adjusted to the github IO URL --> https://fairicube.github.io/resource-metadata/

after doucmention that mail chain, I consider this case closed
Just adding the related diagram:
![FAIRiCUBE_meatdata_ingestion_v2 (003)](https://github.com/FAIRiCUBE/catalog/assets/13329248/c02dcf89-cc78-425e-a726-eaa97e53281f)

after meeting with StefanJ and internal discussion:  updated graphic, harmonizing processes
![metadata_ingestion_process_v3](https://github.com/FAIRiCUBE/FAIRiCUBE-Hub-issue-tracker/assets/4849380/fd4405cb-6920-4c94-8e05-31613545084e) I think this diagram should also go into the D4.1 revision