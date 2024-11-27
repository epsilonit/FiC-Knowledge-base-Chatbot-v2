Climate data processing with dask extremly slow
This script https://github.com/FAIRiCUBE/uc1-urban-climate/blob/master/notebooks/f02_cube/subcubes_utci_stats.py processes 1 year of hourly climate data (around 14GB) to produce daily statistics for selected EU cities. I use dask to (hopefully) speedup data loading and processing. However, the script runtime is considerably slower when executed on FAIRiCube Hub than when executed locally. 

The basic steps are _(in parentheses the running times on local machine vs. on FAIRiCube Hub)_:
- Download data from climate data store _(same running time)_
- Unzip data _(4 mins vs 15 mins)_
- Lazy-load data as xarray _(2 mins vs. 15 mins)_
- start dask local cluster
- compute statistics _(8 mins vs 35 mins)_
- save statistics as shapefile _(same running time)_
- delete original data from s3 to save space

My question is why is the processing so slow? Is there a problem in the code?
After cross-checking with our experts:  its doesn't seem to be the script. 
It seems more likely that it is coming from the Storage. The permanent storage available is NFS-mounted and compared to your local, probably SSD, there is a big speed difference (up to 10-fold). 
But it could also be a limitation on the RAM side, so that the process is permanently swapping, which slows it down. 
What is your inital RAM setting?
When starting the Hub I have the following settings:

CPU: 0.2
Memory: 213.55 MB
**Host CPU**
0.6% used on 8 CPUs
**Host Virtual Memory**
Active: 820.46 MB
Available: 29.19 GB
Free: 27.51 GB
Inactive: 1.74 GB
Percent used: 4.8%
Total: 30.67 GB
Used: 1.03 GB
Wired: 0.00 B

Does it help?
Hello, I have performed some other benchmarks. I have tested the new UC1 large profile (with doubled resources). I think that the problem is not related to the memory size/number of CPUs. The script takes the same amount of time regardless of the resources (on FAIRiCube Hub).
Here again some info about the machines (recorded with the [Measurer](https://github.com/FAIRiCUBE/common-code/tree/main/record-computational-demands-automatically) developed by
The "large" profile on FAIRiCube has double the resources of my local machine.

|                            | Fairicube Hub large (365 days) | Local machine (365 days)                           |
| -------------------------- | ------------------------------ | -------------------------------------------------- |
| Data size (MB)             |                              | 90.7421875                                         |
| Main memory available (GB) | 61.68314362                    | 23.94339371                                        |
| Main memory consumed (GB)  | 0.333139                       | 0.339320951                                        |
| CPU/GPU Machine type       |  x86_64                        |  AMD64                                             |
| CPU/GPU Processor type     |  x86_64                        |  Intel64 Family 6 Model 60 Stepping 3 GenuineIntel |
| CPU/GPU Number of physical cores | 8  | 4 |
| CPU/GPU Number of logical cores  | 16 | 8 |
| Network traffic (MB)  | 119131.431390762 | 14.760983467102 |
| Wall time in seconds | 8504.97295928001 | 682.038042545318 |

Can you see why is there such a difference in performance by looking at this data? The only thing I see is the difference in network traffic. Could it be that the process is slowed down by having to access the data on S3 bucket?








































I learned from my colleagues that many small request to S3 will slow processes down very much, but many small requests wouldn't increase the data volume that much .

I looked at your code and didn't see any loops - so what astonishes me here is that you have 119 TB (!!) of network traffic compared to 14 GB.  
Any ideas how you could possibly create such a high network traffic?
Hi Christian,
the root cause of the problem seems to be the combo NetCDF and cloud storage, cf. [this forum](https://discourse.pangeo.io/t/s3-zarr-netcdf-access-times-using-s3fs/794). Reading NetCDF from S3 is slow because NetCDF is not a cloud optimized, and this is also causing the high network traffic (lots of requests to get the metadata). Zarr seems to be the cloud-optimized alternative to NetCDF. 
Which means that we have to rethink how we handle climate data, since it usually comes in NetCDF format.
Btw, it is 119 GB, not TB, I had forgotten to had the unit (fixed). this is probably information relevant for you as well.
Hi Maria, 
Yes, you seem to be right. Although netCDF was developed for net-access it doesn't seem to work well on Object Storages. 
Network traffic:  119GB is much better but still ~8,5 times of the 14GBs. 
The issue of netCDF with S3 seems to be already known and a search provided some possible solutions you could try to speed things up. The listing is not in any order.
https://pypi.org/project/s3fs/
https://github.com/fsspec/s3fs/issues/168+
https://github.com/meracan/s3-netcdf
https://pypi.org/project/S3netCDF4/
https://stackoverflow.com/questions/43197223/using-aws-s3-and-apache-spark-with-hdf5-netcdf-4-data/60885374#60885374
https://nasa-openscapes.github.io/2021-Cloud-Workshop-AGU/how-tos/Multi-File_Direct_S3_Access_NetCDF_Example.html
https://medium.com/pangeo/cloud-performant-reading-of-netcdf4-hdf5-data-using-the-zarr-library-1a95c5c92314
https://medium.com/pangeo/fake-it-until-you-make-it-reading-goes-netcdf4-data-on-aws-s3-as-zarr-for-rapid-data-access-61e33f8fe68 I don't have the time to read the various pages, but based on the links, seems the general recommendation is to first convert the NetCDF to Zarr.

Regardless - please tell me what solution you come up with!!!
one might do that with its own datasets but for external data stores this will not likely  be an option. 
the links above provide 2-3 alternative S3 access options which could be benchmarked 
Hi
I have reviewed your suggestions:
- with `rioxarray` it is a bit better, but still reading nc files from s3 is 2x slower compared to reading from local disk. Nevertheless   because it solves other errors I had loading the files.
- `S3netCDF4 `looked promising but I cannot install the library (`pip install` throwing errors), and the project seems stale
- I am not sure how `s3fs` can help here, since the s3 bucket is mounted to the EOX workspace, I can navigate it as part of the file system. Or am I missing something?
Issue resolved: traditional file formats (e.g. tiff, netCDF) cause a lot of network traffic and slow down the computation when the file resides on the cloud. Cloud-optimized format like COG, zarr are designed to overcome this problem.