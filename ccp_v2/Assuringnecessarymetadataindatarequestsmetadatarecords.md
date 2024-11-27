Assuring necessary metadata in data requests/metadata records
For practicality, the first version of the [FAIRiCUBE Catalog editor](https://catalog-editor.eoxhub.fairicube.eu/​) is not strict on required fields. While this is OK as a first step, how do we assure later completeness?
good point, maybe we can use stac validation at some point prior to the ingestion step (e.g create a test that basically validates stac items and the PR's will not be merged until the test is passed )
as for now, I would not impose strict rules on completeness of metadata fields. I would really like to get user feedback on how long it takes to provide all metadata information and what as well they see as strictly necessary or "doable".
nevertheless we should prepare:
- short list of really essential meta data fields, they need to be checked for sure
- technical solution like the proposed stac item validation 


When finalizing the metadata webGUI, we did a check on the cardinalities of the metadata concepts described in D4.2, agreed on the following list of mandatory concepts (additions to D4.2 requirements in **BOLD**, reference to Inventory Sheet column in []):
- ID [Column C]
- **Description [D]**
- Data Source [Column E]
- **Owner/Organisation [G]**
- **Horizontal**
  - Horizontal CRS [Column P]
  - Bounding Box (Horizontal) [Column Q-T]
  - Resolution of Horizontal Axis (ie. Pixel Size) [Column W]
  - Units of Measurement [Column U]
- **Temporal**
  - Time (Begin/End) [Column AD-AE]
  - Resolution of Time Axis (Intervall) [Column AH]
  - Unit of measure [Column AF]
- Range Data Type [Column AR]
- **Range Definition [AS]**
- **Range Description [AT]**
- Null values [Column AQ]
- **Legal - License [BA]**
- **Keywords - Keywords [BJ]**