EOXHub: Notebook - CSV ParserError: Error tokenising data on 42 MiB file from S3 bucket
I am trying to read a (semi) large csv file (with species observation data) from the S3 bucket with Pandas read_csv. This fails with a ParserError (see screenshot).

![Screenshot 2023-09-01 at 14 18 10](https://github.com/FAIRiCUBE/FAIRiCUBE-Hub-issue-tracker/assets/1943289/65abdc08-667e-4b79-a05e-c1c57f16b899)

The problem seems to be related to the size of the file and S3, it does not occur locally nor when the file is split in smaller pieces. So the file itself seems to be correct.

Any idea for a solution? Please update the assignee to include the proper person for support questions like this, I just added you as my first guess :-) 
Hello, 
I have tried to load the data as well and it seems like the last row is corrupted. After loading the data locally, it seems like last row - 129792  - do not have properly ended "wkt" field ("SRID=28992;POLYGON((195381.196459009 530485.725643993,195378.267526821 530478.654576181,195371) and data for column "straal" are missing. Could you check the data and see if this issue persist when the columns are filled with data?
Hi Tyna,

Thanks for noticing. It seems that the file got truncated when uploading to the S3 bucket. The original has 454453 lines. I uploaded it again and now all bits made it across. Which resolved the issue.