Documentation for validation of measurer
for the validation of the automatic resource monitoring we need to have more detailed information on what each value in the csv means and how it is derived. if it uses a third party libary, it would be good to know how this libary is coming up with the value as well. for more detailed information we can also reference publications, internet sources but we generally need to know how values have been derived and what they relate to.

if we noticed e.g. a large deviation in memory consumption it could be due different ways of measuring it

bachir has requested this info for D3.3 already, but we should also have it in the repo and the KB.
I have added a table (Table 2) that needs to be filled.
The first column reports the metrics, the second should contain a brief description on how the corresponding metric was computed.
Thanks in advance
Best regards,

-Bachir. 
is this issue still relevant or adressed by the table that Bachir created in D3.3? shouldn't we add the UC partners to the assignees? Not sure which UC are currently using this functionality
Hi

I am opening the following issue, because I came across a minor problem.
I had difficulties running a python script because it uses a third party 'utils' library which is in conflict with our local python file 'utils.py'.
There are some solutions to solve this conflict (e.g., specify paths,...), but I think the best is to rename utils (or use the single file Measurer) to avoid any confusion.
Let me know what do you think.

Thanks in advance,

Best regards,

-Bachir. 
Hi
thanks for your feedback, which I agree with. I was thinking of using two different files to handle the measurer and the utility functions that are not strictly related to it separately. So my suggestion is to rename the "utils" file to "utilities". 

What do you think about this?

cheers
Sure! Thanks We just should probably make sure that no common "utilities" library exists (by running pip install utilities?).
Best,
Hi 

`pip install utilities` returns the error 

```
ERROR: Unable to find a version that meets the utilities requirement (from version: none) 
ERROR: No matching distribution found for utilities
```

so I assume there is no common library. 

If we want to be even safer, we can rename the file to '_measurer_utilities_'.  What do you think about this?

cheers
Looks like it yes!
Yes, sure, the second proposition could also work.
Thanks!
Best,

I just renamed the "utils" file to "measurer_utilities".

cheers