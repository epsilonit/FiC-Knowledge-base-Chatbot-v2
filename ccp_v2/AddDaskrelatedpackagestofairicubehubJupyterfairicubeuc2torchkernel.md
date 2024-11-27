Add Dask related  packages to fairicube hub Jupyter - fairicubeuc2-torch kernel
In FAIRiCUBE Hub Jupyter there is a tab in JupyterLab for Dask, but when I try to import Dask packages in the fairicubeuc2-torch kernel they are not available. Can they please be added (perhaps also pertains to other kernels)?

![Screenshot 2023-11-13 at 14 15 36](https://github.com/FAIRiCUBE/FAIRiCUBE-Hub-issue-tracker/assets/1943289/5186e80c-e264-4813-8553-ace2a4adcb15)

And same for the 'retrying' and 'pyspark' (requiring Apache Spark to be installed as well) packages.

Even though the server is a single node, it can help with writing/experimenting with distributed code and maybe make better use of the available CPU cores on the node.
[Stefan Achtsnit][1 hour ago](https://gitlab.eox.at/tenants/hub/support/-/issues/96#note_88758)

you need to install dask in the conda kernel
what you see on the left is just the extension allowing to open the dash dashboard
Ok, thanks for the reply.