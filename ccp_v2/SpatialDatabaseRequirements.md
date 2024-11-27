Spatial Database Requirements
Based on requirements from the UC for a Spatial Database, we've now reached the agreement that NILU will provide this. Details:

- NILU is currently setting up an own PostgreSQL database with PostGIS & PostGrest extension
  - To be clear: this is not the “NILU tenant”!
  - Meant for testing if this suits the UC needs
  - If required, set up WFS on this DB (GeoServer is quite simple to set up)
  - If the REST interface is not sufficient, NILU developers can take care of a custom API
  - If all works, we have the knowledge to
    - Host the database at NILU
    - Configure / support an installation at the NILU AWS tenant
- Maria has provided example data that can be used to define the DB architecture
  - S4e will test the functionality of the DB
- Preferred solution for me (especially in light of the FAIRiCUBE philosophy)
  - Learn from the NILU installation
    - Provide [temporarily] service to UCs 
  - Transfer learning to NILU AWS tenant
    - Benefit from existing user management
      - Tagging of UCs similar to the actual AWS resources/costs
    - Allow EOX to learn from this additional setup (will ultimately enable this [FAIRiCUBE] service for future projects)
      - This is “by the way” added value that FAIRiCUBE contributes to EOX for free (please keep this in mind next time there is the discussion on 70% financing)
      - I hope that this works both ways (back to the UCs) as e.g. s4e also eligible to only 70% funding!
  - Transfer DB content to NILU AWS tenant (if needed)
    - Depends a bit on how fast we have the PostgreSQL/ PostGIS / PostGrest bundle deployed
    - UC1 had once estimated roughly 25 GB od data for the DB, likely, we reach less than 100 GB for all UCs (still  not big?)
  - Host this database under the NILU tenant also gives insights on how much such a service costs

Any other requirements from UC?
Thanks for this initiative Just some thoughts from my side:
* Ideally the deployment is done on kubernetes using standard concepts like helm charts so that it is easily transferable to the NILU AWS tenant using https://github.com/FAIRiCUBE/flux-config
* How to benefit from existing user management on the AWS NILU tenant needs more thought I believe, at least it is not clear to me
For UC2 I don’t see an urgent need for this at the moment, so I have no additional requirements.

I think we still aim at primarily processing gridded data (so rasterise any spatial vector data needed first), even though some workflow outputs might be in tabular form (these perhaps are not even ‘spatial’). Most likely spatial vector data will only be used for selection (e.g. of regions) and visualisation. There might be some basic (small scale) storage wishes for that at some point.

We would not intend to duplicate existing spatial vector data stores if their API and performance is adequate. couldn't such an additional RDBS also support the approach you proposed a while back for training synchronous UDFs, doing the communication with the routine being trained via DB tables? Sure, a database-centric system that has SQL and tables as its main user interface could make use if it to pass start/stop/status commands for e.g. controlling an ML model training loop, and the progress information from the training process could be made accessible via tables. However I would expect functionality like that to better be integrated into such system, instead of running it via an external relational database. 
just to mention it (again), we could deploy MLflow into your EOxHub workspace for example to track experiments and models Yes, please add that to our EOxHub workspace so we can track ML training experiments. think such functionality would be relevant for all UC! Don't UC1 & UC4 require such functionality? , MLflow  is now available,  , I assume this approach as described in the initial post is outdated?
I can close the issue?

Hi closing this issue as the DB requirements have evolved.