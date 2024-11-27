Python for rasdaman UDFs
Work is already progressing on utilization of Python for the creation of rasdaman UDFs (to date, only C++ & Java were possible).

Current status (feedback from Currently, we are testing the return values from the Python code into the C++ code from the UDF. There seems to be mismatches and we are adjusting the UDF engine + UDFs accordingly. It turn out to be a bit challenging, so we expect to have a preliminary UDF by end of June early July.
python UDFs are available now since August, as well as a UDF for pretrained model application ("prediction"). If needed, a webinar can be presented.

ditto.
That’s great news! A webinar about this functionality would be appreciated. Perhaps for all interested use cases?
Yes please! Doing great things is part of it, but if nobody knows what you've done, almost like you didn't do it! ;)

When can we have such a webinar for relevant UC? any updates on this???
I sensed interest in such a webinar only today. Actually, I summarized steps in the other issue, repeating here:

Using the python UDF functionality is quite straightforward:

-    you know aleady how to create a UDF as such, so skipping this
-    now you use, in the "create function" statement, "language python"
-    put the python code into /opt/rasdaman/share/rasdaman/udf/mylittleudf.py
-    in the UDF code, simply use the input, like:

```
import numpy as np
def mean(arg):
    return np.mean(arg)
```
the Recommended Good Practice is that you first use Jupyter to load data from rasdaman and do the python processing, and only then move the code into the UDF. At the same time, this yields a nice developer documentation of the UDF. what about meeting on this on 

- Fri Oct-06 16:00
- Mon Oct-09 11:00 I am aware of nobody else interested - should anybody need to aprticipate, can you link them in please? I think that the NHM UC partners should definitely be asked. Assuming that the UC1&4 partners may not attend due to your privacy concerns?
Can we schedule a meeting with Outlook or a Doodle please? 
for sure: https://doodle.com/meeting/participate/id/bD0969Bb Can you please notify others as well that might be interested and ask them to fill in the Doodle. We might need a few more options for dates. Unless it will only be me.
maybe a topic for Girona? let me know if we want to meet on Oct-09 11:00, otherwise I reuse it. 
First off - could you answer my question above on if we can send this invite to all FAIRiCUBE partners, or just for UC 2&3?
In addition, would have been nice to let others answer the doodle. By your actions, you've made clear that you would prefer to speak to Rob alone, no interest in anybody else attending.
Guess this means that UDF are not relevant for NHM Please reuse the timeslot for something else, someone seems to have stolen it already in my agenda for some urgent server debugging session. Besides probably good to discuss the UDFs in person in Girona. thanks for letting me know! can we please foresee a UDF slot in Girona; when (ie: in what WP) can we discuss UDFs? Invade WP3? has there been any progress on this issue? Sorry for not being able to support more due to my current health issues. 
I see that the webinar has not been followed up. As the issue is resolved, a summary has been provided above, suggesting to close this issue and then we will arrange for the webinar discussed.

I see issue very related to 
https://github.com/FAIRiCUBE/FAIRiCUBE-Hub-issue-tracker/issues/2
https://github.com/FAIRiCUBE/FAIRiCUBE-Hub-issue-tracker/issues/13
nevertheless, a webinar to explain everyone else the current state of the [python] UDF development is very relevant. it will also help to get Jivitesh up to speed! I will mark this down for either next WP2/WP3 synnergy meeting or the common topic seminar.