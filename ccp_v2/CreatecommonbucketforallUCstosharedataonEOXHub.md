Create common bucket for all UCs to share data on EOX Hub
each UC has now an individual workspace with a S3 bucket for their data. once we want to share ingested data, only the respective UC can access the data. we would therefore need to create common bucket for all UCs to share data, i.e. in addition to a bucket for each UC, we need to have one bucket where all UCs have r/w access

we have talked about this already, but I dont see a corresponding issue. Stefan could you please provide an time estimation for this common bucket to be created, thx
Hi Christian, I would say as soon as possible, ideally this year. From previous communication, I understood that this is not require major effort, but gladly provide a time estimate when this can be available. 
so just to be explicit - we shall create a new bucket in your AWS tenant with one credential set allowing to r/w - and the bucket name and this credential set should be made available to all people in all use cases so everyone is allowed to read and write

please confirm, thanks
that sounds about right!
the bucket `s3://fairicube` got created in `eu-central-1`

necessary environment variables (`S3_FAIRICUBE_STORAGE_xxx`) are automatically injected into JupyterLab sessions of **all** use-cases, i.e. all user associated to a fairicube use-case may read and write on this bucket!

verified by installing s3cmd and running
```
s3cmd ls --access_key=${S3_FAIRICUBE_STORAGE_KEY} --secret_key=${S3_FAIRICUBE_STORAGE_SECRET} --region=eu-central-1 s3://${S3_FAIRICUBE_STORAGE_BUCKET}
```
from within in a use-case profile can you please document accordingly 
storage has been setup and is configured for use by all FAIRiCUBE members
Hei and or test if that works for you? you could try to test-ingest a data set there to see if we can share data across UCs? Many thanks....
Hi,
Thank you for setting this up.
I cannot find it among the server options (check screenshot attached).
Does it correspond to **FAIRiCUBE-UC1 (large)**?

Thanks in advance,

Best regards,

-Bachir.
![image](https://github.com/FAIRiCUBE/FAIRiCUBE-Hub-issue-tracker/assets/117657891/8c11bcb9-7dd4-4435-81ce-06eb9f4c98ba)

Hey Bachir, 
the S3://fairicube bucket is accessible from the inside of every UC. 
see also my email or comment by "achtsnits" above
Hi Christian.

Perfect! Thank you for your quick response.

Best regards,

-Bachir.
Hello,
I have uploaded an example dataset: `climate_data/eobs/tg_ens_mean_0.1deg_reg_v28.0e.nc` (downloaded from: https://surfobs.climate.copernicus.eu/dataaccess/access_eobs.php#datafiles)
and tested connection successfully
Couple of questions:
- What is the preferred folder structure? I'd use a folders for different themes (e.g. land data, climate data, ecological data etc).
- Should we create metadata record (in the data inventory sheet or the like) for the data uploaded to this s3 bucket as well?
Question - what's the difference in using data from the UC specific bucket vs. the common bucket? I'm assuming that this is fairly similar.

Under that assumption, I think it would be best if datasets were generally ingested into this common bucket. That way, if anybody else needs that dataset, it's already available (and if nobody else needs it, there's no additional work involved).

If we go for this shared approach, I'd say that the dataset metadata records are more important on these shared resources (as long as the resource is only hidden in your private bucket, no reason to provide complex metadata for data taken from external sources, just make sure to store the information on source and time of download close to this bucket!)
Hello,

I have also tested it via terminal with Read/Write operations.
I successfully downloaded the test.txt file and uploaded the TestBachir.txt file to the s3/fairicube bucket and local_file.txt to s3/fairicube/climate_data. please check if you can see those files using:
```
s3cmd ls --access_key=${S3_FAIRICUBE_STORAGE_KEY} --secret_key=${S3_FAIRICUBE_STORAGE_SECRET} --region=eu-central-1 s3://${S3_FAIRICUBE_STORAGE_BUCKET} 
```

Thanks in advance,

Best regards,

-Bachir. : agreed! 
the UC and shared bucket can be used / adressed in the same way (from my understanding). so, yes, all data should go there and metadata should be provided through our ingestion pipeline. : we had an issue with the AWS access key for the user "fairicube" that handles/bundles the access rights for the common bucket? I had to delete the previous AWS access key and created a new one. I would assume that you have to update now some environmental variables?

--access_key=${S3_FAIRICUBE_STORAGE_KEY}


I'm currently not available but will recreate and inject new credentials end of the week when I'm back. new AWS credentials got created for the common bucket -> this new credential set is now automatically injected for all use-cases again

i.e. the previously shown example call
```
s3cmd ls --access_key=${S3_FAIRICUBE_STORAGE_KEY} --secret_key=${S3_FAIRICUBE_STORAGE_SECRET} --region=eu-central-1 s3://${S3_FAIRICUBE_STORAGE_BUCKET}
```
works again and can you confirm that you can access and add content to both our new FiC common bucket? it looks fine to me, but I only checked through the S3 browser. if so, we can close the issue
I confirm I can access and add content to the FiC common bucket.
I confirm having access to FiC common bucket through the terminal.
as now confirmed by three parties,  FiC common bucket is available and operational, many thanks to all involved!