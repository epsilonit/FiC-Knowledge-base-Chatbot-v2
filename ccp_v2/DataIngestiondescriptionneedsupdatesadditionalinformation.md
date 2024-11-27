Data Ingestion description needs updates/additional information
I was just looking at the [data ingestion section](https://fairicube.readthedocs.io/en/latest/guide/adding_datasets/#data-request) in rtd, find some incorrect statements:

`Once all metadata and data requirements are fulfilled and confirmed by the data requester, the ingestion handling partners will perform the merge and the Pull request will be closed. `

There is no description of how the data actually gets to the desired platform. On rasdaman this task is covered by CU, automation by students, while on EOX the UC partner must assure that the data is stored on S3. Where is this described?

Also, the bit at the end that the PR will be closed would need some more detail, e.g., requiring confirmation from requesting UC before closing