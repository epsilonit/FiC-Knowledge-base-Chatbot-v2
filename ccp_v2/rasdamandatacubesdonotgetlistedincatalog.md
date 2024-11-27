rasdaman datacubes do not get listed in catalog
While a lot of discussion and clarification was done, the rasdaman datacubes still are not visible in the catalog. 

See also discussion in [this issue](https://github.com/FAIRiCUBE/FAIRiCUBE-Hub-issue-tracker/issues/21) where any and all linkage questions have been resolved AFAICS - no further questions have been raised for a long time.

Hi Peter
The discussion about this subject ended with the agreement that items would be added to the catalog when the data-request PR is merged. 
So far NO rasdaman data requests have been created yet.



> Hi Peter we are certainly not fencing anything or anyone. The discussion about this subject ended with the agreement that items would be added to the catalog when the data-request PR is merged. So far NO rasdaman data requests have been created yet.

I understood this will al lbe done automatically. It isan alien concept that UCs create data requests so that wen create data requests. You have the github ssue, the information, and the rasdaman flag. If you want to do it internally like this (with automated triggers, PRs, merge, etc) fine wth me, but you cannot impose  such a complicated workflow to others.

So again: the LGN, Corine, etc data requests exist, please go ahead creating a catalog record.

just for clarification : no UCs creates a data request so that an other [technical] partner creates another data request. once the GitHub repo issue is resolved, we can get the automatic transfer of items from the inventory sheet into WebGUI data ingestion items (GitHub PR) going and after [successful data ingestion which is the case for some requests already and ] approval from the UCs / data requester, the merge will be complete and data items should show up in the catalog. short cutting this will compromise our agreed procedure and without validation the data quality as well. 
update from [meeting with EOX](https://nilu365.sharepoint.com/sites/Horizon2021_CUBE/_layouts/15/Doc.aspx?sourcedoc={235313bb-424e-4a1e-b1d6-92296d28fbfc}&action=edit&wd=target%28meetings_MoM.one%7C795821e2-7bfb-497f-a691-0625bbf2e405%2F2024_05_21%20%20WP4%5C%2F5%20synchronization%7Ca06ccb2e-4c94-4374-8b33-17e3240becb4%2F%29&wdorigin=NavigationUrl) 2 days ago: catalog will be structured to allow differentiating (i) FAIRiCUBE and externally linked data and (ii) rasdaman and EOX hosted data. EOX (Mussab) suggested to simply have different STAC instances for this. EOX to discuss internally and either come back for more discussion or implement as agreed. I'm a bit confused about this newly proposed approach of splitting the catalogs via certain criteria. Where does this requirement stem from (e.g. which UC has requested?)
To my view, such functionality is easily covered by filtering, see #5. No need to create separate catalogs, requiring a user to enter their search criteria multiple times. 
is this issue related to 
https://github.com/FAIRiCUBE/FAIRiCUBE-Hub-issue-tracker/issues/21
?  if so, this should be aligned? preferable one of the issues needs to be merged = closed.
> is this issue related to #21 ? if so, this should be aligned? preferable one of the issues needs to be merged = closed.

after brief investigation, closed #21.

IMHO we need a way that allows users to clearly determine project data from "imported" data (in the sense of: not generated in FC). As I could not find a simple way to see that in the catalog I raised that issue, and in discussion with EOX Mussab suggested to simply have several instances. Of course there are different ways, with different pros and cons. However, I'd say it is a separate issue, and therefore I'd create a new one if we want to discuss the rasdaman/EOX idea (and alternatives) further. Please advise.

Reiterating what I've already stated, we have the agreement to add additional filter capabilities to the catalog as described in #5 

I haven't seen any functionality that can be attained by splitting the catalogs that cannot be done with less effort via filters. I do however see an added burden on users if they must search 4 different catalogs. what's the timeline on filtering? If we see that there are still deficits when this functionality has been fully implemented, we can revisit the idea of multiple catalogs. I believe the core issue of link/backlink between metadata records and coverages on rasdaman has now been solved, is implemented in several of the coverages on the rasdaman server so can we close this issue?

If there is interest in pursuing the multicatalog approach, please create an issue for that, but be aware of the agreements already reached under #12 
> I believe the core issue of link/backlink between metadata records and coverages on rasdaman has now been solved, is implemented in several of the coverages on the rasdaman server so can we close this issue?

correct, so closing.