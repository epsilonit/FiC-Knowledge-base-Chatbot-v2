Providing Provenance Information
When exploring options for data provenance provision, I recently came across an interesting new activity under OGC integrating the concepts from the [W3C PROV-O](https://www.w3.org/TR/prov-o/) Provenance Ontology within a [STAC provenance extension](https://ogcincubator.github.io/bblocks-stac/bblock/ogc.contrib.stac.item-prov). As this is one of the bits where we'd need to propose something to STAC anyway, I'd prefer to utilize this approach (PROV-O has long been respected within the scientific community)



Input Term | multiplicity | Notes | currently implemented    STAC name | PROV-O element | Comment KS
-- | -- | -- | -- | -- | -- 
Origin | 0...1 | EO-based, produced by countries | stac:provenance_name |    | Think this may be a duplicate of Data Source, thus could be cut
Preprocessing | 0...1 | Description of the preprocessing | stac:preprocessing | missing | [requested](https://github.com/ogcincubator/bblocks-stac/issues/2)
Source Data | 0...1 | link to data | stac:source_data | prov:wasDerivedFrom |
Models | 0...1 | A url link to the processing Model | stac:models | prov:wasGeneratedBy|  
Documents & Publications | 1 |   | stac:documentation | missing |  [requested](https://github.com/ogcincubator/bblocks-stac/issues/2)

Note: the way PROV-O is designed, the concepts can link either directly to an external resource. I'd link to the STAC records for source data and a/p processing resources. See the first [example](https://ogcincubator.github.io/bblocks-stac/bblock/ogc.contrib.stac.item-prov/examples) for direct linking have you made any progress in integrating the PROV-O STAC extension?