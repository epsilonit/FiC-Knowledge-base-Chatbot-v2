Publication of data sets
Soon, we have the first data sets available that we want to publish and we need a long-term solution for storage, referencing and registration of data.

EC / EU recommends https://www.openaire.eu/
which automatically feeds information back to our EC continous reporting portal
and I will explore how to make use of the service and what it generally offers

DOIs shall be generated and I remember that NILU can offer this through a 3rd party, I will follow up on this

Meta data is availble through our FiC Hub service
Hi Stefan, curious to know what is your first impression on OpenAIRE. I had a quick look at it and did not find any immediate solution for us. The "Services" for "Publish data" point to Zenodo, which could be an option. 

Another remark: At the moment we are talking about a small dataset, 6MB, so saving it in the uc1 repository is also an option for us, though probably not a long-term solution for FiC.
I'd also like to explore the option of keeping data within FiC, providing APIs for access (downside of Zenodo etc - we're back to monolithic blobs, harmless with 6MB, but gets ugly with big datasets. I'm still waiting for feedback on what APIs we could put on top of data generated within FAIRiCUBE to make this openly accessible
well, indeed, openaire might be a bit different from what I though it is. it is a collection of services, and we have to browse to see if there is someting for us. as a starting point this serice can be useful:

Plan your data management: Create, link and share Data Management Plans 
https://catalogue.openaire.eu/service/openaire.argos/overview
which links to 
https://argos.openaire.eu/splash/

regarding data storage, I was not expecting to find a solution that already has an API and can be FAIRiREADY, that would have raised some serious questions for our project (^_^). if we however, can deliver/duplicate some data sets on a platform that has a wide EU access range and does not entail costs, it can be a a way forward?

now that I read a bit more, it looks like this zenodo service is mostly used to store data attachements to papers, not necessarily real EU wide analysis ready data sets... In principle it is possible to create WMS layers of raster data ingested into SentinelHub, that can be used without authentication... am I right, 
We are going to use this method to serve data to the EOX visualization tool eodash. 
I am trying to configure one of our datasets to see how it works, but I am encountering errors.. Ill keep you posted.
Yes, in principle only creating a Layer configuration would be needed to access the ingested data through the instance WMS endpoint

feedback from B-Cubed project:
"https://zenodo.org/communities/b3/records?q=&l=list&p=1&s=10&sort=newest

we have a zenodo community, where we upload all our data sets. You can take a look at each specific data set how it was published and where it is available"

I have now created a zenodo communite for us

https://zenodo.org/communities/fairicube

