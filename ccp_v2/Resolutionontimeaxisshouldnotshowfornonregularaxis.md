Resolution on time axis should not show for non-regular axis
When the "regular ?" on Time Axis is unchecked, the resolution should not show or require to be set, because resolution on irregular axis is not a fixed value.

For irregular axis what could be added is an "area of validity" or "footprint" for each coordinate that is added that defines the lower and upper bounds of that point. For example if I add a coordinate "2015-01-01", the area of validity might be 

- lower "2014-01-01"
- upper "2016-01-01

cc  think there's something backwards here, when I deselect regular, the `Resolution `fields stay while the `Lower/Upper Bounds` go away. To my understanding its the `Resolution ` fields should go away while the bounds should always be there.
The Resolution should go away when selecting irregular time dimensions, this bug will be fixed.

As for the lower/upper bounds  - which corresponds to time's `extent`-  irregular time dimension according to [specs](https://github.com/stac-extensions/datacube?tab=readme-ov-file#temporal-dimension-object) should be represented as values ( extent set to null), therefor lower/upper bounds  are removed and users should add individual values ( by clicking the + button) I don't know if there is a way to represent irregular time slots with areas of validity,  do you have an example ? yes I think there's no standardized way currently of representing this concept, you can ignore my suggestion for areas of validity.
> The Resolution should go away when selecting irregular time dimensions, this bug will be fixed.

This should be fixed now