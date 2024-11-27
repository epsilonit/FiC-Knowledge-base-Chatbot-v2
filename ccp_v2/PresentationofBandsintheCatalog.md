Presentation of Bands in the Catalog
The section on Bands (currently listed under "Raster Imagery", don't think this title is suitable to the data we're providing, maybe modify to something more fitting, e.g. "Data Cube Content"), the ordering of the fields being provided is suboptimal, starting with the datatype, number of values.

To my view, it would be more useful for users with the following order: 
- Band Name
- Definition
- Description
- Unit of Measurement (I've never seen the formulation "Unit of Values")
- Data Type
- No-Data Values

![grafik](https://github.com/FAIRiCUBE/catalog/assets/11915304/6066e22b-517f-4ee6-beba-511e1339f8bb)

It would be doable easily to sort the fields alphabetically  ('band_name', 'comment', 'data_type', 'definition', 'description', 'nodata' 'unit').
Would this be accebtable  ?
![Screenshot from 2024-07-24 16-24-03](https://github.com/user-attachments/assets/a3823a82-59f3-4b4d-abf0-07835296408d)

Don't think alphabetical sorting is helping, should be based on what most helps a user understand the dataset. How about the following order:
- 'band_name'
- 'description'
- 'definition'
- 'unit'
- 'comment'
- 'data_type'
- 'nodata'

As technically difficult to not provide in alphabetical order, we'll have to cope