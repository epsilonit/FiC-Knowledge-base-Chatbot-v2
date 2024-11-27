automatic a/p resource metadata items ingestion  can you setup a procedure to automatically ingest the items in the catalog under the respective collection?

- In [no-ml_items](https://github.com/FAIRiCUBE/resource-metadata/tree/main/stac/no-ml_items) there are stac-json items related to  [no-ML collection](https://catalog.eoxhub.fairicube.eu/collections/no-ML%20collection)
- In [ml_items](https://github.com/FAIRiCUBE/resource-metadata/tree/main/stac/ml_items) there are stac-json items related to [ML collection](https://catalog.eoxhub.fairicube.eu/collections/ML%20collection)

Thank you
Dear , I did push changes to the `metadata-resource` and the automated ingestion should be working now, I would love for you to test it and give me some feedback.
removing/adding/editing stac items in the directories should trigger the ingestion pipeline and should delete/add/edit items in the `fast api` server, and results should be viewable in the stac-browser.

Some notes when creating more stac items, so they are valid :
- time must be in ISO format ("2023-04-04Z" ===> "2023-04-04")
- links of `parent` and `root`, should point out at the local (relative) `catalog.json` file (
   https://catalog.eoxhub.fairicube.eu/ ====> ./catalog.json
   https://catalog.eoxhub.fairicube.eu/collections/no-ML%20collection =====> ./catalog.json)
- a valid stac item must have geometry object ( I added world coverage ): 
```json
"geometry": {
        "type": "Polygon",
        "coordinates": [
          [
            [
              -180,
              -90
            ],
            [
              -180,
              90
            ],
            [
              180,
              90
            ],
            [
              180,
              -90
            ],
            [
              -180,
              -90
            ]
          ]
        ]
      }
```
Hi 
I am doing some tests and everything seems to work correctly and as expected. 
I have updated my procedure to implement your notes.
Thank you!