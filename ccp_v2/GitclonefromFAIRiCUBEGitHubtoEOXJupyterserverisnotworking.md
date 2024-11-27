Git clone from FAIRiCUBE GitHub to EOX Jupyter server is not working
When I try to git clone the UC2 repository from the FAIRiCUBE GitHub into a running EOX Jupyter server, it does not work because of host key verification failure. 

Is there a way to make this possible?

![Screenshot 2023-08-23 at 15 54 32](https://github.com/FAIRiCUBE/FAIRiCUBE-Hub-issue-tracker/assets/1943289/50b165e5-9952-4dc4-83c5-6700f2971107)

Nevermind, I was trying using it over SSH. It works using HTTPS.
How are you trying to clone the repo? I just successfully cloned it using the terminal and also the GIT UI extentsion
Posts just crossed, see my comment before.
> Nevermind, I was trying using it over SSH. It works using HTTPS.

I see. I guess you'd need to provide your ssh key in this case.
Yes, I probably can do that from a terminal. But when in Jupyter there is no such option (at least I could not find it).
> Yes, I probably can do that from a terminal. But when in Jupyter there is no such option (at least I could not find it).

You can simply open a terminal in JupyterLab and run any command like a `git clone` after placing your ssh key in the `.ssh` directory.