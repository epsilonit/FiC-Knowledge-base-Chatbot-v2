Providing License Information
The License pull-down list is a bit of a disaster as VERY long and not alphabetical :(

Is it possible to introduce a cascade, e.g. a subgroup with all GNU licenses?

Also, is the Copernicus license listed? This is what I was looking for when I noticed the wider order issue
Indeed grouping/sorting of items would definitely help! 
Would it be a good idea to add a conditional free text field when "Other" is selected, to paste a link to a non-standard license?
+1 to the proposal of many datasets are with non-standard license.
a new commit was added to the editor:
- When selecting "Other" as a license user must provide a url link to the license text, this will be added to the stac links ( relation will be "license").

The implementation above follows this [specs](https://github.com/radiantearth/stac-spec/blob/master/item-spec/common-metadata.md#licensing)
Grouping/sorting still not ideal. What are you sorting by?

![grafik](https://github.com/user-attachments/assets/000c7234-f0ba-43e3-8092-32f924ce6f3a)

Check what licenses are provided in FAIRiCUBE records, provide these first
Then provide entire list in alphabetical order (including the common suspects at the top)
license list now shows first the most (7) common used list, then the rest of the list sorted alphabetically.