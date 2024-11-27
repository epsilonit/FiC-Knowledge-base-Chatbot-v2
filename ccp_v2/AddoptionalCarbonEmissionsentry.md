Add (optional) Carbon Emissions entry
Hugging Face has an interesting option in their Model Cards to add info on carbon emissions, see:
https://huggingface.co/docs/hub/model-cards-co2

It is based on this paper: https://arxiv.org/abs/1906.02243 and https://mlco2.github.io/impact/

Perhaps something to include as well?
Cool idea, my only worry is how we can calculate the carbon emissions, as this is not only dependent on the amount of processing resources, but also where the processing takes place. : are either of you aware of such carbon intensity metrics on the HW you provide? "How much CO2 is produced by KwH of electricity"

 
It's a best estimate, for sure.

In the calculator you can specify which cloud provider or on-prem infra, and which region. With more work we could customise the calculations I think.

https://github.com/Green-Software-Foundation/sci/blob/main/Software_Carbon_Intensity/Software_Carbon_Intensity_Specification.md

On Linux the perf tool can be used to get energy usage of a process. And cloud providers have also started to provide tools, e.g. https://aws.amazon.com/aws-cost-management/aws-customer-carbon-footprint-tool/.

I think this is a great idea. We at EPSIT agree (should anyone be against this addition please comment here asap). 
Should we all agree on doing this addition, we should also agree on how to document the use and purpose of the new field in the form. As Rob suggested regarding the emission value calculation, we can use ML CO2 Impact and the tools that some providers make available, at least in these early stages.  But how do we explain to the user of the form? Let’s not forget that we are creating a STAC extension to be proposed to the STAC community so it is important that we document and justify in as much details as possible. suggestions more than welcomed 

For simplicity I would just (more or less) copy the explanation that Hugging Face gives, it is quite understandable.

I would expect the estimates at first to be about orders of magnitude and for creating awareness, perhaps later users would use them to compare models when selecting between alternatives and compute locations (specifically for ML training). 

For the infrastructures that FAIRiCUBE controls we maybe can provide more accurate estimates and fill them automatically?
we can happily add such information to any metadata providers want to make available (see discussion on metadata structure), but we will not invest own activities into this. does this confirm that you can provide CO2 is produced by KwH of electricity for your servers, in addition, KwH per some processing metric? This is what we'd need from the infrastructure providers to enable this no, nothing can be done on our side here. what's the status from EOX on this issue, can you provide such info on used resources? 
EOxHub is running on NILU resources so maybe can derive some infos but I'm not aware this information is provided by AWS. would need investigation
To my understanding, the static web page is not what eats resources, it's the processing done on AWS, so please investigate!

I also see this as a nice trigger towards AWS to start thinking about such issues!