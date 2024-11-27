Dataset Metadata concepts I just scanned the existing version of D4.2, noticed that most of the concepts under data provenance are missing from the STAC mapping table in section 3.1	Metadata mapping. However, they are covered by the catalog editor, are also in the inventory sheet. Could you please make sure D4.2 gets updated accordingly?

As already mentioned by would be good to have a working copy of this doc on TEAMs ASAP! I think we really need some clarification on some of the metadata concepts, I'm getting increasingly lost between what was originally envisioned (D6.2 first version), what's in the list of metadata requirements (D4.2) and what I see in the stac catalog entries.

I believe that a bit of the confusion on actor roles (#16 e.g. data provider, owner...) is due to this issue. We've discussed holding a session with UC partner on MD requirements, before this session we need to think through some of this!

Simple example on the concept Documentation:
- D4.2: stac:fic:purpose
- JSON: "documentation"


is this resolved a part of the delivery of D4.2 or maybe even related to 
https://github.com/FAIRiCUBE/catalog/issues/35 ?
Just checked, not related to #35. There it's about display names in the GUI, here it's about the JSON encoding, alignment with STAC standard. At present, we're still not really using STAC, just freestyle JSON