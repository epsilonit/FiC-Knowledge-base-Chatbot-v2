Validity of XML from rasdaman we've got regular issues with the validity of the XML being provided by rasdaman instances.

As an example, see CoverageDescription for [near_surface_air_temperature](https://github.com/FAIRiCUBE/data-requests/blob/main/encoding-examples/near_surface_air_temperature.xml), has an impressive [error log](https://github.com/FAIRiCUBE/data-requests/blob/main/encoding-examples/near_surface_air_temperature-ErrorLog.txt) :(

When can we expect valid XML from rasdaman? any updates?
I just checked the 2 versions of the LGN dataset currently being provided, neither DescribeCoverage response is valid XML

Issues on both:
- xsi:schemaLocation only provides http://www.opengis.net/gml/3.3/rgrid, reference to http://www.opengis.net/wcs/2.0 is also required for wcs:CoverageDescriptions
- gmlrgrid:ReferenceableGridByVectors has the attribute gmlrgrid:id, should be gml:id

Issues on Coverage ID LGN:
You provide ISO-Date strings in fields expecting a double value in several places:
- /wcs:CoverageDescriptions/wcs:CoverageDescription/gml:boundedBy/gml:Envelope/gml:lowerCorner
- /wcs:CoverageDescriptions/wcs:CoverageDescription/gml:boundedBy/gml:Envelope/gml:upperCorner
- /wcs:CoverageDescriptions/wcs:CoverageDescription/gml:domainSet/gmlrgrid:ReferenceableGridByVectors/gmlrgrid:origin/gml:Point/gml:pos
- /wcs:CoverageDescriptions/wcs:CoverageDescription/gml:domainSet/gmlrgrid:ReferenceableGridByVectors/gmlrgrid:generalGridAxis[1]/gmlrgrid:GeneralGridAxis/gmlrgrid:coefficients

Issues on Coverage ID LGN_virtual_coverage_index:
wcs:ServiceParameters are in the wrong order (sequence order is mandatory in XML), <wcs:CoverageSubtype> must be provided before <wcs:CoverageSubtypeParent> thanks for checking, Kathi! correct those, and check the others as well for these issues.  re this below:
> Issues on Coverage ID LGN: You provide ISO-Date strings in fields expecting a double value in several places:

This is an issue OGC never resolved. Of course nobody wans "seconds since epoch", but the well known ISO 8601 syntax. Since at least 2021 I have been picking on the CRS.SWG to allow string-valued coordinates, but they refused for many years. Only with 19111:2019 they showed a broadened mind - but that was too late for CIS 1.0, and anyway GML never made a corresponding adjustment.

Hence, everybody doing timeseries in CIS 1.0 consciously violates the schema.

How can you enforce CIS 1.1 output? In rasdaman we have added 
In CIS 1.1 I have defined a direct position as a sequence of anySimpleType. 

For WCS Describe/GetCoverage request, it means to use version 2.1.0 with the extra output:

`
...&service=WCS&
version=2.1.0&
request=DescribeCoverage&
coverageId=test_mr&
outputType=GeneralGridCoverage
`

For WCPS, it means to use something like:

`for c in (test_irr_cube_2) return encode( c[ansi("2008-01-01T02:01:20.000Z"), E(75042.72735943:85042.72735943), N(5094865.55794:5099865.55794)], "gml", "{\"outputType\":\"GeneralGridCoverage\"}")
`
Am I correct in my understanding that temporal dimensions cannot be correctly provided under WCS 2.0?

I'd love to try the WCS 2.1 version, but have not been able to provide my credentials with the URI (seem not to be accepted), the GUI provided by rasdaman uses WCS 2.0.

In addition, while one of the errors I mentioned pertain to the issue with providing an ISO Timestamp in a double field, there were several other issues that have not been addressed.

Guess we're stuck with invalid XML I've updated petascope on https://fairicube.rasdaman.com/rasdaman/ows

For CIS 1.0:

- It has the schema URL added for DescribeCoverage
- gml:id instead of gmlrgrid:id
- For LGN_virtual_coverage_index coverage, then, wcs:CoverageSubtype provided before wcs:CoverageSubtypeParent

The WSClient now has CIS 1.1 by default and you can select CIS 1.0 as well in WCS DescribeCoverage / GetCoverage in GML format). many thanks!
A few more validation issues:
- xsi:schemaLocation only contains http://www.opengis.net/wcs/2.1/gml, http://www.opengis.net/wcs/2.0 must also be provided 
- CoverageId & ServiceParameters must be in the wcs2.0 namespace
- Sequence is defined in XSD, rasdaman CoverageDescriptions doesn't respect this. Order must be:
  - wcs20:CoverageId
  - gml:coverageFunction
  - cis11:Envelope
  - cis11:DomainSet
  - cis11:RangeType
  - wcs20:ServiceParameters

