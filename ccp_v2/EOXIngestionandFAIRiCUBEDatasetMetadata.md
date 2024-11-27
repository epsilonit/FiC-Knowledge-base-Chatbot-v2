EOX Ingestion and FAIRiCUBE Dataset Metadata 
When ingesting data under rasdaman, we now have a fairly consistent process where we first create an issue containing all relevant information for the desired dataset (either directly as a data request [Issue](https://github.com/FAIRiCUBE/data-requests/issues), or by activating the entry for the relevant dataset via the [Project](https://github.com/orgs/FAIRiCUBE/projects/1/views/1)), triggering:
- ingestion of the dataset
- creation of a metadata record for the dataset by extracting this information from the request issue

When ingesting data under EOX, to my understanding the UC partners have to fend for themselves, upload the data to the bucket, fill the metadata forms for EDC/SentinelHub. How is the corresponding metadata record for this dataset generated? Does this utilize the EDC approach with AWS compliant YAML? How will dataset metadata concepts not covered under EDC be provided? any updates on this alignment? Additional fields resulting from this should be added to the [additional fields issue](https://github.com/FAIRiCUBE/data-requests-issue-archive/issues/49) IS ANYBODY OUT THERE???
just to document, from our side we have [this issue](https://github.com/FAIRiCUBE/FAIRiCUBE-Hub-issue-tracker/issues/21) for discussing rasdaman/metadata linkage. when can we expect documentation on this process under EOX, expect to soon find it in read-the-docs
once available, ticket can be closed?