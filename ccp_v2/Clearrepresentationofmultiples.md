Clear representation of multiples
When using the WebGUI and providing multiple entries for some fields (e.g. Bands), it becomes very difficult to see the individual entries. Please provide some sort of visual divider  could you elaborate more please ? take a look at the Bands section of the SPARTACUS_v2.1 Dataset. As this dataset has many bands, it becomes very difficult to understand which fields belong to which band. Some sort of divider between these fields, or a frame around each individual band, would be most helpful.

Same logic applies to all concepts where we can provide multiple entries, each entry consists of multiple fields
it looks like the branch was merged (so it is not visible in the editor), and looking at the actions of the [pull request](https://github.com/FAIRiCUBE/data-requests/pull/273) I can see it was not ingested successfully, this is a bug within the pipline, I will work on fixing it.
In the meantime I would recommend to revert the PR done, [here](https://github.com/FAIRiCUBE/data-requests/pull/274)


Thanks and yes I saw what you are refering to, I will edit the css so that border lines in the divs are visible again.
added some margins for multiple fields in the Webgui

![Screenshot from 2024-06-24 19-29-29](https://github.com/FAIRiCUBE/catalog/assets/28819736/ea1b326c-8e0b-4fee-84e8-6d7000e5b47f)