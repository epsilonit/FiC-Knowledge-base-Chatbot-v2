Python packages to bridge between standard EO types such as zarr and xarray and rasdaman WCPS requests?
Are there any operational/maintained Python packages available that bridge between the rasdaman WCPS queries and the standard types and classes used in EO, such as xarray and zarr? Or any known documentation / best practices?

Ultimately I would need something that works with PyTorch Datasets and DataLoaders, or TorchGeo, to get the data into machine learning format.
no, I am not aware of anything out there, and we do not provide any packages in that direction either.
Ok, will have to figure out something then.
My workaround will probably (have to) be to (pre)download (using WCS or WCPS requests) the required data from the data cubes as (sliced) geotiff or netcdf files, which I can then import into xarrays or acces as a TorchGeo Dataset for machine learning as needed.
not necessary - you get the data results as generic python arrays, see the Jupyter [notebooks](https://standards.rasdaman.com/demo_jupyter-python.html) we have provided. That can be numpy, but I would expect that they can be transformed into xarray etc. easily.
Thanks, I will have a look at that example Notebook.
is this issue closed maybe? ?
The question has been answered indeed. When we get to it in our use case I will write the code that requests data from rasdaman via WCPS and then gets it into PyTorch for model training. For performance reasons (to keep the GPU busy) perhaps it has to be a download script first to get all training/test data (tiles) and write it to local files. 

I will close the issue with this.