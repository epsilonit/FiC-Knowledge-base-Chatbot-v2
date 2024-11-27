Sharing example of data on S3 bucket
this issue is somewhat related to https://github.com/FAIRiCUBE/FAIRiCUBE-Hub-issue-tracker/issues/73 and the ongoing investigation on API capabilities on the EOX stack

All/most of the UCs started now to work on data stored on S3 buckets. We have learned that we can expose / share this data to others through registration on Sentinell hub and configuration of the S3 bucket (?). We need a working example to show and test this! This is essential to make sure that we are not creating a dead-end on S3 but can actually give other people access - even if it means that we pay for it. By testing this, we can answer : How the access looks like, if this data can be querried... 

any dataset from any S3 bucket from any UC can be a starting point. Only someone from EOX could prepare this example or provide clear guidance!
I've added some documentation regarding data storage and sharing. Please review the pull request either at https://fairicube--8.org.readthedocs.build/en/8/guide/storage/ or https://github.com/FAIRiCUBE/collaboration-platform/pull/8
one thing I briefly discussed with a colleague was to "publish" a dataset (e.g. csv) file through github push into the repo from where you can create a link to fetch the data from grafana. this of cause only works for smaller files...
the FiC documentation referenced above lacks exact details but provided hints for further reading / testing, I will therefore close the issue for now