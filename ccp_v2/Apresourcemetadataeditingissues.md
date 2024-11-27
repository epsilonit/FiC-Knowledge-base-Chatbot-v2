A/p resource metadata editing issues
Hi,

I tried to edit the metadata of an a/p resource using the form.

(1) Why do I have to copy the ID of the resource in this screen, since it is known already? Or is it intended to be able to modify the ID?

![Screenshot 2024-06-07 at 15 18 37](https://github.com/FAIRiCUBE/FAIRiCUBE-Hub-issue-tracker/assets/1943289/2934c941-004e-4b4a-816a-a8568871e66b)

(2) There is some  exact matching needed between these two required input fields, that I could not get to work. When I tried to submit a mismatch error was reported every time, so eventually I gave up. Could this maybe be improved with a better way to input the information? 

![Screenshot 2024-06-07 at 15 33 35](https://github.com/FAIRiCUBE/FAIRiCUBE-Hub-issue-tracker/assets/1943289/503a2ca7-b39c-4efc-8302-95091eca9b3d)

![Screenshot 2024-06-07 at 15 33 46](https://github.com/FAIRiCUBE/FAIRiCUBE-Hub-issue-tracker/assets/1943289/2605b820-5638-4be4-b3be-9ff4c5829526)

(3) What do we use to link to datasets in Rasdaman? A WCS URI (if it is public?)? And for model(s)? all I can try to answer is point 3, linking to rasdaman data. To my view, should be same process when linking to data on EOX.

We've been discussing how to reference a dataset in various contexts, e.g., for dataset provenance information we'd like to provide:
- a set of links to the source datasets used to derive this dataset 
- a set of links to the a/p resources used to derive this dataset

In both cases, these should be links to the corresponding metadata. Same approach should be used here when linking to datasets from a/p resource metadata.

Question is do we link to:
- the human readable metadata entry in the catalog 
https://catalog.eoxhub.fairicube.eu/collections/index/items/{DatasetID}
- the JSON record 
https://stacapi.eoxhub.fairicube.eu/collections/index/items/{DatasetID}

My take would be linking to the JSON record. Thoughts?

I find this tricky. The input field is described as ‘A link to data (or related metadata) to which the a/p resource has been applied’. So it can be both and doesn’t even have to be related to any kind of catalogue at all, it is very open as it is now. It is also an assumption that all input data used is spatio-temporal data that fits into STAC? any feedback on points above?
Hi,

- Regarding point 1), entry of the ID is required in order to be able to select the metadata to be edited from the various metadata available. The ID cannot be changed at the moment.
- I have fixed a bug in the system that was the cause of the error in point 2). The metadata should now be editable.
- Regarding point 3), even if the editor currently accepts any input, the idea should still be to use links to the related metadata.

What do you think about?
I'm wondering if the simplest solution for 3) would be providing both, link to catalog (viewable) and link to dataset
Hi
on our opinion providing the link to viewable catalog only is better, to first view relevant information on the dataset and then access it (using the link in the dataset metadata).