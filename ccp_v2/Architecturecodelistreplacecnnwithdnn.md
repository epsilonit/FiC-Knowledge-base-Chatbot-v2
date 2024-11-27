Architecture codelist - replace 'cnn' with 'dnn'
<!-- Please fill out this issue to the best of your knowledge, this will help the governance and release process move forward.-->

# Change proposal description
<!-- Provide a brief description of the change proposal - e.g. specify if you are proposing a new value or want to update /replace an existing one -->

## Codelist name
<!-- Specify the name of the codelist addressed. -->
Architecture

## Codelist value
<!-- Specify the name of the codelist value. -->
cnn

## Codelist value description
<!-- Specify the description of the codelist value. -->
why only Convolutional neural networks?

## Issue faced
<!-- Provide a comprehensive description of the change proposal, e.g. new value, change of existing value -->
too specfic

## Change proposer
<!-- Specify the submitting person, organisation or group of people/organisations. -->
dnn
deep neural networks 

## References
<!-- If relevant, provide links to more detailed documentation / online discussions in publicly available resources (e.g. GitHub repositories, Forum discussions ...). -->


Hi I agree with your observation. 
I have already edited the value by replacing "cnn" with "dnn" to make it less specific. Thank you!
Hi the value 'cnn' was again added to the codelist of the architecture field along with other architectures to better specify the type of neural network. Therefore, now both 'dnn' and 'cnn' and other architectures are in the codelist. 
Can this issue be closed as resolved?
I still dont quite understand why we want to differentiate between dnn og cnn (as I beleive cnns can be seen as a subgroup of dnns). from the user persective DNN might be enough? for a datascientist CNN might add info but not a lot?!
In my opinion, it makes sense to provide as many details as possible when specifying the architecture used and its algorithm because this helps in understanding the resource.
Now, both values ('cnn' and 'dnn') are present in the codelist of the 'Architecture' field, so we can use either the more specific or the less specific one depending on the situation.
Maybe I'm just stupid, but if the catalog also wants to address non-ML-experts, maybe full text in place of acronyms would help? CNN I can now parse, DNN I'm still lost.

Remember that we also want domain scientists like Martin to be able to find something useful! I agree with you and we can use both the name and the acronym. For example 'CNN - Convolutional-Neural-Network' instead of just the CNN acronym. What do you think? 
Perfect! Best of both worlds!!! :)
I have just updated the codelist using the structure <acronym - full name>. Specifically, I updated the values of the fields 'architecture' and 'algorithm'.