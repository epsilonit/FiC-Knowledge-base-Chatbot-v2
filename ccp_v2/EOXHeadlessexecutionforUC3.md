EOX: Headless execution for UC3
Hi,

In UC3, we want to test the headless execution. provided a comprehensive step-by-step guide on running a notebook headlessly in the following issue: [How to?: Headless execution](https://github.com/FAIRiCUBE/flux-config/issues/1#issuecomment-1689488002).
I understand there are two options for UC3:

  1. Use UC2 headless server link, in that case, we need the credentials to access it.
  2. Set up a new headless server for UC3.

Can you help us with this?

Thanks in advance.  

Best regards,

-Bachir.

A headless access for UC3 has now been set up ans is ready for use:
```
curl -X POST -v https://headless-fairicubeuc3.hub.eox.at/processes/execute-notebook/jobs \
    -u USERNAME:PASSWORD \
     --header 'Content-Type: application/json' \
     --data-raw '{"inputs": {
  "notebook": "s3/NOTEBOOK_IN_BUCKET.ipynb",
  "cpu_requests": "1",
  "cpu_limit": "1",
  "mem_requests": "4G",
  "mem_limit": "4G",
  "node_purpose": "user",
  "kernel": "conda-env-eurodatacube8-torch-py",
  "parameters_json": {"a": "b", "c": 123}
}}'
```
Instructions the same as  for UC2, see:  https://github.com/FAIRiCUBE/flux-config/issues/1#issuecomment-1689488002
Username and PW are provided via personal communication 
Thanks
I have tested it and it works, thanks!
I only have an issue regarding the kernal. I understand "conda-env-eurodatacube8-torch-py" is the only available kernal as for now, right? A library that we use is missing, how can I install the library into this kernal?

Thanks in advance.  
Yes, "conda-env-eurodatacube8-torch-py" is the only kernel for your headless usage
what libraries are you missing? it is "codecarbon".
I was able to run our code, but I need to install the same libraries for each run (by dedicating the first notebook cell to install missing libraries).
 
Before closing this issue, I came across a minor problem.
In our code, we use the S3 common bucket environment variables to read/write.
Headless running of the code did not provide us with access to these variables.
I had to copy/past the name, key and secret codes to the script (which is obviously not safe).
Is there a way to fix this? (Maybe passing the environment variables through parameters directly in the curl command?) 
Hey  
we have now upgraded the kernels for headless (conda-env-eurodatacube8-torch-py) and jupyter-lab (fairicubeuc3-torch) 

- adding codecarbon to both

-  also the availability of the environmental variables in the headless execution should now be fixed 
I have tested it and it works.
I close the issue here.
Thanks for your help.
Hi 

I reopen the following issue to request headless execution for the remaining UCs (UC1, UC4 and UC5) with priority to UC4 where we need GPUs.

Thanks in advance,

Best regards,

-Bachir. should they all only execute on gpu then ?
or does only UC4 require gpu. it is only UC4 for now.
the conda kernel from eurodatacube8 (called torch) got replicated to eurodatacube17 and eurodatacube18. This was necessary to avoid conflicts between running jobs.

These are now the new access URLs for the UCs: 
https://headless-fairicubeuc2.hub.eox.at/:  using kernel  -> eurodatacube8/torch
https://headless-fairicubeuc3.hub.eox.at/:  using kernel  -> eurodatacube17/torch
https://headless-fairicubeuc4.hub.eox.at/:  using kernel  -> eurodatacube18/torch

The credentials are available at: https://nilu365.sharepoint.com/sites/Horizon2021_CUBE/_layouts/15/Doc.aspx?sourcedoc={235313bb-424e-4a1e-b1d6-92296d28fbfc}&action=edit&wd=target%28technical%20library.one%7C18ca003a-ff29-4de7-925e-1f11804605c2%2FEOxHub%20headless%20execution%7C6d55dc09-f667-4dac-99d1-6d67687afc59%2F%29&wdorigin=703

Thanks 
I have tested headless execution for UC4, but still getting an error (500 Internal Server Error).
You can find the example I am using at the end of: https://nilu365.sharepoint.com/sites/Horizon2021_CUBE/_layouts/15/Doc.aspx?sourcedoc={235313bb-424e-4a1e-b1d6-92296d28fbfc}&action=edit&wd=target%28technical%20library.one%7C18ca003a-ff29-4de7-925e-1f11804605c2%2FEOxHub%20headless%20execution%7C6d55dc09-f667-4dac-99d1-6d67687afc59%2F%29&wdorigin=703

Please let me know if I am doing something wrong. 
I know that you followed the example provided above - try it with the request below: 
It's still not working but the error looks more like a script issue then a server error (but I'm not sure):
      `{ "code":"InvalidParameterValue",  "description":"invalid request data"}`

