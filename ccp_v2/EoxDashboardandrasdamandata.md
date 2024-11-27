Eox-Dashboard and rasdaman data
as discussed during today's UC Synergy meeting, we'd like to test accessing and visualizing data from rasdaman into the Eox-Dashboard.

As an example dataset, we've identified the LGN dataset provided by at WER:
- STAC Record: https://stacapi.eoxhub.fairicube.eu/collections/index/items/LGN
- WCS DescribeCoverage: https://fairicube.rasdaman.com/rasdaman/ows?&SERVICE=WCS&VERSION=2.1.0&REQUEST=DescribeCoverage&COVERAGEID=LGN&outputType=GeneralGridCoverage it seems the item also describes a WMS endpoint which currently is used for the "thumbnail" generation, we could use this endpoint for integrating visualization of the data into the dashboard.

My question would be, what intent is behind the WCS access? What would be expected from that, the possibility to download the raw data? Thanks for having a look at this issue. The ultimate goal is to be able to mix data retrieved from EOX data cubes and Rasdaman data cubes and use it for analysis, e.g. in a Jupyter notebook. As much on-the-fly / in-memory as possible, avoiding having to copy files between systems or creating local files.  the aspect of multi data cube analysis within an environment (such as Jupyter) notebook is out of scope for the dashboard visualization that was discussed with .
I think this needs to be discussed in another thread / topic.

We currently investigated easier integration of rasdaman provided services into eodash, this was now implemented as an additional [resource](https://github.com/eurodatacube/eodash-catalog/wiki/Resource) in the eodash_catalog generator so that visualization of the data is possible in an eodash dashboard instance.  we currently have the UC1 dashboard and catalog repositories, could i add there the LGN data integration example? is there a UC2 dashboard available? I'd like to avoid polluting the UC1 dashboard.

As accessing data from external Web Services or APIs is a fairly general task, I'm wondering if we shouldn't create a dedicated dashboard for this as an example, in addition provide a short tutorial under our [RTD pages](https://fairicube.readthedocs.io/en/latest/). a UC2 dashboard has been setup, it can be found here:
https://fairicube.github.io/uc2-eodash-client/?indicator=LGN
The UC 2 catalog repository can be found here:
https://github.com/FAIRiCUBE/uc2-eodash-catalog/
The eodash instance repository can be found under:
https://github.com/FAIRiCUBE/uc2-eodash-client/

How a rasdaman WCS source can be added is described in the catalog generator wiki (plus additional general information on how to configure new datasets):
https://github.com/eurodatacube/eodash-catalog/wiki/Resource#rasdaman-wcs
How eodash can be instantiated is described here:
https://eodash.org/first_steps.html
Maybe these resources can be added to the pages?

If you agree i would propose to close this ticket trying to understand, is my assumption correct that for each dataset you wish to display with EODash you must first add it to the UC collection under the UC 2 catalog repository? Assuming this is totally separate from the FAIRiCUBE Catalog? Is this the same for datasets stored on S3 or available via Sentinel-Hub? how do you see this solution? yes, correct, there are configurations that can be done for SentinelHub and for supported open datasets on a bucket and is separate from the current FAIRiCUBE Catalog. 
A potential option could be to use the eodash catalog generator to create the base (STAC files) for the FAIRiCUBE Catalog (or somehow update it) I think it could be ok as a use case specific 'end' product, e.g. if at the end we can do story telling with specific datasets. Including end results from analysis. Looks good, so closing this issue
I've created a new issue #82 on providing documentation.