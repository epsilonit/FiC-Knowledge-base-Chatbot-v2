OGC API - Coverage versions
When comparing the approaches taken by rasdaman vs. AD4GD on implementing OGC API - Coverage, one sees very different approaches:
- Rasdaman: https://fairicube.rasdaman.com/rasdaman/oapi/collections
- AD4GD: http://callus.ddns.net/cgi-bin/mmdc.py/collections?f=json 

When I discussed this with Joan Maso (author of the AD4GD API running on ODC), he stated that what he sees in the rasdaman version looks like the initial discussions under OGC API - Coverage versions, what he implemented is aligned with the latest version.

I'd like to compare these approaches, see which is closer to where OGC API - Coverage versions is currently going.
I get "The connection has timed out" on the AD4GD URL, does it work for you?
for sure there are version differences, given the continuous changes in OAPI-Coverages (AFAIK SensorThings has not even bothered adopting OAPI). As there are no resources for changing back-and-forth we would wait until adoption and a stable version (if ever).

But talking about comparison, we might start into a comparison of the two pillars in Fairicube, otherwise why do we have two. 

No idea why you're mentioning STA here, not being used in the project. Background of that accepted standard goes way before the current OGC APIs, but based on our latest analysis during the STA 2.0 update, not fit for purpose. Has been accepted by OAB, STA is formally an OGC API.

That said, the focus here is services for gridded data, where we don't have the level of development as we have for terrestrial point based data with STA (standardized well-deployed API), due to the lack of outlook for WCS we're refocusing on APIs. Based with discussions with EOX, they are at present not capable of providing standardized APIs, so no way to compare these 2 pillars.

However, based on our sibling collaboration with AD4GD we do have access to an OGC API - Coverage endpoint based on the latest version of the OAPI-Coverages - the topic in this thread is the comparison of these 2 approaches if you're in contact with AD4GD you could let them know their service is down.
For reference: rasdaman implemented [OGC API - Coverages - Part 1: Core (0.0.2)](https://developer.ogc.org/api/coverages/index.html) a year ago which I think corresponds to the current [draft version](https://docs.ogc.org/DRAFTS/19-087.html); generally all parts are supported except metadata filters (queryables) and coverage tiles.

Within Testbed-19: Jerome, the main editor of the draft standard, tested and used the rasdaman service at the end of 2023 with his own Ecere GNOSIS client and it all worked very well; Benjamin Pross from 52north also developed a testing suite which rasdaman passed.
Just checked, AD4GD OAPI-C is up again.

From what I can see, there were major changes between V0.0.2 and 0.0.7, I'll get in touch with Jerome to better understand the differences.

Is there an outlook for updating the rasdaman OAPI-C or are we stuck with the status-quo? 
Rasdaman is compliant with the 0.0.7 draft as far as I know: it has not changed since we did the implementation last year.
The [Testbed-19 GeoDataCubes Engineering Report](https://docs.ogc.org/per/23-047.html#_12504791-f9c8-4f09-9a38-cc349c936d64) lists OGC API - Coverages 0.0.6, although the link points to the 0.0.7 draft. I think there's no difference, but you could clarify with Jerome.