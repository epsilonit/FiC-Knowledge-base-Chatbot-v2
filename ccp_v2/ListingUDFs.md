Listing UDFs
There is work in progress on providing a listing of available UDFs from a rasdaman instance. Still unclear if this will be a dedicated request, or integrated into the GetCapabilities of the WCS.

Please provide updates on progress here
This has been implemented in rasdaman as a dedicated request, see relevant [documentation](https://doc.rasdaman.com/stable/05_geo-services-guide.html#apis-for-wcps-udf). I guess we can close this issue? formally, I guess one can see this as completed, but I do wonder how useful this approach is. Apart from not taking a standardized approach (as the UDF are executed as part of WCPS, would have expected integrated either there, alternatively via OGC API - Processes), I'm not sure the provided information really helps a potential user. In the simple approach its just a list of names, listdetails provides the following:

dtm.aspect (?...) return ?
dtm.aspect_default (?...) return ?
dtm.hillshade (?...) return ?
dtm.hillshade_default (?...) return ?
dtm.slope (?...) return ?
dtm.slope_default (?...) return ?
example.avg2 (Coverage) return Number
example.quantile (Coverage, Number) return Coverage
fairicube.predictcropclass (?...) return ?
fc.predictcropclass (?...) return ?
filters.coveragefilter (Coverage, Coverage, Coverage, Coverage, Number) return Coverage
image.stretch (?...) return ?
stats.stddev_pop (Coverage) return Number
stats.stddev_samp (Coverage) return Number
stats.var_pop (Coverage) return Number
stats.var_samp (Coverage) return Number how do you see this?
UDFs are not standardized in WCPS and rasdaman does not implement OGC API - Processes, so we report this information with separate API.

Some of the UDFs haven't been updated to include the type information for listdetails, that's why we see the ? in those cases (we'll fix those).
I guess more information can be added per function? Or a reference to documentation about the algorithm used?

But it can also be a question how much documentation is needed for a function that calculates an average or quantiles. The more complex ones can use some description though. Especially if they can not be freely applied to just any data. Which often will be the case for (complexer) models. that's a very reasonable suggestion, we'll think about how to best present UDF documentation in the output of [listdetails](https://fairicube.rasdaman.com/rasdaman/admin/udf/listdetails). What about a JSON structure similar to this?

 ```json
 [
   {
     "name": "stddev_pop",
     "namespace": "stats",
     "input_types": ["Coverage"],
     "output_type": "Number",
     "documentation": "This function calculates the population standard deviation of a given coverage argument."
   },
   {
     ...
   }
 ]
```
Excellent. I would be trying to generate something like that from the UDF source code :-) Or the UDF registration? Anyway, I am sure you find a good method to provide the information.
Yes the UDF code would have a method `getDocumentation()` or so that provides that extra information.