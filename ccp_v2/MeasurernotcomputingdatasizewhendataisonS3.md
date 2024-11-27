Measurer not computing data size when data is on S3
I have noticed that the Measurer returns 0 for `Data size` if the data is stored on an S3 bucket (as is the case for FAIRiCube/EOX Hub current architecture).  do you see the same behavior? Do you have the possibility to test it? 
Hi
I fixed the problem using [boto3](https://boto3.amazonaws.com/v1/documentation/api/latest/index.html) to access Amazon AWS and get the bucket and its contents.
The [aws_example.py](https://github.com/FAIRiCUBE/common-code/blob/main/record-computational-demands-automatically/aws_example.py) file is an example of use with AWS S3.
What do you think about it?
Hi Antonio,

I am not sure I understood how it works. I have set the AWS credentials and the bucket name. 
```
from measurer import Measurer
from types import ModuleType
import boto3
import os
import xarray as xr

import pandas as pd

aws_session = boto3.Session(
    aws_access_key_id=os.environ.get("username"),
    aws_secret_access_key=os.environ.get("password") 
)

def your_function():
    # Use the name of the AWS S3 bucket
    data_path = 'hub-fairicube0'
    measurer = Measurer()
    tracker = measurer.start(data_path=data_path, aws_s3=True, aws_session=aws_session)
    shape = []

    print("Hello")

    measurer.end(tracker=tracker,
                 shape=shape,
                 libraries=[v.__name__ for k, v in globals().items() if type(v) is ModuleType and not k.startswith('__')],
                 data_path=data_path,
                 program_path=__file__,
                 csv_file='benchmarks.csv',
                 aws_s3=True,
                 aws_session=aws_session)
    
    
if __name__ == "__main__":
    your_function()

```
I understand that this way it should compute the total memory size of the bucket, right?
But the result is always 0.

And if I want to get the memory size of only one file/folder?
Hi 
in this version is returned the disk space used by the measured script. Therefore, because _print("Hello")_ doesn't use disk space, the measurer returns zero. 
Currently the disk space used by the script is calculated as  `bucket size at the end - bucket size at the start`.
If there is a need to get the total size of the bucket (or a file) instead of the size change I modify the meter
Hi, so in other words, it measures if new data is written to the bucket?
yes, written or deleted
Hi Antonio,
thanks for the clarification, I got it wrong, I thought it was measuring the disk size of data found in the `data_path`, regardless whether it was used in the script or not. I tested the Measurer again (writing data to the bucket) and saw that the "default" solution works for S3 as well  (at least in the Fairicube hub, where the bucket is mounted in the file system).
So, I think we can close this issue. Maybe we can update the readme to clarify what is being measured.
Hi
the important aspect is to have solved the problem  
Maybe it is better to leave the updated version that gives the possibility to measure the space even when the bucket is not mounted. In this way, if the bucket is mounted in the file system, you can use both methods.
Yes, if there is no need to also return the total size of the bucket we can close the issue.
I agree about detailing this in the readme.
Thank you!
Closing the issue after having updated the readme with a description of the measured parameters.