[Full valid example for grassland_change_2018_index](https://github.com/FAIRiCUBE/FAIRiCUBE-Hub-issue-tracker/blob/main/Example%20Files/CoverageEncoding/grassland_change_2018_index_fixed.xml) I just checked the GetCoverage response for WCS 2.1, here the XML errors:

- <gml:coverageFunction> is not permitted in the current location
- <cis11:RangeType> is not permitted in the current location
- RangeSet must be in the cis11 namespace, not gml
- <cis11:metadata>  <cis11:Metadata> (Capital M!)
- PartitionSet not valid under cis11:Metadata. 
Reason: The following elements are expected at this location (see below)
'{##any except ##local http://www.opengis.net/cis/1.1/gml}' I just checked the GetCapabilities response for WCS (GUI still providing WCS 2.0), here the XML errors:
- Integration of INSPIRE Extended capabilities, but missing elements
- Issues in provision of the ows:BoundingBox, as this type expects only double values. Errors occur when ISO Timestamps are provided. It seems WCS 2.0 has substantial issues here. For DescribeCoverage, the protocol used by the GUI has been changed to WCS 2.1, but the Capabilities still are provided in WCS 2.0 Example:
<ows:LowerCorner>"2015-01-01T08:12:02.000Z" 842000.0 844000.0</ows:LowerCorner>
- As mentioned above, GUI still uses WCS2.0 for the Capabilities, inconsistent with DescribeCoverage 

 > Integration of INSPIRE Extended capabilities, but missing elements

Thanks for checking, please specify which missing elements are when you saw, otherwise I don't know what to with it.




> the GUI has been changed to WCS 2.1, but the Capabilities still are provided in WCS 2.0 

GUI never changed completely to WCS 2.1 , it sets the default CIS 1.1 in WCS DescribeCoverage / GetCoverage tabs with the dropdown to select WCS 2.0.1. Do you want to set in WCS GetCapabilities as well? I now also checked the GetCapabilities under WCS 2.1, seems this isn't even defined, at least I cannot find it in the WCS Schemas provided by OGC at https://schemas.opengis.net/wcs/2.1/gml/ 

Please clarify where the correct schema for this has been hidden on the INSPIRE extended capabilities, for the moment I'd just leave this out as not required. Fear we have TO MANY other validation issues for me to provide these details at present I don't have any experience with WCS 2.1 XML validation. I have for WCS 2.0.1 because I needed to make it pass OGC CITE test. You would need to contact people from OGC for the schema to validate which is your expertiste. do you have any XML validation tools available? I've yet to access any rasdaman response that is fully valid :(

As for providing support for the correct provision of data under WCS2.1, while I'm happy to help, this goes way beyond what I can provide via FAIRiCUBE, some alternative needs to be found
> I now also checked the GetCapabilities under WCS 2.1, seems this isn't even defined, at least I cannot find it in the WCS Schemas provided by OGC at https://schemas.opengis.net/wcs/2.1/gml/
> 
> Please clarify where the correct schema for this has been hidden

OWS Common ? I've some tools, e.g. http://www.eisenhutinformatik.ch/gml/gmlcheck/ (I used it mostly) or even with XMLSpy for validating against WCS 2.0.1 schema (no idea with WCS 2.1 as I never tried). Ideally one uses multiple XML validators, as from experience the validators also have validation issues ;)
I use XML Spy, so theoretically you should have found all the issues I've been highlighting.
Sad note: I have yet to get a valid response from a rasdaman endpoint (this goes way beyond FAIRiCUBE) :( For WCS 2.0.1 you would not get any valid response because of the datetime in ISO format. There is no 2D RectifiedGridCoverage coverage on Fairicube so you cannot get a result for DescribeCoverage which has no validation error.
But, the WCS 2.1.0 options are also all not valid :(

> the WCS 2.1.0 options are also all not valid 

I've not looked in WCS 2.1.0 schema validation yet, but can you confirm that you tested with this https://schemas.opengis.net/wcs/2.1/gml/?


 I found a 2D RectifiedGridCoverage coverage on Fairicube

```
curl -u username:passwd 'https://fairicube.rasdaman.com/rasdaman/ows?&SERVICE=WCS&VERSION=2.0.1&REQUEST=DescribeCoverage&COVERAGEID=sentinel2_2018_flevopolder_10m_7x4bands' -o DescribeCoverage.xml

```

Then please validate the`DescribeCoverage.xml` to WCS 2.0.1 schema to have a valid response. cool! First valid XML I've gotten!!!
At the same time, seems to be the only dataset ingested by a UC partner, take a look at the metadata, I get the feeling Rob ingested this on his own.

Question - I've never understood why one didn't use 2D RectifiedGridCoverage on all the datasets that only provide data for a single year. Can you explain the rationale behind this decision of providing a temporal dimension with extent 1 year? Because the data is in a time series, for example, you would have  multiple 2D coverages:

- coverage_2012
- coverage_2015
- coverage_2017
....

but they can be combined nicely as a 3D ReferenceableGridCoverage coverage with time coefficients (irregular time axis is my favorite).

If you have only 1 file and there is nothing else, even if the file is produced for a year for an interval e.g. 2006-2020, then it should be 2D RectifiedGridCoverage. but right now we have both! Example:
- dominant_leaf_type_2012_index
- dominant_leaf_type_2015_index
- dominant_leaf_type_2018_index
- dominant_leaf_type_virtual_coverage_index 4 coverages which you are listed are all 3D ReferenceableGridCoverage. Technically, source coverages for the 4th virtual coverage like 2012, 2015, 2018 would be hidden so you wouldn't see them.

It would not be possible to create the "virtual coverage" if the source coverages are 2D (they must have the same axes). so the _coverage_index Coverages rely on the individual year Coverages? At least based on the size, it looks like a copy. Or does this virtual Coverage show the data volume for the contained Coverages?

Also - I'm concerned as while the different _index coverages have the same axes, these axes do not have the same resolution over the years. The complete _coverage_index Coverage has the lowest resolution of the contained years the virtual coverage contains the source coverage (_year_index). have a look in the DescribeCoverage in WCS 2.0.1 of this coverage dominant_leaf_type_virtual_coverage_index below:

```
<PartitionSet>
                        <Partition>
                            <CoverageRef>dominant_leaf_type_2012_index</CoverageRef>
                        </Partition>
                        <Partition>
                            <CoverageRef>dominant_leaf_type_2015_index</CoverageRef>
                        </Partition>
                        <Partition>
                            <CoverageRef>dominant_leaf_type_2018_index</CoverageRef>
                        </Partition>
                    </PartitionSet>
```

In the coverage example dominant_leaf_type_virtual_coverage_index it has time axis (not time CRS but Index1D CRS), other source coverages: 2015_index,... also have the same axis. This axis is irregular (irregular axis has no resolution, only regular axis has resolution).

on a side note, this approach has been presented in depth at the Girona meeting, and details can also be found in Deliverable D5.something. the `<PartitionSet>` is one of the bits of the XML that is invalid, no schema provided. [Details](https://github.com/FAIRiCUBE/FAIRiCUBE-Hub-issue-tracker/issues/26#issuecomment-1816543410)
I interpreted the content to mean that the listed Partitions are the source of the individual years. What I don't yet understand (and haven't found in the deliverables) is:

- If the virtual_coverage_index is a copy of the individual coverages listed in the PartitionSet, or references these
- How the conversion between different resolutions is performed
>       Reason: The following elements are expected at this location (see below)
>       '{##any except ##local [http://www.opengis.net/cis/1.1/gml}](http://www.opengis.net/cis/1.1/gml%7D)'

small fix: URL is http://schemas.opengis.net/cis/1.1/gml/

> the `<PartitionSet>` is one of the bits of the XML that is invalid, no schema provided. [Details](https://github.com/FAIRiCUBE/FAIRiCUBE-Hub-issue-tracker/issues/26#issuecomment-1816543410) I interpreted the content to mean that the listed Partitions are the source of the individual years. What I don't yet understand (and haven't found in the deliverables) is:
> 
>     * If the virtual_coverage_index is a copy of the individual coverages listed in the PartitionSet, or references these

reference; this is in the rasdaman documentation. But this is not essential for working with it - for users we always try to push as much as technical detail as possible "behind the curtain". 

> If the virtual_coverage_index is a copy of the individual coverages listed in the PartitionSet, or references these

It is the umbrella has the references to the source coverages (_year_index). 

> How the conversion between different resolutions is performed

You wanted to ask about lat/lon resolutions. It would be better when you mentioned about which axis label. If it is like that, then it is done by gdal library (in rasql it is called projection() operator) - and in petascope it does the hard work behind the scene to generate the rasql query. The error text I posted was provided by XMLSpy
As for the correct URI, in the schema itself, the namespace URI is defined as `xmlns:wcs21="http://www.opengis.net/wcs/2.1/gml"` in the official schema, no final `/` I'm giving up on my question on if the virtual coverage is a copy or a reference to the parts from the PartitionSet
As for the conversion performed, axis label Y X, spatial resolution seems to have been resampled from a 20m source to 10m. Resampling without clear understanding of the nature of the underlying data leads to errors (for details, see Manuel's [presentation on Grids and Resampling](https://nilu365.sharepoint.com/:p:/r/sites/Horizon2021_CUBE/Shared%20Documents/WP2%20-%20use/uc_urban_climate/PPT/FAIRiCUBE_resample_ppt_Manuel.pptx?d=w1f8e01202e3c42838c618b49f07e04d3&csf=1&web=1&e=iBdvmT)). At the very least, the resampling method should be documented with the data.
> The error text I posted was provided by XMLSpy As for the correct URI, in the schema itself, the namespace URI is defined as `xmlns:wcs21="http://www.opengis.net/wcs/2.1/gml"` in the official schema, no final `/`

hm, "www" instead of "schemas" may be wrong indeed, we need to check - thanks for spotting this! And yes, my trailing "/" should not be there. pls collect all such issues so that we can work it off once the dust has settled. I'm well aware of resampling, the default interpolation is nearest neighbor. Because the data is provided in different resolutions (20m and 10m for some years) so the virtual coverage must upscale 20m from source coverage to 10m. If one wants to have select the 20m coverage then go with the source coverage instead of slicing it from the virtual coverage. fear you're confusing namespace (http://www.opengis.net/...) with schemalocation (https://schemas.opengis.net/...) I understand the issue of the different spatial resolutions over time, we've been discussing this for the last many months. However, for correct data provision, such resampling must be documented. 
And while nearest neighbor mostly works for datasets the datasets currently available, it can also go badly wrong. 
> fear you're confusing namespace (http://www.opengis.net/...) with schemalocation (https://schemas.opengis.net/...)

oops, could be - late in the evening, and years after I tortured XML schema. Or schema me... I suggest you open another ticket for any irrelevant discussion to GML from this thread. It has many mixed up discussion which has nothing to do with XML validation. https://fairicube.rasdaman.com/rasdaman/ows updated. Now it has:

- Returns WCS 2.1.0 GetCapabilities by default, but that doesn't mean it will be valid XML for ISO dateTime string format, because: https://schemas.opengis.net/wcs/2.1/gml/ doesn't have `wcsGetCapabilities.xsd` (WCS 2.0.1 has https://schemas.opengis.net/wcs/2.0/wcsGetCapabilities.xsd). OGC will solve this issue later in some meetings.

- I updated the responses of  CIS 1.1 DescribeCoverage and GetCoverage (test with small subsets as it is text format) in GML so now they are valid XML (FYI: I found this online validation tool https://www.freeformatter.com/xml-validator-xsd.html which is faster than XMLSpy) Many thanks!!!

I just checked the GetCapabilities, in addition to the issue with the provision of ISO dateTime, there seems to be an issue with the /wcs:Capabilities/ows:OperationsMetadata/ows:ExtendedCapabilities/inspire_dls:ExtendedCapabilities section. Please check and assure that valid information is being provided.
I find it worrisome that WCS 2.1.0 encoding is not fully specified :( Any outlook when this will be finalized?

DescribeCoverage and GetCoverage looks good, I just checked with the corine_land_cover_virtual_coverage_index dataset. I'll check these responses again once we get the [RangeType information](https://github.com/FAIRiCUBE/data-requests-issue-archive/issues/8) correctly provided, as well as the [link to the STAC metadata record](https://github.com/FAIRiCUBE/FAIRiCUBE-Hub-issue-tracker/issues/21).

Conclusion: DescribeCoverage and GetCoverage are now valid, whether we'll get a valid GetCapabilities is up in the air.

Btw - I checked the online validator you proposed, but this provided very different feedback. To my experience, it's best to utilize multiple validators, as each one catches different aspects. Online validator output for GetCapabilities:

```
    S4s-elt-character: Non-whitespace Characters Are Not Allowed In Schema Elements Other Than 'xs:appinfo' And 'xs:documentation'. Saw '301 Moved Permanently'., Line '2', Column '35'.
    S4s-elt-character: Non-whitespace Characters Are Not Allowed In Schema Elements Other Than 'xs:appinfo' And 'xs:documentation'. Saw '301 Moved Permanently'., Line '4', Column '34'.
    S4s-elt-character: Non-whitespace Characters Are Not Allowed In Schema Elements Other Than 'xs:appinfo' And 'xs:documentation'. Saw 'CloudFront'., Line '5', Column '23'.
    The Element Type "hr" Must Be Terminated By The Matching End-tag "</hr>"., Line '6', Column '3'.
    The Element Type "hr" Must Be Terminated By The Matching End-tag "</hr>".
``` thanks for your checks.

> I find it worrisome that WCS 2.1.0 encoding is not fully specified :( Any outlook when this will be finalized?

Unfortunately, I cannot answer that, it will take long time from OGC.

I've updated https://fairicube.rasdaman.com/rasdaman/ows and indeed your experience with different XML validation tools gave me a good hint here to fix WCS GetCapabilities result.

- XMLSpy has the error with `/wcs:Capabilities/ows:OperationsMetadata/ows:ExtendedCapabilities/inspire_dls:ExtendedCapabilities` which you posted but it doesn't show the correct suggestion for that.
- https://www.freeformatter.com/xml-validator-xsd.html has some weird errors like you posted `S4s-elt-character: Non-whitespace Characters Are Not Allowed In Schema Elements Other Than 'xs:appinfo' And 'xs:documentation'. Saw '301 Moved Permanently'., Line '2', Column '35'`, but this is the key point for me to fix it properly.  It had the invalid namespace URLs before:

 ```
xmlns:inspire_dls="http://inspire.ec.europa.eu/schemas/inspire_dls/1.0" xmlns:inspire_common="http://inspire.ec.europa.eu/schemas/common/1.0"
  ```

correct ones are:

```
xmlns:inspire_dls="https://inspire.ec.europa.eu/schemas/inspire_dls/1.0/inspire_dls.xsd" xmlns:inspire_common="https://inspire.ec.europa.eu/schemas/common/1.0/common.xsd"
``` fear you made an error with the inspire_dls namespace, should be "http://inspire.ec.europa.eu/schemas/inspire_dls/1.0", you're providing the schema location instead of the namespace. The effect is that the inspire_dls schema is no longer loaded because under schemaLocation, the reference is still to the correct namespace http://inspire.ec.europa.eu/schemas/inspire_dls/1.0, but this is not associated with the inspire_dls namespace. Nice trick to avoid the underlying issue, but not a solution.

I've now done the necessary analysis for you, you're missing the inspire_dls:SpatialDataSetIdentifier entry. The full /inspire_dls:ExtendedCapabilities section should be as follows:

```
<inspire_dls:ExtendedCapabilities>
	<inspire_common:MetadataUrl>
		<inspire_common:URL>https://fairicube.rasdaman.com/rasdaman/ows</inspire_common:URL>
		<inspire_common:MediaType>application/vnd.iso.19139+xml</inspire_common:MediaType>
	</inspire_common:MetadataUrl>
	<inspire_common:SupportedLanguages>
		<inspire_common:DefaultLanguage>
			<inspire_common:Language>eng</inspire_common:Language>
		</inspire_common:DefaultLanguage>
	</inspire_common:SupportedLanguages>
	<inspire_common:ResponseLanguage>
		<inspire_common:Language>eng</inspire_common:Language>
	</inspire_common:ResponseLanguage>
	<inspire_dls:SpatialDataSetIdentifier>
		<inspire_common:Code>FAIRiCUBE</inspire_common:Code>
	</inspire_dls:SpatialDataSetIdentifier>
</inspire_dls:ExtendedCapabilities>
``` thanks for correcting me with namespace and schemaLocation URLs.

For your example, I don't think it is just like what you've posted below (it is just to bypass XMLSpy validation). 

```
<inspire_dls:SpatialDataSetIdentifier>
		<inspire_common:Code>FAIRiCUBE</inspire_common:Code>
	</inspire_dls:SpatialDataSetIdentifier>
```

Because, this section is used to list the INSPIRE coverages only (on Fairicube you have none).

A valid example should contain `Code` and `Namespace` of an INSPIRE coverage like below:

```
 <inspire_dls:SpatialDataSetIdentifier metadataURL="https://www.nationaalgeoregister.nl/geonetwork/srv/api/records/15d0b9b0-1067-4ef6-a214-77526e8e8750/formatters/xml">
                    <inspire_common:Code>INSPIRE_WNZ_5_NAP</inspire_common:Code>
                    <inspire_common:Namespace>http://inspire.rasdaman.org/rasdaman/ows</inspire_common:Namespace>
  </inspire_dls:SpatialDataSetIdentifier>
```

Am I wrong?

> thanks for your checks.
> 
> > I find it worrisome that WCS 2.1.0 encoding is not fully specified :( Any outlook when this will be finalized?
> 
> Unfortunately, I cannot answer that, it will take long time from OGC.

infinitely. The Capabilities document is derived from OWS Common, and the derivatino mechanisms of XML do not allow to extend types from number to string. OWS Common is abandoned in the sense that there is no SWG to work on it. And anyway OGC will not want to fix OWS Common (which is known to have several flaws) as they want to sell OAPI now and it is against this interest to improve OWS Common. In short: no change will happen. While the coverages we provide go way beyond what's been defined in INSPIRE, as the DGGS will be building on INSPIRE, it probably doesn't hurt to leave in these INSPIRE specific parts. I wouldn't bother to list the datasets, especially as they're beyond INSPIRE anyway, thus my shortened version.

Btw - I've now found a compliant rasdaman based endpoint, NL elevation data: https://coverage.wetransform.eu/rws/hoogte_nl_1m/2023-08/ows#/services

As we're discussing INSPIRE, any reason I've been locked out of https://inspire.rasdaman.org/rasdaman/ows? ok, so every service will have a  new setting called `inspire_dls_spatial_dataset_identifier` in petascope.properties when one can define the name of the service, here it is `FAIRiCUBE`

```
<inspire_dls:SpatialDataSetIdentifier>
		<inspire_common:Code>FAIRiCUBE</inspire_common:Code>
	</inspire_dls:SpatialDataSetIdentifier>
```


> As we're discussing INSPIRE, any reason I've been locked out of https://inspire.rasdaman.org/rasdaman/ows?

Please always post the request and output you tried, no one knows what you meant without the context, this service is public accessible. petascope is updated on https://fairicube.rasdaman.com/rasdaman/ows, now it has this element in WCS GetCapabilities response:

```
<inspire_dls:SpatialDataSetIdentifier>
		<inspire_common:Code>FAIRiCUBE</inspire_common:Code>
	</inspire_dls:SpatialDataSetIdentifier>
```
I don't see any other things which I can do in petascope for XML validation, if you think that as well then this ticket can be closed. many thanks - the inspire_dls part of the XML is now valid.

I'll close providing a summary of the status of XML validation shortly
Here an overview of the issues with XML on WCS. For details on failed validation, see full [protocol](https://github.com/FAIRiCUBE/FAIRiCUBE-Hub-issue-tracker/blob/main/Example%20Files/CoverageEncoding/WCS%20XML%20Issues.txt)

Based on the statements above, there seems to be no outlook on getting these issues resolved, the update to WCS 2.1 just brought new issues. Conclusion: WCS cannot be utilized if valid XML is required.

## WCS 2.0
- **GetCapabilities: Fail**
  - ows:BoundingBox only supports double values as common in spatial bounding boxes, not suitable for provision of temporal information. Also not suitable for categorical dimensions.
  - wcs:CoverageId doesn't support the identifiers stemming from ds.earthserver.xyz as not NCName

- DescribeCoverage 
  - grassland_status_virtual_coverage_index: valid
  - **ds.earthserver.xyz:7000:C3S_satellite_soil_moisture_active_daily_sensor: Fail**
    - ID not NCName (causes many issues)
	- Timestamp not valid in gml:Envelope, gml:pos
  - **near_surface_air_temperature: Fail**
    - Timestamp not valid in gml:Envelope, gml:pos, gmlrgrid:coefficients
  - corine_land_cover_virtual_coverage_index: valid
	
- GetCoverage
  - **grassland_status_virtual_coverage_index: Fail**  
  - ds.earthserver.xyz:7000:C3S_satellite_soil_moisture_active_daily_sensor: works via the GUI, but not via WCS request
  - near_surface_air_temperature - 1day slice: Valid
  - **near_surface_air_temperature - 3day subset: Fail**
    - Timestamp not valid in gml:Envelope, gml:pos, gmlrgrid:coefficients
  - corine_land_cover_virtual_coverage_index: Valid

## WCS 2.1
- **General: Schema construction for WCS 2.1 is corrupt, utilizes concepts from 2.0 instead of including within 2.1**

- **GetCapabilities: not available for WCS 2.1**

- DescribeCoverage 
  - grassland_status_virtual_coverage_index: valid
  - **ds.earthserver.xyz:7000:C3S_satellite_soil_moisture_active_daily_sensor: Fail**
    - wcs20:CoverageId must be NCName
  - near_surface_air_temperature: Valid	
  - corine_land_cover_virtual_coverage_index: Valid	
  
- GetCoverage
  - **grassland_status_virtual_coverage_index: Fail**  
  - ds.earthserver.xyz:7000:C3S_satellite_soil_moisture_active_daily_sensor: works via the GUI, but not via WCS request
  - near_surface_air_temperature - 1day slice: Valid
  - near_surface_air_temperature - 3day subset: Valid  
  - corine_land_cover_virtual_coverage_index: Valid	
reopening, as this requires more elaboration.

- it wildly mixes, in its presentation, concerns about rasdaman specifically with concerns about OGC standards generally. Needs clean disentangling (separate issues) so that an ordered response is possible.
- under the headline "XML" several concerns are mixed, including calendar addressing of which the writer of the rant is informed that it is under development
- previous explanations are simply ignored, statements are made with a tone that seems to seek points of accusation. We should strive for an amicable collaboration, rather than provocation. Repeating here what has been observed earlier. from a FAIRiCUBE perspective, this test addressed the validity of the XML provided by the WCS instance powered by rasdaman. This does not require an analysis of the source of the error, just the existence. The four different coverages on which tests were performed were selected to represent the different coverage structures being provided.

As for the errors ensuing from the provision of ISO formatted date strings in a field defined as double, while it is appreciated that work is now underway enabling spatio-temporal cubes (to date only spatial was possible), we will encounter the same underlying issue in the WCS standard when we advance to categorical dimensions, as required for the Occurrence Cubes.

In addition, the lack of a GetCapabilities Response for WCS 2.1 in addition to the mix of 2.1 and 2.0 concepts in the 2.1 schema is concerning.

Thus my conclusion that valid XML encoding of spatiotemporal coverages is not possible under the OGC WCS standards the way they are currently defined.
coming back with some insights.

- "works via the GUI, but not via WCS request" - no documentation was provided, but we guess that URL-escaping was forgotten
- "GetCapabilities: not available for WCS 2.1" - needs an update in the standard; good catch, but probably difficult  
- "Trimming subset in WCS must follow this syntax 'lowerBound,upperBound', given '2019-08-04T12:00:00.000Z' " - likely quotes were forgotten
- "Failed internal rasql query on virtual coverage" - recommended to read the error message this query returns, selected data was too big (restricted to 200 MB). Let us know if you want to download Big Data, we can change quota anytime.
- "the component coverages are no longer available, just the resampled data from the virtual_coverage_index" - they are there, but have been blacklisted to improve overview, a concept explained earlier. If you want to see them visibly listed, let us know.

PS: The Technical Coordinator might consider  [common IT bug report standards](https://www.browserstack.com/guide/how-to-write-a-bug-report).  kind sir, example failing request:
 https://fairicube.rasdaman.com/rasdaman/ows?&SERVICE=WCS&VERSION=2.1.0&REQUEST=GetCoverage&COVERAGEID=imperviousness_virtual_coverage_index&SUBSET=time(2015,2018)&SUBSET=Y(3000000,3000020)&SUBSET=X(3000000,3000020)&FORMAT=application/gml+xml&outputType=GeneralGridCoverage

PS: the WP5 responsible may wish to first validate data before providing 

your example failed because you selected two timeslices: 2015 and 2018 on a virtual coverage. You know already that this virtual coverage is a reference two source coverages: _2015 and _2018 which have different resolutions and internally rasdaman need to project both of them to be unified.

That is why you got the error below (if you don't put anything else, so I assume you used curl and your username is `fairicube_nhm`):

```
curl -u fairicube_nhm:PASS  'https://fairicube.rasdaman.com/rasdaman/ows?&SERVICE=WCS&VERSION=2.1.0&REQUEST=GetCoverage&COVERAGEID=imperviousness_virtual_coverage_index&SUBSET=time(2015,2018)&SUBSET=Y(3000000,3000020)&SUBSET=X(3000000,3000020)&FORMAT=application/gml+xml&outputType=GeneralGridCoverage'

```

and you have error because your user has limited quota

```
<ows:ExceptionText>Data download is restricted to 200 MB.</ows:ExceptionText>
```


If you selected only one time slice, it works fine, e.g.


```
curl -u fairicube_nhm:PASS  'https://fairicube.rasdaman.com/rasdaman/ows?&SERVICE=WCS&VERSION=2.1.0&REQUEST=GetCoverage&COVERAGEID=imperviousness_virtual_coverage_index&SUBSET=time(2015)&SUBSET=Y(3000000,3000020)&SUBSET=X(3000000,3000020)&FORMAT=application/gml+xml&outputType=GeneralGridCoverage'

```






- For this test, I accessed data via the GUI, but I get the same results via CURL (how I did all the tests above - btw - your curl syntax doesn't work on windows)
- I fail to understand how I go over the 200MB limit when requesting a 20x20m subset of a 10m grid. For 1 year, I get 4 values. Over the 5 years for which we have data, this comes to 20 values, quite a bit below 200MB
- While I'm aware of the problem with the different resolutions as I've been working with this dataset for a while, but this fact is not indicated anywhere. The component coverages are no longer available, so one cannot check these. What is the purpose of putting all the years in one cube when one can then not query over years? DescribeCoverage states 10m resolution, how should a user understand that this information is incorrect? on Windows you need to use: `curl -u 'user:pass'` (on Linux it doesn't require ' character). That is what you told me in another ticket before.

> The component coverages are no longer available, so one cannot check these.
No, they are still available. 

They are hidden from GetCapabilities but still there, if you look at the metadata  of `imperviousness_virtual_coverage_index` you see the source coverages there.

```
                    <PartitionSet>
                        <Partition>
                            <CoverageRef>imperviousness_2006_index</CoverageRef>
                        </Partition>
                        <Partition>
                            <CoverageRef>imperviousness_2009_index</CoverageRef>
                        </Partition>
                        <Partition>
                            <CoverageRef>imperviousness_2012_index</CoverageRef>
                        </Partition>
                        <Partition>
                            <CoverageRef>imperviousness_2015_index</CoverageRef>
                        </Partition>
                        <Partition>
                            <CoverageRef>imperviousness_2018_index</CoverageRef>
                        </Partition>
                    </PartitionSet>
                
```

Example for you:

```
curl -u 'fairicube_nhm:PASS'  'https://fairicube.rasdaman.com/rasdaman/ows?&SERVICE=WCS&VERSION=2.1.0&REQUEST=DescribeCoverage&COVERAGEID=imperviousness_2015_index'

```

Where is it documented that these component coverages, not displayed under GetCapabilities, can be queried by DescribeCoverage?

In addition, I still don't understand why CU insisted on merging these different resolution coverages if they cannot be queried over time.

And yes, many thanks for providing the Windows curls I've been using for all this work It is documented in rasdaman enterprise https://doc.rasdaman.com/stable/05_geo-services-guide.html but you don't have access to it.

Source coverage of a virtual coverage are hidden by default to not have so many coverages in GetCapabilities. But if you are interested in source coverages, then just look into metadata of the virtual coverage to find out and Get data from them.

For this 

> I fail to understand how I go over the 200MB limit when requesting a 20x20m subset of a 10m grid. For 1 year, I get 4 values. Over the 5 years for which we have data, this comes to 20 values, quite a bit below 200MB

Dimitar from rasdaman team has found that in rasql and this will be improved in the next release so it will not say you are querying that big data size.
> In addition, I still don't understand why CU insisted on merging these different resolution coverages if they cannot be queried over time.

We found a bug in rasdaman causing the error you've been getting, we'll work to fix that soon.
> "Trimming subset in WCS must follow this syntax 'lowerBound,upperBound', given '2019-08-04T12:00:00.000Z' " - likely quotes were forgotten to clarify on Peter's response here, I think you did have double quotes in your request but didn't URL-escape them, and the client with which you made the request removed them. If your request arrived with double quotes to our endpoint, then you'd have gotten an error from Tomcat about malformed URL. many thanks for the link to the inaccessible documentation! are all these caveats documented anywhere? Maybe in the enterprise documentation Bang mentioned, where we don't have access???

In any case, the result is that the UC partners are increasingly desperate, only dataset they can currently use on rasdaman is the near-surface-temperature - thin result for 17 months of work :(
Which caveats in particular? This thread is pretty long and I'm not sure what you refer to.

I'll send you credentials to access the documentation.
> only dataset they can currently use on rasdaman is the near-surface-temperature

I acknowledged that we are aware of an issue with accessing the imperviousness data, and are working to address that. Which other datasets precisely are not accessible?

We already pointed out that many of your requests failed because you didn't URL-encode the requests. This has nothing to do with rasdaman: some characters are not allowed in URLs and such characters must be percent encoded, such as double quotes; documentation here: https://en.wikipedia.org/wiki/Uniform_Resource_Identifier#Syntax I whitelisted source coverages of local virtual coverages on [fairiceu](https://fairicube.rasdaman.com/rasdaman/ows) so you will see them in GetCapabilities, e.g. `imperviousness_2006_index`
Good move keep in mind to whitelist the source coverages when creating virtual coverages in future, as wcst_import blacklists them by default. accessing `imperviousness_virtual_coverage_index` has been fixed, so your request will work now without triggering an error. federated coverage Ids on https://fairicube.rasdaman.com/rasdaman/ows now is also valid NCName (before it has `:` character which is not valid in this pattern).

I suggest that you revise your report at https://github.com/FAIRiCUBE/FAIRiCUBE-Hub-issue-tracker/blob/main/Example%20Files/CoverageEncoding/WCS%20XML%20Issues.txt.

We've tried friendly to help you with your issues.

The one which is out of hands is with ISO DateTime format supported from WCS schema and missing WCS 2.1. GetCapabilities schema, but this will be brought to OGC next meeting next year.
seems this issue is not monitored any longer by the creator. We have put much effort into explaining the situation wrt. standards and hope it has been understood. So closing. reopening this issue as it was not resolved when it got closed. [Full overview here](https://github.com/FAIRiCUBE/FAIRiCUBE-Hub-issue-tracker/issues/26#issuecomment-1828017430)

With the switch to CIS 1.1 encoding, the issues on DescribeCoverage and GetCoverage have been resolved. However, the problem with invalid XML in the GetCapabilities response persist. I've now taken a closer look at the OWS specifications (OGC 06-121r3), see a way out:
In `Table 21 — Minimum parts of DatasetSummary data structure`, we find the following specification for boundingBox:

| Names  | Definition | Data type |  Multiplicity and use |
| ------------- | ------------- | ------------- | ------------- |
| bounding Box - Bounding Box | Minimum bounding rectangle surrounding dataset, in available CRS <sup>b</sup> | BoundingBox data structure, see Subclause 10.2 |  Zero or more (optional) Include when relevant and available <sup>c</sup> |

**b** More generally, definition of the horizontal, vertical, and temporal extent of this specific dataset. Zero or more BoundingBoxes are allowed in addition to one or more WGS84 Bounding Boxes to allow more precise specification of the Dataset area in Available CRSs.
**c** If multiple bounding boxes are included with the same CRS, this shall be interpreted as the union of the areas of these bounding boxes.

The first thing I notices what that the bbox is to be provided in `available CRS`. As to my understanding, there are currently no valid temporal CRS (AnsiDate has serious issues), I read this to mean that we don't need to include temporal aspects in the bbox.

This leaves open the question of how/where to provide a non CRS bbox for these additional dimensions. Looking for possible extension points, see we have the options:
- ows:AbstractMetaData: this would allow for the provision of any structure, one could do a parallel of the OWS bbox, but defining the arrays for LowerCorner and UpperCorner as Strings to provide a bbox for the non-CRS dimensions
- AdditionalParameter: Name/Value pairs, whereby the Value can be multiple. For each non-CRS dimensions, one could provide an AdditionalParameter entry with 2 values for Lower and Upper
  - For Nominal Dimensions, all values could be provided. it's certainly an issue that the GetCap response doesn't validate.

The schema still wouldn't allow datetime strings regardless of whether a valid temporal CRS exists or not, right? I'm not sure this point is really relevant.

Putting some parts of the bbox separately elsewhere will create confusion. In my opinion the correct solution is to update the schema, or alternatively maybe think of a number encoding for datetime strings. the OGC bbox elements foresee an array of double, works well for coordinates, breaks with timestamps as strings. Thus, I see 2 ways forward:
- taking all dates down to POSIX time, as foreseen by the temporal SWG (I don't like this approach, but for POSIX there are functions that the users can apply locally to get the magic number from the date)
- creating a parallel bbox structure for the non-numeric components

While the current approach mostly works in a prototypical environment, where users manually access data via URIs, this will break when automation is applied (from my experience when using professional XML tools, if the XML is invalid, no further steps are possible). While one can revert to parsing the XML as a text file, then figuring out what's a string is technically feasible, this solution is far from satisfactory.

I'd be all for updating the schema, but have seen no activity in this direction in the OGC, where the focus is on the APIs. In addition, there has been a recent statement by Ingo Simonis, OGC Chief Innovation Officer that `Web APIs are currently receiving full attention and that W*S is not being actively developed further` 

Do you see any outlook here, or must we accept this deficit in the standard?