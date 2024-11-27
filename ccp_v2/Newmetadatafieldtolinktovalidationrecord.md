New meta data field to link to validation record
While we were discussion how to best integrate our FAIRiCUBE validate to ongoing processes: For data validation and processing validation it would make the most sense if we can create an [external] validation record (in the simplest way an online excel sheet from a MS / Google forms) and link it to the data meta record and a&p meta data record, respectively. 

We would therefore need a meta-data field in both these meta data schemas which can hold a link to the validation record as text URL. 
Hi 
regarding the analysis and processing resources metadata catalog, our idea is to create a new mandatory URI-type field called 'Validation' containing a link to a validation report. 
Waiting for approval to implement.
In the catalog editor there are already two fields, data quality and quality control. Could these be used for data validation? can either of the two fields store URLs?
![image](https://github.com/user-attachments/assets/05316bad-bc85-4394-8cbc-6208ea94bfd0)

> Hi regarding the analysis and processing resources metadata catalog, our idea is to create a new mandatory URI-type field called 'Validation' containing a link to a validation report. Waiting for approval to implement.

that sounds good but we should not make the field mandatory as we do not intend to enforce the validation. Not every individual processing step might have an individual validation record. I foresee that we rather have a validation record for each processing pipeline. One way forward is then to use the same link for all processing steps or leave some validation fields empty... 

If there is a field called "Quality Control" in the data catalog that we can use, we might also call it QC in the a&p metadata? ok to set the multiplicity as 0..* I doubt about the appropriateness of Quality Control field to store one or more URIs pointing to a validation report.
Semantically speaking, from D4.2 I see that the "custom" field Quality control is meant to "_Describe any quality measures (standardised calibration, repeated samples or measurements, data capture, data entry validation, peer review of data, or representation with controlled vocabularies)_".
A validation report should instead contain information about the results of a validation process.
Very likely it could happen that the results of a validation process represent just a subset of information to be provided as Quality control.
Of course, it depends also on what the validation process consist of.
As conclusion, my proposal is to create a new Validation "custom" field, to be added as third field of Data Quality section in the dataset metadata template, as well as as new field in the a/p resource metadata template.
Alternatively (but it is my less preferred option), we could use the already existent Quality control field in the dataset metadata template and create a new Quality control "custom" field in the a/p resource metadata template, modifying the current definition as follows: "_Describe any quality measures (standardised calibration, repeated samples or measurements, data capture, data entry validation, peer review of data, or representation with controlled vocabularies) or provide one or more URIs pointing to a validation report_".
What do you think?
we dont want to bend / corrupt our meta data concept too much. if the clean solution is to create a new field "validation" in both meta data stacks, then we should do it....
After having spent additional time to cross-check in D1.2 the overall and specific FAIRiCUBE validation approach (including the validation process), I'm even more convinced that the option to create a new field "validation" is the best one.
So, let's go for it!
EPSIT will take care of it in the a/p resource metadata template. could you please take care of it in the datasets metadata template?
Thanks! 
I will add a `validation` field, which is a url link to the validation sheet.

There are currently no stac field or an extension to use here ( in the final stac product), so it will be some new proposed `validation` field

Hi
probably this new _validation_ field should be inserted in the `Data Quality` section in Maria's screenshot. Maybe under _Data Quality_ and _Data Control_?

What do you think? 
Can confirm that this should be under `Data Quality` - please propose fitting fields.