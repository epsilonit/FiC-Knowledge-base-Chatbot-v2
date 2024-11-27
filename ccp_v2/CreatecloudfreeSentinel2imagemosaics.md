Create cloud free Sentinel 2 image mosaics
Hi Dimitar as a part of our UC data collection and pre-processing process we need to prepare cloud free Sentinel 2 image mosaics. I would like to know what you could propose as best solution from Rasdaman perspective and possibilities available. In fact we need 4 Sentinel2 bands (R,G,B,NIR) which are in 10m resolution for the years 2014, 2018 and 2022 and in 4 different seasonal time steps for each year (roughly Feb, Apr, Jun, Sep). It makes in total 48 image layers for the area roughly 110 x 90 km in the central Netherlands. The main steps in the workflow are:
1. Data collection
2. Cloud and cloud shadow masking based on S2 Quality assessment layer 
3. Clod free image parts mosaicking
Would we need to make WCPS request or what would be the best way to get data needed?
Thanks.
Marian this morning I understood that you'd like the NDVI from these cloud corrected layers either as a WCPS request or a UDF. Now reading just the base data cloud-corrected. Also missing everything we discussed on this being a slightly different task from calculating NDVI across EU as
- much smaller spatial scope (thus more likely to find non-obscured images)
- shorter temporal coverage, not for a year, just for a month

:?
Hi calculating NDVI or any other vegetation index is the last and relatively easy step and for sure could be just added at the end of workflow. 
Area of interest is relatively small image mosaics around dates in year I indicated should be sufficient.
Marian could you provide the aoi lat/lon coordinates?
X [136000,223870]
Y [428495,535555]  it’s in EPSG: 28992
Sentinel-2 is in UTM zones, UTM 31/32 I guess for the Netherlands. It will need to be reprojected to EPSG: 28992?
Also, the first data available is from August 2015 or so, there's no Sentinel-2 data for 2014.
We have all the rest of data in EPSG: 28992 so it would be good to have also S2 images.


If there are not S2 images in 2014, I would say it we could use some with lower resolution like Landsat or even lower. Would that be an option with Rasdaman? we don't have Landsat 8/9 in rasdaman, it would be necessary to first download and prepare this data from https://earthexplorer.usgs.gov/

It's not clear to me what dates of source data can be used to make the 2014 feb time step for example? Is it scenes in that particular month?

Btw, the lat/lon bbox is
```
<ows:LowerCorner>5.105442156372468 51.8408106730232755018</ows:LowerCorner>
<ows:UpperCorner>6.4084296850757855487 52.80724811984848</ows:UpperCorner>
``` The goal is to have scenes as close as possible to 15th of the given month (e.g. 15th of February) which are cloud free and covering whole bbox. So if one image has not sufficient coverage due to clouds, remaining parts should be filled with available images further in time to create cloud free seasonal mosaic.

I see that Landsat mosaics we will need then create separately. I think typically there is also some computation involved when filling in missing band values due to clouds, so that the end result is a somewhat harmonised dataset. Can we be specific on that (as part of pre-processing metadata)? I think it is best if you prepare the data as you know best how it should be selected and processed. I can provide you login on the fairicube VM to upload it there. The computation involves cloud masking and creating mosaic. At cloud masking are image pixels of original image replaced by "no data" values at location identified by quality layer as "cloud". Quality layer is normally available with other bands at download. At mosaicking the list of cloud masked images is provided in specific order. At top is the image closest to target date, Images further in list are used to fill "no data" pixels of images higher to create cloud free mosaic. We try to find if there is a way to get image data directly at Rasdaman since they are available and not needed to be ingested and do simple pre-processing before to have analysis ready data to use in models. In case that there are not any pre established processing workflows for cloud masking and mosaicking we could create them maybe. Otherwise way is to download data locally from Sentinel provider, do pre-processing and ingest analysis ready data. 
The base Landsat 8 is not available in rasdaman, we have only Sentinel 2 L2A datacubes. The scenes of interest first need to be downloaded (by you) and imported in rasdaman (by me). The cloud masking can be done afterwards with WCPS queries. if rasdaman does not have access to the data, maybe check what's available on sentinel-hub (if you have problems, maybe ask Maria)

As to the approach of doing as much preprocessing as possible locally, this bypasses FAIRiCUBE. If all the data must be pre-processed locally, probably easier to just get on with things on EOX where more processing is possible can you elaborate on what you mean by "bypasses FAIRiCUBE"? As I explained in my previous comment, the data just needs to be imported as-is in rasdaman, no processing is really necessary. the original plan of FAIRiCUBE was that basic EO data is available via the Hub, further processing takes place there. 

