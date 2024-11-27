Catalog entry without a time axis
We have some data that is just 2D and has no time axis. But the catalog seems to not allow submitting such a request, this should be fixed.
I will work on allowing 2D data, but  the generated stac item must have either begin/end time or a datetime value in order to be ingested  into pgsatc db ( this is why the start/end =  1900/2999 or datetime= 2000 values are assigned to items when no time is specified).

Any suggesgion on how to do it differently ? 
That's unusual, non-timeseries data is pretty common. Could be 2D, or volumetric 3D, etc. I'm not familiar with the catalog standard/spec so don't really have any suggestions. Maybe has some insights.
> must have either begin/end time or a datetime value

But does it have to be associated with an actual axis? You could put a datetime as a metadata of when the data was created or so, but shouldn't require a time axis for that.
> > must have either begin/end time or a datetime value
> 
> But does it have to be associated with an actual axis? You could put a datetime as a metadata of when the data was created or so, but shouldn't require a time axis for that.

yes, providing creation time should suffice, sorry for the confusion!
is this also done and only needs approval?

Hi so to clarify: when the data refers to a single point in time, you recommend to use the fields Date:Creation Date, instead of the Time axis?
> Hi so to clarify: when the data refers to a single point in time, you recommend to use the fields Date:Creation Date, instead of the Time axis?

yes I'd worry about using creation date, as that usually refers to the time the dataset was calculated, especially with modeled data this can be years after the fact.

While we could implement a different approach for datasets that cover only one point in time, I do like keeping things consistent. Any reason we cannot just use the Time Axis component, provide same begin and end time for such datasets?