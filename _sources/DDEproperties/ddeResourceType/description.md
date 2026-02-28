## DDE Resource Type

Constrains `schema:additionalType` to require at least one `schema:DefinedTerm` from the DDE `ResourceTypeCode` codelist (`dde:codelist/ResourceTypeCode`).

The DDE vocabulary defines 42 geoscience-specific resource types including geological maps, geochemical datasets, paleontological datasets, drillholes, stratigraphic sections, and more. The `schema:termCode` must match one of the enumerated codes.

This building block is used by `ddeRequired` to enforce DDE resource typing on metadata records.