I'm concerned as many UC partners prefer to bypass processing on FAIRiCUBE infrastructure, its easier to do locally with known tools. However, if this is the common approach, no need for FAIRiCUBE, folks can download their data from Copernicus and get on with things, the way they've worked to date
That's still the plan, but we don't have the basic EO data in this case (Landsat) so it needs to be downloaded into FAIRiCUBE first. I see no problem with doing that on the EOX platform, and then importing it in rasdaman so that integrated queries can be done with all the other data UC2 that is already available.
I see that Landsat data has to be downloaded and prepared first locally because it's not available at Rasdaman, For the rest 2/3 of data which is Sentinel we have 2 options:
1. Collect all available images within the bounding box at Rasdaman within given year (if possible, apply max cloud coverage filter) and do cloud masking and mosaicking with WCPS queries
2. Collect all available images with access to Sentinel hub at EOX hub and use there known python tools for cloud masking and mosaicking
I'm fine with either of two options, just trying to find more straight forward solution. We talked shortly about this with Manuel and Maria on last meeting. There is an example notebook at GitHub showing how to access Sentinel hub. We also checked if there is any example code which can do directly cloud masking and creating mosaic within the Copernicus/Sentinel user community but it seems that we have to come just with our own.
Landsat 8 is 30m resolution btw, it will need to be upscaled to 10m I suppose to match the Sentinel-2 data? Yes, we will have to match it with Sentinel and all other data we have. But it's highest resolution which we can have at year 2014. I processed Sentinel-2 for 2018 and 2022 and will make this datacube available on rasdaman. The processing is as follows - let me know if it seems good:

1. collect the relevant Sentinel-2 bands for the months you indicated for 2018 and 2022, intersecting the central Netherlands bbox (including the SCL classification band and CLDPRB cloud probability band)
2. remove scenes with more than 50% cloud cover based on the CLOUDY_PIXEL_PERCENTAGE from the scene metadata
3. create a cloud / shadow and invalid pixels mask, which marks any pixels with CLDPRB > 20% and SCL values different from 2, 4, 5, 6, 7, 11 (these are documented in the Sentinel-2 product guide)
4. the SCL mask contains a lot of bogus small patches of a few pixels: these are removed with an erosion step with a 5x5 kernel
5. the remaining cloudy patches are dilated by 5 pixels to avoid halos or other artifacts as much as possible
6. the mask is applied to the bands
7. the masked bands are reprojected to EPSG: 28992 and clipped to the bbox
8. the multiple scenes per day are mosaiced into one file per day
9. for a whole month, the daily files are stacked by taking the *median* pixel value. I tried first your suggestion for starting from 15th of the month and filling in the masked gaps, but this produces a lot of artifacts. The median or mean method seems to be standard for producing cloud-free mosaics from what I read.

Hi Dimitar The processing workflow looks fine to me. It could be though that there will not be cloud free pixels in a period of one month around the target date in our study area in high cloudy conditions in Netherlands. Then I suppose that this period could be extended. Using median value as you explained sounds good to me. Thanks a lot for this support. Marian you can go to https://fairicube.rasdaman.com/rasdaman/ows, login with your github user, then go to ProcessCoverages tab. Here you can enter and execute WCPS queries. Here are two examples:

1. NDVI map for 2018-06 (you can adapt the date or other details of the formula):
```
for $c in (S2_L2A_Netherlands_CloudFree)
return encode(
    ((float)(($c.nir - $c.red) / ($c.nir + $c.red)))[time("2018-06")]
    , "image/tiff"
)
```
2. A mask of "high vegetation" where NDVI values > 0.7 are true (white), and the rest is false (black):
```
for $c in (S2_L2A_Netherlands_CloudFree)
return encode(
    ((($c.nir - $c.red) / ($c.nir + $c.red)) > 0.7)[time("2018-06")]
    , "image/png"
)
```
Hi Dimitar , I think that here is a problem with my login as a github user. I got a message "No matching user found in Rasdaman please contact admin". Could you give me an access or how I can proceed?
I see a user mvittek, you should have credentials for it. I also enabled access for your github account now in any case.
Hi Dimitar I did some test and it seems that all works well. Thank you. In case that we would like to apply the same workflow for Landsat data at Rasdaman for year 2014 we would need to download data first from https://earthexplorer.usgs.gov/ and then upload, right? What would be in that case the best solution?
Yes, just make the data available for download and I'll update the code for it as the classification bands are different for Landsat 8 I think.