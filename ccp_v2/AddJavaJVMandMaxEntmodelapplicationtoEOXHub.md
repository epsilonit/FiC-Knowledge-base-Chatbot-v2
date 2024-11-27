Add Java JVM and MaxEnt model application to EOXHub
For our use case we want to run the Java-based MaxEnt model for species distribution modelling. It can be used directly from the command line, without using the graphical user interface of the program. 

The MaxEnt model is open source and available here:
https://biodiversityinformatics.amnh.org/open_source/maxent/

However since it is a Java based application, it also requires a Java JVM to be available.

Is it possible to include both into the/a fairicube server on eoxhub? The current one that I can start (which has the PyTorch packages as well) does not have Java installed and it does not allow me to self-install programs.

It should work with a recent (open) JDK, like Temurin 21.0.1.
Response from Hub Team:
can you please get it packaged as docker container together with an example config how to invoke it, then we  can have a look on it on how to deploy it at the Hub
Ok, I will add that to my list to look into. 
Creating a Docker container with Java and the MaxEnt model should be doable, but will take some effort from us to figure it out and how to create a usable container. It would e.g. need terminal access to run the maxent commands, and a data volume to share the input and output data of the model.

Before investing further UC time in this we decided to evaluate the MaxEnt model further locally first, and when we decide it is the best suitable species distribution model for our use case we will look into containerising it.
Closing issue for now, until local testing has been completed.
as now also UC1 required Maxent for its work, I re-open the issue... 
Hi Rob,  what was the outcome of your experiments with containerizing  Java & Maxent I experimented briefly with it hoping to use it purely as a command line based application, but it very quickly pops up dialogs on the screen, e.g. with error messages and an 'ok' button to click. It also likes to display charts etc. It is very much a Java Desktop application. Maybe using X11 the display can be forward to an X client on a laptop so that application can be used that way.

But that is where I stopped spending time on it and considered that it might be more useful for our use case to try to find an alternative maximum entropy model implementation for Python or R, or even another way to do the species distribution modelling we need (e.g. with a random forest model). Or run the Java MaxEnt application locally if there are no other options. thanks for sharing your findings. 
Do I understand correctly that even the command line based application doesn't work without GUI ?
Perhaps if everything is perfect :-) But it is a desktop application so very likely that many things will cause GUI dialog boxes and windows to be shown for the user to interact with.  I think otherwise in the many years that it exists someone would already have put it on a server or in a container. Unless its license even doesn't allow that, which would also be an issue.
Thanks. 
Just checked: the license is MIT, so no problem from this side. 
But overall, this doesn't sound very promising. 
Thanks for checking. And indeed, it is probably not very suitable to be run as a container. Maybe with the X11 forwarding of the display, but that would be cumbersome and not for the average Windows user I guess. You would want something that can do long-running computations in the background without a change of it popping up GUI stuff and waiting for a response.
Hi

have you looked into these two Python implementations of MaxEnt?
- https://pypi.org/project/intros-MaxEnt/
- https://earth-chris.github.io/elapid/sdm/maxent/

Hi ,

I have suggested elapid as an alternative, but I don't know if anyone has actually tried it yet. This is mostly a question for and Masoume who are doing the species distribution modelling and looking into the models. So I am hoping to hear from them :-) 
Hi and
Let's make an overview of knowledge gained so far about the MaxEnt implementation. I created an issue in the common code section: https://github.com/FAIRiCUBE/common-code/issues/9#issue-2500885045 
since running  Maxent on FiC-Hub is not a real option, how shall we proceed ?

I `elapid` (https://pypi.org/project/elapid/) or the 2 python versions (see above https://github.com/FAIRiCUBE/FAIRiCUBE-Hub-issue-tracker/issues/36#issuecomment-2309792566) an option you can go with?