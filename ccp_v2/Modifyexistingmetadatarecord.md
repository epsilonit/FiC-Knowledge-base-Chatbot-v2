Modify existing metadata record
Is it possible to update finalized metadata records (PR merged, record available in the catalog)?

We now have a MD record for [ERA5 land monthly](https://catalog.eoxhub.fairicube.eu/collections/index/items/ERA5_Land_monthly?.language=en), but only linked to rasdaman. To my understanding, [requested ](https://github.com/FAIRiCUBE/FAIRiCUBE-Hub-issue-tracker/issues/53)this dataset for EOX. How can we add a link to this data on EOX?

I also need to add the API. How can I do?
+1 it has to be possible to update published records.

I wonder if the ERA5_Land_monthly record can actually be reused as is for the EOX data, which if I understand correctly has just a subset of the available bands?
To my understanding, on EOX, instead of downloading all data and using it locally, the UC partners utilize an API provided by Google. Thus, believe all bands are also available there.

Would be really nice if we could have the one entry point to both of these sources! any updates on this?
Normal UC partners should not be able to do this (too dangerous), only admins
Should be done via STAC API please provide a link to this API
https://stacapi-write.eoxhub.fairicube.eu/api.html
" you have to use credentials (the old basic authinticaition username/password) : username= fairicube)"