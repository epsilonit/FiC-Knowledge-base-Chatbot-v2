EOX-Hub Installations and API Updates
1. Is it possible to have a java installation (uc specific or system wide) to run jar files on the EOX hub? If not are there alternatives?

2. When using the Copernicus Climate Data Service API i get the following warning message:

> 2024-09-04 13:09:36,803 INFO Welcome to the CDS. As per our announcements on the Forum, this instance of CDS will be decommissioned on 26 September 2024 and will no longer be accessible from this date onwards.
Please update your cdsapi package to a version >=0.7.2, create an account on CDS-Beta and update your .cdsapirc file. We strongly recommend users to check our Guidelines at https://confluence.ecmwf.int/x/uINmFw
2024-09-04 13:09:36,804 WARNING MOVE TO CDS-Beta

Do Use Cases need to do this by themselves individually?

Also my request never moved out of the pending/queue phase, not sure if this is due to that error message or has other reasons.  I assigned you to that as well, since that is still the case and we wanted to try to get ERA5 data from this year with the CDS-API on EoxHub. Do you have any information concerning this issue?
More info here: https://forum.ecmwf.int/t/the-new-climate-data-store-beta-cds-beta-is-now-live/3315 
I assume this needs to be done for FairiCube Use Cases as well?  

re1. Do you want to run a jar file purely from the command line or would this involve a java GUI like starting one from the jar file?

re2. is looking into updating the kernels for all use cases. 

1. Running a jar file only would be sufficient in our case.

2. Okay, thank you! 
cdsapi==0.7.3  has been added to the Jupyther kernels:
- fairicubeuc3/torch
- fairicubeuc2/torch
- fairicubeuc1/fairicube_env
- fairicubeuc1/cube_env

as well as to the headless kernels:
- eurodatacube8/torch 
- eurodatacube17/torch
- eurodatacube18/torch


a kernel for testing to run JAR files will follow...   openjdk has been added: 
These are the new HEADLESS KERNELS for the UCs (now providing torch, openjdk and a new cdsapi) and the NAMESPACES:
eurodatacube8  -> headless-fairicubeuc2.hub.eox.at using bucket s3://hub-fairicubeuc2
eurodatacube17 -> headless-fairicubeuc3.hub.eox.at using bucket s3://hub-fairicubeuc3
eurodatacube18 -> headless-fairicubeuc4.hub.eox.at using bucket s3://hub-fairicubeuc4
eurodatacube19 -> headless-fairicubeuc1.hub.eox.at using bucket s3://hub-fairicube0   <-- legacy
eurodatacube20 -> headless-fairicubeuc5.hub.eox.at using bucket s3://hub-fairicubeuc5

The corresponding JUPYTERLAB KERNELS (now provide torch, openjdk, and a new cdsapi) are:
fairicubeuc1/torch_openjdk
fairicubeuc2/torch
fairicubeuc3/torch
fairicubeuc3/torch_openjdk
fairicubeuc5/torch_openjdk

You my test running JAR files now