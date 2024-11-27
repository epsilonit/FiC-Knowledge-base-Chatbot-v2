rasdaman datacubes
### Name of resource

rasdaman datacubes

### ID

https://fairicube.rasdaman.com/rasdaman/ows

### Description

This service endpoint gives access to the FAIRiCUBE datacubes available through rasdaman, through standards-based APIs.

### Main category

ML

### Other category

_No response_

### Pubblication date

2022

### Objective

data-transformation

### Platform

Rasdaman

### Framework

OpenCV

### Architecture

None

### Approach

unsupervised

### Algorithm

Random-Forest-Classifier

### Processor

cpu

### OS

linux

### Keyword

datacube

### Reference link

_No response_

### Example

_No response_

### Input data

not sure what to write here

### Characteristics of input data

not sure what to write here

### Biases and ethical aspects

_No response_

### Output data

datacube extracts and analysis results

### Characteristics of output data

not sure what to write here

### Performance

_No response_

### Conditions for access and use

wtfpl

### Constraints

disclaimer: most of the above information is not correct because no adequate categories exist.
Hi
by "no adequate categories exist" do you mean missing codelist values (in this case can you provide the values you need using the related template?) or are the fields not sufficient for rasdaman resources and in case please post here as a reply?
Thank you!
Is it true that the issues posed here have been clarified in #5?

first, small typo: Pubblication date

next, I try to adjust the form but even in edit mode it does not allow. What am I expected to do?
If you're talking about the [Data Request Form](https://github.com/FAIRiCUBE/data-requests/issues), to my memory, this was set up by EOX (Stefan Brand). Subsequently, from EPSIT used the same approach for providing [resource metadata](https://github.com/FAIRiCUBE/resource-metadata/issues/new?assignees=&labels=&template=metadata-request.yml) can we then close the issue? 
Hi both,
I still see quite a few "not sure what to write here" in the original attempt, think this should be clarified before this is closed.
Metaquestion: is this description for a specific ML resource on rasdaman, or just a first attempt?
the data request form seems edited by EOX, so I assume it is consensus now.

just went through the metadata request form which - as I assume - should be used for the data we + others ingest, findings:

- "Other category" still undefined
- "Framework" is mandatory, but I would have no idea what to write there
- "Approach" likewise - what would we write for Sentinel-2, for example?
- why is "OS" important, how does this matter? Would anybody know what to write for Sentinel-2?
- same for "Input data used"
- as we want to import, what is "Output data obtained"?
- what is "a/p"?
- "Performance" talks about "hyperparameters"...I am lost!
- "Reference link" is optional, but IMHO the central piece of information: how to get to the data these metadata describe.


Hi you are talking about data and this form (and this repository) is for analysis and processing resources (called 'a/p resources'). So, you need to repost this in the 'data-request' repository.  :wink: you are right - sorry for mixing it up. So here is the revised list, I am trying to provide what I would see for our datacube services - can you please check whether this is what you expect?

- "Framework": OGC datacube services
- "Approach": Web requests
- "OS": any
- "Input data used": none, data provided in cube database
- "Output data obtained": sets of datacubes or scalars
- "Performance": great :)
- "Reference link": https://fairicube.rasdaman.com/rasdaman/ows
-