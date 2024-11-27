Progress on rasdaman (Deep Learning) UDFs
What's the status on creating rasdaman UDFs?
The requirements were discussed in Bremen, should be clear. If not, please ask!
Details in the [UC2 presentation from Bremen](https://nilu365.sharepoint.com/:p:/r/sites/Horizon2021_CUBE/Shared%20Documents/General/workshops_meetings/2023_04_19-20_status_meeting/2023_04_20_FAIRiCUBE_UC2_Architecture_Reflections.pptx?d=w5c4ec689e35844b2b160cfe3a8c259da&csf=1&web=1&e=f8zwSg). as of right now we are still working on the following:

1. Linking the Python pytorch implementation from Rob into the UDF mechanism. The idea is the replace the existing c++ implementation so that python can be used instead, this will definitely simplify future UDF implementations as well as reduce development time.
2. Saving a trained model as a collection in rasdaman for further reference from other UDFs.
3. Designing a catalog mechanism for listing and linking what models can be used with what UDFS.

We will keep you updated with our results as they come. 
1. Very cool! Think being able to create Python based UDF will make this much easier for "normal" users! :)
2. ah... what's a collection in rasdaman?
3. This work should be coordinated with what is doing on D4.3 Processing Resource Metadata

More generally (and maybe contained in points 2&3), how can a user see what UDF are available? Or can users only access their own UDF? 

1. Indeed I believe the same that is why we are focusing all efforts towards this solution.
2. It means storing the model inside rasdaman. A collection in rasdaman is equivalent to a table in a relational database.
3. maybe we can have a quick concall to discuss how we relate your catalog with what rasdaman could provide

**More generally (and maybe contained in points 2&3), how can a user see what UDF are available?** 
-> There is a query in rasdaman query language, rasql, that is specifically designed to list all available UDFs, regardless of the user. I believe that in a web environment using WCS, WCPS, or WMS would be preferred, this part I need to check with  because this involves a standard, if not then we need to think of another solution. 
**Or can users only access their own UDF?**
-> So far any user can access all the UDFs rasql and WCPS, is this acceptable to you?
On providing a listing of available UDF, to my view, WCPS getCapabilitities would be my first candidate, in addition to exposing via the processing resource metadata. Please include me on the call sorting this!

On all users being able to access existing UDF, works for me. We should check with the UC partners just to be sure, but pretty sure we won't have the issues we have with sensitive data on sensitive models.
ML models trained on sensitive data might need restricted access as well. For instance depending on the user agreement of the data (what derived products are allowed, often not clearly specified for ML models), or wether the training of the model has sufficiently hidden the sensitive (input) data points (otherwise an ML expert might be able to extract them from the model, as a kind off reverse engineering). Out of curiosity (also relates to 'how to catalogue' and 'what might be restricted'): Do you intend to treat a trained model as a whole, or to split it up into the computational graph and the trained parameters?  (chiming in here) dissecting a model is a rabbit hole from our perspective, and I can see no advantage - we would treat a model always as a black box. 
> ML models trained on sensitive data might need restricted access as well. 

Accepted, at some time access control will be necessary - just not at this stage where we have only 1 anyway :) turning statement around, do you see a situation where we provide the same model with 2 sets of trained parameters?
Sure, for example the same CNN model that we used so far can be trained for other (semantic segmentation) tasks (similar though, since the model architecture expects 28 features as input), or it can be trained for a different region. Both would use the same model architecture (= computational graph), but learn different weights. Splitting these two is the basis for what is known as transfer learning in ML. So for inference you can have a model architecture and load it with matching weights and biases for a number of similar prediction tasks. [For sure this is more difficult to implement than a pure black box approach and there might be no short term benefits.]

Libraries such as Tensorflow, Keras, and PyTorch all have methods that support this type of working with deep learning models. The usually long training times makes it a rather common approach to quickly start experimenting.
status: pytorch-based UDFs work, Jupyterhub almost installed (need Rob's help for completion -> Mohit will contact)  am I correct that if you have a model trained on 2 different datasets, you'd provide this as 2 different models (most of the info the same, but different input data, maybe different spatial validity)? Yes, the models learn to represent the different datasets. When they are 'too different', it will result in distinct models. When the datasets are different but still similar, a single, more robust, model can be trained on them. So there can be exceptions :-) any insight as to what impact these exceptions have on the a/p resource metadata? There, we have the following fields forseen:
- Input data:URI 1..* : Link to input data/metadata, helpful for a better understanding of context and domain.
- Characteristics of input data: CharacterString 1 : This field contains a textual description of the main characteristics of each input data to the resource. This field will also include e.g., description of sampling techniques, version of the data (if multiple versions are available), and, in the case of ML resources, also the percentages of training, validation and testing sets. This field may contain details on the suitability of the resource for the chosen geographic area and thematic context.

Can you use these to describe what you'd need to know? I think so. In some cases I would mention an existing (trained) model (or its saved weights) as ‘input data’, and use ‘characteristics’ to explain how it was used.

(Maybe we need a better minimum length for ‘characteristics’? 1 Character doesn’t seem very helpful to me. I would prefer either 0, or enforce some longer text (200+ characters?).)  
- shouldn't we differentiate between:
  - Input Data: data the model has been trained on
  - Configuration/weights: how the model has been parameterized
- on Characteristics of input data, this is of type CharacterString, so free text. This has worried me, as difficult to explain the individual inputs in such a block, but my request to align the cardinality with Input Data was not taken into account 
- Should we add an entry for model configuration/weights?
- Should we align the cardinality of the input data description with that of the input data? Yes, we can split it into configuration/initialisation data and input (training) data, to make the difference in purpose more clear. 

- we agree on adding an entry for model configuration/weights & will do asap
-  pertaining to the "align the cardinality of the input data description with that of the input data",  current solution we have implemented (a couple of months ago) is using bulleted lists in which each entry is paired with its characteristics. Pic below refers to current online a/p md request form.

![image](https://github.com/FAIRiCUBE/FAIRiCUBE-Hub-issue-tracker/assets/13329248/f8263878-7c75-4202-b4fb-f39e644fc554)

When the MD is displayed in the catalog, this solution turns out in what can be seen in pic below
![image](https://github.com/FAIRiCUBE/FAIRiCUBE-Hub-issue-tracker/assets/13329248/0a8a62f5-82bd-44d9-8170-89adcd8342bd) does this work for you?

Summarizing the status of rasdaman UDFs:

- trained models + datacube regions of interest can be passed for evaluation to pytorch using the UDF mechanism; the corresponding UDF package `nn` is deployed,  it offers function `predict()` for this purpose.
- general python UDFs can be created through a `create function` statement and copying the code into the rasdaman UDF space (those users who have worked on this already have a login, other prospective users please contact us  to create a login).
- an example model provided by WER has been deployed as a proof of concept on https://fairicube.rasdaman.com .

Let me know if you feel something missing on pytorch UDFs.

Jivitesh is now assigned to look into the python UDF implementation (testing and verification). this will provide another UC view and can serve as validation. 
in light of the new issue which formulate the requirements for more ML models in short

https://github.com/FAIRiCUBE/FAIRiCUBE-Hub-issue-tracker/issues/57

I will close this ticket.