Reformatted Request: 
```
curl -X POST -v https://headless-fairicubeuc4.hub.eox.at/processes/execute-notebook/jobs \
    -u <username:password> \
     --header 'Content-Type: application/json' \
     --data-raw '{"inputs": [
      {"id": "notebook", "value": "s3/scripts/Roof_height_ML.ipynb"},
      {"id": "cpu_requests", "value": "1"},
      {"id": "cpu_limit", "value": "1"},
      {"id": "mem_requests", "value": "4G},
      {"id": "mem_limit", "value": "4G"},
      {"id": "node_purpose", "value": "userg1"},
      {"id": "kernel", "value": "conda-env-eurodatacube18-torch-py"}
    }}'
```
see also https://github.com/FAIRiCUBE/flux-config/issues/1#issuecomment-1689488002
Thanks yes, now I have a similar error, thanks.
Note that the script/request I shared worked for UC3 a few weeks ago.

Best,

-Bachir.
- Sysadmns restarted pygeoapi now and triggered again -> can see pygeoapi-job-ee64cd3e-847b-11ef-9e15-6e556aa22337-5qr6h job pending now as it is starting up GPU node -> to be checked later....

pygeoapi-job-ee64cd3e-847b-11ef-9e15-6e556aa22337 in eurodatacube18 (UC4) indicates "succeeded", please check
Thanks!
I am still having the error above.
In summary:
1- The following runs successfully under UC3:  
```bash #

curl -X POST -v https://headless-fairicubeuc3.hub.eox.at/processes/execute-notebook/jobs \
    -u user:psw \
     --header 'Content-Type: application/json' \
     --data-raw '{"inputs": {
      "notebook": "s3/Slicing using Headless Execution/Slicing_Headless.ipynb",
      "cpu_requests": "1",
      "cpu_limit": "1",
      "mem_requests": "4G",
      "mem_limit": "4G",
      "node_purpose": "userg1",
      "kernel": "conda-env-eurodatacube8-torch-py"
    }}'

```
2- The following under UC4 has error ```500 Internal Server Error```:
```bash #

curl -X POST -v https://headless-fairicubeuc4.hub.eox.at/processes/execute-notebook/jobs \
     -u user:psw \
      --header 'Content-Type: application/json' \
      --data-raw '{"inputs": {
       "notebook": "s3/scripts/Roof_height_ML.ipynb",
       "cpu_requests": "2",
       "cpu_limit": "2",
       "mem_requests": "8G",
       "mem_limit": "8G",
       "node_purpose": "userg1",
       "kernel": "conda-env-eurodatacube18-torch-py"
     }}'
```
3- The following under UC4 has error ```"code":"InvalidParameterValue",  "description":"invalid request data"```:
```bash #
curl -X POST -v https://headless-fairicubeuc4.hub.eox.at/processes/execute-notebook/jobs \
    -u user:psw \
     --header 'Content-Type: application/json' \
     --data-raw '{"inputs": [
      {"id": "notebook", "value": "s3/scripts/Roof_height_ML.ipynb"},
      {"id": "cpu_requests", "value": "1"},
      {"id": "cpu_limit", "value": "1"},
      {"id": "mem_requests", "value": "4G},
      {"id": "mem_limit", "value": "4G"},
      {"id": "node_purpose", "value": "userg1"},
      {"id": "kernel", "value": "conda-env-eurodatacube18-torch-py"}
    }}'
```


The following updates have been performed to to FAIRiCUBE Hub:

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


