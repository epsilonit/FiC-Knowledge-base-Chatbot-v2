Feedback to the a/p resource metadata web gui
Hello
I have tested the a/p resource metadata web gui (here the result Here the metadata: https://catalog.eoxhub.fairicube.eu/collections/no-ML%20collection/items/26YU5NATNB?.language=en). I have some immediate feedback, mainly about the user experience:
- field objective: is it really necessary? aren't the fields title and description not enough?
- framework: there is no possibility to choose "no framework" or non-python frameworks. for example in my case I have pointed to a gdal command, which does not requires any of the listed frameworks
- use of double asterisks is annoying, the meaning is not immediate (I know it is explained at the beginning, but still annoying). Suggestion: divide the form in sections: section 1: general, section 2: ML specific (so no need for **); alternatively hide ML-related fields when main category is not ML-related
- in general I find the ordering of the fields a bit unordered, I would prefer to have similar fields grouped in sections (general, ML-related, input data, output data...)
- input data (used/characteristics/bias): better to enforce the input encoding instead of expecting the user to follow it. additionally, those three fields should belong togehter, no? Suggestion: add a group of three text fields (link, characterisitcs, bias) for each single input data link, with the option to add more groups.
![image](https://github.com/FAIRiCUBE/resource-metadata/assets/123374844/0bb0ad2f-5153-435d-98e4-0690563595e2)
- output data: same as for input data

Thanks for your work so far! 
Hi
many thanks for your feedback. 

About the first two comments: 
- *objective* field is needed to better understand the purpose of the resource and to allow queries on the resource's objective
- *framework* field takes values from a codelist where we can add new frameworks, please use the [Codelist change proposal](https://github.com/FAIRiCUBE/resource-metadata/issues/new?assignees=&labels=&projects=&template=codelist_change_proposal.yml) to propose the new value and I will add.

Regarding the other comments, I am currently working on a better organisation of the form fields. 
Hi
thanks for the answer, I have opened a new codelist change proposal #19.
For me this "issue" here is resolved :)