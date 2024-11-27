GPU python kernel for UC3 / UC4
we would like to request a small GPU python kernel for UC3 / UC4 under the EOX Lab environment which will further test headless execution functionality and the the provisioning of GPU resources to the UCs. one multi-GPU machine with e.g. 8x A100 would needed for testing and semi-production.
As we understand from 's [comment](https://github.com/FAIRiCUBE/flux-config/issues/1#issuecomment-1689488002):

>"The _**node_purpose**_ can either have the value _**user**_ to use a CPU node with 2 CPUs and 8 GB memory or _**userg1**_ to use a GPU node with 4 CPUs, 16 GB memory, and a Tesla T4 GPU with 16 GB memory."

The _**node_purpose**_ takes only two values _**user**_ or _**userg1**_ (if we need GPUs). We are wondering if there is another value/option with higher GPU configuration? 
Correct, `node_purpose` currently supports two values: `user` for a CPU instance and `userg1` for a GPU one. For the GPU one the AWS EC2 type `g4dn.xlarge` is used providing one NVIDIA T4 GPU. This type costs less than 1€ per hour.

I believe we can configure an additional instance type like for example a `p4d.24xlarge` providing 8 NVIDIA Tesla A100 GPUs but the costs are more than 40€ per hour. Please also note that we have no particular experience with multi-GPU machines.

Anyway, I'll request the configuration from our DevOps team and keep you informed.
All the headless endpoints can be started either with:
 - node_purpose "**user**" (for regular CPU)
 - node_purpose "**userg1**" (for single GPU) or
if node_purpose is not passed then "**userg1**" is default

In addition the smallest Multi GPU VM available on eu-central-1 g**4dn.12xlarge** (4 x NVIDIA T4 16 GiB) -> **$4.89 per hour** on "**userg2**"  has been configured.
**Only  UC3** (eurodatacube17) and **UC4** (eurodatacube18) are whitelisted for using "**userg2**".

For further usage (syntax) information see:  https://github.com/FAIRiCUBE/FAIRiCUBE-Hub-issue-tracker/issues/70