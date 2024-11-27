More user-friendly UI
The user interface of the catalog-editor can be improved to be more user-friendly. As it is, a lot of guessing is involved in what each field should contain, and what the effects of setting it are.

TOC by catalog editor section

- [Top](#Top)
- [General](#General)
- [Organizations](#Organizations)
- [Horizontal Axis](#HorizontalAxis)
- [Time Axis](#TimeAxis)
- [Bands](#Bands)
- [Re-projection axis](#Reprojectionaxis)
- [Legal](#Legal)
- [Keywords](#Keywords)
- [Provenance](#Provenance)
- [Data Quality, Accessibility, Dates](#DataQualityAccessibilityDates)
- [Internal](#Internal)

<a name="Top"/>

## Top

- At the top add a sentence or two on what this form is about, and what happens when it's submitted.

<a name="General"/>

## General

- Title: what is the effect of this value, does it become the Title in the catalog?
- ID: should it be the datacube ID or it can be anything?
- Data Source: should it be a link to a page where the data is described, or can be downloaded, or something else?
- Source Type: I only guess "grid" is correct because that's what I've seen in previous requests. Is a better name "Data Type"? If it's just about grid and vector then make it a drop-down selection, or radio buttons or so?
- total area cover: capitalize
- CRS: I've seen EPSG:4326 and 4326 as values both used, which one is correct? Is it a CRS for the whole datacube? Not sure why it's necessary when it exists later on when the axes are described

![Screenshot_20240726_140052](https://github.com/user-attachments/assets/ed0b9030-8ded-4712-a0cd-8fb41f50ee80)

<a name="Organizations"/>

## Organizations

- Both `Organization` and `Name` expect the same name ("The name of the organization which produced the dataset.")?
- Project Purpose: should it be "Organization purpose"?
- Documentation Link: documentation of the organization?

![Screenshot_20240726_140758](https://github.com/user-attachments/assets/1f1aa884-21e6-49aa-91e6-d2aa2a8be7a6)

<a name="HorizontalAxis"/>

## Horizontal Axis

- Unit of Measure could be automatically set from the CRS definition, e.g. https://epsg.io/3035
- Interpolation/Aggregation: this needs a description, I still have no idea what it is about

![Screenshot_20240726_141053](https://github.com/user-attachments/assets/55604900-77bd-4ea3-8016-1eaad5bb0583)

<a name="TimeAxis"/>

## Time Axis

- For irregular time axis, the + button should be more prominent and required to enter at least one value. Best change to "+ Add time coordinate"
  - If only one value is entered, the editor currently automatically adds a second one with value "2999..." which should not be done
- Interpolation/Aggregation: same as for Horizontal Axis
- Resolution should not be shown/required for irregular axis #30

![Screenshot_20240726_141923](https://github.com/user-attachments/assets/5aee7eaf-8ff5-41b6-b98f-8e1f03f54d24)

<a name="Bands"/>

## Bands

- **Important**: expand this with one band ready to be filled in, and make one band mandatory. I think nobody bothers to click on the "+ Add bands" otherwise and no bands end up specified.
- cell components: capitalize; should be just one? should be a name right, maybe call this field "Band name"?
- Unit of Measure: add a hint ".. of the pixel values in [UCUM-compliant format](https://en.wikipedia.org/wiki/Unified_Code_for_Units_of_Measure)"
- Null values: can it be more than one? if yes how should they be separated, with commas?
- Definition: add a hint of what should this contain?
- Category List: add a hint, should it be comma-separated or what format?

![Screenshot_20240726_141948](https://github.com/user-attachments/assets/c3b7e1d0-e503-4065-9d5c-ddddec0d7f80)

<a name="Reprojectionaxis"/>

## Re-projection axis

Add description what is this about, I don't understand in which case would it need to be filled in.

![Screenshot_20240726_142501](https://github.com/user-attachments/assets/5d39b86b-9d8b-437f-9032-9b938090565b)

<a name="Legal"/>

## Legal

- License: in case of Other allow to enter a link or description of a non-standard license
- Personal Data: add a hint what is this about?

![Screenshot_20240726_142603](https://github.com/user-attachments/assets/e380bd7e-c950-4d65-8466-915da71ad2f5)

<a name="Keywords"/>

## Keywords

- Comma-separated? Add a hint about the format

<a name="Provenance"/>

## Provenance

- Origin - link to data download or something else?
- Preprocessing (description): of the data before it was imported in the datacube by us, or by the original data distributor?
- Source Data (links) - what's the difference to Origin?

![Screenshot_20240726_142825](https://github.com/user-attachments/assets/38a8d26b-cb86-4013-88a4-5e97a6903a15)

<a name="DataQualityAccessibilityDates"/>

## Data Quality, Accessibility, Dates

Add hints to the fields, what exactly should they contain?

<a name="Internal"/>

## Internal

- Ingestion Status (rasdaman): This could best be a dropdown with a couple of options: Downloading, Preprocessing, Importing, Completed?
- Assignees: what is the effect of assigning to someone? For rasdaman maybe just set Dimitar M by default MANY THANKS for your work!!! these are the reasons why I generally prefer to first specify such applications, then develop! Please go through comments, define what you'll do about the problems addressed, create issues for what will be changed (and then change)

General comment: on mandatory fields, I now see the text `XXX is required, you can submit now successfully but the validation test will fail.` - as I user I find this statement confusing (you can submit, but it's not gonna help you! ;) ). I'm wondering if we need to differentiate between `Submit` (record done, please process) and Save (done for today, but still work to be done)?
Hi ,
From our last call I understand that the blocking point to update the GUI copy is that you need an updated mapping of field/label/Help message, correct? let's update or start a table with the desired text, so that Mussab can implement it. E.g.
|Field|Label|Help message|
|-|-|-|
|stac:common:title|Title|The title of the collection which will be displayed in the Catalog.|
|stac:common:description|Description| Detailed description of the dataset.|
|stac:item:id|Unique Identifier|Provider identifier. Must be unique within the Catalog.|
|...|...|...|

I am sure there is already this document somewhere, but I could not find it. where did you get the labels/help messages from initially? no particular source, some help messages were in the github issue template (e.g deiscription). 
Okay, I have set up an Excel Sheet in our Teams: [catalog_editor_GUI_text](https://nilu365.sharepoint.com/:x:/r/sites/Horizon2021_CUBE/Shared%20Documents/WP4%20-%20share/Catalog/[catalog_editor_GUI_text.xlsx](https://nilu365.sharepoint.com/:x:/r/sites/Horizon2021_CUBE/Shared%20Documents/WP4%20-%20share/Catalog/catalog_editor_GUI_text.xlsx?d=w303dddb27fcb4e7c82e851c3476738f7&csf=1&web=1&e=4VLx5W)?d=w303dddb27fcb4e7c82e851c3476738f7&csf=1&web=1&e=4VLx5W)
It is essentially a copy of Table 1 Mapping from metadata requirements to STAC of Deliverable 4.2, with a new column for the item description that will appear in the GUI.  does this work for you? Can you help fill out the descriptions?
I'm not an expert on this topic, I just left feedback here from the perspective of a user that used the catalog editor. while I'd like to provide support in descriptive texts for the GUI, at present, as the mapping to STAC has gone badly sideways, I no longer know what goes where. Example:
Data source -> Name was originally mapped to `stac:contacts:name`, defined as `The name of the responsible person.`
Now mapped to `stac:common:providers:organization_name`

Issues:
- STAC PROVIDERS doesn't exist!!!
- Entry was for the **responsible person**, now morphed to organization_name

;(