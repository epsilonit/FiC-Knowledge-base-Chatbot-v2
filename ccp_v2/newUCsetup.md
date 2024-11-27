new UC setup
for the upcoming training day, we need to setup new "UC" = student group environments in order to provide access to FAIRiCUBE resources. this is an excellent chance to document what needs to be configured where to give access, i.e. testing the user access and management routines

what I did / could do so far:
- create new repo https://github.com/FAIRiCUBE/uc1_training
- created new git hub user sjetuc1 
- added sjetuc1 to FiC project (invite member)
- Joined with this new user
- created a new folder in the flux-config repo https://github.com/FAIRiCUBE/flux-config/tree/master/fairicubeuc1_training 
- copied customer.yaml from UC5, replaced all references from UC5 to UC1_training

whats next?
- create EOX server profile ? (@achtsnits)
- create AWS resource tag to monitor costs that this UC consumes in the AWS billing overview (@achtsnits )
- further changes in customer.yaml file needed ?
- create account for data ingestion WebGUI  (Mussab?)
- create rasdman account using the sjetuc1 github handle (Dimitar?)

essential input from the technical partners
- what did you do to enable the new user to enter the system
- it is enough to verbally describe it briefly when the system is not "exposed to FiC", i.e. only you can do it
- it should be documented here what needs to be done when the configuration files are exposed to FiC, e.g. the customer.yaml
- later, we will compile a separate documentation on FiC KB
> create rasdman account using the sjetuc1 github handle (Dimitar?)

Yes, just send me an email like recently for the new NHMW project member.
first learning : the configuration in the customer.yaml does not accept underscores in the meta-data and namespace names. Christian changed the " fairicubeuc1_training" to " fairicubeuc1-training" and the setup finished. 

the following internal error message appeared via slack which is not visible for EOX-externals:
>>"...subdomain must consist of lower case alphanumeric characters, '-' or '.', and must start and end with an alphanumeric character..."

in any case, the server option appears now under 
https://eoxhub.fairicube.eu

![grafik](https://github.com/user-attachments/assets/8aad37ea-5aeb-4925-86cc-ceec40989c53)

but returns an error while starting

![grafik](https://github.com/user-attachments/assets/c090436d-4d3c-46c0-b811-83cd599447f0)



the AWS setup for the new nodepool was done now and the UC1 training profile got connected to it

just successfully opened this profile and a corresponding node of this nodepool got started

hourly consumption can be tracked via "useruc1training" tag


excellent, I can confirm that I can start now the server option for UC1-training. I dont see the tag yet in the AWS cost analysis but this might take more time (or actually resources consumed under this option) or : it looks like we have the setup almost complete now. 

What is missing is the configuration of SentinelHub. can you please include this new server profile so that we can access SentinelHub data?
we now need more "UC accounts" for the students as there are potentiall many interested students working on the GIS course session.  I have therefore 

create new repo https://github.com/FAIRiCUBE/uc4_training
create new repo https://github.com/FAIRiCUBE/uc5_training

created a new folder in the flux-config repo https://github.com/FAIRiCUBE/flux-config/tree/master/fairicubeuc4_training
created a new folder in the flux-config repo https://github.com/FAIRiCUBE/flux-config/tree/master/fairicubeuc5_training

we now need to do the AWS setup for the new nodepools and connect the UC4/5 training profile to it
the hourly consumption shall be tracked via "useruc4training" tag
the hourly consumption shall be tracked via "useruc5training" tag

for this and the uc1_setup we need to configure the SentinelHub access.
we completed the necessary setup for fairicubeuc4-training and fairicubeuc5-training

just could successfully launch these new profiles, the corresponding SH credentials are injected there (note: should work for fairicubeuc1-training as well)

not sure if this has been discussed before but important to repeat here: as soon as the training is done you must not just delete the created customer objects but follow the following process
1) if you still want to grant access for the training users to download code/notebooks,.. you should set the expirationDate on the customer object -> will show expired on login screen but still allows login with limited resources (just enough to start JupyterLab and download files)
2) if you want to remove access for them you should remove (or comment) the line(s) starting with profile_ in the customer object

hope that helps
many thanks, for the super fast help and the additional comments. that is much appreciated. I will test the access now and keep the advice in mind once we are closing off the training period. just to be sure. what do you mean by "customer objects" is this the whole customer.yaml file or just single entries there?

when disabling single users, I cannot just remove the entry like ?

  - approvalTimestamp: "2024-08-21T00:00:00Z"
    creationTimestamp: "2024-08-21T00:00:00Z"
    email: sjet+awsuc1@nilu.no
    userName: sjetuc1
    role: user

the only "expiration date" is defined under the section "spec" but this applies to all whole customer.yaml file?
all new server profiles start up normally, can you please verify the sentinel hub access and then we can close the issue
you can always add and remove/comment users but you shouldn't just delete the whole customer object  or change the name within
I confirm that the three server options (UC1, 4, 5 training) have sentinel hub access (you can check the testing notebook in each s3 bucket).