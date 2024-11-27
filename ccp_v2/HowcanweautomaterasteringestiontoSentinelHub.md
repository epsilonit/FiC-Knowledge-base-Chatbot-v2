How can we automate raster ingestion to SentinelHub?
This is the ingestion template [notebooks/f01_ingestion/ingestion_00_template.ipynb](https://github.com/FAIRiCUBE/uc1-urban-climate/blob/master/notebooks/f01_ingestion/ingestion_00_template.ipynb).
Also another example: [notebooks/z99_demo/demo_processing.ipynb](https://github.com/FAIRiCUBE/uc1-urban-climate/blob/master/notebooks/z99_demo/demo_processing.ipynb)

Some improvements towards automation could be (just a draft, feel free to correct where needed!)

- [ ] add script to convert raster into COG, either within this notebook or as separate script (or else?)
- [ ] are there alternatives to ingesting tile one by one? (cell 2)
- [ ] add ingestion routine for multitemporal collections
- [ ] make the notebook executable from terminal with argument, e..g, the data folder any insights as to how to streamline this process?
sorry, this issue somehow slipped through. here are some thoughts in response to your points:
- [ ] I believe this script is in most cases a single `gdal_translate` call per tile as detailed at https://docs.sentinel-hub.com/api/latest/api/byoc/#gdal-example-command and the parameters would be almost identical or am I missing something?
- [ ] I'm afraid this is the way to go: `Once the collection is created you can add tiles. Note that only a single tile can be added in one step.` (see https://docs.sentinel-hub.com/api/latest/api/byoc/#ingesting-the-tiles).
- [ ] Sounds like a second `for` loop to iterate through the available times doesn't it?
- [ ] An alternative would be to tag a cell as `parameters` and use in headless execution mode. seems you're confirming that there are no predefined scripts for data ingestion on SentinelHub, users must script themselves? No streamlining foreseen? Currently seems like a fairly painful manual process.

Also, I remember reading in various deliverables that while initially, we must rely on sentinel-hub documentation, this will eventually be tailored to FAIRiCUBE requirements. Any progress on this?
- tiff to COG: yes, it is "just" a `gdal_translate` command. The thing is in this "just" there is a world of parameters/options to be set, and going through the documentation to understand what does what is very time consuming. Since this is a routine task, it would be great if the Hub could provide a default way of doing it. For example, a script (but even better with a UI) that takes as input the tif file(s) and outputs the COG file(s). We already have in common-code the script that does the job, but what would be the best way to integrate it in the system?
- agreed
- agreed
- ok, I could imagine a notebook that packages all the steps from tiff files to ingested collection (including the previous points), and only requires the user to pass the path to the tiff file(s) as a parameter (and maybe name of the collection etc.). Would it be possible to provide such a headless notebook by default in an FAIRiCube user profile? I agree with on all points above. How can we create an extension of the existing TIFF->COG script to make this painful repetitive task a bit easier?