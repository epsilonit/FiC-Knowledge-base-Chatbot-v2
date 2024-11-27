Wormpicker Result Validation with rasdaman slicing via WCPS/WCS requests
Hi everybody, 
I am glad we had such a constructive meeting today. In the attachment I've put the output of the Wormpicker, based in the way it is coded at the moment. There is also a log file generated, from which one can read the input parameters, generated query, calculated grid indices and others parameters.

I would be glad if one could check if the generated data and values are correct, in order to decide how to proceed. 
Apologies for the non-optimal structure of the log file. If you have questions let me know.

[IndexBasedResult.csv](https://github.com/user-attachments/files/16030283/IndexBasedResult.csv)
[WormpickerRequest.log](https://github.com/user-attachments/files/16030284/WormpickerRequest.log)

We had a quick look and we think that you may have swapped the original coordinates. E.g. in the first request in the log

```
COORDINATES: 46.8136889 13.50794792
```

Is 46.8136889 lon or lat? We think it's lat = Y, but as far as I can tell you've translated it to a subset on X. When you swap them you should get a valid value for the first request at least with this query:

```
for $c in (corine_land_cover) return 
encode( $c[time("2016-08-01T00:00:00Z"), Y:"EPSG:4326"(46.8136889), X:"EPSG:4326"(13.50794792) ], "text/csv")
``` with Dimitar's comment on https://github.com/FAIRiCUBE/uc3-drosophola-genetics/issues/2#issuecomment-2197085734. It answered your issue why you got `null` values in most of your requests.

Can you please confirm that?
> with Dimitar's comment on [#2 (comment)](https://github.com/FAIRiCUBE/uc3-drosophola-genetics/issues/2#issuecomment-2197085734). It answered your issue why you got `null` values in most of your requests.
> 
> Can you please confirm that?

Sonja is on holidays and will respond when she is back 
Hi all.
I could not find a direct axis confusion in the code but a switch up in resolution of axis positive and negative value (+/- ) . However, I found that the way I was sending requests was using grid indiced based on the formula provided by rasdaman last year with the following format:

**for $c in (corine_land_cover) return encode($c[time("2016-08-01T00:00:00Z"),X :"CRS:1"(17397:17397),Y :"CRS:1"(9112:9112) ], "text/csv")**

seems to provide the non-correct values, independet from resolution switch up.
I realised now with your example query, that our formula is obsolete, since even though the layer is encoded in a different CRS, it is possible to query the coordinates in EPGS:4326 now. 
I adjusted the code to use the suggested format.             

> for $c in (corine_land_cover) return 
> encode( $c[time("2016-08-01T00:00:00Z"), Y:"EPSG:4326"(46.8136889), X:"EPSG:4326"(13.50794792) ], "text/csv")

I now also changed the code to take the information of covered area exclusively from the XML field ['ows:WGS84BoundingBox'] in order to et the "NOT COVERED".

Could you have a look again, if now the results are valid and also if the "NOT COVERED" statement is correctly applied?

I suspect querying the grid-index based way is now obsolete?
It would be really really helpful to have this documented in https://tutorial.rasdaman.org or similar. The [WCPS tutorial link](https://tutorial.rasdaman.org/rasdaman-and-ogc-ws-tutorial) is broken.


I guess these very detailled changes are part of why we always have struggle to use our already implemented code. 

We already touched last time, but now I ran into the issue again, that a blueprint of the XML schema in general might be really helpful. I figured that my code is not reading information correctly for certain layers that are not coverage subtype "ReferenceableGridCoverage" like e.g. near_surface_air_temperature. 


[WormpickerRequestWGS.log](https://github.com/user-attachments/files/16068446/WormpickerRequestWGS.log)
[4326BasedResult.csv](https://github.com/user-attachments/files/16068448/4326BasedResult.csv) Dimitar has vacation this week and will reply from next week. I reply some thing here based on my opinion.


In `4326BasedResult.csv` it is hard to understand what you tested:

```
Sample,corine_land_cover,dominant_leaf_type,dominant_leaf_type_2012,dominant_leaf_type_2015,eu_demography_2018
AT_Kar_See_1_2014-08-17,2014-08-17T00:00:00Z,112,{0},0,NOT COVERED,NOT COVERED
```

I suggest that you could change to this format instead:

```
sample,               lat,          lon,               date_slice,                  corine_land_cover,     dominant_leaf_type,     dominant_leaf_type_2012,    dominant_leaf_type_2015,     eu_demography_2018
AT_Kar_See_1,         46.8136889,   13.50794792,       "2016-08-01T00:00:00Z"       112,                   {1}                     NOT COVERED,                1                            NOT COVERED
```

with the WCPS query used to get the value from each tested coverage below:

```
for $c in ( COV ) return
encode( $c[time("2016-08-01T00:00:00Z"), Y:"EPSG:4326"(46.8136889), X:"EPSG:4326"(13.50794792) ], "text/csv")
```

with `COV` is one of `corine_land_cover, dominant_leaf_type, dominant_leaf_type_2012, dominant_leaf_type_2015, eu_demography_2018`


In this test, yes, it is correct that `dominant_leaf_type_2012` and `eu_demography_2018`  coverage DO NOT cover the time slicing when you look at their temporal bounds via `DescribeCoverage` request.

-  `dominant_leaf_type_2012`  has extent `["2012-01-01T00:00:00.000Z", "2014-12-31T23:59:59.999Z"]`
- `eu_demography_2018` has extent `["2018-01-01T00:00:00.000Z", "2020-12-31T23:59:59.999Z"]`


FYI: the results of `DescibeCoverage` with WCS 2.0.1 are followed by OGC WCS standard, so you will see quite different XML for:
- `RectifiedGridCoverage` (e.g. `near_surface_air_temperature`)  - This coverage has **regular** `time` axis
- `ReferenceableGridCoverage` (e.g. `corine_land_cover`) - This coverage has **irregular** `time` axis

You should use new WCS requests instead which will return results in `CIS` (Coverage Implementation Schema version 1.1). The XML of these two below request will have only **one difference** in XML schema in term of `IrregularAxis` and `RegularAxis` for `time` axis:

- https://fairicube.rasdaman.com/rasdaman/ows?&SERVICE=WCS&VERSION=2.1.0&REQUEST=DescribeCoverage&COVERAGEID=corine_land_cover&outputType=GeneralGridCoverage
- https://fairicube.rasdaman.com/rasdaman/ows?&SERVICE=WCS&VERSION=2.1.0&REQUEST=DescribeCoverage&COVERAGEID=near_surface_air_temperature&outputType=GeneralGridCoverage

First coverage `corine_land_cover`  has

```
<cis11:IrregularAxis axisLabel="time" uomLabel="d">
<cis11:C>"1990-01-01T00:00:00.000Z"</cis11:C>
<cis11:C>"2000-01-01T00:00:00.000Z"</cis11:C>
<cis11:C>"2006-01-01T00:00:00.000Z"</cis11:C>
<cis11:C>"2012-01-01T00:00:00.000Z"</cis11:C>
<cis11:C>"2018-01-01T00:00:00.000Z"</cis11:C>
</cis11:IrregularAxis>
````

Second coverage `near_surface_air_temperature` has

```
<cis11:RegularAxis axisLabel="time" uomLabel="d" lowerBound="2010-12-31T12:00:00.000Z" upperBound="2018-12-31T12:00:00.000Z" resolution="1"/>
```

Based on that, you just need to parse for `IrregularAxis` and `RegularAxis` with the new XML of CIS 1.1 instead of struggling with the old XML format of `WCS 2.0.1` with CIS 1.0. I have some questions for you:

- You have some test samples (test locations with coordinates and with some timeslices), and you wanted to test that with **some specified** coverages or **ALL** Fairicube coverages? 
  - In my opinion, the tests are valid only if the time slice and the the geo extents exist in the coverage's extents (e.g. coverage `dominant_leaf_type_2012` has time extent `["2012-01-01T00:00:00.000Z", "2014-12-31T23:59:59.999Z"]`, then input time slicing should be between `"2012"` and `"2014"` only)

- You **always use coordinates in EPSG:4326** and **NOT use the coverage's native CRS** (e.g. `corine_land_cover` has `EPSG:3035` which has irregular swap to `Y,X` order) for doing WCS/WCPS requests?
  - If you always use `EPSG:4326` then, you will need to know the most common axis names from CRS defininion:
    - Axis with type `X` labels: `Long`, `Lon`, `long`, `lon`, `X`, `E`
    - Axis with type `Y` labels: `Lat`, `lat`, `Y`, `N`
   - Then, you will know where to pass coordinates from your longitude and latitude coordinates accordingly. For example, you have `AT_Kar_See_1`  with `lat (Y axis) = 46.8136889` and `lon (X axis) = 13.50794792`, and `corine_land_cover` has `Y` and `X` axes, so you passed subset `$c[time("2016-08-01T00:00:00Z"), Y:"EPSG:4326"(46.8136889), X:"EPSG:4326"(13.50794792) ]`



Thank you for your detailed answer.

1.)	I've updated the format to include sample ID, latitude, longitude, date_slice, etc., I will upload a new version soon in this repo with the next sync.

2.)	The queries used will then be listed in the log file to prevent them from cluttering the CSV.

3.)	Temporal coverage:
Even if we see beforehand that the temporal coverage is not given for all of our samples in certain layers, it is useful for us to confirm via the Code as well and to check that we do not get other data because of faulty workflow.  We want to address the "not covered" data explicitly, so that we can also know which information actually is valuable for our Use Case. It’s important to have a clear table indicating which samples lack data coverage so we can choose datasets that are complete enough for reliable statistical testing.
I will have a look into the topic of  IrregularAxis and RegularAxis next week, thanks for providing the links!

Concerning your questions: 
1.	 These samples are our actual samples for analysis, and we aim to investigate the environmental conditions corresponding to the exact sampling dates. If a sample isn’t covered temporally, we are open to using approximation methods, but we need to determine which approximation methods are even valid. It’s crucial to know if many samples lack temporal coverage within a dataset because then we can estimate the amount of missing data and temporal distance. 
2.
3.	Until recently, our workflow was as follows (Index based):
-	Input coordinates for our samples in EPSG:4326.
-	Retrieve the original CRS from the layer.
-	Convert the sample coordinates to coordinates of the corresponding CRS with gdal.
-	Use these converted coordinates to get grid indices based on the formula provided by rasdaman.
-	Query the layer information for the grid indices of the sample coordinates.
The code dynamically adjusts axis names based on the layer information to avoid mismatches. For example, if an axis is labeled “X,” our script sends a query using “X.” If the axis labels is lon the query will contain lon etc. 

Do I now need to change this and keep **Y:"EPSG:4326"(coordinates) and X:"EPSG:4326"(coordinates) fixed**, independent from axis labelling and CRS?
Or would you recommend to use  **AxisLabelY:"CRS"(transformed coordinates) and AxisLabelX:"CRS"(transformed coordinates)**? thanks for your answer to my questions.

Regarding to your workflow:

> Convert the sample coordinates to coordinates of the corresponding CRS with gdal.
> Use these converted coordinates to get grid indices based on the formula provided by rasdaman.

Soon during this summer, we will have a major release which will have a new fixed formula which returns consistent `geo` to `grid` coordinates. So you don't have to do these two steps above (i.e. you just use slicing with EPSG:4326 coordinates directly). Currently, you can use that already in WCPS queries to fairicube petascope, although the current formula will  in some cases might return a grid pixel which is shifted by one (i.e a bit inconsistent).
 
First, you need to check the geo axis labels of a coverage to know which axis label is type `X` and which one is type `Y`. You can simply rely to the **axis label names** from WCS DesribeCoverage result in CIS 1.1:

- https://fairicube.rasdaman.com/rasdaman/ows?&SERVICE=WCS&VERSION=2.1.0&REQUEST=DescribeCoverage&COVERAGEID=LGN&outputType=GeneralGridCoverage
  - It has `axisLabels="time X Y"`
- https://fairicube.rasdaman.com/rasdaman/ows?&SERVICE=WCS&VERSION=2.1.0&REQUEST=DescribeCoverage&COVERAGEID=corine_land_cover&outputType=GeneralGridCoverage
  - It has `axisLabels="time Y X"`

From that `axisLabels` string, you can extract the geo axis labels, they are typically named as these constant values:
- `AXIS_TYPE_X` labels: `Long, Lon, long, lon, X, E`
- `AXIS_TYPE_Y` labels: `Lat, lat, Y, N`

So with two imported coverages, you will know that a coverage is `XY` or `YX` axis order (although it is not important for you as you only need to know which axis is type `X` and which one is type `Y`):
- `LGN` is `XY` order with CRS `EPSG:28992`
- `corine_land_cover` is `YX` order with CRS `EPSG:3035` 


So, given a pair of coordinates in `EPSG:4326`: 
- `lon` (axis type `X`): 13.50794792
- `lat` (axis type `Y`): 46.8136889

When you determined which axis label is type `X` , which axis is type `Y`  from a coverage, you can just request with WCPS with the geo subset directly in `EPSG:4326`:

```
$c[time("2016-08-01T00:00:00Z"), 
   AXIS_LABEL_WITH_TYPE_X:"EPSG:4326"(13.50794792) 
   AXIS_LABEL_WITH_TYPE_Y:"EPSG:4326"(46.8136889), ]
```

For example with `corine_land_cover`, the WCPS query would be:

```
for $c in ( corine_land_cover ) return
encode( $c[

time("2016-08-01T00:00:00Z"),
   X:"EPSG:4326"(13.50794792) ,
   Y:"EPSG:4326"(46.8136889)

],
 "text/csv")
``` I got a bit out of the loop on this discussion last week, but I think Bang has been helpful. Please let us know if you still have any problems or things to be clarified.
Thanks for your answers, I will have a look into it as soon as possible and come back to you! :)  FYI: new petascope with the consistent formula deployed at https://fairicube.rasdaman.com/rasdaman/ows
How is it going with your check?
For reference, the consistent formula that Bang refers to is explained [here](https://nilu365.sharepoint.com/sites/Horizon2021_CUBE/_layouts/15/Doc.aspx?sourcedoc=%7B235313bb-424e-4a1e-b1d6-92296d28fbfc%7D&action=edit&wd=target%28use%20cases.one%7C3de8cdde-47ec-4a25-9bf1-cd0f21c90526%2FRasdaman%20subsetting%20behavior%7Cdc4d8197-2cda-483e-a9b8-7b6536a4cac0%2F%29&wdorigin=NavigationUrl).
Hi,
sorry for my delayed response, there was a lot of urgent tasks followed up by holidays.

So I was working on that topic again today, and I would say the way query construction is taking **axisLabels** as string from the xml. Since conversion to grid indices is not needed anymore, and all our sample coordinates are given in time EPSG:4326,  it can be parsed in the suggested formula independent from the order, right? 

I ran it again with some layers and it seems to query for the right values (which does not avoid samples not being covered geographically by the layer or it being null values). I will provide again some data, if you could spot check if these are correct once more I would be very thankful.

In this file, you can se the result for one sample for multiple layers, giving "good" values for near_surface_air_temperature and ERA5 bands, but the others are null values.
[Query.log](https://github.com/user-attachments/files/16600573/Query.log)



If the querying is correct and the null values are no conceptual mistake then we can continue with a more sophisticated way of validating the data.
Thanks :) thanks for your log file. For your question

>  it can be parsed in the suggested formula independent from the order,

Yes, if you put correct coordinates for `Lat` and `Long` to `Y` and `X` axes types respectively, then it will work. Certainly, it will not work, if you put coordinate for `Lat` to axis `X` for example, that will returns error / wrong result.

For this file https://github.com/user-attachments/files/16600573/Query.log, there are a few things:

1. Coverages (like `near_surface_air_temperature`, `ERA5`) which are already in CRS EPSG:4326 with `lat` and `lon` axes,  you don't need to add subsetting CRS expression `:"EPSG:4326"`, e.g.

```
for $c in (near_surface_air_temperature) return encode($c[time("2014-08-24T00:00:00Z"),lon :"EPSG:4326"( 23.50280556 ), lat:"EPSG:4326"( 49.32991667) ], "text/csv")
```

You can just change to:

```
for $c in (near_surface_air_temperature) return encode($c[time("2014-08-24T00:00:00Z"),lon( 23.50280556 ), lat:( 49.32991667) ], "text/csv")
```


2. `ds.earthserver.xyz--7000--ERA5` has `time lat lon` axes, not `time lon lat` axes.

3. You put correct subsets for coverage `near_surface_air_temperature` with: 

```
for $c in (near_surface_air_temperature) 
return encode($c[time("2014-08-24T00:00:00Z"),lon :"EPSG:4326"( 23.50280556 ), lat:"EPSG:4326"( 49.32991667) ], "text/csv")
```

but it is incorrect (note that coordinates for `lat` and `lon` are swapped) for coverage `ds.earthserver.xyz--7000--ERA5` with:

```
for $c in (ds.earthserver.xyz--7000--ERA5) 
return encode($c[time("2014-08-24T00:00:00Z"),lat :"EPSG:4326"( 23.50280556 ), lon:"EPSG:4326"( 49.32991667) ], "text/csv")
```

4. Subsets lat/long: `49.32991667, 23.50280556` touches a place in Ukraine (not part of EU) which has no data value, hence, it returns null values. You should select a pair of `lat/lon` coordinates (e.g. `46.8136889, 13.50794792`)   in Oestereich instead.



Okay, I see it now and fixed that, thank you!