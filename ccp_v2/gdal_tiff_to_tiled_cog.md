# GDAL command for splitting/tiling raster file into tiles and converting to COG

Use this to split large raster files in tiles before registering the data to SentinelHub.

## Notes

gdal_retile.py is a Python script, and will only work if GDAL was built with Python support.

Documentation: [https://gdal.org/programs/gdal_retile.html](https://gdal.org/programs/gdal_retile.html)

## Parameters

- -of Format (→ COG = Cloud Optimized GeoTIFF)
- -ps Tilesize (Number of cells per line and column)

## Example

```{bash}
python gdal_retile.py -of COG -co BLOCKSIZE=1024 -co COMPRESS=DEFLATE -co PREDICTOR=YES -ps 100000 100000 -levels 1 -s_srs EPSG:3035 -r near -tileIndexField clc18_tile_ -targetDir <target_dir> <file_path>
```
