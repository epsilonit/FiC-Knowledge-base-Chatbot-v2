catalog-editor generates invalid WGS84 bbox
(original discussion [here](https://github.com/FAIRiCUBE/data-requests/pull/303#issuecomment-2252210543))

Original bbox:
```
      "x": {
        "axis": "x",
        "extent": [
          900000,
          7400000
        ],
        "reference_system": "3035",
        "type": "spatial",
        "unit": "metre",
        "step": 10
      },
      "y": {
        "axis": "y",
        "extent": [
          900000,
          5500000
        ],
        "reference_system": "3035",
        "type": "spatial",
        "unit": "metre",
        "step": -10
      },
```

The catalog editor generates a max lat coordinate 58.95275, but the correct one is 72.66. This is likely because it transforms individual coordinates which is incorrect. This can be verified with `gdaltransform`:

```
$ gdaltransform -s_srs EPSG:3035 -t_srs EPSG:4326
Enter X Y [Z [T]] values separated by space, and press Return.
7400000 5500000
72.906136759009 58.9527510923263 0
```

It gives 58.952 like the catalog-editor and not 72.66.

The correct way is to transform the whole bbox, e.g. with `gdalwarp`:

1. Create a bbox_epsg3035.vrt file with content
```xml
<VRTDataset rasterXSize="650000" rasterYSize="460000">
  <SRS>EPSG:3035</SRS>
  <GeoTransform>900000, 10, 0, 5500000, 0, -10</GeoTransform>
  <VRTRasterBand dataType="Byte" band="1">
    <NoDataValue>0</NoDataValue>
    <SimpleSource>
      <SourceFilename relativeToVRT="1">dummy.tif</SourceFilename>
      <SourceBand>1</SourceBand>
      <SourceProperties RasterXSize="1" RasterYSize="1" DataType="Byte" BlockXSize="1" BlockYSize="1"/>
      <SrcRect xOff="0" yOff="0" xSize="1" ySize="1"/>
      <DstRect xOff="0" yOff="0" xSize="1" ySize="1"/>
    </SimpleSource>
  </VRTRasterBand>
</VRTDataset>
```
2. put whatever dummy.tif file in the same directory, it's irrelevant because all information is in the VRT file, but it's required.
3. check the bbox with gdalinfo:
```
Origin = (900000.000000000000000,5500000.000000000000000)
Pixel Size = (10.000000000000000,-10.000000000000000)
Corner Coordinates:
Upper Left  (  900000.000, 5500000.000) ( 56d30'18.51"W, 56d29' 4.75"N)
Lower Left  (  900000.000,  900000.000) ( 23d49'33.58"W, 24d17' 3.04"N)
Upper Right ( 7400000.000, 5500000.000) ( 72d54'22.09"E, 58d57' 9.90"N)
Lower Right ( 7400000.000,  900000.000) ( 40d39'45.75"E, 25d32'40.96"N)
```
4. reproject the file:
```
gdalwarp -t_srs EPSG:4326 -of VRT bbox_epsg3035.vrt bbox_epsg4326.vrt
```
5. check the result bbox with `gdalinfo bbox_epsg4326.vrt`:
```
Origin = (-56.505141901704370,72.663269668344356)
Pixel Size = (0.000128060211603,-0.000128060211603)
Corner Coordinates:
Upper Left  ( -56.5051419,  72.6632697) ( 56d30'18.51"W, 72d39'47.77"N)
Lower Left  ( -56.5051419,  24.2841707) ( 56d30'18.51"W, 24d17' 3.01"N)
Upper Right (  72.9061049,  72.6632697) ( 72d54'21.98"E, 72d39'47.77"N)
Lower Right (  72.9061049,  24.2841707) ( 72d54'21.98"E, 24d17' 3.01"N)
```
The reprojection is done on the client side, using a library `reproject-bbox` which is using `proj4` as it's dependecy.

I'm aware with the distortion related shift in coordinates near (lat 60, -60) from/to WGS84, I tested some wms samples ( in our backend case, and it was fine).

I'll do  more invistigation and see what it is can be done in this case.
Any updates?
Hi any update on this issue? I have pushed a couple of md records that use the Austrian projection, but the leaflet map shows something else:
![image](https://github.com/user-attachments/assets/88c39c21-098a-44f1-a511-5fcceafaf487)