All 5 endpoints have a uniq basic-auth configured --> TEAMS (https://nilu365.sharepoint.com/sites/Horizon2021_CUBE/_layouts/15/Doc.aspx?sourcedoc={235313bb-424e-4a1e-b1d6-92296d28fbfc}&action=edit&wd=target%28technical%20library.one%7C18ca003a-ff29-4de7-925e-1f11804605c2%2FEOxHub%20headless%20execution%7C6d55dc09-f667-4dac-99d1-6d67687afc59%2F%29&wdorigin=703)

All the headless endpoints can be started either with:
 - node_purpose "user" (for regular CPU)
 - node_purpose "userg1" (for single GPU) or
If node_purpose is not passed then "userg1" (GPU) is the default!

In addition, the smallest Multi GPU VM available on eu-central-1 g4dn.12xlarge (4 x NVIDIA T4 16 GiB) -> for $4.89 per hour on "userg2"  has been configured.
Only  UC3 (eurodatacube17) and UC4 (eurodatacube18) are whitelisted for using "userg2"!

The following calls for headless execution are tested and work.
Please also notice the syntax change at the URL (jobs -> execution)


```
#--------------
UC1 - with CPU:
#--------------
curl -X POST -v https://headless-fairicubeuc1.hub.eox.at/processes/execute-notebook/execution \
    -u <USERNAME>:<PASSWORD> \
     --header 'Content-Type: application/json' \
     --data-raw '{"inputs": {
      "notebook": "s3/pytorch_verification.ipynb",
      "cpu_requests": "1",
      "cpu_limit": "1",
      "mem_requests": "4G",
      "mem_limit": "4G",
      "parameters_json": {"a": "b", "c": 111},
      "kernel": "conda-env-eurodatacube19-torch-py",
      "node_purpose": "user"
    }
}'

#--------------
UC2 - with CPU:
#--------------
curl -X POST -v https://headless-fairicubeuc2.hub.eox.at/processes/execute-notebook/execution \
    -u <USERNAME>:<PASSWORD> \
     --header 'Content-Type: application/json' \
     --data-raw '{"inputs": {
      "notebook": "s3/common-code/pytorch-verification/pytorch_verification.ipynb",
      "cpu_requests": "1",
      "cpu_limit": "1",
      "mem_requests": "4G",
      "mem_limit": "4G",
      "parameters_json": {"a": "b", "c": 222},
      "kernel": "conda-env-eurodatacube8-torch-py",
      "node_purpose": "user"
    }
}'

#--------------
UC2 - with GPU:
#--------------
curl -X POST -v https://headless-fairicubeuc2.hub.eox.at/processes/execute-notebook/execution \
    -u <USERNAME>:<PASSWORD> \
     --header 'Content-Type: application/json' \
     --data-raw '{"inputs": {
      "notebook": "s3/common-code/pytorch-verification/pytorch_verification.ipynb",
      "cpu_requests": "1",
      "cpu_limit": "1",
      "mem_requests": "4G",
      "mem_limit": "4G",
      "parameters_json": {"a": "b", "c": 222},
      "kernel": "conda-env-eurodatacube8-torch-py",
      "node_purpose": "userg1"
    }
}'

#--------------
UC3 - with CPU:
#--------------
curl -X POST -v https://headless-fairicubeuc3.hub.eox.at/processes/execute-notebook/execution \
    -u <USERNAME>:<PASSWORD> \
     --header 'Content-Type: application/json' \
     --data-raw '{"inputs": {
      "notebook": "s3/Test Headless Execution/pytorch_verification.ipynb",
      "cpu_requests": "1",
      "cpu_limit": "1",
      "mem_requests": "4G",
      "mem_limit": "4G",
      "parameters_json": {"a": "b", "c": 333},
      "kernel": "conda-env-eurodatacube17-torch-py",
      "node_purpose": "user"
    }
}'

#--------------
UC4 - with CPU:
#--------------
curl -X POST -v https://headless-fairicubeuc4.hub.eox.at/processes/execute-notebook/execution \
    -u <USERNAME>:<PASSWORD> \
     --header 'Content-Type: application/json' \
     --data-raw '{"inputs": {
      "notebook": "s3/pytorch_verification.ipynb",
      "cpu_requests": "1",
      "cpu_limit": "1",
      "mem_requests": "4G",
      "mem_limit": "4G",
      "parameters_json": {"a": "b", "c": 444},
      "kernel": "conda-env-eurodatacube18-torch-py",
      "node_purpose": "user"
    }
}'

#--------------
UC5 - with CPU:
#--------------
curl -X POST -v https://headless-fairicubeuc5.hub.eox.at/processes/execute-notebook/execution \
    -u <USERNAME>:<PASSWORD> \
     --header 'Content-Type: application/json' \
     --data-raw '{"inputs": {
      "notebook": "s3/pytorch_verification.ipynb",
      "cpu_requests": "1",
      "cpu_limit": "1",
      "mem_requests": "4G",
      "mem_limit": "4G",
      "parameters_json": {"a": "b", "c": 555},
      "kernel": "conda-env-eurodatacube20-torch-py",
      "node_purpose": "user"
    }
}'

```
Thank you for the update 
I have tested for UC4 and I have had the following error:
```
<title>500 Internal Server Error</title>
<h1>Internal Server Error</h1>
<p>The server encountered an internal error and was unable to complete your request. Either the server is overloaded or there is an error in the application.</p>
* Connection #0 to host headless-fairicubeuc4.hub.eox.at left intact
```  
Logs showed the following: 
OSError: [Errno 107] Transport endpoint is not connected: '/home/jovyan/s3/headless/2024-10-21/20241021-101340-349402-Roof_height_ML'
[2024-10-21T10:14:36Z] {/pygeoapi/pygeoapi/api.py:3339} ERROR - Invalid control character at: line 5 column 44 (char 211)
[2024-10-21T10:14:36Z] {/pygeoapi/pygeoapi/api.py:3854} ERROR - invalid request data

pod was restarted
Test request was successful afterwards. I am not sure why, but I am getting <title>500 Internal Server Error</title> now!
sorry,  there is a problem with the mounting of the s3, it fails sometimes and then the pod needs a restart, which currently is done manually since we haven't quite figured out yet how to check for the failure. 
It works again - tested it myself.