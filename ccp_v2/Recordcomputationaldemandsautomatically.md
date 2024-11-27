Record computational demands automatically
see summary of deliverable D3.3 

All use cases have now started to execute data driven processing techniques to answer their respective scientific questions and we have only started to document the computational resources that have been used. This collection will grow over time and will be a valuable catalogue to estimate the demands for similar tasks, give insights for further optimization and is essential input to weight computational costs with gained improvements. As for now, we have started to standardize the collection of numerical parameters as tables, and we will see if there will be additional parameters to be included in the future. Further focus will be on how to collect, manage and make available these parameters more systematically, automatically and transparently. Ideally these parameters are collected as runtime variables while executing the code and piped into a metadata recording system. 

Can we create a python function that is called at the beginning of a script and end and that is automatically record all essential parameters like  execution time, memory consumption, etc.... see table 1 (and the others that follow)? output should be a csv file according to table 1
Hi we are working on this issue and at the moment, with a very simple block of code to evaluate, the 'measurer' returns this table in CSV format. 
Do you have any ideas or comments?

![Screenshot 2023-06-28 161846](https://github.com/FAIRiCUBE/FAIRiCUBE-Hub-issue-tracker/assets/122694156/b5b49f40-7f63-413d-b09b-7948371c0516)


In addition, we are working on making the calculation even more efficient and on including the section regarding costs. These however seem to depend very much on the platform, type of resource, geographical region, contract, etc. 
Is there any more information we can use?
Cool! is the expert, but a few things I noticed:
- Data size in grid points: in the UC tables, this is the number of points, provided in X x Y, e.g. 5490px x 2170px (x 28 channels), not MB. What looks really strange is the 2 different MB values due to different decimal precitions
- Under UC2 & UC3, I see the values for Data size in grid points split into input and output. Wondering if we shouldn't repeat these rows specifically for input and output data?
- CO2 consumed: this one worries me, as to my understanding this differs depending on where the computer is executing. How are you calculating this? do we really want to keep this?
that is a very good start, many thanks for this! might have some time to test this out some UC examples. where is the code and how can this be called?
regarding the measures the following measures
- date size in grid points (MB), that might be confusing to give the number in grid points and MB, why not give the absolute grd points only? how is this determined? do you agrregate all grid points of all allocated variables?
- if data size in MB is just rounded from data size in grid points, then this is a duplicate. but I would like to have real number of grid points (not in MB) anyway
- in and output grid size would be lovely but I am afraid this might be complicated as the user needs to flag his "i/o" with a tag somehow?
- I am looking forward to see how/where you get the memory consumed value! this goes as well for engery or co2. we have to be careful to provide 10 decimals which suggest that we can measure this super accurate, but I dont think we can!

in a nutshell, this is really all good and we need to look into the actual implementation.... regarding cost, this might not be possibel as this going deep into the AWS world, but if you can get it at runtime (pricing, type of AWS compute instances) that would be awesome!

Clearly there was an error in 'Data size in grid points' (thanks to both for spotting this) and we are working on fixing it :smile:
At the moment, these values are passed using a variable while a more automatic solution is investigated. 

The value of CO2 consumed is calculated using [CodeCarbon](https://mlco2.github.io/codecarbon/index.html)'s EmissionsTracker (its online version automatically detects the location of execution).
For the main memory usage measurement, [tracemalloc](https://docs.python.org/3/library/tracemalloc.html#module-tracemalloc) is used. While, for other metrics, [psutil](https://pypi.org/project/psutil/).

A current version of the code has been uploaded into the common-code repository under [record-computational-demands-automatically](https://github.com/FAIRiCUBE/common-code/tree/main/record-computational-demands-automatically).
This version is not yet final and we are working on improving and if necessary correcting it.
Any ideas or comments welcome thank you for this interesting code, it will be helpful for us.
I started testing the code (following the example you provide), but I keep having the following error:
**NotADirectoryError: [WinError 267] The directory name is invalid: 'C'**
Am I missing something? 
Thanks in advance,

-Bachir.

Hi 
I just made a small update to the code. 
Could you please try to run it again and let me know which command gives the error. 
Thanks!  :smile:
Hi,

Thanks
Looks like already succeeded to run the code (after correcting minor typos).
She will also shortly propose some improvements.
Thanks again for this work.

Best regards,
-Bachir. 
Hi, yes the code works for me (the bug I found was the same you found ;) )
I opened a PR to add logging functionality as an option (I like to log in-between steps of the processes so that I can trace back later what was going on and when) .
Also opened an issue because the `Essential libraries` part does not work as I expected.
Happy to contribute further!
Hi,
- I merged the request opened by 
- Regarding the [issue](https://github.com/FAIRiCUBE/common-code/issues/2), the full name is now returned instead of the alias and I am working on trying to solve the problem of undetected libraries.

Thanks for the ideas and input  
Hi 
I just made some changes to the measurer and the example code to solve the problem of missing libraries.
Any ideas or comments welcome!
I consider this as complete, no open issues, can be re-opened if needed