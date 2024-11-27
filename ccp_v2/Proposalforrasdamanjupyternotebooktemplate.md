Proposal for rasdaman jupyter notebook template
I have updated the repo with the newest version of the jupyter notebook for the [rasdaman ML UDF proof of concept](https://github.com/FAIRiCUBE/uc2-agriculture-biodiversity-nexus/blob/main/rasdaman-ml-udf/proof_of_concept/FAIRICUBE%20Machine%20Learning%20UDF%20Proof%20of%20Concept.ipynb). This will be the endpoint for the [Resource metadata](https://github.com/FAIRiCUBE/resource-metadata/issues/7) under the **Reference** subheader.

It would be nice if we agree on the structure of this notebook so that it can later be used as a template for the other ML UDFs. Maybe even use it to programmatically generate the preamble of new ML notebooks with the metadata provided by [Resource metadata](https://github.com/FAIRiCUBE/resource-metadata/issues/7).
Indeed, good to streamline it a bit and clarify what should go into the resource metadata, a reference notebook (if available) and an example (also a notebook)? And how one can automatically be created from another, otherwise it will be a lot of work to fill everything in and keep it in sync (as there might be multiple variants and versions of models). 
Maybe this should be moved to the central FAIRiCUBE Hub issue list? I suppose something similar will apply to a/p resources on EOX.
my 2 cents:
- don't know what a reference notebook is. We planned to do just the example one.
- I do not believe we can automate this fully, data are just too heterogeneous and need different handling.
First off, I transferred the issue to the general FAIRiCUBE-Hub-issue-tracker as you correctly noted that it's far wider than UC2 (Hope this works!)

Comparing the information in the [rasdaman ML UDF proof of concept](https://github.com/FAIRiCUBE/uc2-agriculture-biodiversity-nexus/blob/main/rasdaman-ml-udf/proof_of_concept/FAIRICUBE%20Machine%20Learning%20UDF%20Proof%20of%20Concept.ipynb) with the corresponding [A/P Resource Metadata](https://github.com/FAIRiCUBE/resource-metadata/issues/7), I see some divergence, e.g. on the Inputs:
- Jupyter NB: 
  - sentinel2_image: [subset] of preprocessed sentinel2 image (ingested to rasdaman)
  - maxes_sentinel2_image:: Per band maxes of the whole sentinel2 image (ingested to rasdaman)
- A/P Resource MD: 
  - Feature data: 7 Sentinel-2 images, R,G,B,NIR bands, representative of the Dutch growing season 2018. The data was in UTM projection and only cloud free images have been used. It covered a study area in the North-East of the country.
  - Label data: The Dutch agricultural land registration data from 2018 of the study area has been used as ground truth data. It contains the farm parcel boundaries and the planted crops. The full list of crops has been reduced to 76 major types that were at least present in the region and thought to be potentially recognisable from the feature data. Still, the labels are significantly imbalanced.

Defining a base structure for the Jupyter NBs that's aligned with the A/P Resource MD would be very valuable, make it far easier to maintain alignment between the NBs and the MD describing them.  could EPSIT provide a first proposal for this?
I see your point. 
However, I am not so much in favour of generating the _preambles of new ML notebooks with the metadata provided by Resource metadata._ In my opinion, this would somehow lead to JN containing information that is very different from the content one would expect to find there, i.e. too much detail . 
I would keep the two things (JN and a/p MD) separate. Maybe a good idea would be linking from the JN to the related MD.
What do you think? 
However, it is important to start creating metadata for the actual resources in the use cases.  
We could label what is already in the issue tracker as "test" and ask the UCs to start creating 'true' metadata now that they have something concrete to create metadata for. Or maybe we just close the current issues to have a 'cleaner' issue tracker?
I like the idea of agreeing on a way of providing a link to the a/p MD resource from the JN. Saves us the duplication, all information is where it's required.

On issues, I'd prefer truly deleting the initial tests, as I'm very much hoping that we'll also close real issues. Then we can no longer differentiate if an issue was closed because it was a non-test-issue, or closed because it was resolved. While I'm aware that we can sort a lot with labels, I think it would be far easier to handle if we didn't always have to filter out test issues
We seem to have agreement on not providing too much additional metadata within the JN, instead just providing a link to the a/p resource record (direct link to STAC JSON)

However, we've lost the original topic of this issue, providing clean JNs that illustrate tricky bits as templates. I've heard requests for the like from the UC over the last months, so believe this point is still valid. These would also be valuable for the KB.

Question is where to collect them? Maybe in the code section of FAIRiCUBE-Hub-issue-tracker as neutral ground? thoughts?
I don't think we have adjusted these ideas yet after the rasdaman UDF approach has changed from supporting C++ code to being able to run Python code. The old proof of concept (code) and (template) Notebook might no longer apply? It might be good to have the seminar about the new Python UDFs first, and then discuss how it affects what we got from the initial proof of concept and what needs to be updated? is there already a date for the seminar about the new Python UDFs? Maybe in the frame of the data science seminars or UC Synergy WSs? Think that the UDF topic is interesting for all UC.
using the python UDF is quite straightforward:

- you know aleady how to create a UDF as such
- now you use, in the "create function" statement, "language python"
- put the python code into /opt/rasdaman/share/rasdaman/udf/mylittleudf.py
- in the UDF code, simply use the input, like:

```
import numpy as np
def mean(arg):
    return np.mean(arg)
```

Curious on your plans, maybe you can share. But then again: the Recommended Good Practice is that you first use Jupyter to load data from rasdaman and do the python processing, and only then move the code into the UDF. At the same time, this yields a nice developer documentation of the UDF.

Bottom line and good news: you don't need to wait for continuing. where is this documented outside and your brain? 

From my experience, examples/templates are very useful, have been requested by the UC
the tutorial will have material. Our plans are still to use the functionality for deep learning on multi-dimensional input data. So going from applying a simply numpy mean function, can you provide an example that works similar like what we had in the C++ world? So input is a spatial data cube (via the Java Petascope connection?), and output a new spatial data cube that has predicted class values and prediction probabilities. It is fine to start with that first and later consider other output dimensionalities and tracking progress and prediction accuracies.
Ok, posts crossed. Fine if it is in the tutorial material. I can wait for that. it would be tremendously helpful to have a Jupyter notebook demonstrating your exact plans - I have no clue what data to be accessed, how they will be selected, where does the model come from, etc. Many thanks!

So far the tutorial can cover exactly rasdaman UDFs as sletched [here in the issue](https://github.com/FAIRiCUBE/FAIRiCUBE-Hub-issue-tracker/issues/13#issuecomment-1743359874), nothing more.

Fair enough, so best then to let our use case progress further first developing the science and methodology we need, locally code the Notebooks for the machine learning involved, and get back to you with those sometime later next year. We can then see what can still be incorporated as UDFs in rasdaman and how to do that.
sounds good, Rob - looking forward to some JN; can be rough and sketchy, no need to polish for us, just to give us an understanding of what course the ship goes :)
Still a bit unclear to me if this issue still applies and what to do with the previous C++ based UDF and example I worked on with Otoniel. Do we discard all that?  definitely not discard, but you can (and should) continue using it. Only once you want to do things that require direct python access you need to resort to the new path.

Would you share your big picture with us, what actually you are heading for? This would allow us to give even better support. For now it looks like you have the opportunity to run a model, and that was the goal which now is achieved. What else? If I recall correctly, and please correct me if further developments have happened, we left the C++ UDF proof of concept at a prototype stage that was dedicated to running the example Sentinel 2 input based Dutch crop classification model. Which was an example that I provided to get development going quickly, but it is not something we want to do in our use case. There we want to do species distribution modelling / estimation, and casual inference between farming activities and biodiversity changes (<= that is still our big picture). From what I remember about the C++ UDF code it will not suffice, for example because required input data will be different which will not match the stuff that was hard coded (like the data normalisation). We left that PoC with a few open ends, that might not have been addressed because the development path switched to prefer the Python based solution. to my understanding, the initial C++ model you'd deployed as a UDF was a pure proof-of-concept, nothing to do with your actual UC. Thus, while I'd be all for storing this code for future reference (nice example of a C++ UDF), not anything we'll actually be using. Please provide to for inclusion in the KB. in order to enable Rob to deploy actual ML routines (that require Python), getting the Python kernel running #14 is a prerequisite. I don't see how understanding details of the content of Rob's UC will accelerate that process.   I have already provided the resource descriptions for both the data and the model. Fine by me if you want to keep using it as a dedicated hard code C++ UDF showcase.
Jivitesh is now assigned to look into the python UDF implementation (testing and verification). this will provide another UC view and can serve as validation.. he will now also clarify with the rasdaman counter part (Bang) how to create a suitable python kernel. this task (UDF in python / C++) is more suitable to our rasdaman technical leader: Dimitar (username: misev). I put him on the ticket, so please discuss with him instead. : you have been appointed by Peter to be the main data science contact point for FiC users, specifically Jivitesh.  Please clarify internally if it should be Dimitar instead
Python UDFs are documented in [4.18.6. Writing Python UDF Code](https://doc.rasdaman.com/stable/04_ql-guide.html#writing-python-udf-code) along with examples, and registering such a Python UDF in rasdaman is covered in [4.18.3. Creating a UDF](https://doc.rasdaman.com/stable/04_ql-guide.html#creating-a-udf).

I'll gladly help with any questions on details you may have. As far as I know everyone should have credentials to access the above links. Bang is the primary contact and he connects me when that is needed, such as in this case.
Hi , I found the documentation on the fairicube.rasdaman.com VM that we are using for the project. It mentions an interesting example, but it is rather brief and leaves out many details. Is it possible to get a walkthrough of the full example and code? Perhaps in a FAIRiCUBE webinar. Probably at least will be interested as well.

<img width="738" alt="Screenshot 2024-01-19 at 11 03 40" src="https://github.com/FAIRiCUBE/FAIRiCUBE-Hub-issue-tracker/assets/1943289/8b6cad43-bd71-4686-9f4c-4126c81845f5">

I would like to propose the 20.02. 13:00 -14:00 for such a webinar, it is a FAIRiCUBE common topic seminar, should be reserved in all FiC member calendars already...

Hi Rob, the code left out from that example is not relevant to the mechanism of how to write the UDF-specific parts of a UDF and register it in rasdaman. The relevant part is the function signature (parameters and result returned), while what is done with the parameters to achieve the result is code specific to a use case: I imagine you already have this use case code and are just missing the part where you connect it to rasdaman as a UDF?

Which particular details of the example are unclear? I will not be available in the mid two weeks in February.
Hi Dimitar, that is correct of course. However, a walkthrough of code and some additional explanation usually helps in getting up to speed faster, and probably will save us (as novice rasdaman users) some time otherwise spend on trail and error and restarts of rasdaman on the VM.
If you have the Python code ready I could look at creating the UDF initially in rasdaman, so we can make progress faster. In any case the process with Python UDFs is a lot more straightforward in comparison to the complexity of C++.
Alright, I guess we need to work first then on getting more UC data in rasdaman and training a model based on that. And then can come back to the UDF model inference code. indeed, you can help us in working out the webinar:
> Which particular details of the example are unclear?

Your input will be valued for crafting the tutorial.

The (single) example we have is already good enough for doing a tutorial.

Excellent, happy to help.

I think the tutorial/webinar is not just for me :-) so an end-to-end tutorial about Python UDFs for machine learning inference using PyTorch or TensorFlow would be great. If you want to use our crop classification example for it please go ahead.

Specifically to the example in the manual my (more detailed) questions are e.g. about error propagation, logging mechanism, how to return multiple outputs (e.g. classification + probabilities), how to create a geospatial output (I assume everything works via WCPS as well), where are the files with trained weights stored (is there a standard command to get a list?), how are these versioned, what is the python environment used, how is it updated, Is the mentioned EfFormer indeed an actual Python module or just a single file? Etc.

And perhaps for FAIRiCUBE, how does it link into the catalogues? I enjoy how we dive into it step by step - I see already many sub-topics arising from this which allow to spawn sub-issues. Anyway, the original topic does not fit any longer here. OK to continue under UC2?

We moved (this issue) from UC2 specific discussion to this more general repo, so I would suggest to keep it here :-) But renaming or starting separate issues as needed sounds good. Maybe best following the webinar? that's very helpful, I'll try to cover some of those topics in the documentation as well next week.

In WCPS it is also necessary to create a UDF that invokes the rasql UDF ([doc](https://doc.rasdaman.com/stable/05_geo-services-guide.html#user-defined-functions-udfs-re)); we're working out how to semiautomate this part as it is relatively straightforward. we updated the [documentation](https://doc.rasdaman.com/testing/04_ql-guide.html#writing-python-udf-code) with more details on [error handling](https://doc.rasdaman.com/testing/04_ql-guide.html#udf-python-error-handling), [logging](https://doc.rasdaman.com/testing/04_ql-guide.html#logging), [PYTHONPATH and Python versions](https://doc.rasdaman.com/testing/04_ql-guide.html#paths-libraries-versions), and a note about multiple return values:

> Complex return types, such as tuples or objects are not supported. Multiple arrays as long as they are of the same shape (spatial domain) can be returned as one multiband numpy array.

This part is more specific:

> where are the files with trained weights stored (is there a standard command to get a list?), how are these versioned

These files are not managed by rasdaman. They have to be stored with system permissions that allow the rasdaman system user (that executes the Python UDF) to read the files. as all seems to be done excpet that an intro webinar was desired, can we schedule that? Would you take a lead, or should we?
webinar was scheduled already for the 20.02. 13-14:00 during the "FAIRiCUBE common topics seminar" slot. If not suitable, glaldy reschuled or get back to me. Please arrange with Rob & Jivitesh what to cover (re-cap from previous activities). I cannot attend but trust all parties to inform each other and to record the session for me.
ok, so we will fit it in there - perfect. I will take care.

as the webinar was given, content was documented, can this issue be closed?
I am not aware of anything unresolved in this issue, so closing it. Everybody feel free to reopen if a question remains, or add a new issue. I've reimplemented the C++ UDF as a Python UDF versioned [here](https://github.com/FAIRiCUBE/uc2-agriculture-biodiversity-nexus/blob/main/rasdaman-ml-udf/proof_of_concept/rasql_python_udf/fc.py).

You'll notice it's much more straightforward and I think it's a good basis for implementing support for further models. This UDF is deployed on our fairicube VM and available in rasdaman, I compared outputs of both the C++ and the Python one and they are equivalent. Definitely much more pythonic :)