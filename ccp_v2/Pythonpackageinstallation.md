Python package installation
Hi, I would like to know the procedure for installing certain Python packages on the EOX server when they are not present in the existing kernel. Specifically, I need to understand whether the installation must be performed by the EOX host, or if guests have the permissions to do this themselves. Currently, I am using the following script to install missing packages via pip: 
import sys
!{sys.executable} -m pip install {the name of intended Python package}
As you are already installing packages, you are answering a part of the question already yourself.

Summing it up - you may install packages:
 - directly yourself, in your workspace only available to your user
 - EOX creates a new kernel using the conda store (you provide us with the necessary information)
 - yourself, by creating a new kernel using the conda store which makes the kernel available to the whole team and headless execution (user needs to be enabled by EOX)

In any case it is highly recommended:
 - to use conda store (over pip) to reduce problems with dependencies and sharing the kernel
 - use fixed versions of the packages to reduce problems with dependencies
 - when installing directly with pip, it is also important that this is done in user space  i.e. it is not accessible for other users in the team and also not for headless is this resolved for you?