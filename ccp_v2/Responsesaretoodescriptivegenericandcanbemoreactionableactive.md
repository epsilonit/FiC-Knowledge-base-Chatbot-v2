Responses are too descriptive/generic and can be more actionable/active
I tried the chatbot a little bit, it could be very helpful. We need more 'knowledge' to feed it of course, and perhaps it need to be more strict in responding that there is no relevant information available when such is the case. 

The responses now are very wordy where I would prefer them to be more direct and providing practical actions to take to use FAIRiCUBE. See the partial output in the included screenshots. It depends on the aim of the chatbot (or Q&A system), but in this case the lengthy descriptions are not needed imho (the font is also not the most readable one for long text).

![Screenshot 2024-11-13 at 15 11 21](https://github.com/user-attachments/assets/dd783da4-1158-46ee-a8a2-37fe368bd60b)

(Good that eventually there is a link included, but I would prefer it to be at the top)


![Screenshot 2024-11-13 at 15 11 56](https://github.com/user-attachments/assets/3fb2b76b-387b-4c56-bb52-0980bce87cb2)

(In a previous response it was mentioned there exists a FAIRICUBE AI toolkit, but the system can not direct me to it)

Agreed that less prose would be more valuable!
Btw - is there an easy way to save a chat protocol other than screen shot or cut&paste?
Hi 
I agree with you that the quality of the answers produced by the chatbot is extremely dependent on the "quality" and quantity of the documents in the Knowledge Base Digital Library, as this is the source of the chatbot's specific and contextual knowledge. Without many "high quality" documents, the only knowledge the service can access is the intrinsic knowledge of the LLM model (gpt-4o), where there is limited specific knowledge of FAIRiCUBE.

This aspect triggers the opportunity for all partners to contribute to increase quantity and "quality" of the Knowledge Base Digital Library.

Regarding the length of the answers, this is given by two parameters: 
1. the value given in the prompt (_"Write your answer in 500 words or less."_)
2. _MAX_TOKENS_ 

MAX_TOKENS defines the maximum number of tokens that can be produced by the model, while the prompt specifies the number of words in which the answer is to be produced.
Currently the two parameters are MAX_TOKENS=700 and the prompt asks the model to generate an answer in 500 words to avoid truncated answers. This is because the model tries to answer in 500 words (parameter 1) while it stops producing tokens after 700 (parameter 2).

The use of longer answers is related to the idea that for a user who is not familiar with the project (or who does not have much information about it anyway), it might be useful to have longer sentences and answers.

After this first test phase will be closed, we can play with the parameters and try different configurations.