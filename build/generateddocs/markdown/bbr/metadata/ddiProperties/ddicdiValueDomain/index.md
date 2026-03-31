
# DDI-CDI Value Domain (Schema)

`cdif.bbr.metadata.ddiProperties.ddicdiValueDomain` *v0.1*

DDI-CDI ValueDomain building block for CDIF metadata, covering both SubstantiveValueDomain (subject-matter values) and SentinelValueDomain (processing/missing-value codes). Inherits from ValueDomain. Defines properties: @type, @id, cdi:catalogDetails, cdi:displayLabel, cdi:identifier, cdi:recommendedDataType, cdi:isDescribedBy, cdi:takesValuesFrom, cdi:takesConceptsFrom, cdi:platformType (sentinel only).

[*Status*](http://www.opengis.net/def/status): Under development

## Examples

### Example DDI-CDI value domains (substantive and sentinel).
Two value domains for a height variable: a SubstantiveValueDomain
with a described numeric domain (0-3 meters, Ratio level), and a
SentinelValueDomain with missing value codes for the SAS platform.
#### json
```json
{
    "@context": {
        "cdi": "http://ddialliance.org/Specification/DDI-CDI/1.0/RDF/"
    },
    "@graph": [
        {
            "@type": ["cdi:SubstantiveValueDomain"],
            "@id": "urn:example:svd:height-meters",
            "cdi:identifier": {
                "@type": ["cdi:Identifier"],
                "cdi:uri": "urn:example:svd:height-meters"
            },
            "cdi:displayLabel": {
                "@type": ["cdi:LabelForDisplay"],
                "cdi:languageSpecificString": {
                    "@type": ["cdi:LanguageString"],
                    "cdi:content": "Height in meters",
                    "cdi:language": "en"
                }
            },
            "cdi:recommendedDataType": {
                "@type": ["cdi:ControlledVocabularyEntry"],
                "cdi:entryValue": "xsd:decimal",
                "cdi:vocabulary": {
                    "@type": ["cdi:Reference"],
                    "cdi:uri": "http://www.w3.org/2001/XMLSchema#",
                    "cdi:description": "W3C XML Schema Definition Language datatypes"
                }
            },
            "cdi:isDescribedBy": {
                "@type": ["cdi:ValueAndConceptDescription"],
                "@id": "urn:example:vcd:height-description",
                "cdi:classificationLevel": "Ratio",
                "cdi:description": {
                    "@type": ["cdi:InternationalString"],
                    "cdi:languageSpecificString": {
                        "@type": ["cdi:LanguageString"],
                        "cdi:content": "Real number between 0 and 3 representing height in meters",
                        "cdi:language": "en"
                    }
                },
                "cdi:minimumValueInclusive": "0",
                "cdi:maximumValueInclusive": "3"
            }
        },
        {
            "@type": ["cdi:SentinelValueDomain"],
            "@id": "urn:example:sentinel:height-missing-sas",
            "cdi:identifier": {
                "@type": ["cdi:Identifier"],
                "cdi:uri": "urn:example:sentinel:height-missing-sas"
            },
            "cdi:displayLabel": {
                "@type": ["cdi:LabelForDisplay"],
                "cdi:languageSpecificString": {
                    "@type": ["cdi:LanguageString"],
                    "cdi:content": "Height missing value codes (SAS)",
                    "cdi:language": "en"
                }
            },
            "cdi:platformType": {
                "@type": ["cdi:ControlledVocabularyEntry"],
                "cdi:entryValue": "SASNumeric",
                "cdi:name": "SAS Numeric Missing Values",
                "cdi:vocabulary": {
                    "@type": ["cdi:Reference"],
                    "cdi:uri": "urn:ddi-cdi:cv:platformType",
                    "cdi:description": "DDI-CDI platform type vocabulary"
                }
            },
            "cdi:isDescribedBy": {
                "@type": ["cdi:ValueAndConceptDescription"],
                "@id": "urn:example:vcd:height-sentinel-description",
                "cdi:classificationLevel": "Nominal",
                "cdi:description": {
                    "@type": ["cdi:InternationalString"],
                    "cdi:languageSpecificString": {
                        "@type": ["cdi:LanguageString"],
                        "cdi:content": "SAS missing value codes: . (system missing), .R (refused), .N (not recorded)",
                        "cdi:language": "en"
                    }
                },
                "cdi:regularExpression": {
                    "@type": ["cdi:TypedString"],
                    "cdi:content": "^\\.[RN_]?$",
                    "cdi:typeOfContent": {
                        "@type": ["cdi:ControlledVocabularyEntry"],
                        "cdi:entryValue": "PCRE"
                    }
                }
            },
            "cdi:takesConceptsFrom": {
                "@id": "urn:example:sentinel-conceptual-domain:missing-reasons"
            }
        }
    ]
}

```

#### jsonld
```jsonld
{
  "@context": [
    {
      "cdi": "http://ddialliance.org/Specification/DDI-CDI/1.0/RDF/"
    },
    "https://cross-domain-interoperability-framework.github.io/metadataBuildingBlocks/build/annotated/bbr/metadata/ddiProperties/ddicdiValueDomain/context.jsonld",
    {
      "cdi": "http://ddialliance.org/Specification/DDI-CDI/1.0/RDF/"
    }
  ],
  "@graph": [
    {
      "@type": [
        "cdi:SubstantiveValueDomain"
      ],
      "@id": "urn:example:svd:height-meters",
      "cdi:identifier": {
        "@type": [
          "cdi:Identifier"
        ],
        "cdi:uri": "urn:example:svd:height-meters"
      },
      "cdi:displayLabel": {
        "@type": [
          "cdi:LabelForDisplay"
        ],
        "cdi:languageSpecificString": {
          "@type": [
            "cdi:LanguageString"
          ],
          "cdi:content": "Height in meters",
          "cdi:language": "en"
        }
      },
      "cdi:recommendedDataType": {
        "@type": [
          "cdi:ControlledVocabularyEntry"
        ],
        "cdi:entryValue": "xsd:decimal",
        "cdi:vocabulary": {
          "@type": [
            "cdi:Reference"
          ],
          "cdi:uri": "http://www.w3.org/2001/XMLSchema#",
          "cdi:description": "W3C XML Schema Definition Language datatypes"
        }
      },
      "cdi:isDescribedBy": {
        "@type": [
          "cdi:ValueAndConceptDescription"
        ],
        "@id": "urn:example:vcd:height-description",
        "cdi:classificationLevel": "Ratio",
        "cdi:description": {
          "@type": [
            "cdi:InternationalString"
          ],
          "cdi:languageSpecificString": {
            "@type": [
              "cdi:LanguageString"
            ],
            "cdi:content": "Real number between 0 and 3 representing height in meters",
            "cdi:language": "en"
          }
        },
        "cdi:minimumValueInclusive": "0",
        "cdi:maximumValueInclusive": "3"
      }
    },
    {
      "@type": [
        "cdi:SentinelValueDomain"
      ],
      "@id": "urn:example:sentinel:height-missing-sas",
      "cdi:identifier": {
        "@type": [
          "cdi:Identifier"
        ],
        "cdi:uri": "urn:example:sentinel:height-missing-sas"
      },
      "cdi:displayLabel": {
        "@type": [
          "cdi:LabelForDisplay"
        ],
        "cdi:languageSpecificString": {
          "@type": [
            "cdi:LanguageString"
          ],
          "cdi:content": "Height missing value codes (SAS)",
          "cdi:language": "en"
        }
      },
      "cdi:platformType": {
        "@type": [
          "cdi:ControlledVocabularyEntry"
        ],
        "cdi:entryValue": "SASNumeric",
        "cdi:name": "SAS Numeric Missing Values",
        "cdi:vocabulary": {
          "@type": [
            "cdi:Reference"
          ],
          "cdi:uri": "urn:ddi-cdi:cv:platformType",
          "cdi:description": "DDI-CDI platform type vocabulary"
        }
      },
      "cdi:isDescribedBy": {
        "@type": [
          "cdi:ValueAndConceptDescription"
        ],
        "@id": "urn:example:vcd:height-sentinel-description",
        "cdi:classificationLevel": "Nominal",
        "cdi:description": {
          "@type": [
            "cdi:InternationalString"
          ],
          "cdi:languageSpecificString": {
            "@type": [
              "cdi:LanguageString"
            ],
            "cdi:content": "SAS missing value codes: . (system missing), .R (refused), .N (not recorded)",
            "cdi:language": "en"
          }
        },
        "cdi:regularExpression": {
          "@type": [
            "cdi:TypedString"
          ],
          "cdi:content": "^\\.[RN_]?$",
          "cdi:typeOfContent": {
            "@type": [
              "cdi:ControlledVocabularyEntry"
            ],
            "cdi:entryValue": "PCRE"
          }
        }
      },
      "cdi:takesConceptsFrom": {
        "@id": "urn:example:sentinel-conceptual-domain:missing-reasons"
      }
    }
  ]
}
```

#### ttl
```ttl
@prefix cdi: <http://ddialliance.org/Specification/DDI-CDI/1.0/RDF/> .

<urn:example:sentinel:height-missing-sas> a cdi:SentinelValueDomain ;
    cdi:displayLabel [ a cdi:LabelForDisplay ;
            cdi:languageSpecificString [ a cdi:LanguageString ;
                    cdi:content "Height missing value codes (SAS)" ;
                    cdi:language "en" ] ] ;
    cdi:identifier [ a cdi:Identifier ;
            cdi:uri "urn:example:sentinel:height-missing-sas" ] ;
    cdi:isDescribedBy <urn:example:vcd:height-sentinel-description> ;
    cdi:platformType [ a cdi:ControlledVocabularyEntry ;
            cdi:entryValue "SASNumeric" ;
            cdi:name "SAS Numeric Missing Values" ;
            cdi:vocabulary [ a cdi:Reference ;
                    cdi:description "DDI-CDI platform type vocabulary" ;
                    cdi:uri "urn:ddi-cdi:cv:platformType" ] ] ;
    cdi:takesConceptsFrom <urn:example:sentinel-conceptual-domain:missing-reasons> .

<urn:example:svd:height-meters> a cdi:SubstantiveValueDomain ;
    cdi:displayLabel [ a cdi:LabelForDisplay ;
            cdi:languageSpecificString [ a cdi:LanguageString ;
                    cdi:content "Height in meters" ;
                    cdi:language "en" ] ] ;
    cdi:identifier [ a cdi:Identifier ;
            cdi:uri "urn:example:svd:height-meters" ] ;
    cdi:isDescribedBy <urn:example:vcd:height-description> ;
    cdi:recommendedDataType [ a cdi:ControlledVocabularyEntry ;
            cdi:entryValue "xsd:decimal" ;
            cdi:vocabulary [ a cdi:Reference ;
                    cdi:description "W3C XML Schema Definition Language datatypes" ;
                    cdi:uri "http://www.w3.org/2001/XMLSchema#" ] ] .

<urn:example:vcd:height-description> a cdi:ValueAndConceptDescription ;
    cdi:classificationLevel "Ratio" ;
    cdi:description [ a cdi:InternationalString ;
            cdi:languageSpecificString [ a cdi:LanguageString ;
                    cdi:content "Real number between 0 and 3 representing height in meters" ;
                    cdi:language "en" ] ] ;
    cdi:maximumValueInclusive "3" ;
    cdi:minimumValueInclusive "0" .

<urn:example:vcd:height-sentinel-description> a cdi:ValueAndConceptDescription ;
    cdi:classificationLevel "Nominal" ;
    cdi:description [ a cdi:InternationalString ;
            cdi:languageSpecificString [ a cdi:LanguageString ;
                    cdi:content "SAS missing value codes: . (system missing), .R (refused), .N (not recorded)" ;
                    cdi:language "en" ] ] ;
    cdi:regularExpression [ a cdi:TypedString ;
            cdi:content "^\\.[RN_]?$" ;
            cdi:typeOfContent [ a cdi:ControlledVocabularyEntry ;
                    cdi:entryValue "PCRE" ] ] .


```

## Schema

```yaml
$schema: https://json-schema.org/draft/2020-12/schema
title: DDI-CDI Value Domain
description: Unified DDI-CDI ValueDomain building block covering both SubstantiveValueDomain
  (subject-matter values) and SentinelValueDomain (processing/missing-value codes).
  Both inherit from ValueDomain. SentinelValueDomain adds an optional cdi:platformType
  property. Supports described domains (ValueAndConceptDescription), enumerated domains
  (EnumerationDomain), and conceptual domain references.
anyOf:
- description: Single ValueDomain node
  $ref: '#/$defs/ValueDomainNode'
- description: Unwrapped @graph array of nodes
  type: array
  items:
    $ref: '#/$defs/ValueDomainNode'
- description: JSON-LD document with @context and @graph
  type: object
  properties:
    '@context': {}
    '@graph':
      type: array
      items:
        $ref: '#/$defs/ValueDomainNode'
  required:
  - '@graph'
$defs:
  ValueDomainNode:
    type: object
    description: A single ValueDomain node (SubstantiveValueDomain or SentinelValueDomain)
    properties:
      '@type':
        description: Must include either cdi:SubstantiveValueDomain or cdi:SentinelValueDomain
        type: array
        items:
          type: string
        anyOf:
        - contains:
            const: cdi:SubstantiveValueDomain
        - contains:
            const: cdi:SentinelValueDomain
        minItems: 1
      '@id':
        type: string
        description: Identifier for this value domain node
      cdi:catalogDetails:
        description: Administrative catalog metadata for this value domain
        $ref: https://cross-domain-interoperability-framework.github.io/metadataBuildingBlocks/build/annotated/bbr/metadata/ddiProperties/ddicdiDataTypes/schema.yaml#/$defs/CatalogDetails
      cdi:displayLabel:
        description: Multilingual display label
        anyOf:
        - $ref: https://cross-domain-interoperability-framework.github.io/metadataBuildingBlocks/build/annotated/bbr/metadata/ddiProperties/ddicdiDataTypes/schema.yaml#/$defs/LabelForDisplay
        - type: array
          items:
            $ref: https://cross-domain-interoperability-framework.github.io/metadataBuildingBlocks/build/annotated/bbr/metadata/ddiProperties/ddicdiDataTypes/schema.yaml#/$defs/LabelForDisplay
      cdi:identifier:
        description: Formal identifier for this value domain
        $ref: https://cross-domain-interoperability-framework.github.io/metadataBuildingBlocks/build/annotated/bbr/metadata/ddiProperties/ddicdiDataTypes/schema.yaml#/$defs/Identifier
      cdi:recommendedDataType:
        description: Recommended physical data type for values in this domain
        $ref: https://cross-domain-interoperability-framework.github.io/metadataBuildingBlocks/build/annotated/bbr/metadata/ddiProperties/ddicdiDataTypes/schema.yaml#/$defs/ControlledVocabularyEntry
      cdi:isDescribedBy:
        description: Description of the value domain constraints (0..1). Inline ValueAndConceptDescription
          or @id reference.
        anyOf:
        - $ref: '#/$defs/ValueAndConceptDescription'
        - $ref: https://cross-domain-interoperability-framework.github.io/metadataBuildingBlocks/build/annotated/bbr/metadata/ddiProperties/ddicdiDataTypes/schema.yaml#/$defs/id-reference
      cdi:takesValuesFrom:
        description: Enumeration domain providing the set of permissible coded values
          (0..1). Inline EnumerationDomain or @id reference.
        anyOf:
        - $ref: '#/$defs/EnumerationDomain'
        - $ref: https://cross-domain-interoperability-framework.github.io/metadataBuildingBlocks/build/annotated/bbr/metadata/ddiProperties/ddicdiDataTypes/schema.yaml#/$defs/id-reference
      cdi:takesConceptsFrom:
        description: Reference to a conceptual domain (SubstantiveConceptualDomain
          or SentinelConceptualDomain) that provides conceptual grounding for this
          value domain (0..1). @id reference only.
        $ref: https://cross-domain-interoperability-framework.github.io/metadataBuildingBlocks/build/annotated/bbr/metadata/ddiProperties/ddicdiDataTypes/schema.yaml#/$defs/id-reference
      cdi:platformType:
        description: '(SentinelValueDomain only) The type of software platform under
          which sentinel codes will be used. Statistical software platforms use different
          sets of codes to indicate missing values. Example values: SASNumeric, SPSSstyle,
          Rstyle, StataNumeric, BlankString, EmptyString, Unrestricted.'
        $ref: https://cross-domain-interoperability-framework.github.io/metadataBuildingBlocks/build/annotated/bbr/metadata/ddiProperties/ddicdiDataTypes/schema.yaml#/$defs/ControlledVocabularyEntry
    required:
    - '@type'
  ValueAndConceptDescription:
    type: object
    description: DDI-CDI description of value constraints and measurement level (cls-ValueAndConceptDescription)
    properties:
      '@type':
        type: array
        items:
          type: string
        contains:
          const: cdi:ValueAndConceptDescription
        minItems: 1
      '@id':
        type: string
        description: Identifier for this description node
      cdi:classificationLevel:
        type: string
        description: Level of measurement
        enum:
        - Continuous
        - Interval
        - Nominal
        - Ordinal
        - Ratio
      cdi:description:
        description: Textual description of the value domain constraints
        $ref: https://cross-domain-interoperability-framework.github.io/metadataBuildingBlocks/build/annotated/bbr/metadata/ddiProperties/ddicdiDataTypes/schema.yaml#/$defs/InternationalString
      cdi:formatPattern:
        description: Expected format pattern for values
        $ref: https://cross-domain-interoperability-framework.github.io/metadataBuildingBlocks/build/annotated/bbr/metadata/ddiProperties/ddicdiDataTypes/schema.yaml#/$defs/ControlledVocabularyEntry
      cdi:identifier:
        description: Formal identifier
        $ref: https://cross-domain-interoperability-framework.github.io/metadataBuildingBlocks/build/annotated/bbr/metadata/ddiProperties/ddicdiDataTypes/schema.yaml#/$defs/Identifier
      cdi:logicalExpression:
        description: Logical expression constraining valid values
        $ref: https://cross-domain-interoperability-framework.github.io/metadataBuildingBlocks/build/annotated/bbr/metadata/ddiProperties/ddicdiDataTypes/schema.yaml#/$defs/ControlledVocabularyEntry
      cdi:maximumValueExclusive:
        type: string
        description: Exclusive upper bound for valid values
      cdi:maximumValueInclusive:
        type: string
        description: Inclusive upper bound for valid values
      cdi:minimumValueExclusive:
        type: string
        description: Exclusive lower bound for valid values
      cdi:minimumValueInclusive:
        type: string
        description: Inclusive lower bound for valid values
      cdi:regularExpression:
        description: Regular expression constraining valid value syntax
        $ref: '#/$defs/TypedString'
  TypedString:
    type: object
    description: DDI-CDI typed string value (dt-TypedString)
    properties:
      '@type':
        type: array
        items:
          type: string
        contains:
          const: cdi:TypedString
        minItems: 1
      cdi:content:
        type: string
        description: The string content
      cdi:typeOfContent:
        description: Type or format of the content
        $ref: https://cross-domain-interoperability-framework.github.io/metadataBuildingBlocks/build/annotated/bbr/metadata/ddiProperties/ddicdiDataTypes/schema.yaml#/$defs/ControlledVocabularyEntry
  EnumerationDomain:
    type: object
    description: DDI-CDI enumeration domain providing a set of permissible coded values
      (cls-EnumerationDomain)
    properties:
      '@type':
        type: array
        items:
          type: string
        contains:
          const: cdi:EnumerationDomain
        minItems: 1
      '@id':
        type: string
        description: Identifier for this enumeration domain node
      cdi:identifier:
        description: Formal identifier
        $ref: https://cross-domain-interoperability-framework.github.io/metadataBuildingBlocks/build/annotated/bbr/metadata/ddiProperties/ddicdiDataTypes/schema.yaml#/$defs/Identifier
      cdi:name:
        description: Structured name for this enumeration domain
        anyOf:
        - $ref: https://cross-domain-interoperability-framework.github.io/metadataBuildingBlocks/build/annotated/bbr/metadata/ddiProperties/ddicdiDataTypes/schema.yaml#/$defs/ObjectName
        - type: array
          items:
            $ref: https://cross-domain-interoperability-framework.github.io/metadataBuildingBlocks/build/annotated/bbr/metadata/ddiProperties/ddicdiDataTypes/schema.yaml#/$defs/ObjectName
      cdi:purpose:
        description: Purpose of this enumeration domain
        $ref: https://cross-domain-interoperability-framework.github.io/metadataBuildingBlocks/build/annotated/bbr/metadata/ddiProperties/ddicdiDataTypes/schema.yaml#/$defs/InternationalString
      cdi:uses:
        description: Reference to a LevelStructure that organizes the enumeration.
          @id reference only.
        $ref: https://cross-domain-interoperability-framework.github.io/metadataBuildingBlocks/build/annotated/bbr/metadata/ddiProperties/ddicdiDataTypes/schema.yaml#/$defs/id-reference
      cdi:isDefinedBy:
        description: References to Concepts that define the enumerated values. Array
          of @id references.
        type: array
        items:
          $ref: https://cross-domain-interoperability-framework.github.io/metadataBuildingBlocks/build/annotated/bbr/metadata/ddiProperties/ddicdiDataTypes/schema.yaml#/$defs/id-reference
      cdi:references:
        description: Reference to the CategorySet providing the value categories.
          @id reference only.
        $ref: https://cross-domain-interoperability-framework.github.io/metadataBuildingBlocks/build/annotated/bbr/metadata/ddiProperties/ddicdiDataTypes/schema.yaml#/$defs/id-reference
x-jsonld-prefixes:
  cdi: http://ddialliance.org/Specification/DDI-CDI/1.0/RDF/

```

Links to the schema:

* YAML version: [schema.yaml](https://cross-domain-interoperability-framework.github.io/metadataBuildingBlocks/build/annotated/bbr/metadata/ddiProperties/ddicdiValueDomain/schema.json)
* JSON version: [schema.json](https://cross-domain-interoperability-framework.github.io/metadataBuildingBlocks/build/annotated/bbr/metadata/ddiProperties/ddicdiValueDomain/schema.yaml)


# JSON-LD Context

```jsonld
{
  "@context": {
    "cdi": "http://ddialliance.org/Specification/DDI-CDI/1.0/RDF/",
    "@version": 1.1
  }
}
```

You can find the full JSON-LD context here:
[context.jsonld](https://cross-domain-interoperability-framework.github.io/metadataBuildingBlocks/build/annotated/bbr/metadata/ddiProperties/ddicdiValueDomain/context.jsonld)

## Sources

* [DDI-CDI 1.0 Specification](https://ddialliance.org/Specification/DDI-CDI/1.0/)

# For developers

The source code for this Building Block can be found in the following repository:

* URL: [https://github.com/Cross-Domain-Interoperability-Framework/metadataBuildingBlocks](https://github.com/Cross-Domain-Interoperability-Framework/metadataBuildingBlocks)
* Path: `_sources/ddiProperties/ddicdiValueDomain`

