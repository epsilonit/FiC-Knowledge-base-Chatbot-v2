Jupyter Hub installation on rasdaman VM
we need a jupyther hub installation on the rasdaman VM to bring our python scripts close to the rasdman data base engine.
a first installation is in place 

https://fairicube.rasdaman.com/jhub/

and credentials have been generated for Rob, Marian, Sonja, Martin, Kathie and Stefan J but there are some issues
-  Rob and Marian cannot log in to the JHub
- Sonja & Stefan can log in but when trying to execute adummy notebook, there is a message that it cannot connect to a kernel

![screenshot](https://github.com/FAIRiCUBE/FAIRiCUBE-Hub-issue-tracker/assets/116027495/8022e699-1949-458d-b609-c57557887f32)

Mohit fixed the problem I had with logging in to JupyterHub, that now works for me. But then I run into the Notebook kernel connection issue as well unfortunately.
At the moment the url is not working anymore though.
Hi.
Yes the jupyterhub is down. I closed it to look at the kernel issue.
what is this issue about? What can be done?
> what is this issue about? What can be done?

My guess would be some networking / proxy setting issue, maybe websockets are not allowed somewhere along the route? Hoping that the cause shows up somewhere in the logs. I could check browser client logs when it is back online.
Hi

The kernel connection issue is still blocking using JupyterHub. On the client side (my browser) I can find nothing further than that the server considers the websocket (wss) requests that are needed invalid and responds to them with a 400 Bad Request error. See the screenshot. Any idea how this can be solved?

![Screenshot 2023-05-24 at 08 58 54](https://github.com/FAIRiCUBE/FAIRiCUBE-Hub-issue-tracker/assets/1943289/c595778e-8fcf-4811-b262-b1716b860221)

Hmmmmm...... Looks like I am unable to resolve the kernel issue and caused a rasdaman outage while working on it. Can we have a meeting later today (1 pm is a good time) and where you can help me in resolving ?
> Hmmmmm...... Looks like I am unable to resolve the kernel issue and caused a rasdaman outage while working on it. Can we have a meeting later today (1 pm is a good time) and where you can help me in resolving ?

I don't have experience setting up JupyterHub for multiple users, but I can help troubleshooting we have fixed the jupyter hub kernel issue and now the code can be executed. Pending tasks are:

1. secure (https) connection.
2. Install ML libraries in the VMs Python will be in charge of these tasks, and I will serve as support that mains that the JHub is still not accessibale from outside? if I try to reach https://fairicube.rasdaman.com/jhub/ I get "The requested URL was not found on this server." Yes correct. Good that you reminded me, to access jupyterhub you have to do it through this URL: http://fairicube.rasdaman.com:8888/. We will still work on it so it will, change but we will let you know through email + github
I will stop jupyterhub for sometime, in an attempt to install https.
Hi , is JupyterHub still stopped? Or did the URL change? I can't reach it anymore.
The jupyterhub is still down.
I have set it back up-
http://fairicube.rasdaman.com:8888/hub/login?next=%2Fhub%2F

Will resume work on https after some time.
Hi , I was wondering if there is any progress on making the connection secure, and on installing a kernel with ML packages available?
Hi , unfortunately I tried establishing a secure connection again, but I wasn't successful. At this stage, I have created a ticket for my colleague so that they could help me install it. can you recommend us a kernel with ML packages available? These are the packages I currently have in my local Python environment for FAIRiCUBE work. I think you can base a first version of the kernel on that:
[requirements.txt](https://github.com/FAIRiCUBE/FAIRiCUBE-Hub-issue-tracker/files/12359799/requirements.txt) ok, so it is local. Can you give us download site and package name? I wish, but sadly it is not that easy in the Python world. As you can see in the file there are many references to lib’s, which of course are OS dependant. The proper way is to start with a base Python environment on the system, install all needed packages there (e.g. using the conda package manager), and then turn this environment into a Jupyter kernel.

All packages I use (on macOS) are listed in the requirements.txt file. If you start with installing the high level ones (e.g. numpy, pandas, geopandas, rasterio (with gdal), sklearn, pytorch (with torchvision)) lots of the others will automatically be added as dependencies. And then you can check which are still missing.

You can also try to initialise a Python environment using conda and taking the requirements.txt as input. But versions and OS differences might make this less straightforward. I installed most packages using pip on jupyter notebooks, except the ones in the attachment. How to install the remaining ones ?

[remaining_packages.txt](https://github.com/FAIRiCUBE/FAIRiCUBE-Hub-issue-tracker/files/12395931/remaining_packages.txt) Most of those sound like OS libraries or system dependencies that should not need explicit installation (with pip).
so looks like we can consider this task done. when can you evaluate this deployment, confirm that this task has been completed. 
_Note: I get the fact that this task has been lying dormant for a while, you may not currently have resources available. Thus I'd like an honest statement_ done is when it's been validated. In the future, when something has been technically resolved, please request validation from relevant partners before stating completion.
I had a quick look:
- JupyterHub works and I can start a Python Notebook
- The connection is unsecure, needs to be https
- There is only one Python kernel available, which does not have the needed Python packages
- When I try to load packages (with pip) I get weird exceptions please check the issues Rob reported!
We (finally) have a https connection now !

https://fairicube.rasdaman.com:8888/ have you had the time to check if your issues are resolved?
Just did quickly, there is still only a single Python kernel available that lacks the machine learning packages. ok, can please give us exact guidance on what commands to execute to install the kernel you are requesting. Please see this JupyterHub documentation about managing the user kernelspecs: 

https://jupyterhub.readthedocs.io/en/stable/howto/configuration/config-user-env.html ok, this is useful for the howto indeed - but what exactly do you miss? Every Jupyter Notebook needs a kernel. This is basically the environment that executes the code in the notebook cells. In case of Python, the kernel needs to be configured with the Python packages that are used in the code. Like numpy and pandas. Besides the regular ones, for our use case we need specific machine learning packages, such as PyTorch. That’s the requirements list we discussed before in this topic.

In essence, kernels (kernelspecs as Jupyter calls it in their documentation) will be needed for all the specific configurations or types of Python tasks that you want to support. E.g. Deep learning with PyTorch, or standard geospatial work, and so on. Because of all dependencies that exist between Python packages (aka dependency hell) you will not want to attempt to include everything into a single kernel. There is high chance of it breaking down quickly.

Once such specific Python environment has been prepared on the rasdaman JupyterHub server, you will need to register it as a kernel and make it accessible to users.
Right now there is only one Python kernel available, which is the basic / generic one. This does not have the PyTorch and other machine learning packages that are needed for use case 2. thanks for the background, Rob - but still: how to do it? If you can just write down the 2 - 3 commands (I am sure you can do that right off the top of your head) then we can go ahead. Thanks in advance! The command to install a Python kernelspec is something like this (see also the JupyterHub documentation about it):

`/path/to/python-env -m ipykernel install --prefix=/usr/local`

I guess user access rights follows the underlying OS, unless JupyterHub adds a custom layer. But that I do not know (as I never installed a full JupyterHub installation myself). is there a clear list of Python packages you'd need for the Python kernel? Could you provide? I already provided such a list, please scroll back up to Aug 16th. It seems Mohit already was working on it but I don’t know what happened with it in the end. thanks!  here the list: [requirements.txt](https://github.com/FAIRiCUBE/FAIRiCUBE-Hub-issue-tracker/files/12359799/requirements.txt) actually I see that I have two kernels in Jupyterhub  - a. IPythonKernel, b. My Kernel.

Also, a quick !pip list command shows the required packages (including torch, as you mentioned PyTorch libraries).

![image](https://github.com/FAIRiCUBE/FAIRiCUBE-Hub-issue-tracker/assets/13696328/b5ca7a28-a76f-4ecc-bf38-4ec47c660c81)

Please check.

I created a python file to run the pip command for the entire requirement list, and most packages got installed.
> I installed most packages using pip on jupyter notebooks, except the ones in the attachment. How to install the remaining ones ?
> 
> [remaining_packages.txt](https://github.com/FAIRiCUBE/FAIRiCUBE-Hub-issue-tracker/files/12395931/remaining_packages.txt)

If the packages are indeed available, what should I do with this list ?
Hi , I still only have access to the base kernel, but it now has all the packages from the list it seems. Looks like you have installed everything in the Python base environment on the server now?

![IMG_0140](https://github.com/FAIRiCUBE/FAIRiCUBE-Hub-issue-tracker/assets/1943289/886c3ea9-b6dd-4e51-98ea-2bfdcebd309c)

Yes, I did. Does this satisfy your requirements ?
I think we can close this specific issue now, also because it is already running very long. Let’s create new tickets when questions turn up on actual usage of the rasdaman JupyterHub by me or other people.

Personally I don’t think adding all Python packages to the base environment is a good approach. But I leave that as an advice to the maintainers of the system :)