now we only need members and content (^_^)
I will further research how to generate DOIs
Hi in the meantime I had created a folder in the uc1 repository for the dataset: https://github.com/FAIRiCUBE/uc1-urban-climate/tree/master/data/city_features_collection. I will try now to upload the dataset and related files to Zenodo and provide feedback. 
Question: what about the DOI?
see above comment (*_^) : "I will further research how to generate DOIs"
Hello, I have published the dataset on Zenodo, for now in restricted mode. I think it is a good solution for publishing use case results. The size limit for a single dataset is 50GB.
If we are going to adopt this solution, it must be well integrated with the FAIRiCube Data Catalog. For example, ensure that the same metadata fields have the same labeling; that there are no discrepancies in the information provided; that the two services are cross-linked.
During our investigations to contribute to possible governance strategies for project data/results, we came across the following interesting opportunity.
We would suggest our coordinators  and to register FAIRiCUBE in the Zenodo [EU Project Community](https://zenodo.org/communities/eu/pages/join), a pilot open repository for EU-funded projects (adding also a link to the existing [FAIRiCUBE Zenodo community](https://zenodo.org/communities/fairicube/records?q=fairicube&l=list&p=1&s=10&sort=bestmatch)).
We envisage the following advantages:
•	Benefit from the “Trusted repository” which offers “200 GB per record upload limit”
•	Increase project visibility, having also the opportunity to contribute to the pilot phase until Autumn 2024, becoming an "early adopter".
What do you think?
very good suggestion, I just did it! can I encourage everyone to join our Zenodo community? maybe especially you Giacomo as well, I would like to make you a "manager" there...
Thank you and glad to contribute!

Because, people willing to become members of a community should receive an invitation from the Community Owner or Manager, how do you suggest to organise this?
For the time being, I can see only 1 member (you). 

What looks a bit strange to me (but I'm still in my learning curve :), is that anyone logged-in (even without being a member) seems able to upload records.
To this end, may I ask you which "Review policy" is currently applied to our community (i.e. "Review all submissions" or "Allow curators, managers and owners to publish without review"?
For you to know, I'm reading these aspects from a test community that I've just created ;)

However, we should work on setting some additional and more specific governance here, becasue it could be safer for us, looking at Zenodo generic "rules". 
I am currently struggling to find out how to invite people to our community. I test with maria and dont see her, I see no-one really to invite... I need to find out which username/full name / github name to search but I hope to have a solution with the help of maria! 
regarding the review policy is set to "review all submission" can change once we have actually other roles defined (and other members in the community)

update : udpate zenodo users need to update their profile with a full name, this way, I see the users and can invite them...
To recap how to join the Zenodo community (from user perspective):
1.	Sign up to Zenodo, ideally with Github account
2.	In Settings > Profile, update Full name and make sure that Profile visibility is set to Public 
3.	Notify Community manager and wait for invitation
4.	Check if invitation has arrived (~Zenodo does not send email notifications~ Zenodo mail notifications system is slow)

Just updated my profile (username = gmartirano) with my full name.
Waiting for the invitation ;)
Dear all,
meanwhile the members of our Zenodo project community will increase, I'd see the following initial steps:

- agree on an overall governance strategy for project identifiers, including, for instance:
   - definition of a procedure to generate (and reuse) a DoI from Zenodo,
   - creation of a script to align Zenodo “metadata” with our existing pipelines which create datasets and a/p resource metadata (currently dealt with in #52).
- investigate to what extent we can use the Zenodo [EU Project Community](https://zenodo.org/communities/eu/pages/join) to store project results (mainly datasets, because publications, documentation, a/p resources, etc. can be stored in the Zenodo FAIRiCUBE Community).
- and many other steps (new or follow-up of the above listed ones) to come ...
  I herewith notify you as community manager to invite me (and the rest of us who have now managed to register!) 
;)

I'd also recommend sending a followup mail from your mail on April 22nd where you requested us all to register, point out that they still have to notify you to be invited.

A bit more generally, I think we need some documentation pages on this, not expect all partners to trawl through this thread to find [Stefan's instructions](https://github.com/FAIRiCUBE/FAIRiCUBE-Hub-issue-tracker/issues/48#issuecomment-2145068346). For a start I'd just set up a documentation folder in the code section of this repo until the documentation process becomes clearer.
For the time beeing, I create the following FAIRiBOOK page:
https://nilu365.sharepoint.com/sites/Horizon2021_CUBE/_layouts/15/Doc.aspx?sourcedoc={235313bb-424e-4a1e-b1d6-92296d28fbfc}&action=edit&wd=target%28communication.one%7Cf7d5501c-56a8-4acc-a07e-64a0b906c4ca%2F[Zenodo](onenote:https://nilu365.sharepoint.com/sites/Horizon2021_CUBE/SiteAssets/FAIRiCUBE%20Notebook/communication.one#Zenodo&section-id=f7d5501c-56a8-4acc-a07e-64a0b906c4ca&page-id=9337808f-28e0-4206-ae79-1896e3083434&end)%7C9337808f-28e0-4206-ae79-1896e3083434%2F%29&wdorigin=703

> Dear all, meanwhile the members of our Zenodo project community will increase, I'd see the following initial steps:
> 
> * agree on an overall governance strategy for project identifiers, including, for instance:
>   
>   * definition of a procedure to generate (and reuse) a DoI from Zenodo,
>   * creation of a script to align Zenodo “metadata” with our existing pipelines which create datasets and a/p resource metadata (currently dealt with in [Integration of Zenodo in FAIRiCUBE Data Catalog #52](https://github.com/FAIRiCUBE/FAIRiCUBE-Hub-issue-tracker/issues/52)).
> * investigate to what extent we can use the Zenodo [EU Project Community](https://zenodo.org/communities/eu/pages/join) to store project results (mainly datasets, because publications, documentation, a/p resources, etc. can be stored in the Zenodo FAIRiCUBE Community).

after a further investigation, it seems that, once the step of joining the EU Project Community will be succesfully completed (including the link with the FAIRiCUBE Community), we can use only the FAIRiCUBE Community to store all our records, including big datasets. It seems indeed that the 200GB "capability" will be automatically assigned to our FAIRiCUBE Community. And this would be a nice "simplification".
Waiting for testing ... 
> * and many other steps (new or follow-up of the above listed ones) to come ...