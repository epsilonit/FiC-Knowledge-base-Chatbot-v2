JupyterLab git push interface throws error
Hello,

since a while, every time I try to use the JupyterLab interface to push changes to the remote repository, I get the following error
![image](https://github.com/FAIRiCUBE/uc1-urban-climate/assets/123374844/821afb9f-c084-4b8d-a7e2-14cfe965a088)

From the command line, `git push` does work without errors.

Also is experiencing the same problem.
we just checked - we have a quite recent version of https://github.com/jupyterlab/jupyterlab-git installed

```
13:19 $ jupyter labextension list
        v0.43.0 enabled OK (python, jupyterlab-git)
```

also also later versions (like 0.44 resp. 0.50) won't change anything in behavior
The main "problem" is not the generic git extension but the fact that github as specific git provider disabled https and just allows push via ssh (note: the extension only allows git clone via https so the subsequent push is also using https!)
so to work with git against github repos you need to

1. create ssh key (without passphrase) and upload to github as well as your JupyterLab workspace
2. do a git clone via command line
3. now you can use the extension to browse through branches and commit/push

this workflow has been verified by [@mallingerb](https://github.com/mallingerb) above
With the ssh key it now works, thanks for looking into this.