Sharing data across FAIRiCUBE UCs on EOXHub
In preparation of the training days, I am testing how to share / exchange data from one UC to another.

as described in  https://fairicube--8.org.readthedocs.build/en/8/guide/storage/#file-storage

there is the 
1) shared folder mounted under /shared/fairicube/
**but the folder has only read-access which makes it impractical for adding new data for exchange. can this folder please get write rights as well?**

2) common FAIRiCUBE bucket
where I once found the email instructions 

The new S3 bucket (s3://fairicube) is accessible using standard S3 tools and got created in eu-central-1.

Access can be verified/tested e.g. in the Terminal using (in one line): 
s3cmd ls --access_key=${S3_FAIRICUBE_STORAGE_KEY}  --secret_key=${S3_FAIRICUBE_STORAGE_SECRET} --region=eu-central-1  s3://${S3_FAIRICUBE_STORAGE_BUCKET}

but how to actually us the common FiC bucket in command line commandos to copy files is something that I have to find out. Both surely needs to be documented somewhere.

3) in addition 
the "team-extra" folder? What is this? A shared folder across members of a use case? could be included in the RtD if seen relevant.
 
Related to #61 
I have now tried both routes on the jupyterlab terminal:

pip install awscli
which would give me commands like 
aws s3 cp s3://source-bucket-name/file.txt s3://destination-bucket-name/file.txt
resulting in
fatal error: An error occurred (403) when calling the HeadObject operation: Forbidden

s3cmd cp s3://source-bucket-name/file.txt s3://destination-bucket-name/file.txt
resulting in 
ERROR: Copy failed for: 's3://fairicube/test.txt' (403 (AccessDenied): User: arn:aws:sts::419258156769:assumed-role/fairicube-core-eks-node-group-role/i-091e50c22c5885161 is not authorized to perform: s3:GetObjectAcl on resource: "arn:aws:s3:::fairicube/test.txt" because no identity-based policy allows the s3:GetObjectAcl action)

I guess this has something to do with privileges / access rights in both buckets (by design), I now have to find out how to get something like  scp user@destination_server:file user@target_server:file  sorry to hear your struggle. We're trying to improve and simplify the setup based on your feedback. For this we've implemented a new feature that allows us to mount 2 buckets in parallel.

We've configured this for testing in the fairicubeuc1 profile and mounted the common bucket to `~/.fairicube-bucket/`. Note the starting `.` which in linux denotes a hidden directory and thus it is not automatically listed in the file browser. We recommend to mount this in a hidden directory as listing the content in the file browser might slow down the whole JupyterLab instance significantly if there are a lot of files inside. If you want to see it in the file browser anyway feel free to add a symbolic link with a command like `ln -s .fairicube-bucket/ your-name`.

Please have a look and let us know if we should roll out this configuration to the other use case and profiles and maybe the training profiles as well.

In general we recommend using the buckets to share data as the shared folder is also used to store system files and wrong use might lead to side effects. I've update the documentation accordingly (see https://fairicube--10.org.readthedocs.build/en/10/user_guide/storage/).

The `~/team_extra/` is an additional file storage shared among the users in one team/customer but again we recommend to use the team bucket which is mounted under `~/s3/`.

Please let me know if you want me to adjust any configuration like for example directory names where buckets are mounted.
Hello I think it is a viable solution.
I have tried
`s3cmd put ~/.fairicube-bucket/vienna_data/100m/r* s3://hub-fairicubeuc1-training/UC1_training`
from the fairicubeuc1 profile. It works! 
I had to manually copy the credentials for the hub-fairicubeuc1-training bucket as they are not available in the fairicubeuc1 profile, but if the common bucket is mounted in the right profile then its no issue.

Side note: Having now multiple profiles, I realized it is not obvious to know which one is active while inside the workspace. For example, which s3 bucket is mounted to `/s3`? Would it be possible to name the `/s3` folder after the bucket name? since we didn't get an answer from you we simply assume that the setup is ok and will roll it out the configuration to mount the common bucket to all 8 currently configured teams and all profiles therein. thanks for your feedback. Yes, we could easily change the name of the directory where the team or use case specific bucket is mounted. However, I'd like to point out that all scripts and notebooks would need to be updated to reflect that change and in addition you couldn't easily write a script that works across use cases anymore. I'd recommend to simply put an empty file in the root of the bucket that shows the use case name instead. Anyway, please let me know if you still want to change the name.


I asked to test as I was / am on schoolvacation break. As it seems to work fine and is confirmend by Maria, it is good to have this rolled out to all profiles. Many thanks. if there is no further development missing (apart from changing the name of the bucket), we can close the ticket?
Hi I see your point on losing interoperability between profiles ;) no need to change the name
I have just had a meeting with to confirm that everything works.
I can see bucket content when I run ```ls ~/.fairicube-bucket``` but only on fairicubeuc1 server option as for now.
the buckets are now configured for all profiles 
Thanks
I confirm that I have access from my available server options.