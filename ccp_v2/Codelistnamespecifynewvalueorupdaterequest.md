<Codelist name> - <specify 'new value' or 'update' request>
<!-- Please fill out this issue to the best of your knowledge, this will help the governance and release process move forward.-->

# Change proposal description
<!-- Provide a brief description of the change proposal - e.g. specify if you are proposing a new value or want to update /replace an existing one -->

duplication of information

## Codelist name
<!-- Specify the name of the codelist addressed. -->
Architecture / Algorithm 

## Codelist value
<!-- Specify the name of the codelist value. -->

## Codelist value description
<!-- Specify the description of the codelist value. -->
as an example : 

Architecture = random forest 
Algorithm  = random forest classifier

## Issue faced
<!-- Provide a comprehensive description of the change proposal, e.g. new value, change of existing value -->

## Change proposer
<!-- Specify the submitting person, organisation or group of people/organisations. -->

remove the architecture term from algorithm name list, makes the Architecture / Algorithm combination more consistent and flexible 

## References
<!-- If relevant, provide links to more detailed documentation / online discussions in publicly available resources (e.g. GitHub repositories, Forum discussions ...). -->

comment in an early version of deliverable document... (^_*)

Has this been clarified, or is this issue still open?
I have been receiving a technical answer from Antonio : 
The name of the architecture in the name of the algorithm is present in some cases, such as random forest classifier or decision tree, but it is not the general rule.
In my opinion, it is right to use the full name of the algorithm (and e.g. not to replace it with 'classifier', thus eliminating the name of the architecture) for two main reasons:
•	as a matter of clarity, because it is important that we use the name by which the algorithm is known
•	although it may sometimes seem correct to remove the architecture name in the algorithm and avoid duplication this may cause problems if generic  'classifier' value is used in the presence of architectures that implement very different algorithms to perform same task, e.g. a classification. In this case the "classifier" term could be miss-used. For example, an ensemble architecture may perform a classification through both Voting and Bagging (or others), thus offering a fairly obvious distinction that it is good to maintain. Therefore, we want to avoid a situation where a user in the presence of an ensemble architecture uses the generic term 'classifier' instead of the specific algorithm (e.g. Voting, Bagging and others). 

and replied with: 

I generally agree with you and after refreshing my memory on the items of the architecture drop down, I understand this better as well. Unfortunately and this weights heavy, it nicely proofs that we create a very technical form that only very technical people will understand. I consider myself – to some degree – a technical person and I struggled to know what to choose … If users don’t understand, they will likely fill out incorrect information or ignore it completely.
As a starting point I would suggest removing the architecture field as this could be reconstructed by the choice of the algorithm anyway?!

so far, no further reply to this. I still believe that we either duplicate information or make the form too technical...
Problem is, we have a very wide potential audience. For the ML experts, the differentiation Antonio mentions makes a lot of sense, so we shouldn't just drop it. For the non-ML-idiots like me, we may even want to add a simpler explanation of what the algorithm does.

The first example in [D4.3](https://nilu365.sharepoint.com/:w:/r/sites/Horizon2021_CUBE/Shared%20Documents/General/deliverables_milestones_archive/2023/D4_3%20Public%20Listing%20(Catalogue)%20of%20FAIRiCUBE%20processing-analysis%20resources_V1.2.docx?d=wafb6e9f6bc4f4bc5a2cde884acbc8470&csf=1&web=1&e=bRVoTV) nicely illustrates the levels:

- Architecture	CNN
- Algorithm	LeNet

My only request might be spelling out Convolutional-Neural-Network for us idiots ;) I agree with you and we can use both the name and the acronym like for example 'CNN - Convolutional-Neural-Network' instead of just the acronym. What do you think? 
same a #2 - YES!!! :)