Assets accordion does not open to show details
The accordion header does not expand to show details. E.g. when clicking on "original CLMS thumbnail" nothing happens. Example Item:  https://catalog.eoxhub.fairicube.eu/collections/index/items/Vienna_imperviousness_density_2018?.language=en

![image](https://github.com/user-attachments/assets/33d00c7c-2883-4e0d-94fd-31ba89a2528f)

looking at the stac item I can see that the assets keys has spaces in their values ( `"COG file on fairicube S3 Bucket"` & `"original CLMS thumbnail"` ). Which should be detected earlier, that's a good catch, I will make sure that the editor validation catches this before submitting.

I fixed the values (to `"COG_file_on_fairicube_S3_Bucket"` & `"original_CLMS_thumbnail"`) and now the browser is functioning normally.

Thanks for looking into it. You could add also this requirement in the field description, e.g. "The name of the data asset, without whitespace"