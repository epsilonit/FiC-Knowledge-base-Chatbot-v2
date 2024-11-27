Optimize fix costs for usage of EOX Hub
currently, almost 90% of the AWS costs invoiced to FAIRiCUBE are not related to the UC work but due to the provision of the control plane by booking a dedicated m6a.4xlarge EC2 instance. there could be potential for cost optimization by e.g. considering a smaller EC2 instance to provide the landing page, catalogue, etc.  This needs to be tested.

Further input how to reduce fix costs (if feasible and possible) are welcome as well
any update here? reducing fix costs  would really have impact! As such optimization is also essential knowledge for future users, this should be described in read-the-docs
FAIRiCUBE Cluster upgrade - Tue. 11th June 2024:
- cluster upgrade to k8s 1.28 considered successful
- some cost reduction should be visible in upcoming bill as the always running general node is using smaller instance size now
excellent, is this the achievement of the optimal reduction? i.e. if no further reduction can be considered / tested, I can close this issue?

lets wait what the next bill shows
I just had a look at the AWS cost explorer and indeed, static cost went down from about 600 USD to 400 USD

![image](https://github.com/user-attachments/assets/ebb2a5eb-48f3-4a24-b55d-2e7f865872b8)

can we find out how much load was on that smaller instance node, can it even be smaller?

The `general` usage went down to ~$308/month as expected.

I see a typical CPU utilization of less than 25%:
![Screenshot from 2024-09-22 16-35-10](https://github.com/user-attachments/assets/809b6ced-4f4e-4b6c-ad4e-376e247bfcb9) do you think we could even use a smaller `m6a.xlarge` instance or would that not work in peak loads? as mentioned previously another option to reduce costs would be to commit to a longer usage and reserve an instance for e.g. 12, 24, or even 36 months. Upfront payment would increase the cost saving even further. A 12 months commitment with full upfront payment would reduce the price from ~$3700 to $2239 for example.
The general node group hosts both the user-facing-capability control-plane (including JupyterHub and Conda-Store) as well as all components in regard to provisioning (like storage, GitOps tooling,...) as well as observability (like Metrics Server, Prometheus, Grafana, Loki,...). Currently, memory usage is consistently around 50% with higher demand under usage, so this smaller node type is no option.

As Stephan proposed, a reserved instance (i.e. commitment) of the current node-type for the remaining project period will save costs.
many thanks for the explanations and monitoring of resources. committing to a longer payment period might indeed be something to consider for next time. as we might to switch to NHM funding during the last year of FAIRiCUBE, it might be too late. but we have now a good understanding of "fixed costs" which is essential input for the development of a business plan?! I consider the issue resolved