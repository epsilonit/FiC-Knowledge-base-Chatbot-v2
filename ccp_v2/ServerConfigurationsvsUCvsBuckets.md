Server Configurations vs. UC vs. Buckets it seems there is a great deal of confusion on Server Configurations vs. UC vs. Buckets.

At present, we have 3 Server configurations (2 for UC1, 1 for UC4), 5 UC working on EOX, no clarity on what Buckets are linked to what Servers, available to what UC.

I'm wondering if part of the confusion is due to the naming of the Servers by UC, is this still valid?

Please clarify!
The respective kernels (to be selected on the beginning of a session) influences the software tools available (as requested by each UC), but has no influence on the availability of the shared/private folders (==> https://github.com/FAIRiCUBE/FAIRiCUBE-Hub-issue-tracker/issues/61) could we better name and document these servers? At present, it looks like only UC1&4 can work on EOX. Also, where can UC see what SW tools are available on which server?
> could we better name and document these servers? 

all kernels are named after their UC, what name-changes do you envision ?

>  At present, it looks like only UC1&4 can work on EOX. 

Sorry I don't understand what you mean? why can only UC1 & UC4 perform some work?  All UC environments work.


> Also, where can UC see what SW tools are available on which server?

Inside UC environment you also need to select the Kernel you ar interested in, then issue:
In the terminal  issue:
  $>   conda list
In a notebook cell: 
  $>  !conda list
 both provide the listing of currently installed tools for the respective/selected Kernel

Summary: 
The UC selection defines the Environment (i.e., RAM, CPU, etc.)
The Kernel defines the Software which is installed (e.g. ML-FLOW, DoWhy, etc.) I assume by "Server" you mean the JupyterLab profiles that you can select when accessing https://eoxhub.fairicube.eu where the list is headed by "Server Options" right? Each user only sees the profiles/server options of these use cases they are associated with. Your user is meanwhile associated with all UCs, e.g., UC1 (https://github.com/FAIRiCUBE/flux-config/blob/master/fairicubeuc1/customer.yaml#L91) and you should see all profile options. where can I find this documented?
related to #61