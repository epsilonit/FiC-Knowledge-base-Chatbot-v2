NameError: name '_C' is not defined
I have been trying to validate the monitoring by repeating a previous run of a python script, now using the Measurer

after installing all python libaries I get the following error message:

Traceback (most recent call last):

  File "/home/sjet/repos/uc3-drosophola-genetics/projects/gap_filling/src/data/load_tsv_kmeans_elbow_sli.py", line 28, in <module>
    from measurer import Measurer

  File "/home/sjet/repos/uc3-drosophola-genetics/projects/gap_filling/src/data/measurer.py", line 6, in <module>
    import torch

  File "/home/sjet/tf_gpu/lib/python3.8/site-packages/torch/__init__.py", line 465, in <module>
    for name in dir(_C):

NameError: name '_C' is not defined

as the original script is on the FiC repo, one can reproduce the issue?
this might have something to do with torch and not our scrpts:
https://github.com/pytorch/pytorch/issues/1633

restarting the kernel from within spyder actually worked. just running in another issue:

https://medium.com/@codingInformer/solved-importerror-cannot-import-name-paramspec-from-typing-extensions-dfd9aa2027c1
but solved according to the suggested solution. works now

works now with the posted solution