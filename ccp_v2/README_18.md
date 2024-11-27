# Challenge 6: Effects of environmental variables on air temperature in urban context
Challenge 6 of Geoscripting course at WUR, in collaboration with FAIRiCUBE.

Contact: ricci@space4environment.com

## Description
The urban heat island effect (urban areas are generally hotter than their surroundings) is one of the consequences of climate changes that cities strive to mitigate. Often, city administrations have difficulties in assessing the effectiveness of mitigation measures because of lack of data. Having reliable estimates of air temperature usually requires expensive flight campaigns or a dense network of in-situ weather stations. Remote sensing land surface temperature (LST) can be used as a proxy for air temperature, but their relationship need to be carefully modelled, especially in complex contexts such as the urban one, by taking into account different environmental factors, such as seasonality, sealing degree and the height of buildings, vicinity to water features and vegetation. 

FAIRiCUBE aims at solving these and similar problems using data cubes and cloud infrastructures - learn more about the project here. 

This challenge is divided into two parts. First, assess which enviromental variables most exhacerbate or mitigate high temperature values near weather station locations. Secondly, use the results to predict air temperature throughout the entire urban area, based on LST. As a use case, we focus on the city of Vienna. 

## Data
Below a list of data available in the FAIRiCUBE Hub.
### Data on S3 bucket
EO data:
- DEM,
- land cover,
- land use,
- sealing degree,
- building footprint and height,
- green areas
- ...

### SentinelHub API
Land surface temperature can be obtained from Landsat-8 or Sentinel-3 with the following processing scripts:
- https://custom-scripts.sentinel-hub.com/custom-scripts/landsat-8/land_surface_temperature_mapping/ 
- https://custom-scripts.sentinel-hub.com/custom-scripts/sentinel-3/land_surface_temperature/

Other EO data (e.g. NDVI) can be obtained from SentinelHub as well.

### Weather stations data API
- https://data.hub.geosphere.at/dataset/klima-v2-1d
- https://www.zamg.ac.at/cms/de/wetter/wetterwerte-analysen/wien

### Background reading material on similar projects
- https://storymaps.arcgis.com/stories/134065bbccdf4e5c8d2aa6f9cade8297 
- [Modeling urban air temperature using satellite-derived surface temperature, meteorological data, and local climate zone pattern—a case study in Szeged, Hungary | Theoretical and Applied Climatology (springer.com)](https://link.springer.com/article/10.1007/s00704-024-04852-7)
- https://research.wur.nl/en/publications/estimation-of-air-temperature-using-the-temperaturevegetation-ind 
- https://www.frontiersin.org/journals/environmental-science/articles/10.3389/fenvs.2021.791336/full 
- https://figshare.com/s/e59d943d907c3097ab1f 
- [Downscaling daily air-temperature measurements in the Netherlands | Theoretical and Applied Climatology (springer.com)](https://link.springer.com/article/10.1007/s00704-020-03313-1?fromPaywallRec=true)

## Processing
- How to access data from the S3 bucket: https://github.com/FAIRiCUBE/uc1_training/blob/main/notebooks/Vienna_data_cube.ipynb
- How to access and process data using SentinelHub APIs: https://github.com/FAIRiCUBE/uc1_training/blob/main/notebooks/demo_processing.ipynb


