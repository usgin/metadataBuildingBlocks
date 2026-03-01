## ECRR Dataset Profile

Complete metadata profile for registering dataset resources in the EarthCube Resource Registry.

### Composition

1. **ecrrBase** — mandatory identity and classification
2. **ecrrCommon** — optional shared properties
3. **ecrrAssessment** — resource assessment properties
4. **ecrrDataset** — dataset-specific (distribution, spatial/temporal coverage, variables, containing catalog)

### Type Requirements

- `@type` must include `schema:CreativeWork` and `schema:Dataset`
- `mainEntity` must reference `http://cor.esipfed.org/ont/earthcube/ECRRO_0000214` (Dataset)

### Reused Building Blocks

The dataset-specific properties heavily reuse existing building blocks:
- `schema:distribution` via dataDownload and webAPI
- `schema:spatialCoverage` via spatialExtent
- `schema:temporalCoverage` via temporalExtent
- `schema:variableMeasured` via variableMeasured
