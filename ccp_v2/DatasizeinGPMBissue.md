Data size in GP / MB issue
I have been running my resources-montoring valdiation for the script:

https://github.com/FAIRiCUBE/uc3-drosophola-genetics/blob/main/projects/gap_filling/src/data/load_tsv_kmeans_elbow_sli.py

where I had a previous "manual" estimate. 

While the monitoring itself works smooth (after solving the typical issue of installing libaries) I am missing some info:

0      Data size in grid points                              
>> No value provided   
>> can glaldy be the size of the largest array or ideally be the sum of the largest arrays (if possible)
1                 Data size (MB)                                          0.1484375     
>> the original idea was to describe the size of the input data, but this might be a manual process to find out. what is measured here? in any case, less than 1 MB is rather small....

8           Network traffic (MB)                                   0.23931884765625
>> well, it was a local job nothing loaded from the network, but it is a small number, so maybe "noise"
Hi 

- **Data size in grid points** currently takes a user-specified value as size. Currently, this mode is in use because the format of the data can be different and, in addition, the user has the option of specifying the size as most appropriately for the use case. Also, through simple calls (for example _my_array.shape_) it is easy to retrieve the size. 

- **Data size (MB)** measures data consumed or freed up on disk (by creation or deletion of files)

- **Network traffic (MB)** has a small value because the 'codecarbon' library uses remote calls to calculate emissions. 
From codecarbon's official page:
_"An offline version is available to support restricted environments without internet access. The internal computations remain unchanged; however, a country_iso_code parameter, which corresponds to the 3-letter alphabet ISO Code of the country where the compute infrastructure is hosted, is required to fetch Carbon Intensity details of the regional electricity used."_
Because the network used is small and the required code cannot always be easily found, I preferred to use the "online version"

What do you think about it?
ahja, now I undertstand, is there a chance that you add a short routine that checks for the largest allocated array and take this as dimension. in any case then need to call this parameter "largets allocated array (in grid points)". if possible at all, we could sum up the allocated variable sizes of the whole workspace, it is sort of similar to the allocated main memory but more specific to the script.

I have tested further the "data size" and while running my script
https://github.com/FAIRiCUBE/uc3-drosophola-genetics/blob/main/projects/gap_filling/src/data/load_csv_apply_GapFil_write_csv.py

and export 82 MB data, the Measurer says: 0.0 MB can you have a look there? any update here? we would need some clarification to provide input for deliverable D3_3
Hi 

I'm working on a procedure to obtain:
- the sum of all variables allocated by the script (in GB) 
- the largest array allocated in grid points. It is the variable, instance of _np.ndarray_, with the highest size value. In NumPy _size_ is the number of elements in the array (the product of the array’s dimensions).

The procedure is already available and working in the _measurer.py_ updated a few minutes ago on GitHub (please also have a look at the updated _example.py_).  I'm currently doing further tests on the procedure which seems to work well.
What do you think about it?

Regarding the test mentioned above, was the file already present in the folder? In this case, an overwriting occurs and the measurer returns 0 (this also happened to me during one of my tests :smile:)
Otherwise, can you provide me a few screenshots of the use of the measurer in the script?
Hi ,
thanks for the update. I have pulled the latest version but I see no change. even if I delete the csv file before execution, the data size in MB states 0.0 but should be around 80 MB. Data size in GP is as specified, largest allocated array is empty as well... 
the example above 

https://github.com/FAIRiCUBE/uc3-drosophola-genetics/blob/main/projects/gap_filling/src/data/load_csv_apply_GapFil_write_csv.py

should run within a cloned repo (only directory name needs to be modified). you could reproduce my exact situation.
cheers
Stefan

Hi

I did a copy and paste of the file https://github.com/FAIRiCUBE/uc3-drosophola-genetics/blob/main/projects/gap_filling/src/data/load_csv_apply_GapFil_write_csv.py updating the paths and adding start and end of the measurer.

