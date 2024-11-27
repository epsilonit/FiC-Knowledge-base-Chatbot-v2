Data Ingestion Process
In order to facilitate the transition from the initial inventory.xls to the formalized [data request process](https://github.com/FAIRiCUBE/data-requests/issues), EOX created a tool that extracts the information from the Excel, provides this within a [GitHub Project](https://github.com/orgs/FAIRiCUBE/projects/1). From this project, the requests can then be transferred to individual data request issues. However, I haven't seen this functionality used.

Once the information has been transferred to a Data Request Issue, I'd like to see the internal discussions on missing bits done via the GitHub issues; provide the questions as a comment on the issue, assign the UC responsible to the issue.

I see different backgrounds to missing information, requiring different responses:
- Information was not covered in the inventory.xls, but is covered in the data request issues: UC responsibles should provide the missing information in the data request issue
- Information should be provided within the data request issue, but wasn't: same as above, bother the UC responsibles
- Information is relevant for Ingestion, but not covered in the data request issue template: propose and implement an extension to the template
We could provide a Web request to the metadata folks where they tell rasdaman the metadata link and rasdaman puts it into the metadata slot of the coverage. Would this work? providing the link to the STAC metadata element is described in Metadata for [datacubes FAIRiCUBE/FAIRiCUBE-Hub-issue-tracker#21](https://github.com/FAIRiCUBE/FAIRiCUBE-Hub-issue-tracker/issues/21)

The issue here is different. Please take a look at the [GitHub Project](https://github.com/orgs/FAIRiCUBE/projects/1) set up for the ingestion process by EOX. If used this, he wouldn't have to manually copy&paste from the inventory.xls, and it would be easier to track the ingestion process. 

> Please take a look at the [GitHub Project](https://github.com/orgs/FAIRiCUBE/projects/1) set up for the ingestion process by EOX. If used this, he wouldn't have to manually copy&paste from the inventory.xls, and it would be easier to track the ingestion process.

Ideally, the metadata link is added automatically (ie: via script without human intervention) during rasdaman datacube initialization. Is that possible with the EOX thingie? I admit I cannot see a way there.

If EOX cannot provide a script that our server can invoke automatically, here an alternative suggestion:

- The catalog provides a Web request for generating a new record
- During datacube creation, rasdaman makes a call to obtain a reference which it adds to the metadata (@Mohinem: you need to use import hooks)
- likewise, a next call by rasdaman feeds the metadata known at this time into this new record
- lateron, any other entity can edit + extend this metadata record

Thoughts?

-  transforming the content of the data request forms is on EOX. Provision of the metadata link within the Coverage is clarified in [Metadata for datacubes FAIRiCUBE/FAIRiCUBE-Hub-issue-tracker#21](https://github.com/FAIRiCUBE/FAIRiCUBE-Hub-issue-tracker/issues/21). What should be done jointly is clarifying what's missing in the data request form, that we'd need for metadata

What I'm explicitly asking here is why JUB refuses to utilize the Project provided by EOX to automatically transform the individual rows of the inventory sheet to data request issues. If there is a clear reason why you refuse to utilize the agreed process, then please explain why.


seems clarified after our direct exchange: we will use the tool.
As rasdaman is now utilizing GitHub issues for ingestion as agreed, closing this issue.