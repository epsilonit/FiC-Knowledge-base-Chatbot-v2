# Coverage XSDs

The [OGC Coverage Implementation Schema](https://www.ogc.org/standard/wcs/) standards come with their [normative schema for the XML encoding](https://schemas.opengis.net/cis/). 
This schema defines structures for the domain ("where do values sit in space/time?"), the range (the "payload"), and the range type ("what do these data mean?") - if this does not sound familiar you may want to see [this intro](https://earthserver.eu/wcs/).

For application-specific metadata, only an envelope is provided under which "any" data can be agglomerated. It is recommended for applications to define their own, independent "compartments" having their own schema. Example compartments include: INSPIRE, FAIRiCUBE, rasdaman. Having an open-ended list of such compartments allows applications to only look into "their" compartment and ignore all others, which greatly enhances modularity.

In order to enable reuse, such extensions must be documented. This requires the provision of a Schema file, together with a description of the meaning of the types provided by this schema.

## FAIRiCUBE Relevant Schemas

In FAIRiCUBE, the following non-standard compartment schemas are relevant:

- Metadata link from CIS Coverages into the FAIRiCUBE catalog: [MD-LinkSchema.xsd](https://fairicube.github.io/Schemas/CoverageXSDs/MD-LinkSchema.xsd)

## CIS Coverage Data Standard Version

(This discussion is independent from the metadata-specific schema and is just added for completeness here.)

There is a slight difference between CIS 1.0 (offering RectifiedGridCoverage and ReferenceableGridCoverage) and CIS 1.1 (adding GeneralGridCoverage). CIS 1.0 has some known schema issues inherited from GML, therefore it is recommended to use CIS 1.1 which has a more understandable structure and validates. In rasdaman, the outputType parameter allows choosing GeneralGridCoverage:

- in WCS, as extra request parameter: `https://.../...?outputType=GeneralGridCoverage`
- in WCPS, as extra encoding parameter: `return encode( $c, "json", "{\"outputType\":\"GeneralGridCoverage\"}")`


