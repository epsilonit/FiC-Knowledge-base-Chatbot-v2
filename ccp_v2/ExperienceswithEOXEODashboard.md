Experiences with EOX EO-Dashboard I picked up on an [issue on the EOX Dashboard](https://github.com/FAIRiCUBE/flux-config/issues/13) a while back but was missing the context, has now clarified that the "Dashboard was made available to UC1" after "Stephan presented EO-Dash to FiC users at Oslo Mid-term rehearsal as an additional visualization"

My question to you:
- Have you evaluated this for usage within FAIRiCUBE
- If so - what's your feedback? the eodash dashboard is stilll in "testing", you can see the prototype here: https://fairicube.github.io/uc1-eodash-client/dashboard?indicator=tree_cover_density&x=1812922.0624&y=6399070.2510&z=5.0297&datetime=2024-05-15T08:37:53.248Z.
There are a couple of technical issues that are being addressed (e.g. the SentinelHub watermark on the layers registered there). If/Once resolved I will provide more feedback. 
Update: the technical issues are mostly solved now. I think that this can be a good visualization solution for FiC, provided that datasets are served through: 
  - SentinelHub: the main drawback is that [not all CRSes are supported by SH](https://docs.sentinel-hub.com/api/latest/api/byoc/). This is for example a problem for UC1 using local CRSes (e.g. [EPSG:31256](https://epsg.io/31256))
  - WMS: UCs would need to set up a WMS for the datasets they intend to visualize through the dashboard.
  - [there are other supported resource types](https://github.com/eurodatacube/eodash-catalog/wiki/Resource) but I think these two are the most interesting for us now.

 Also: EOX recently published [some new documentation about the eodash ecosystem](https://eodash.org/welcome.html).