In my test, the value of "_Data size_" is around 81 MB and the "_Largest allocated array in grid points_" is [50024, 334].

Probably the problem in your test is related to the paths used by the measurer (maybe the value of the parameter 'data_path' in the start/end methods is different from the one on which the file is saved).

The folder https://github.com/FAIRiCUBE/common-code/tree/main/record-computational-demands-automatically/test/uc3_test contains:
- the python script
- the benchmark csv file
- a screenshot of the output in the terminal 

cheers
Hi ,
many thanks for your example. I have tried to compare your script with mine and at the end just copy& pasted your uc3_test.py into my UC3 repo, changed the path to relative (so it runs completely within the repo):

https://github.com/FAIRiCUBE/uc3-drosophola-genetics/blob/main/projects/gap_filling/src/data/load_csv_apply_GapFil_write_csv_test.py

still, the same issue occurs. data size is reported to 0.0 MB. what can I test now to get to your "working environment"
cheers
Stefan

Hi, 

Thank you for this nice work.
I have created a general testing python script (you can find it here: https://github.com/FAIRiCUBE/common-code/blob/main/record-computational-demands-automatically/test/General_Test.py).
'Data size' seemed to work for me (it reported 31.5MB == size of the csv I wrote into disk). can you try this general test from your side?
Make sure that the csv file is not present when you run the code, otherwise it will report a size of 0.0 (overwriting file with same data = same size = no change in the hard disk state). 

Best regards,

-Bachir.


Hi, I have also tested the code provided in https://github.com/FAIRiCUBE/uc3-drosophola-genetics/blob/main/projects/gap_filling/src/data/load_csv_apply_GapFil_write_csv_test.py
I have changed the out_file_name to "Test.csv" and I got a data size of 78.42 MB.

Best regards,

-Bachir.

Hi and ,
I tested the script General_Test.py and that works for me as well, reporting 31.5 MB in data size. When I run my example 

https://github.com/FAIRiCUBE/uc3-drosophola-genetics/blob/main/projects/gap_filling/src/data/load_csv_apply_GapFil_write_csv_test.py

I get different results for every run. sometimes negative numbers, sometimes closer to 0, never anything like around 80 MB...  and 
ah well, I believe I see the problem now. 

first I tought : the Measurer "listens" to i/o access to  a specific path (not data_path ?). if I change the output destination of my csv output to the same directory where I write my measurer-statistics, then I see the 81 MB data size. if both files are written to different folders, then the Measurer does not report the correct data size... 

now I tested a bit more and it actually is a matter of whether the program outfile exists already or not. if I remove my output, Measurer works fine, if my program "overrides" the output with the same size and data, Measurer does not detect it! I see the same behavior for the General_Test.py. Once you run it more than once, it gives wrong results.
Hi,  yes, If I am not mistaken, I believed this is what had in mind for 'Data Size'.
'Data Size' answers the following question: **How much does my program delete or write in the hard disk?**.
So if your program does not change data in the hard disk (it just overwrites an existing one) then your program contributed with 0 MB (hence Data size of 0). Otherwise, if it deletes some file, then it returns a negative value indicating removing data. please correct me if I am wrong.

Best regards,

-Bachir.


Hi 
yes, this is my idea for the "Data size" field.  

cheers
many thanks for the clarification. I see the point and I believe it adds value to our table. However, "I/O stream volume" (amount of data being written - and actually beeing read as well) is also important and it was my original thought when we "requested the table". We can now keep the data_size but have to explain that properly. in addition, can you, think of a metrics to determine size of output regardless of file existence?
Hi 

I can add two fields:
- **Data written**: calculated as _[(system-wide number of bytes written at end of script) - (system-wide number of bytes written at start of script)]_ 
- **Data read**: calculated as _[(system-wide number of bytes read at end of script) - (system-wide number of bytes read at start of script)]_

For these fields I can use the [psutil.disk_io_counters](https://psutil.readthedocs.io/en/latest/#psutil.disk_io_counters) function which returns system-wide disk I/O statistics. 

This also adds up the overwrites but, on the other hand, because it is a system-wide calculation, eventual writes/readings concurrently with the script are also added. 
The quality of these measures depends on how many and which processes are using the disk during the execution of the script. Therefore, if the system during runtime only (or nearly only) executes the script that reads/writes to disk (as could frequently happen), the measurements could be quite accurate.

Regarding 'Data size' we can also consider renaming it and making it more explanatory.

What do you think about? that sounds very good!
Hi

Thanks again for your work.
For the 'Data size' field, I propose the following names: **'Disk I/O'** or **'Disk Activity'**

Best regards,

-Bachir. 
Disk I/O or disk activity actually means something different for me. it is the amount of data read and written to disk, regardless of a files being overwritten. 
for "data size"  I rather think of something like "data size added to storage" or "created data size"
Hi

Thank you again for your work.
I have recently tested the measurer on EOXHub.
I had some issues on the Compute resources metrics (see screenshot attached):

1- Main memory available (GB) was reported as 61 GB, whereas 32 GB are available.
2- Processor frequency was reported as 0.0 GHz. 
 
Can you investigate this?

Thanks in advance,

-Bachir.

![IssueMemoryEOX](https://github.com/FAIRiCUBE/common-code/assets/117657891/0ac26259-a844-466a-9473-c72b4647e98c)

![IssueMemoryEOX2](https://github.com/FAIRiCUBE/common-code/assets/117657891/0aae30b1-9bf0-4cfe-a765-dbc88b5fb491) : while I see the issue on data I/O and disk spaced added to disk can be resolved by a proper labeling of the value, what about the reporting by Bachir at EOX Hub, can this be worked on in the near future?
Hi, I have performed several tests and in none of them did I encounter this issue. For the I/O values, the fields have been renamed while, regarding main memory, could it be that less memory (32 GB) is allocated to the development environment than the entire machine (61 GB)? can you please have a look, if the issue has indeed resolved, we can close the issue here...
Hi, thank you for checking this.
While the reported main memory might be of the entire machine (instead of the allocated one), I still have 0.0 Ghz reported as frequency.
I am not sure if you have access to one of FiC EOX profiles?
It might be good to test it under the server from your side.

PS: I took the liberty to correct a typo in measurer.py 

Thanks in advance.

Best regards,

-Bachir.
Hi 
I can test the measurer on the server. 
I probably don't have access to a FiC EOX profile, I'll request one.

PS: tanks for the correction in measurer.py I have added you to the UC4 EOX group... you can start logging into https://eoxhub.fairicube.eu/
Hi
thanks for adding me to UC4 EOX group.

I have just made changes to the measurer, using a different library for the CPU frequency.

As a test on EOXHub, I trained a CNN on the CIFAR10 example dataset and the results returned by the measurer are all consistent. The files are in [EOX_HUB_test](https://github.com/FAIRiCUBE/common-code/tree/main/record-computational-demands-automatically/EOX_HUB_test) folder.

Thanks for your feedback
Hi, I have recently used the measurer in the EOX Lab. I get incorrect results in the Data Size. I get
|Measure| Value|
|-| -|
| Data size (MB)  |    0.0 |
|  Data read (MB) |     0.0 |
|  Data written (MB) |       0.15 |

But two new files (7.5MB and 350 Bytes) are created. This was the first run of the script, so the files were not overwritten.
From the above discussion I get that Data written should be ~7.6MB, right?
I'd expect the other two metrics to be non-zero as well, as I am loading into memory several tiff files. I am using the AWS API `s3fs` for reading the files, and I have set up the measurer as described in the [aws_example.py](https://github.com/FAIRiCUBE/common-code/blob/main/record-computational-demands-automatically/aws_example.py). Any idea what is going wrong?