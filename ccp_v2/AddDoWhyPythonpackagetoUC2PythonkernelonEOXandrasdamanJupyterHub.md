Add DoWhy Python package to UC2 Python kernel on EOX and rasdaman Jupyter Hub
Hi,

For our use case (UC2) we want to look into the DoWhy library for causal modelling. Can that package (with dependencies) please be added to our Python kernels on Jupyter Hub, both on EOX and rasdaman. DoWhy v.0.11.1 is preferred.

https://www.pywhy.org/dowhy/v0.11.1/index.html

Thanks!
forwarded request to our EOxHUB Team 
The DoWhy 0.11.1  has been added to  "fairicubeuc2_torch".
Let me know if you need it somewhere else, too.
Please test it, and if it works and close this issue. 
Else let me know. 

I have tested it on EOX, working fine  Any update regarding installation on the rasdaman Jupyter Hub for UC2? can you have a look?
It's installed now on the rasdaman jupyter hub, please check if it works.
Thanks for installing it Python still has trouble finding it from a Jupyter Notebook, see screenshot. Something seems to be out of sync, any idea?

![Screenshot 2024-06-13 at 10 23 39](https://github.com/FAIRiCUBE/FAIRiCUBE-Hub-issue-tracker/assets/1943289/0b8ff668-4a08-4e3d-a9e7-510307081dfe)

It works fine for me, not sure how to reproduce your issue?

![Screenshot_20240614_160307](https://github.com/FAIRiCUBE/FAIRiCUBE-Hub-issue-tracker/assets/4786324/6317d407-4ff3-4954-b770-b2e699094e0e)  I don't know how rasdaman is handling Jupyter, but make sure that you really start up a NEW Jupyter session.  Just an idea... Following the discussion today I setup our JupyterLab with GitHub login. You'd need to logout and then click on Sign in with GitHub.  Thanks. GitHub sign in to JupyterLab is working :)

The DoWhy package now can be imported as well in my notebook. So that issue seems to have resolved itself as well.

(The machine seems to be a bit sluggish though at the moment.)