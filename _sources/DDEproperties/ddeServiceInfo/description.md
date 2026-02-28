## DDE Service Information Properties

Conditional extension for service resources. This building block is not included in the base DDEDiscovery profile — it should be composed into a DDEServiceProfile sub-profile when describing web services.

### Required
- **`schema:distribution`**: Must contain at least one `schema:WebAPI` with `schema:serviceType` specified as a `schema:DefinedTerm` from the DDE `ServiceTypeCode` codelist (`dde:codelist/ServiceTypeCode`). Supported service types include OGC services (WMS, WFS, WCS, WMTS, WPS, CSW, SOS, SensorThings, API-Features, etc.), ESRI services (MapServer, FeatureServer, ImageServer), SPARQL, OpenDAP, and THREDDS.

### Optional
- **`schema:additionalProperty`**: Additional properties for service resources, such as identifiers of operated datasets using `schema:PropertyValue` with appropriate propertyIDs.
