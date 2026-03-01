## DDE Geographic Dataset Properties

Conditional extension for geographic dataset resources. Implements MD_SpatialRepresentation from DDE spec Table 5. This building block is composed into the DDEDataset profile when the resource type is `geographicDataset`.

### Required
- **`schema:spatialCoverage`**: Spatial extent of the geographic dataset. Mandatory for geographicDataset resource type.

### Optional (via `schema:additionalProperty`)
- **`dde:spatialRepresentationType`**: Type of spatial representation (from SpatialRepresentationTypeCode, e.g., "vector", "grid", "tin").
- **`dde:spatialResolution`**: Spatial resolution of the dataset (free text, e.g., "1:1000000", "30m").
- **`dde:referenceSystemType`**: Type of coordinate reference system (from ReferenceSystemTypeCode).
- **`dde:referenceSystemIdentifier`**: Identifier for the coordinate reference system (e.g., "EPSG:4326").
