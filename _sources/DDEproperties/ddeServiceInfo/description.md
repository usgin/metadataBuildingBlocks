## DDE Service Information Properties

Conditional extension for service resources. Implements SV_ServiceIdentification from DDE spec Table 2. This building block is not included in the base DDEDiscovery profile — it is composed into the DDEService profile when describing web services.

### Required
- **`schema:serviceType`** (on WebAPI in `schema:distribution`): Service type specified as a `schema:DefinedTerm` from the DDE `ServiceTypeCode` codelist (`dde:codelist/ServiceTypeCode`). Supported types include OGC services (WMS, WFS, WCS, WMTS, WPS, CSW, SOS, SensorThings, API-Features, etc.), ESRI services (MapServer, FeatureServer, ImageServer), SPARQL, OpenDAP, and THREDDS.

### Optional
- **`schema:potentialAction`** (on WebAPI): Operations available on the service (containsOperations). Array of Action objects with name and description.
- **`schema:termsOfService`** (on WebAPI): Access properties and constraints for the service. String or CreativeWork.
- **`schema:documentation`** (on WebAPI): Endpoint description document URL or CreativeWork reference.
- **`schema:dataset`** (on root): Datasets operated on by the service (operatedDataset). Array of URIs or Dataset references.
- **`schema:additionalProperty`**: Additional properties for service resources.
