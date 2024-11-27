Query Tool upgrade to query both a/p resources and datasets metadata
As an outcome of the common topic seminar held on 21.05.2024 [(see presentation)](https://github.com/FAIRiCUBE/Knowledge-Base/blob/main/documents/common-topic-seminar_21may2024.pdf), below the proposed requirements for Query Tool upgrade to query both a/p resources and datasets metadata.

Concluding remark of the seminar was to document objective/verifiable reasons to discard option b), e.g. inefficiency of querying the catalog via STAC API, especially if querying two or more different collections and using many parameters.
Should option a) be approved, the related requirements and implementation plan are listed in the table below.

| Req. Id |	Requirement |	Plan |	Deadline |
| ------- | ----------- | ---- | --------- |
| 1       | Create an ingestion procedure to ingest dataset metadata in the Knowledge Base database. | 1. Read json dataset metadata from GitHub <br> <br> 2. Mapping from json dataset metadata to database table <br> <br> 3. Definition of mechanism to run the procedure | 31.10.2024 |
| 2       | Upgrade current Query Tool to allow complex queries. | 1. Definition of additional queries <br> <br> 2. Definition of layout changes <br> <br> 3.	Implementation | 31.12.2024 |

Feedback welcome!
I see the decision between option a and option b as the first step, when can we expect this?

Assuming for steps 2.1 & 2.2 (updating the design for the Query Tool), you'll be getting feedback from partners?
> I see the decision between option a and option b as the first step, when can we expect this?
>
I would say after leaving to the coordinators and to the interested partners a reasonable time to provide their feedback. 1 or 2 weeks from now (also depending on possible feedback)?

> Assuming for steps 2.1 & 2.2 (updating the design for the Query Tool), you'll be getting feedback from partners?
>
It would be good to jointly finalise the requirement, e.g. agreeing on what type of "complex queries" may be useful to implement. trying to weigh the strengths and weaknesses of options A vs. B.

Are you sure that option B is that much more complex to query? Is the query functionality on the STAC API so bad?

On option A, how to you foresee maintaining alignment between the data catalog and your internal DB, nightly updates?

On steps 2.1 & 2.2 (updating the design for the Query Tool), I'd appreciate a dedicated workshop on this functionality. As we also need a workshop with the UC partners on dataset metadata requirements, could make sense to do this jointly
> trying to weigh the strengths and weaknesses of options A vs. B.
> 
> Are you sure that option B is that much more complex to query? Is the query functionality on the STAC API so bad?
> 
Not entirely sure, but quite confident :)
However, because disregarding option B, should be this the final decision, will have to be based on some evidence, we will investigate a bit more.
> On option A, how to you foresee maintaining alignment between the data catalog and your internal DB, nightly updates?
> 
Let's say regular updates. Nightly could be an option.
> On steps 2.1 & 2.2 (updating the design for the Query Tool), I'd appreciate a dedicated workshop on this functionality. As we also need a workshop with the UC partners on dataset metadata requirements, could make sense to do this jointly

Ok, good idea!