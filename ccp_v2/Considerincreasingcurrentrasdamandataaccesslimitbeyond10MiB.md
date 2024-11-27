Consider increasing current rasdaman data access limit beyond 10 MiB
When I try to access a dataset on rasdaman I often run into the current data access limit of 10 MiB, as indicated by this exception:

```
"ows:Exception": [
      {
        "@exceptionCode": "RasdamanRequestFailed",
        "ows:ExceptionText": "Data access is restricted to 10 MB."
      },
```

I was wondering if this can be increased to e.g. 100 MiB or even larger, since 10 MiB seems rather small to me for EO datasets? So I would very quickly be required to write code to loop over accessing the data in chucks. Or are there better/existing libraries that already handle this (via WCS)? this is for clarification. Is it fine at our end to increase the data access limit ? (I believe there can be various reasons why the access limit was chosen, and am asking for a green signal to go ahead). done, please check.
THe new limit is 200 mb Nice, thanks.