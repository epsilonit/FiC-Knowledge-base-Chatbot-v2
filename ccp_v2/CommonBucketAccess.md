Common Bucket Access
Hi all.
So far I have only worked on the EOX Hub in the UC specific bucket and only via eoxhub.fairicube.eu.
It's still not entirely clear to me how to get access to the FiC common bucket.

There is a reference to https://s3browser.com/ in the [FAIRiCUBE Notebook](https://nilu365.sharepoint.com/sites/Horizon2021_CUBE/_layouts/15/Doc.aspx?sourcedoc={235313bb-424e-4a1e-b1d6-92296d28fbfc}&action=edit&wd=target%28technical%20library.one%7C18ca003a-ff29-4de7-925e-1f11804605c2%2FS3%20Bucket%20browser%7C99988fa6-7c22-40fc-871e-e28bfdb5ab99%2F%29&wdorigin=NavigationUrl).
Our IT team has now informed us that we at the NHM cannot use the software because the application is only licensed for personal use and we at the NHM would need a paid ProVersion which is not feasable at the NHM.
I wanted to point this out first, maybe it is relevant for others as well.

There are several alternative AWS clients, including the command line tool **s3cmd.**
Do you also have something like that? Experience and can recommend an alternative or including instructions?

FYI, I tried s3cmd myself:
However, s3cmd cannot be successfully configured with the environmental variables from the FiC hub alone. The following parameters are required and my information about them would be in bold
(Keys for this email are of course not written out but left as a variable):
Access Key: ($**S3_FAIRICUBE_STORAGE_KEY**)
Secret Key: ($**S3_FAIRICUBE_STORAGE_SECRET**)
Default Region [US]: Default **US**
S3 Endpoint [s3.amazonaws.com]: Default **s3.amazonaws.com**
DNS-style bucket+hostname:port template for accessing a bucket: **fairicube.s3.amazonaws.com** 
I then get an Access Denied Error including “Are you sure your keys have s3:ListAllMyBuckets permissions?” which is documented [here](https://github.com/s3tools/s3cmd/blob/22949ba66532f902497c5c68cf54408433aa9570/README.md?plain=1#L144)...
Do our keys have those permissions?

If you have any tips or advice, that would be very helpful!


Best regards
Sonja when registering the bucket with s3browser I also get the error "not allowed to list all buckets", but it should be possible to connect to a named bucket anyway. Another thing that you should check is the AWS region, it should be eu-central-1.

FYI, I have set up an example script that shows how to connect to an s3 bucket with the Python library `s3fs` here: https://github.com/FAIRiCUBE/common-code/blob/main/access_data_apis/access_s3_bucket.py Thank you Maria!! The AWS region was essential configuration value and also I integrated your script now :)
I was able to access the Vienna100m data  
the issue is therefore resolved now? Yes the issue is resolved
just adding a link to the FAIRiCUBE Notebook page as well:

https://nilu365.sharepoint.com/sites/Horizon2021_CUBE/_layouts/OneNote.aspx?id=%2Fsites%2FHorizon2021_CUBE%2FSiteAssets%2FFAIRiCUBE%20Notebook&wd=target%28technical%20library.one%7C18CA003A-FF29-4DE7-925E-1F11804605C2%2FS3%20Bucket%20browser%7C99988FA6-7C22-40FC-871E-E28BFDB5AB99%2F%29
onenote:https://nilu365.sharepoint.com/sites/Horizon2021_CUBE/SiteAssets/FAIRiCUBE%20Notebook/technical%20library.one#S3%20Bucket%20browser&section-id={18CA003A-FF29-4DE7-925E-1F11804605C2}&page-id={99988FA6-7C22-40FC-871E-E28BFDB5AB99}&end