Location for Schema files
For some data provision options, we have to create additional schemas (e.g. extending the Coverage data models as required, here we need to provide XSD files). We need a place to provide them.
Based on discussions, we'll set up GitHub pages for this purpose, based off the main FAIRiCUBE Repo
Text for base page:
For some data provision options, additional schema files are required (e.g. when extending the Coverage data models as required, XSD files are required to document the extensions). These are provided here

Please provide a subfolder for Coverage XSDs, then we can create other subfolders as required
Hi
I have just created the repository [Schemas](https://github.com/FAIRiCUBE/Schemas) available at the link https://github.com/FAIRiCUBE/Schemas. The GitHub pages are enabled and available at the link https://fairicube.github.io/Schemas/ . Thanks! Works!

[MD-LinkSchema.xsd](https://fairicube.github.io/Schemas/CoverageXSDs/MD-LinkSchema.xsd) exciting news as you can see: we have a schema repo - pls have a look and then mail me directly so that we discuss next steps.  that's great to have, thanks for establishing! I have added some prose now as a start. Note that it will take some time to update all coverages with the new schema location, stay tuned. Likely the metadata reference listed in the repo [schema](https://fairicube.github.io/Schemas/CoverageXSDs/MD-LinkSchema.xsd) is still under work, and this below will be replaced:
`<documentation>-- Definition --
Monitored environmental medium.</documentation>`

Thanks for finding that cut&paste error, fixed. Something seems to have gone wrong with the GitHub pages, https://fairicube.github.io/Schemas/ no longer resolves

Could you please take a look at this?
Hi thanks for finding the error, fixed. Thanks!!!
looks nice now, can we close it? if you look closely, you will see that this issue was closed a while back (May 9th). The schema for the metadata link I provided is available there. 

Still missing schemas for the proprietary aspects of rasdaman metadata, when can we expect this?