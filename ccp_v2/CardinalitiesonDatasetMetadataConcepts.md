Cardinalities on Dataset Metadata Concepts as recently discussed, there are some issues with the cardinalities on dataset metadata concepts as described in D4.2. I've copied in the agreement on mandatory concepts from our mail exchange, please update in D4.2 accordingly.

Mandatory Columns (additions in bold)
- ID [Column C]
- **Description [D]**
- Data Source [Column E]
- **Owner/Organisation [G]**
- **Horizontal**
  - Horizontal CRS [Column P]
  - Bounding Box (Horizontal) [Column Q-T]
  - Resolution of Horizontal Axis (ie. Pixel Size) [Column W]
  - Units of Measurement [Column U]
- **Temporal** (if dataset has temporal extent)
  - Time (Begin/End) [Column AD-AE]
  - Resolution of Time Axis (Intervall) [Column AH]
  - Unit of measure [Column AF]
- **Range Description**
  - Range Data Type [Column AR]
  - Range Definition [AS]
  - Range Description [AT]
- Null values [Column AQ]
- **Legal - License [BA]**
- **Keywords - Keywords [BJ]**

In addition, some fields can be filled with defaults, e.g.

- Metadata Standard: STAC
- Provision Date: Date being provided
Just checked, still some issues with the following metadata items:
- ID [Column C]: still optional
- Data Source [Column E]: still optional
- Owner/Organisation [G]: contained fields mandatory, but Organization entry optional
- Range Description: contained fields mandatory, but Bands entry optional
- Null values [Column AQ]: only mandatory if Bands provided, but Bands still optional

Please provide 2 separate lists:
- fields that must be provided to save
- fields that must be provided to merge PR