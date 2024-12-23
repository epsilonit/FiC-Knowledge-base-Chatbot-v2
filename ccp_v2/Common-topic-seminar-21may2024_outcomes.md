Outcomes of the common topic seminar held on 21.05.2024 - Brainstorming on a roadmap for future 
developments of the KB services & metadata pipeline. 
 
TOPIC 1 - Inclusion of computational resources information in a/p resources metadata 
Req. Id Requirement Plan Deadline 
1 
Include the possibility to query the 
information related to the consumed 
resources (always combined with the 
information about the environment 
configuration) in combination with the other 
parameters. (Suggested by Stefan) 
• Investigate the best implementation solution, i.e. 
if creating a new Query Tool to perform the new 
type of queries (leaving the current Query Tool to 
perform the current queries) or to upgrade the 
existing Query Tool to optionally query also the 
computational resources. 
• Implement the selected option. 
31.12.2024 
2 
Consider additional libraries for 
water/energy consumption (Suggested by 
Rob) 
• Investigate the feasibility to implement the 
requirement, focusing on the existence and 
added value of new libraries. If feasible, then: 
o Update the current tool to include the selected 
additional libraries. 
31.12.2024 
3 
Assign “labels” (as done for buildings, 
products, etc.) to a/p resources to classify 
them in terms of “energy efficiency” or 
“energy performance”. (Suggested by Rob) 
• Investigate the feasibility to implement the 
requirement. If feasible, then: 
o Define criteria to assign “labels” (as done for 
buildings, products, etc.) to a/p resources to 
classify them in terms of “energy efficiency” or 
“energy performance” 
o Implement the criteria to calculate the label 
o Add a new attribute in the md profile 
31.12.2024 
4 
• Create a new metadata for the new 
version of the a/p resource, including the 
version in the title (as it happens for the 
name of sw versions and allowing to keep 
previous versions). 
• Add in the description field an 
explanation of what the new version is 
bringing. 
Original suggestion (by Kathi) was to include 
versioning of a/p resource metadata, to be 
used each time a new version of the 
resource is released. 
If the requirement is approved, it is immediately 
applicable N/A 
5 Display information in a more user-friendly 
way, e.g. using graphs (Suggested by Rob) 
• Investigate the best implementation solution. 
• Implement the selected option. 31.12.2024 
 
 
TOPIC 2: Query Tool upgrade to query both a/p resources and datasets metadata 
Concluding remark was to document objective/verifiable reasons to discard option b), e.g. inefficiency of querying the catalog via STAC 
API, especially if querying two or more different collections and using many parameters. 
Should option a) be approved, the related requirements and implementation plan are listed in the table below. 
Req. Id Requirement Plan Deadline 
1 
Create an ingestion procedure to ingest 
dataset metadata in the Knowledge Base 
database. 
• Read json dataset metadata from GitHub 
• Mapping from json dataset metadata to database 
table 
• Definition of mechanism to run the procedure 
31.10.2024 
2 Upgrade current Query Tool to allow 
complex queries. 
• Definition of additional queries 
• Definition of layout changes 31.12.2024 
• Implementation 
  
