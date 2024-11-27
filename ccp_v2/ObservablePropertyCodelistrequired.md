Observable Property Codelist required
### Codelist name

ObservableProperties

### Codelist value

Water and Wetness

### Codelist value description

The HRL Water and Wetness 2018 provides primary products in full spatial resolution of 10m x 10m (as compared to 20m x 20m resolution in 2015). The main product is a classified layer, differentiating the classes of permanent water, temporary water, permanent wet, temporary wet, and dry areas, derived from water and wetness occurrences in the period 2012-2018. Table 1 below summarizes the detailed definitions for these classes.

Source: https://land.copernicus.eu/en/technical-library/hrl-water-wetness-2018-user-manual/@@download/file

### Issue faced

Copernicus does not expose these as ObservableProperties, thus we must make our own

### Change proposer 

### References

https://land.copernicus.eu/en/technical-library/hrl-water-wetness-2018-user-manual/@@download/file We now have a list of required observable properties for the datasets on rasdaman in the issue 
[Observable Property codelist entries](https://github.com/FAIRiCUBE/data-requests/issues/318). Before I can provide an import file for these properties, I'd need to understand the input format (assuming CSV): what columns are foreseen within such an import file? I first updated the [repo readme](https://github.com/FAIRiCUBE/resource-metadata/blob/main/README.md), in which I’ve created 2 tables, one for a/p resource metadata codelists and one for dataset metadata codelist, providing a quick overview of codelists structure and content.
Regarding how to link the codelist values to the related dataset, EPSIT suggests to create a FAIRiCUBE codelist register, implementing an instance of the Re3gistry software. To this end, the codelists to be inserted into the register can be provided in a csv file containing, for each codelist provided in a separate sheet, the two mandatory fields (both of character string type): codelist value label, codelist value definition and a third optional field (of URI type) containing the source link (which is better to keep in a separate field to ensure that it is clickable).