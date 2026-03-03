# Agents Guide: OGC Building Blocks Repository

This document explains how to work with this repository — the building block structure, authoring rules, validation workflow, and the schema resolver tool.

## What This Repository Is

This repository contains modular schema components following the [OGC Building Blocks](https://opengeospatial.github.io/bblocks/) pattern. Each building block is a self-contained directory with a JSON Schema, JSON-LD context, metadata, and description. Building blocks compose into profiles that define complete metadata schemas for specific use cases.

The repository is included as a git submodule in the [IEDA Data Submission Portal](https://github.com/smrgeoinfo/IEDADataSubmission) monorepo.

## Repository Structure

```
metadataBuildingBlocks/
├── _sources/                        # All building block sources
│   ├── schemaorgProperties/         # Core schema.org property types
│   │   ├── person/                  # schema:Person
│   │   ├── organization/            # schema:Organization
│   │   ├── identifier/              # schema:identifier (PropertyValue)
│   │   ├── definedTerm/             # schema:DefinedTerm
│   │   ├── additionalProperty/      # schema:PropertyValue for soft-typed properties
│   │   ├── variableMeasured/        # schema:variableMeasured (PropertyValue)
│   │   ├── spatialExtent/           # schema:Place (bounding box)
│   │   ├── temporalExtent/          # schema:temporalCoverage
│   │   ├── dataDownload/            # schema:DataDownload
│   │   ├── labeledLink/             # schema:LinkRole
│   │   ├── funder/                  # schema:funder / schema:Grant
│   │   ├── webAPI/                  # schema:WebAPI
│   │   ├── action/                  # schema:Action
│   │   └── agentInRole/             # schema:Role wrapping Person/Org
│   ├── cdifProperties/              # CDIF-specific property types
│   │   ├── cdifCatalogRecord/       # dcat:CatalogRecord metadata-about-metadata
│   │   ├── cdifMandatory/           # CDIF mandatory property group
│   │   └── cdifOptional/            # CDIF optional property group
│   ├── adaProperties/               # ADA (Astromat Data Archive) property types
│   │   ├── stringArray/             # Reusable string array utility type
│   │   ├── creativeWork/            # schema:CreativeWork labeled links
│   │   ├── spatialRegistration/     # Pixel coordinate system registration
│   │   ├── instrument/              # NXinstrument + prov:Entity analytical instruments
│   │   ├── laboratory/              # NXsource + schema:Place facilities
│   │   ├── details/                 # 16 instrument-specific detail types ($defs)
│   │   ├── physicalMapping/         # DDI-CDI flat per-column variable mapping (CDIF 2026)
│   │   ├── image/                   # ada:image with componentType classification
│   │   ├── imageMap/                # Spatially registered image maps
│   │   ├── supDocImage/             # Supplemental document images
│   │   ├── tabularData/             # CDI TabularTextDataSet with CSVW properties (CDIF 2026)
│   │   ├── collection/              # Sets of related files
│   │   ├── dataCube/                # CDI StructuredDataSet multidimensional data (CDIF 2026)
│   │   ├── document/                # Supplemental documents (calibration, methods, logs)
│   │   ├── otherFile/               # Non-standard file formats (EMSA, OBJ, STL, XLSX)
│   │   └── files/                   # File-level metadata (generic, type constraints at profile level)
│   ├── provProperties/              # W3C PROV provenance types
│   │   ├── generatedBy/             # prov:wasGeneratedBy (Activity)
│   │   └── derivedFrom/             # prov:wasDerivedFrom
│   ├── cdiProperties/               # DDI-CDI data description types
│   │   └── cdiVariableMeasured/     # CDI variable measured extension
│   ├── qualityProperties/           # Data quality types
│   │   └── qualityMeasure/          # Quality measure definitions
│   ├── xasProperties/               # X-ray Absorption Spectroscopy types
│   │   ├── xasSample/               # XAS sample (extends schema:Product)
│   │   ├── xasInstrument/           # XAS instrument (beamline, synchrotron)
│   │   ├── xasFacility/             # XAS facility (synchrotron source)
│   │   ├── xasGeneratedBy/          # XAS analysis event (extends cdifProv)
│   │   ├── xasHDF5DataStructure/    # HDF5 data structure for XAS
│   │   ├── xasXdiTabularTextDataset/ # XDI tabular text dataset
│   │   ├── xasRequired/             # XAS mandatory property group
│   │   ├── xasOptional/             # XAS optional property group
│   │   └── xasSubject/              # XAS subject classification
│   ├── DDEproperties/               # DDE (Deep-time Digital Earth) types
│   │   ├── ddeSubject/              # DDE profile conformance (CatalogRecord extension)
│   │   ├── ddeResourceType/         # DDE resource type (32 types from DDE spec Table 18)
│   │   ├── ddeRequired/             # DDE mandatory property group
│   │   ├── ddeOptional/             # DDE optional properties
│   │   ├── ddeImagery/              # DDE imagery (9 MD_Imagery properties, all optional)
│   │   ├── ddeServiceInfo/          # DDE service info (SV_ServiceIdentification, 5 elements)
│   │   └── ddeGeographicDataset/    # DDE geographic dataset (MD_SpatialRepresentation)
│   ├── ecrrProperties/              # ECRR (EarthCube Resource Registry) property types
│   │   ├── ecrrBase/                # ECRR identity: mainEntity, @type contains CreativeWork, description, license
│   │   ├── ecrrCommon/              # ECRR common properties (creator, keywords, distribution, etc.)
│   │   ├── ecrrAssessment/          # Resource assessment (maturity, lifetime, usage, stewardship)
│   │   ├── ecrrCatalog/             # Catalog/repository-specific properties
│   │   ├── ecrrCollection/          # Bundled object/collection-specific properties
│   │   ├── ecrrDataset/             # Dataset-specific properties (legacy; ECRRDataset profile uses CDIFcomplete instead)
│   │   ├── ecrrSemanticResource/    # Semantic resource-specific properties
│   │   ├── ecrrService/             # Service instance-specific properties
│   │   ├── ecrrSoftware/            # Software-specific properties
│   │   └── ecrrSpecification/       # Specification-specific properties
│   └── profiles/                    # Top-level profiles that compose BBs
│       ├── cdifProfiles/
│       │   ├── CDIFDiscovery/       # CDIF Discovery profile
│       │   └── CDIFcomplete/        # CDIF Complete profile (discovery + cdifProv + data description)
│       ├── DDEProfiles/
│       │   ├── DDEDiscovery/        # DDE Geoscience Discovery profile (base)
│       │   ├── DDEDataset/          # DDE Dataset (dataset, dataCatalog, geographicDataset, nonGeographicDataset)
│       │   ├── DDECollection/       # DDE Collection (aggregate, collection, series, learningResource, guide)
│       │   ├── DDEDocument/         # DDE Document (document, article, thesis, book, poster, webPage)
│       │   ├── DDEImage/            # DDE Image (image, map, photograph, explanatoryFigure)
│       │   ├── DDEService/          # DDE Service (repository, service, webAPI)
│       │   ├── DDESoftware/         # DDE Software (software)
│       │   ├── DDEEvent/            # DDE Event (initiative, fieldSession)
│       │   ├── DDEFunctionalResource/ # DDE Functional Resource (application, webApplication, model)
│       │   ├── DDESemanticResource/ # DDE Semantic Resource (semanticResource, definedTermSet)
│       │   └── DDEAudioVisualProduct/ # DDE AudioVisual Product (movie, sound)
│       ├── adaProfiles/
│       │   ├── adaProduct/          # ADA product metadata profile (v3, CDIF 2026)
│       │   ├── adaEMPA/             # Electron Microprobe Analysis technique profile
│       │   ├── adaXRD/              # X-ray Diffraction technique profile
│       │   ├── adaICPMS/            # ICP Mass Spectrometry technique profile
│       │   ├── adaVNMIR/            # Very-Near Mid-IR / FTIR spectroscopy profile
│       │   └── ada{TECHNIQUE}/      # 31 more technique profiles (generated by generate_profiles.py)
│       └── ecrrProfiles/
│           ├── ECRRDataset/         # ECRR Dataset (extends CDIFcomplete + ecrrBase + ecrrAssessment)
│           ├── ECRRService/         # ECRR Service Instance
│           ├── ECRRSoftware/        # ECRR Software
│           ├── ECRRSpecification/   # ECRR Specification
│           ├── ECRRCatalog/         # ECRR Catalog/Registry
│           ├── ECRRCollection/      # ECRR Bundled Object/Collection
│           ├── ECRRSemanticResource/ # ECRR Semantic Resource
│           ├── ECRRUseCase/         # ECRR Use Case
│           ├── ECRRInterface/       # ECRR Interface/API
│           ├── ECRRInterchangeFormat/ # ECRR Interchange Format
│           └── ECRRPlatform/        # ECRR Platform
├── tools/
│   ├── resolve_schema.py            # Schema resolver (see below)
│   ├── convert_for_jsonforms.py     # JSON Forms converter (see below)
│   ├── generate_profiles.py         # Data-driven profile generator (see below)
│   ├── update_conformsto.py         # Stamp dcterms:conformsTo with profile URIs
│   ├── validate_instance.py         # Profile-aware validation tool
│   ├── augment_register.py          # Adds resolvedSchema URLs to register.json
│   └── cors_server.py               # CORS dev server for local testing
└── .github/workflows/               # Validation + JSON Forms generation + custom Pages deploy
```

## Building Block Structure

Each building block directory contains:

| File | Required | Purpose |
|---|---|---|
| `bblock.json` | Yes | Metadata: name, status, tags, version, links, sources |
| `schema.yaml` | Yes | JSON Schema with `$ref` cross-references to other BBs |
| `context.jsonld` | Yes | JSON-LD namespace prefix mappings |
| `description.md` | Yes | Human-readable description |
| `examples.yaml` | No | Example snippets with `ref:` pointing to example JSON files |

### `bblock.json` Required Fields

Every `bblock.json` must include all of these fields:

```json
{
  "$schema": "https://raw.githubusercontent.com/opengeospatial/bblocks-postprocess/refs/heads/master/ogc/bblocks/metadata-schema.yaml",
  "name": "Human-readable name",
  "abstract": "One-line description",
  "status": "under-development",
  "dateTimeAddition": "2026-01-01T00:00:00Z",
  "itemClass": "schema",
  "register": "ogc-building-block",
  "version": "0.1",
  "dateOfLastChange": "2026-01-01",
  "link": "https://github.com/usgin/metadataBuildingBlocks",
  "maturity": "development",
  "scope": "unstable",
  "tags": ["tag1", "tag2"],
  "sources": []
}
```

Missing `dateOfLastChange` or `link` will cause the validation workflow to fail.

### `schema.yaml` Cross-Reference Rules

Schemas reference other building blocks using relative `$ref` paths:

```yaml
$defs:
  Person:
    $ref: ../../schemaorgProperties/person/schema.yaml
  Identifier:
    $ref: ../../schemaorgProperties/identifier/schema.yaml
```

**Critical rules:**

1. **Always reference `schema.yaml`, never standalone `.json` files.** The postprocess tool resolves `$ref` to GitHub Pages URLs. References to `.json` files cause 404 errors because only `schema.yaml` files are published to GitHub Pages.

   ```yaml
   # CORRECT
   $ref: ../../cdifProperties/cdifCatalogRecord/schema.yaml

   # WRONG — will cause 404 in validation
   $ref: ../../cdifProperties/cdifCatalogRecord/cdifCatalogRecordSchema.json
   ```

2. **Use correct relative paths.** Paths are relative to the current `schema.yaml` file. Building blocks in `xasProperties/` that reference `schemaorgProperties/` need `../../schemaorgProperties/...`, not `../...`.

3. **Reference `$defs` within another schema.yaml** using fragment syntax:
   ```yaml
   $ref: ../../schemaorgProperties/additionalProperty/schema.yaml#/$defs/propertyID_item
   ```

### `examples.yaml` Rules

1. **`ref:` must match the actual filename** in the building block directory. Copy-paste errors referencing files from other BBs (e.g., `exampleWebAPI.json` in a non-webAPI BB) will cause validation failures.

2. **Schema prefix must use `http`, not `https`**, with a trailing slash:
   ```yaml
   # CORRECT
   prefixes:
     schema: http://schema.org/

   # WRONG
   prefixes:
     schema: https://schema.org
   ```

## Validation Workflow

A GitHub Actions workflow (`Validate and process Building Blocks`) runs on every push. It uses the `ogc/bblocks/postprocess` Docker container to:

1. Validate all `bblock.json` files have required fields
2. Resolve all `$ref` paths in `schema.yaml` files
3. Fetch resolved references from GitHub Pages URLs
4. Validate examples against their schemas
5. Generate annotated schemas and documentation

If the workflow fails, check the error log for:
- Missing `bblock.json` fields (especially `dateOfLastChange`, `link`)
- 404 errors fetching resolved `$ref` URLs (usually means a `.json` reference instead of `schema.yaml`)
- `FileNotFoundError` for example files (wrong `ref:` in `examples.yaml`)
- Date format errors (must be `YYYY-MM-DD`, not e.g. `2025-11=04`)

## Vocabulary Namespaces

| Prefix | URI | Used In |
|---|---|---|
| `schema` | `http://schema.org/` | Core metadata (name, description, identifier) — all BBs |
| `ada` | `https://ada.astromat.org/metadata/` | ADA-specific types and properties |
| `cdi` | `http://ddialliance.org/Specification/DDI-CDI/1.0/RDF/` | Data structure descriptions |
| `prov` | `http://www.w3.org/ns/prov#` | Provenance (instruments, activities) |
| `nxs` | `http://purl.org/nexusformat/definitions/` | NeXus instrument/source classes |
| `csvw` | `http://www.w3.org/ns/csvw#` | Tabular data descriptions |
| `spdx` | `http://spdx.org/rdf/terms#` | File checksums |
| `dcterms` | `http://purl.org/dc/terms/` | Conformance declarations |
| `dcat` | `http://www.w3.org/ns/dcat#` | Catalog record typing (cdifCatalogRecord) |
| `geosparql` | `http://www.opengis.net/ont/geosparql#` | Spatial geometry types |

## ADA Building Blocks

The ADA (Astromat Data Archive) metadata schema (37 `$defs` from `adaMetadata-SchemaOrgSchema-v3.json`, aligned with CDIF 2026 / DDI-CDI / CSVW) has been decomposed into 18 modular building blocks in `adaProperties/` plus 36 profiles in `profiles/` (adaProduct + 35 technique-specific profiles). 31 of the technique profiles are generated by `tools/generate_profiles.py`.

### Composition Hierarchy

```
profiles/adaProduct/                    ← Top-level ADA product profile (v3, CDIF 2026)
├── schema:creator → @list of person, organization
├── schema:contributor → person, organization, schema:Role
├── schema:funding → funder (organization, @id ref)
├── schema:identifier → identifier (PropertyValue, string, array)
├── schema:license → creativeWork
├── schema:variableMeasured → variable_type (dual: PropertyValue + cdi:InstanceVariable)
│   └── cdi:role, cdi:intendedDataType, cdi:uses, cdi:describedUnitOfMeasure, etc.
├── prov:wasGeneratedBy → array of analysis events
│   ├── @type: allOf [prov:Activity, schema:Event]
│   ├── prov:used → instrument
│   ├── schema:location → laboratory
│   └── schema:mainEntity → (sample inline, identifier as array)
├── schema:distribution → oneOf:
│   ├── Branch 1: single file (files + @type contains DataDownload)
│   │   ├── spdx:checksum → {spdx:algorithm, spdx:checksumValue}
│   │   ├── schema:encodingFormat → array of strings
│   │   └── anyOf [image, imageMap, tabularData, ...] (file-type props at file level)
│   ├── Branch 2: archive (files + DataDownload + schema:hasPart)
│   │   └── schema:hasPart → items (files + NOT DataDownload)
│   │       └── allOf: [common file props] + anyOf [file type BBs]
│   │           ├── image (componentType enum)
│   │           ├── imageMap (componentType: empa_detail or enum)
│   │           ├── supDocImage (componentType enum, incl. ada:other)
│   │           ├── tabularData (TabularTextDataSet + CSVW + oneOf delimited/fixedWidth)
│   │           │   └── cdi:hasPhysicalMapping → physicalMapping (flat per-column)
│   │           ├── collection (componentType enum + filelist + nFiles)
│   │           ├── dataCube (StructuredDataSet + hasPhysicalMapping + locator)
│   │           ├── document (componentType enum + schema:version, schema:isBasedOn)
│   │           └── otherFile (componentType + encodingFormat enum)
│   └── Branch 3: WebAPI (schema:WebAPI)
│       ├── schema:serviceType, schema:documentation, schema:termsOfService
│       └── schema:potentialAction → action (schema:Action)
│           └── schema:result → oneOf [single file, archive] (reuses Branches 1 & 2)
└── schema:subjectOf → metadata record
```

### Technique Profiles

There are 35 technique profiles extending `adaProduct` via `allOf`. Each constrains `schema:additionalType` (human-readable product-type labels only, no `ada:` URIs) and `schema:hasPart` component types. File-type constraints come from the shared `files/schema.yaml` building block via `allOf` composition.

**Original 4** (hand-authored): adaEMPA, adaXRD, adaICPMS, adaVNMIR

**Generated 31** (via `tools/generate_profiles.py`):

| With Detail BB (12) | Without Detail BB (19) |
|---|---|
| adaARGT, adaDSC, adaEAIRMS, adaICPOES, adaL2MS, adaLAF, adaNanoIR, adaNanoSIMS, adaPSFD, adaQRIS, adaSLS, adaXCT | adaAIVA, adaAMS, adaFTICRMS, adaGCMS, adaGPYC, adaIC, adaLCMS, adaLIT, adaNGNSMS, adaRAMAN, adaRITOFNGMS, adaSEM, adaSIMS, adaSVRUEC, adaTEM, adaToFSIMS, adaUVFM, adaVLM, adaXANES |

Example structure (adaEMPA):
```
profiles/adaEMPA/    ← extends adaProduct
├── schema:additionalType contains EMPA product type or "Electron microprobe analysis"
├── schema:distribution hasPart additionalType constrained to:
│   EMPAImageMap, EMPAImage, EMPAQEATabular, EMPAImageCollection,
│   + 22 standard supplement/supporting types (analysisLocation, calibrationFile,
│   methodDescription, instrumentMetadata, contextPhotography, plot, quickLook,
│   supplementaryImage, supplementaryTabular, supplementaryData, etc.)
└── (file-type props from files/schema.yaml: componentType, cdi:isStructuredBy, etc.)
```

TermCode sub-variant mappings: `FIB-SEM` → adaSEM, `VLMBasemap` → adaVLM, `HR-ICP-MS`/`Q-ICP-MS`/`MC-ICP-MS` → adaICPMS.

### Detail Types

The `details/` building block contains 16 instrument-specific detail type definitions as `$defs`. 12 are aligned with v3 source schema; 4 are extensions (marked with *):

```
v3-aligned (12):
  argt_detail, dsc_detail, empa_detail, eairms_detail, l2ms_detail,
  laf_detail, nanoir_detail, nanosims_detail, psfd_detail, vnmir_detail,
  slsshapemodel_detail, xrd_detail

Extensions (4):
  basemap_detail*, icpoes_detail*, qris_detail*, xctimage_detail*
```

Referenced as: `../details/schema.yaml#/$defs/empa_detail`

Key detail types used in technique profiles:
- **empa_detail**: `spectrometersUsed`, `signalUsed` (used in adaEMPA)
- **xrd_detail**: `geometry`, `sampleMount`, `stepSize`, `timePerStep`, `wavelength` (used in adaXRD)
- **vnmir_detail**: 20+ properties including `detector`, `beamsplitter`, `spectralRangeMin/Max`, etc. (used in adaVNMIR)
- ICP-MS has no detail type; component types are enum-only

### Integration with CZ Net Portal

The ADA building blocks define the JSON-LD schema structure. The CZ Net Data Submission Portal (`dspback`) has a JSON-LD translation endpoint (`POST /api/metadata/ada/jsonld`) that accepts JSON-LD conforming to the `adaProduct` profile and translates it to the flat format used by the portal's form schema (`schema.json`). See the [IEDADataSubmission agents.md](https://github.com/smrgeoinfo/IEDADataSubmission/blob/main/agents.md) for translation details.

## DDE Building Blocks

The DDE (Deep-time Digital Earth) metadata schema extends CDIF Discovery with geoscience-specific properties: resource type classification (32 DDE resource types from spec Table 18), topic and acquisition type keywords from DDE controlled vocabularies, browse graphics, and conditional extensions for imagery, services, and geographic datasets. The 7 building blocks in `DDEproperties/` plus 11 profiles in `profiles/DDEProfiles/` (1 base + 10 resource-type-specific) compose on top of the existing CDIF mandatory/optional building blocks.

### Namespace

| Prefix | URI | Used In |
|---|---|---|
| `dde` | `https://www.ddeworld.org/resource/` | All DDE building blocks |

Vocabulary URIs: `dde:codelist/ResourceTypeCode` (32 codes), `dde:codelist/TopicCategoryCode`, `dde:codelist/AcquisitionTypeCode`, `dde:codelist/ServiceTypeCode` (21 codes)

Profile conformance URI: `cdif:profile_ddeCDIF`

### Composition Hierarchy

```
profiles/DDEProfiles/DDEDiscovery/     ← Base DDE profile
├── allOf[0]: cdifMandatory             ← CDIF mandatory (name, identifier, dates, etc.)
│   └── @type: contains pattern         ← Must include ≥1 known type (13 values incl. Event, MediaObject)
├── allOf[1]: cdifOptional              ← CDIF optional (keywords, distribution, spatial, etc.)
├── allOf[2]: ddeRequired               ← DDE mandatory extensions
│   ├── schema:subjectOf → ddeSubject   ← CatalogRecord with dcterms:conformsTo containing cdif:profile_ddeCDIF
│   ├── schema:additionalType → ddeResourceType  ← ≥1 DefinedTerm from dde:codelist/ResourceTypeCode (32 codes)
│   ├── schema:keywords                 ← allOf contains:
│   │   ├── ≥1 DefinedTerm from dde:codelist/TopicCategoryCode
│   │   └── ≥1 DefinedTerm from dde:codelist/AcquisitionTypeCode
│   └── schema:image                    ← ≥1 schema:ImageObject with contentUrl
└── allOf[3]: ddeOptional               ← DDE optional extensions
    ├── schema:alternateName            ← string or array of strings
    ├── schema:measurementTechnique     ← array of string or DefinedTerm
    ├── schema:keywords                 ← additional unconstrained keywords
    └── schema:additionalType           ← additional unconstrained type classifications
```

### Resource Type Profiles (10)

Each profile extends DDEDiscovery and constrains `schema:additionalType` to a subset of the 32-code ResourceTypeCode codelist. Some also compose conditional building blocks or add inline properties:

```
DDEDataset/               ← dataset, dataCatalog, geographicDataset, nonGeographicDataset
DDECollection/            ← aggregate, collection, series, learningResource, guide
│   └── schema:hasPart (required)       ← Collection members (name required)
DDEDocument/              ← document, article, thesis, book, poster, webPage
DDEImage/                 ← image, map, photograph, explanatoryFigure
│   └── allOf: ddeImagery               ← 9 optional MD_Imagery properties
DDEService/               ← repository, service, webAPI
│   └── allOf: ddeServiceInfo           ← serviceType (M), operations, access, datasets, docs
DDESoftware/              ← software
DDEEvent/                 ← initiative, fieldSession
│   └── schema:temporalCoverage (required)
DDEFunctionalResource/    ← application, webApplication, model
│   └── schema:relatedLink contains implementationSoftware
DDESemanticResource/      ← semanticResource, definedTermSet
DDEAudioVisualProduct/    ← movie, sound
    └── schema:duration (optional, ISO 8601)
```

### Conditional Building Blocks

```
DDEproperties/ddeImagery/               ← MD_Imagery (DDE spec Table 3), all optional
├── prov:wasGeneratedBy                 ← Imagery acquisition activity (extends cdifProv)
│   ├── prov:used                       ← Instrument wrappers (schema:instrument sub-key):
│   │   dde:sensorType, dde:platform, dde:equipment, dde:signalGenerator
│   ├── schema:participant              ← DataCollector agent (schema:Role with roleName)
│   ├── schema:startTime                ← ISO 8601 acquisition start (from cdifProv)
│   └── schema:endTime                  ← ISO 8601 acquisition end (from cdifProv)
└── schema:additionalProperty           ← Dataset-level properties:
    dde:wavelength, dde:processedLevel

DDEproperties/ddeServiceInfo/           ← SV_ServiceIdentification (DDE spec Table 2)
├── schema:distribution                 ← contains WebAPI with:
│   ├── schema:serviceType (required)   ← DefinedTerm from dde:codelist/ServiceTypeCode (21 codes)
│   ├── schema:potentialAction          ← containsOperations (optional)
│   ├── schema:termsOfService           ← accessProperties (optional)
│   └── schema:documentation            ← endpointDescription (optional)
├── schema:dataset                      ← operatedDataset (optional)
└── schema:additionalProperty           ← optional service-specific properties

DDEproperties/ddeGeographicDataset/     ← MD_SpatialRepresentation (DDE spec Table 5)
├── schema:spatialCoverage (required)   ← Mandatory spatial extent
└── schema:additionalProperty           ← contains DDE propertyIDs:
    dde:spatialRepresentationType, dde:spatialResolution,
    dde:referenceSystemType, dde:referenceSystemIdentifier
```

### Building Block Details

| Building Block | Description | Required By |
|---|---|---|
| `ddeSubject` | CatalogRecord extension requiring `dcterms:conformsTo` to contain `cdif:profile_ddeCDIF`. Follows xasSubject pattern — extends cdifCatalogRecord with minContains constraint. | ddeRequired |
| `ddeResourceType` | Constrains `schema:additionalType` to require ≥1 DefinedTerm with `schema:inDefinedTermSet: "dde:codelist/ResourceTypeCode"` and `schema:termCode` from enum of 32 resource types (DDE spec Table 18). | ddeRequired |
| `ddeRequired` | DDE mandatory property group. Uses allOf with $ref to cdifMandatory, adds ddeSubject, ddeResourceType, DDE vocabulary-constrained keywords (TopicCategoryCode + AcquisitionTypeCode), and browse graphics (schema:image). | DDEDiscovery |
| `ddeOptional` | DDE optional properties: alternateName, measurementTechnique, additional unconstrained keywords and additionalType. | DDEDiscovery |
| `ddeImagery` | Conditional extension for imagery resources (MD_Imagery, DDE spec Table 3). Uses cdifProv provenance pattern: sensor, platform, equipment, signalGenerator mapped as instruments under `prov:wasGeneratedBy` activity; collector as `schema:participant` agent; startTime/endTime as activity temporal bounds. Wavelength and processedLevel remain as dataset-level `schema:additionalProperty`. All properties optional. | DDEImage |
| `ddeServiceInfo` | Conditional extension for service resources (SV_ServiceIdentification, DDE spec Table 2). serviceType mandatory (21 codes), plus containsOperations, accessProperties, operatedDataset, endpointDescription. | DDEService |
| `ddeGeographicDataset` | Conditional extension for geographic dataset resources (MD_SpatialRepresentation, DDE spec Table 5). Mandatory spatialCoverage, plus optional spatialRepresentationType, spatialResolution, referenceSystemType, referenceSystemIdentifier. | DDEDataset (sub-profile) |

### DDE Profile Summary

| Profile | Resource Type Codes | Type-Specific BB | Extra Constraints |
|---|---|---|---|
| DDEDataset | dataset, dataCatalog, geographicDataset, nonGeographicDataset | ddeGeographicDataset (sub-profile) | — |
| DDECollection | aggregate, collection, series, learningResource, guide | — | `schema:hasPart` required |
| DDEDocument | document, article, thesis, book, poster, webPage | — | — |
| DDEImage | image, map, photograph, explanatoryFigure | ddeImagery | — |
| DDEService | repository, service, webAPI | ddeServiceInfo | — |
| DDESoftware | software | — | — |
| DDEEvent | initiative, fieldSession | — | `schema:temporalCoverage` required |
| DDEFunctionalResource | application, webApplication, model | — | `schema:relatedLink` with implementationSoftware |
| DDESemanticResource | semanticResource, definedTermSet | — | — |
| DDEAudioVisualProduct | movie, sound | — | `schema:duration` (optional) |

### DDE Resource Type Codes (32)

aggregate, application, webApplication, collection, dataset, dataCatalog, geographicDataset, nonGeographicDataset, document, article, thesis, book, poster, webPage, image, map, photograph, explanatoryFigure, initiative, fieldSession, learningResource, guide, model, movie, repository, semanticResource, definedTermSet, series, service, webAPI, software, sound

### DDE Service Type Codes (21)

OGC:WMS, OGC:WFS, OGC:WCS, OGC:WMTS, OGC:WPS, OGC:CSW, OGC:SOS, OGC:SensorThings, OGC:API-Features, OGC:API-Records, OGC:API-Maps, OGC:API-Tiles, OGC:API-Coverages, OGC:API-Processes, OGC:API-EDR, ESRI:MapServer, ESRI:FeatureServer, ESRI:ImageServer, SPARQL, OpenDAP, THREDDS

## ECRR Building Blocks

The ECRR (EarthCube Resource Registry) metadata schema defines 10 resource types from the ECRRO ontology. There are 10 property building blocks in `ecrrProperties/` and 11 profiles in `profiles/ecrrProfiles/` (one per resource type plus ECRRDataset which has a unique composition pattern).

### Namespace

| Prefix | URI | Used In |
|---|---|---|
| `ecrro` | `http://cor.esipfed.org/ont/earthcube/` | ECRR assessment properties (maturity, lifetime, usage, stewardship) |
| `ecrr` | `https://n2t.net/ark:/23942/g2` | ECRR resource identifiers |
| `dct` | `http://purl.org/dc/terms/` | Bibliographic citation |

### Building Block Structure

| Building Block | Description |
|---|---|
| `ecrrBase` | Mandatory ECRR identity: `mainEntity` classification, `@type` must contain `schema:CreativeWork`, requires `schema:description` and `schema:license` |
| `ecrrCommon` | Common properties shared across most resource types: creator, keywords, distribution, contributor, publisher, funding, sameAs, version, datePublished, url, subjectOf, isRelatedTo, isBasedOn, etc. |
| `ecrrAssessment` | Resource assessment: maturity state, expected lifetime, usage level, stewardship, primary publication, registration metadata |
| `ecrrCatalog` | Catalog/repository-specific properties |
| `ecrrCollection` | Bundled object/collection-specific properties |
| `ecrrDataset` | Dataset-specific properties (legacy BB; ECRRDataset profile now uses CDIFcomplete instead) |
| `ecrrSemanticResource` | Semantic resource-specific properties |
| `ecrrService` | Service instance-specific properties |
| `ecrrSoftware` | Software-specific properties |
| `ecrrSpecification` | Specification-specific properties |

### Profile Compositions

All 11 ECRR profiles constrain `schema:additionalType` with a `contains` enum matching ECRRO ontology `rdfs:label` values. Two composition patterns are used:

**Pattern A: ECRRDataset** (extends CDIFcomplete — unique composition):
```yaml
allOf:
  - $ref: CDIFcomplete/schema.yaml       # Full CDIF discovery + data description
  - $ref: ecrrBase/schema.yaml            # ECRR identity
  - $ref: ecrrAssessment/schema.yaml      # Resource assessment
  - properties:                            # Inline ECRR-unique properties only
      schema:additionalType: { contains: { enum: ["EC Dataset"] } }
      schema:alternateName, schema:editor, schema:about, schema:audience,
      dct:bibliographicCitation
```
ECRRDataset does NOT include `ecrrCommon` — CDIF definitions take precedence for overlapping properties (creator, keywords, subjectOf, distribution, etc.). Only ECRR-unique properties (not in CDIF) are included inline.

**Pattern B: All other profiles** (standard ECRR composition):
```yaml
allOf:
  - $ref: ecrrBase/schema.yaml
  - $ref: ecrrCommon/schema.yaml
  - $ref: ecrrAssessment/schema.yaml
  - $ref: ecrrXXX/schema.yaml             # Type-specific BB (if one exists)
  - properties:
      schema:additionalType: { contains: { enum: ["EC Xxx"] } }
```

### Profile Summary

| Profile | additionalType | Type-Specific BB | Composition |
|---|---|---|---|
| ECRRDataset | `"EC Dataset"` | — (uses CDIFcomplete) | Pattern A |
| ECRRService | `"EC Service Instance"` | ecrrService | Pattern B |
| ECRRSoftware | `"EC Software"` | ecrrSoftware | Pattern B |
| ECRRSpecification | `"EC Specification"` | ecrrSpecification | Pattern B |
| ECRRCatalog | `"EC CatalogRegistry"`, `"EC Repository"` | ecrrCatalog | Pattern B |
| ECRRCollection | `"EC Bundled Object"` | ecrrCollection | Pattern B |
| ECRRSemanticResource | `"EC Semantic Resource"` | ecrrSemanticResource | Pattern B |
| ECRRUseCase | `"EC Use Case"` | — (base-only) | Pattern B |
| ECRRInterface | `"EC Interface_API"` | — (base-only) | Pattern B |
| ECRRInterchangeFormat | `"EC Interchange Format"` | — (base-only) | Pattern B |
| ECRRPlatform | `"EC Platform"` | — (base-only) | Pattern B |

### ECRRDataset: ECRR→CDIF Property Mappings

ECRR properties that have CDIF equivalents are NOT included in the ECRRDataset profile. Instead, the CDIF definitions from CDIFcomplete are used. Mapping guidance:

| ECRR property | CDIF equivalent | Notes |
|---|---|---|
| `schema:subjectOf` (other metadata) | `schema:relatedLink` with linkRelationship `"OtherMetadataFormat"` | CDIF reserves `schema:subjectOf` for CdifCatalogRecord |
| `schema:isRelatedTo` | `schema:relatedLink` | CDIF uses relatedLink with linkRelationship for typed relations |
| `schema:isBasedOn` | `prov:wasDerivedFrom` | CDIF uses PROV-O for derivation lineage |
| `schema:includedInDataCatalog` | `schema:subjectOf` → CdifCatalogRecord `schema:includedInDataCatalog` | Handled within the CdifCatalogRecord node |
| `schema:encodingFormat` | `schema:distribution` → DataDownload `schema:encodingFormat` | Property of distribution, not the dataset |
| `dct:conformsTo` | `schema:distribution` → DataDownload `dcterms:conformsTo` | Property of distribution or Action result |

### ECRR-Unique Properties (in ECRRDataset only)

Properties from ecrrCommon that have no CDIF equivalent:

- `schema:about` — science domains (ECRR ADO vocabulary, array of DefinedTerm)
- `schema:audience` — target user communities (ECRR AUT vocabulary)
- `schema:editor` — editors (Person/Organization/`@id` ref)
- `schema:alternateName` — alternative names (string or array)
- `dct:bibliographicCitation` — preferred citation (PropertyValue wrapper)

---

# Schema Tools

## Schema Pipeline

Three tools transform modular YAML source schemas into JSON Forms-compatible Draft 7 schemas and augment the bblocks-viewer register:

```
schema.yaml → resolve_schema.py → resolvedSchema.json → convert_for_jsonforms.py → schema.json
                                                       → augment_register.py → register.json (adds resolvedSchema URLs)
```

## resolve_schema.py

Recursively resolves ALL `$ref` references from modular YAML/JSON source schemas into one fully-inlined JSON Schema. No external references remain in the output — all `$defs` are inlined and removed.

**$ref patterns handled:**
1. Relative path: `$ref: ../detailEMPA/schema.yaml`
2. Fragment-only: `$ref: '#/$defs/Identifier'`
3. Cross-file fragment: `$ref: ../cdifCatalogRecord/schema.yaml#/$defs/conformsTo_item`
4. Both YAML and JSON file extensions

**Usage:**
```bash
# Resolve a profile by name (searches _sources/profiles/{adaProfiles,cdifProfiles}/{name}/)
python tools/resolve_schema.py adaProduct
python tools/resolve_schema.py adaEMPA --flatten-allof -o _sources/profiles/adaProfiles/adaEMPA/resolvedSchema.json
python tools/resolve_schema.py DDEDiscovery --flatten-allof

# Resolve an arbitrary schema file
python tools/resolve_schema.py --file path/to/any/schema.yaml

# Resolve all profiles (list from generate_profiles.py + manual ones)
for p in adaProduct adaEMPA adaICPMS adaVNMIR adaXRD CDIFDiscovery \
         $(python tools/generate_profiles.py --list | awk '{print $1}'); do
  python tools/resolve_schema.py $p --flatten-allof
done
```

**CLI options:** `profile` (positional, profile name), `--file` (arbitrary schema path), `--all` (resolve all schemas with external refs), `-o`/`--output` (output file, default: stdout; ignored with --all), `--flatten-allof` (merge allOf entries into single objects).

**Requirements:** Python 3.6+ with `pyyaml`

**Key implementation details:**
- `deep_merge` with `_is_complete_schema` heuristic: when merging `properties` dicts, overlay properties with `type`/`oneOf`/`anyOf`/`allOf`/`$ref` **replace** the base entirely; partial constraint patches (no composition keywords) are deep-merged
- Two-pass `$defs` resolution: pass 1 resolves external file refs with empty defs dict, pass 2 uses `_inline_unresolved_defs` to replace `$comment` placeholders left by forward cross-def fragment refs
- Circular reference detection via `seen` set (returns `$comment` placeholder)
- Strips metadata keys (`$id`, `x-jsonld-*`) from output

## convert_for_jsonforms.py

Reads `resolvedSchema.json` (from `_sources/profiles/{adaProfiles,cdifProfiles}/{name}/`) and converts to JSON Forms-compatible Draft 7:
- Converts `$schema` from Draft 2020-12 to Draft 7
- Simplifies `anyOf` patterns for form rendering (single-item anyOf unwrapped, duplicate removal)
- Converts `contains` → `enum`, `const` → `default`
- Merges technique profile constraints into distribution `oneOf` branches
- Preserves `oneOf` in distribution (3 branches: single file, archive, WebAPI)
- Merges file-type `anyOf` (from `files/schema.yaml`) into flat hasPart item properties
- Removes `not` constraints and relaxes `minItems`

**Usage:**
```bash
python tools/convert_for_jsonforms.py adaProduct -v
python tools/convert_for_jsonforms.py --all -v
```

**Output:** `build/jsonforms/profiles/{adaProfiles,cdifProfiles}/{name}/schema.json`

## generate_profiles.py

Data-driven generator for ADA technique profile building blocks. A single `PROFILES` dict configures all 31 generated profiles (product types, component types, human-readable labels, file type refs, detail building block references, tags).

**Usage:**
```bash
# Generate all profiles
python tools/generate_profiles.py

# Generate a single profile
python tools/generate_profiles.py adaSEM

# List all available profiles with termcodes and detail info
python tools/generate_profiles.py --list
```

**Output per profile:** `_sources/profiles/adaProfiles/{name}/` with `schema.yaml`, `bblock.json`, `context.jsonld`, `description.md`, `examples.yaml`.

**After generating**, resolve schemas and validate:
```bash
# Resolve all generated profiles
python tools/resolve_schema.py --all

# Validate test metadata
python tools/validate_instance.py --dir /path/to/testJSONMetadata --termcode-fallback --summary
```

**Key design decisions:**
- `additional_type_labels` — each profile's `contains` enum includes human-readable product-type names (from the Products worksheet) and the technique label without abbreviation; no `ada:` URIs
- File type refs are auto-detected from component types using sets that mirror the `adaProperties/*/schema.yaml` componentType enums
- 22 standard supplement/supporting types (analysisLocation, contextPhotography, calibrationFile, instrumentMetadata, methodDescription, plot, quickLook, supplementaryImage, supplementaryTabular, supplementaryData, supplementaryDocument, supplementaryPresentation, supplementarySpreadsheet, supplementaryVideo, supplementaryAudio, supplementaryArchive, supplementaryCode, supplementaryNotebook, supplementaryModel, supplementaryDatabase, supplementaryOther, supplementaryCollection) are added to every profile's hasPart enum

## augment_register.py

Adds `resolvedSchema` URLs to `build/register.json` for each profile building block. Scans bblock identifiers for `.profiles.{name}` patterns and checks whether `_sources/profiles/{adaProfiles,cdifProfiles}/{name}/resolvedSchema.json` exists. If so, adds the GitHub Pages URL as `bblock.resolvedSchema`.

**Usage:**
```bash
python tools/augment_register.py
```

**Why:** The bblocks-viewer fork has a "Resolved (JSON)" button in the JSON Schema tab that fetches the resolved schema from this URL. The OGC postprocessor doesn't know about `resolvedSchema.json`, so this script injects the URLs after the postprocessor generates `register.json`.

**Workflow integration:** The `generate-jsonforms` workflow runs this after `convert_for_jsonforms.py` and stages `build/register.json` alongside `build/jsonforms/`. It is also run by `deploy-viewer.yml` before the Pages upload (see below).

## deploy-viewer.yml Workflow

The OGC postprocessor's reusable workflow deploys GitHub Pages with the upstream `ogcincubator/bblocks-viewer` and generates `config.js` in-memory (never committed). This means the deployed site uses the upstream viewer (which lacks the "Resolved (JSON)" button) and `register.json` without `resolvedSchema` URLs.

`deploy-viewer.yml` re-deploys Pages after the postprocessor, fixing both issues:

1. **Runs `augment_register.py`** — injects `resolvedSchema` URLs into `build/register.json`
2. **Generates `config.js`** — points `window.bblocksRegister` to the local register and sets `baseUrl` for SPA routing
3. **Generates `index.html`** — loads JS/CSS assets from `smrgeoinfo.github.io/bblocks-viewer/` (the fork) instead of the upstream viewer

**Trigger:** Runs after "Validate and process Building Blocks" completes successfully, or via `workflow_dispatch`.

**Workflow chain on push:**
```
push → "Validate and process Building Blocks" (OGC postprocessor)
         ├──→ "Generate JSON Forms schemas" (convert + augment + commit)
         └──→ "Deploy custom bblocks-viewer" (augment + config.js + index.html → Pages)
```

**Key detail:** Both `generate-jsonforms` and `deploy-viewer` run `augment_register.py` independently. `generate-jsonforms` commits the augmented `register.json` to the repo (for future runs). `deploy-viewer` augments the checked-out copy before uploading to Pages (because it can't wait for the other workflow's commit).

**bblocks-viewer fork:** `smrgeoinfo/bblocks-viewer` (forked from `ogcincubator/bblocks-viewer`). The fork's `gh-deploy.yml` workflow builds the Vue app and deploys to `smrgeoinfo.github.io/bblocks-viewer/`. The fork adds the "Resolved (JSON)" button to `JsonSchemaViewer.vue` and `resolvedSchema` to `COPY_PROPERTIES` in `bblock.service.js`.

## Verification

```bash
# Verify distribution has oneOf only (no conflicting anyOf)
python -c "
import json
with open('_sources/profiles/adaProfiles/adaProduct/resolvedSchema.json') as f:
    s = json.load(f)
items = s['properties']['schema:distribution']['items']
assert 'oneOf' in items, 'Missing oneOf'
assert 'anyOf' not in items, 'Conflicting anyOf still present'
print(f'Distribution OK: {len(items[\"oneOf\"])} branches')
"
```

## License

This material is based upon work supported by the National Science Foundation (NSF) under awards 2012893, 2012748, and 2012593.
