add rasdaman datacubes to catalog
A list of datacubes is already provided by rasdaman, but not listed in the catalog. As the automatic procedure is not yet in place, asking herewith to manually add these to the catalog.

The list of datacubes can be harvested through [this WCS request](http://fairicube.rasdaman.com:8080/rasdaman/ows?&SERVICE=WCS&ACCEPTVERSIONS=2.0.1&REQUEST=GetCapabilities). Remaining available for any questions.

Actually, this exercise might help in preparing the automatic procedure for the future.
Hi there, I tried the WCS link and I get an "Unauthorized" exception as a response
```xml
<ows:ExceptionReport xmlns:ows="http://www.opengis.net/ows/2.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xlink="http://www.w3.org/1999/xlink" version="2.1.0" xsi:schemaLocation="http://www.opengis.net/ows/2.0 http://schemas.opengis.net/ows/2.0/owsExceptionReport.xsd">
<ows:Exception exceptionCode="Unauthorized">
<ows:ExceptionText>Missing basic authentication header with username:password encoded in Base64 string from request '/rasdaman/ows?service=WCS&acceptversions=2.0.1&request=GetCapabilities&version=2.0.1'</ows:ExceptionText>
</ows:Exception>
</ows:ExceptionReport>

```
thanks for reporting, we will take care.

at first glance, no login data are passed according to the message, can you check again? you need to provide credentials in the request, use curl for that

e.g. curl -u 'fairicube_eox:PASS' 'https://fairicube.rasdaman.com/rasdaman/ows?service=WCS&version=2.0.1&request=GetCapabilities'
thanks , the thing is I do not have credentials, any idea how to get valid credentials   you could send an email to Peter (he commented above) and he will send the credentials to you.
credentials sent still no information provided from your side, rasdaman still effectively locked out. 
I believe this issue has long been solved, please check and if so, close this issue
I don't think it's been done, Yesterday I spent a whole day to create [20 entries](https://github.com/FAIRiCUBE/data-requests/issues/318#issuecomment-2252525058) manually, predominantly by copy-pasting stuff from the DescribeCoverage responses.

A DescribeCoverage such as [this](https://catalog:JdpsUHpPoqXtbM3@fairicube.rasdaman.com/rasdaman/ows?&SERVICE=WCS&VERSION=2.1.0&REQUEST=DescribeCoverage&COVERAGEID=water_and_wetness_20m&outputType=GeneralGridCoverage) can be parsed and at least provide a skeleton with most details filled in, that can then be fine-tuned. not quite sure what you're expecting here, sounds like you expect the catalog to harvest the basic information from DescribeCoverage, then allow users to complete this information? 

This sounds a bit backwards, as the original agreement was that UC partners fill out a data request, then this gets transformed to a STAC metadata record and in the case of CU/rasdaman, is used to import the requested dataset.

What you're now describing is that CU/rasdaman puts a dataset online, provide part of the information, then let the UC partner clean up. The records you're working on should all be available via the metadata editor (everything from the inventory sheet was imported). What you're now proposing is adding duplicate records, thus just increasing the confusion.

Also, while I'm impressed at how you've packed the required metadata concepts into the Coverage metadata slot, to my memory this is still totally undocumented. More relevant would be getting these metadata concepts to the STAC metadata under the corresponding entries on OGC API - Coverages
The main issue is that the data request that the UC partners have filled in have been generally quite incomplete. Furthermore, some details become apparent once we look closely at the data and model it as a datacube, especially the axis/extents and bands. E.g. [this datacube](https://github.com/FAIRiCUBE/data-requests/pull/299) has 200 bands, and I wouldn't expect anyone to enter all of those manually in the catalog-editor.

If #32 and the other issues are fixed hopefully this will become a smaller issue and functionality like harvesting a DescribeCoverage won't be necessary. But as it is currently, I've had to discover and copy in most of the information myself. I'm aware that this process has gone badly sideways :( However, this was to a large part due to unaligned activities by WP5 responsibles (don't want to go into the sordid details here).

The plan was that WP5 guides the UC partners together with the WP2 lead in finding and accessing required data, but this never happened, leading to the mess you've now encountered. As to my understanding, you're now WP5 lead, I'd much appreciate a proposal at clarification. Issues such as which bands of the pesticide grids you mention above should be clarified with the responsible UC partners.

One more detail: it was clear from the onset of FAIRiCUBE that the UC partners do NOT have a good understanding of the various EO products. The core objective of FAIRiCUBE is `to enable players from beyond classic Earth Observation (EO) domains to provide, access, process, and share gridded data and algorithms in a FAIR and TRUSTable manner.` The gap between what would be possible through the use of EO products vs. what terrestrial scientists can actually access is exactly what we're trying to fill! If this were not a problem, we wouldn't have the project! I think you misunderstood somewhat my comment. I'm not complaining or blaming anybody, just explaining how this process *is* right now in order to clarify why a feature as requested here would be useful. It would certainly make creating catalog entries easier for us and less error-prone. maybe you also misunderstood my answer ;)

If you need more information for any of the requested datasets, please directly contact the requesting UC partner!
As I have done the requests with the catalog editor already I think the "automated procedure" originally discussed in this issue is not that needed anymore, so it can be closed.