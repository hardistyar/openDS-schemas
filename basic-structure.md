---
# Structure of openDS json schemas
---

## Introduction
This page explains the overall structure of the [JSON schema](https://json-schema.org/) for an [open Digital Specimen](https://github.com/DiSSCo/openDS).


## Linking to controlled vocabularies
*Note, 09Feb: This needs further thought/discussion:*

- *link schema properties to the relevant controlled vocabularies, to make open Digital Specimens machine-readable;*
- *make the schema compliant with [Schema.org](https://schema.org/) to provide structured data on the Internet that can be discovered by the major search engines and thus ensure that details of open Digital Specimens are returned in search results. Where necessary this includes supplementing with relevant types and properties from the [bioschemas.org](https://bioschemas.org/) extensions to Schema.org;*
- *also, 9th Feb discovered geoschemas and science-on-schema.org.*
- *Schema.org has to be implemented on web pages (landing pages) that can be crawled, not in the DS itself.*
- *What is the concept of a landing page for an open digital specimen? It's basically the UI page on a web server*.
- *At the bottom of all this is whether we consider an ODS to be of resourceType Dataset from the Schema.org perspective. If not, then do we need to do it? In a sense, an ODS is a dataset because we describe it as an authoritative package of links to other data. It is self-contained, and describable.*


## Context definition

Here, for all schemas: [context definition](context-definition.md).

## Beginning of the schema
This part of the definition defines the schema itself. It specifies what is being defined i.e., an object type with the title "OpenDigitalSpecimen" and a set of properties - that will be defined below - making up the definition of an open digital specimen.

*Notes to self about the fragment below:*
- *do we need to pull in the $context here? Unresolved issue as to how we exploit links to semantics*
- *the deployed schema at http://nsidr.org/objects/20.5000.1025/abdc0e4c69d372a68cec, which is eventually the one that will be pointed to by the `$id` uri does not match what is written here. That out-dated one (v0.2) needs to be replaced at some moment with this one. The `id` will need to be changed to that URL eventually, or maybe to the type definition in a type registry, more likely?*
- *it may well be that the json description of the ***type definition*** of an openDS is different from this schema definition. Logically they will be the same but how they are used is different.*
- *how do we process type definitions? What general purspose software exists for dealing with them? Out of date now, but gives some hints: https://dlib.org/dlib/may07/saidis/05saidis.html.*
- *The schema references the current draft 2019-09 of the JSON-schema specification. However, some schema validator tools work only with earlier versions. e.g., the https://extendsclass.com/json-schema-validator.html works with /draft-07/schema#.*
- *Consider that we might want to enforce a convention (pattern) for any specific propertyNames that are not already in the schema but which might be added to a JSON instance.*
- *Consider that we might want to use the additionalProperties keyword to control how the extensions (E) section works.*

```json
{
  "$schema": "http://json-schema.org/draft/2019-09/schema#",
  "$id": "https://nsidr.org/schemas/opendsschema.json",
  "type": "object",
  "description": "v0.3, 11Feb2021. JSON schema for an open Digital Specimen.",
  "title": "OpenDigitalSpecimen",
  "properties": {
    "id": { "type": "string", "$comment": "doi name" },
    "title": { "type": "string", "enum": ["openDigitalSpecimen"] },
    ... several nested sections of the schema, see below ... ,
  },
  "required": [ "id", "title", "a_section" ]
}
```
As the fragment indicates, an `id` - a registered DOI name - is needed to identify the object and a`title` is needed to make it explicit that this is an ODS object. The Authoritative information (A) section is mandatory as well.

The remainder of the schema is organised as a structure of nested schemea sections, as specified below.

## Overall structure (nested sections of the schema)
At the highest level of structure, the properties of an open Digital Specimen are divided into several sections, each represented in the schema as a nested JSON object, as follows:

- **Authoritative (A) section**, containing essential data about a specimen. The A section is represented by an a_section object containing all the key/value pair properties that are deemed to be authoritative information about a physical specimen. The complete schema definition of the A section corresponds to the information elements expected to be present at MIDS level 2. The A section is mandatory.

- **Images (I) section**, containing references to images or images themselves of the specimen and/or its labels. The I section is represented by an i_section object containing metadata about the available images.

- **Supplementary (S) section**, containing links to supplementary data derived from a specimen. The S section is represented by an s_section object, which itself may contain multiple objects grouping specific kinds of supplementary information (such as external links) together. The definition of the S section corresponds to the additional information elements expected to be present at MIDS level 3 and more.

- **Tertiary (T) section**, containing links to related data associated with a specimen but not directly dervied from the physical specimen.
  
The json schema fragment describing this overall structure is the following:

```json

  / ... beginning of the schema, see above ... ],

    "a_section": {
      "$id": "#/properties/a_section",
      "type": "object",
      "properties": {
        "section": { "type": "string", "enum": ["authoritative information"] },
        <a_section name/value pairs>,
      },  
      "required": [ "section", <other mandatory keys> ]
    },

>>>the rest below need fixing up.

    "i_section": {
      "$id": "#/properties/i_section",
      "type": "object",      
      "title": "images information",
      "type": "array",
      "imagelinks": [
        <object of type array for links to images> ],
      "$comment": "more i_section information elements follow",
      "required": [ <list any mandatory keys> ]
    }, 
    
    "s_section": {
      "$id": "#/properties/s_section",
      "type": "object",
      "title": "supplementary information",
      "type": "array",
      "externallinks": {
        <object of type array for links> },
      "$comment": "insert more s_section objects/variables here",
      "required": [ <list any mandatory keys> ]
    },  
    
    "t_section": {
      "$id": "#/properties/t_section",
      "type": "object",
      "title": "tertiary information",
      "type": "array",
      "externallinks": {
        <object of type array for links> },
      "$comment": "insert more t_section objects/variables here",
      "required": [ <list any mandatory keys> ]
    }

    … <other similar section definitions, if needed e.g., Proprietary extension (E) section> …
  }

```

There are several additional section types for further study, including:

- **Extension (E) section**, *for further study. This is likely to be proprietary to specific organisations/systems.*

- **Operations/methods (O) section**, *for further study.*
  
- **Content/payload (C) section**, *for further study.*
  
- ***Others ...***

## Authoritative a_section information elements
Definitions of individual information elements belonging to the authoritative a_section follow. Each definition is a key/value pair.

Together with `id` and `title` (above) only a very small number of information elements are mandatory for an open digital specimen to come into existence (i.e., to be created). The first of these is `midsLevel`, which is a declared or calculated value of the present MIDS Level reached by the available  specimen data. Also required are the information elements `modified`, `physicalSpecimenId` and `institution`. These correspond to the information elements required at [MIDS level 0](https://github.com/tdwg/mids/issues?q=is%3Aopen+is%3Aissue+label%3AMIDS-0+label%3A%22MIDS+Element%22) according to the [TDWG MIDS definition](https://github.com/tdwg/mids/blob/main/MIDS-definition.md).

Of these, only a very small number are needed for an open digital specimen to be created. Amandatory to identify the open digital and the calculated `midslevel` for the information present are mandatory. 





### modified
*For now, refer to [MIDS Information - Modified](https://github.com/tdwg/mids/issues/8).*

### midsLevel
Integer value in the range 0-3.

### physicalSpecimenId
*For now, refer to [MIDS Information - PhysicalSpecimenId](https://github.com/tdwg/mids/issues/10).*

*Each definition should also contain the corresponding context definition perhaps?*

An identifier for the physical specimen within a collection. May or may not be unique. Examples: physical specimen identifier, barcode, catalog number, etc.

Equivalences: dwc:catalogNumber, abcd:numericUnitID, abcd:physicalObjectId

Required: Yes
Repeatable: Yes
Cardinality: 1..n

Context:

```json
"@context": {
          "physicalSpecimenId": "http://rs.tdwg.org/abcd/terms/numericUnitID"

          "http://rs.tdwg.org/dwc/terms/catalogNumber"
          "http://rs.tdwg.org/abcd/terms/physicalObjectID"
}



```


### institution
*For now, refer to [MIDS Information - Institution](https://github.com/tdwg/mids/issues/11).*

### materialType
*For now, refer to [MIDS Information - MaterialType](https://github.com/tdwg/mids/issues/14).*

### name
*For now, refer to [MIDS Information - Name](https://github.com/tdwg/mids/issues/12).*

### Other a_section properties
*At the present time (09Feb), only the MIDS-0 and MIDS-1 properties have been defined. Others will be added later.*

### Complete JSON fragment for a_section
Here is the complete JSON fragment for the a_section property definitions above.

```json
    "a_section": {
      "$id": "#/properties/a_section",
      "type": "object",
      "properties": {
        "section": { "type": "string", "enum": ["authoritative information"] },
        "modified": { "type": "string", "title": "Modified (date:time)" },
        "midsLevel": { "type": "integer", "minimum": 0, "maximum": 3 },
        "physicalSpecimenId": { "type": "string", "title": "Physical specimen identifier" },
        "institution": {
          "type": "array", "items": [ {"type": "string", "title": "Code (GRSciColl)" },
                                      {"type": "string", "title": "Referent" } ] 
        },
        "materialType": {},
        "name": { "type": "string", "maxLength": 128, "title": "Name" }
      },  
      "required": [ "section", "modified", "midsLevel", "physicalSpecimenId", "institution" ]
    }
```
## Conditional subschemas for different values of midsLevel
- *Consider that, depending on the declared MIDS level, we'd expect to see a certain set of properties present in the ODS. See 'applying subschemas conditionally, using IfThenElse constructs. Need also to look at subschemas and how to structure these as separate schema fragments / type definitions i.e., the type definition of an ODS a_section is different depending on the declared/calculated MIDSLevel.*

*Something along the lines of (which still needs to be checked and tested and optimised):*

```json
    "a_section": {
      "$id": "#/properties/a_section",
      "type": "object",
      "properties": {
        "section": { "type": "string", "enum": ["authoritative information"] },
        "modified": { "type": "string", "title": "Modified (date:time)" },
        "midsLevel": { "type": "integer", "minimum": 0, "maximum": 3 },
      },
      "if": { "properties": { "midsLevel": { "const": 1 } },
      "then": { "properties": {
                  "physicalSpecimenId": { "type": "string", "title": "Physical specimen identifier" },
                  "institution": { "type": "array", "items": [ {"type": "string", "title": "Code  (GRSciColl)" }, {"type": "string", "title": "Referent" } ] 
                   },
                  "materialType": {},
                  "name": { "type": "string", "maxLength": 128, "title": "Name" }
                },
                "required": [ "physicalSpecimenId", "institution" ] },
      },
      "else": { "properties": {
                  "physicalSpecimenId": { "type": "string", "title": "Physical specimen identifier" },
                  "institution": { "type": "array", "items": [ {"type": "string", "title": "Code (GRSciColl)" }, {"type": "string", "title": "Referent" } ] 
                   },
                  "materialType": {},
                  "name": { "type": "string", "maxLength": 128, "title": "Name" },
                  "country": { "type": "string" }
                },
                "required": [ "physicalSpecimenId", "institution" ]
                },
                "required": [ "section", "modified", "midsLevel" ] },
      }
    }
```


## Supplementary s_section information elements
Definitions of individual information elements belonging to the supplementary s_section follow. Each definition is a key/value pair.

*To be completed.*



## Complete JSON schema for validation testing

```json
{
  "$schema": "http://json-schema.org/draft/2019-09/schema#",
  "$id": "https://nsidr.org/schemas/opendsschema.json",
  "description": "v0.3, 11Feb2021. JSON schema for an open Digital Specimen.",
  "type": "object",
  "title": "openDigitalSpecimen",
  "properties": {
    "id": { "type": "string", "$comment": "doi name" },
    "title": { "type": "string", "enum": ["openDigitalSpecimen"] },
    "a_section": {
      "$id": "#/properties/a_section",
      "type": "object",
      "properties": {
        "section": { "type": "string", "enum": ["authoritative information"] },
        "modified": { "type": "string", "title": "Modified (date:time)" },
        "midsLevel": { "type": "integer", "minimum": 0, "maximum": 3 },
        "physicalSpecimenId": { "type": "string", "title": "Physical specimen identifier" },
        "institution": {
          "type": "array", "items": [ {"type": "string", "title": "Code (GRSciColl)" },
                                      {"type": "string", "title": "Referent" } ] 
        },
        "materialType": {},
        "name": { "type": "string", "maxLength": 128, "title": "Name" }
      },  
      "required": [ "section", "modified", "midsLevel", "physicalSpecimenId", "institution" ]
    }
  },
  "required": [ "id", "title", "a_section" ]
}

```
Note, deployed (adapted) into nsidr.org as schema ODStype1802 on 18th February 2021.

Institution array is open-ended, allowing as many items as you like to be added. In CORDRA How do I allow additional referents to be added as format=uri rather than just as uncontrolled strings? Within an array, is it possible in the schema to constrain the number of elements and their order?

Material Type is not specified yet. At the moment in CORDRA, just a string type.


## Example DS coded according to schema
This is an example DS coded according to the above schema definition.

```
{
  "id": "20.5000.1025/123456",
  "title": "openDigitalSpecimen",
  "a_section": {
    "section": "authoritative information",
    "modified": "2021-02-09T16:40:18Z",
    "midsLevel": 1,
    "physicalSpecimenId": "013258549",
    "institution": ["NHMUK", "https://ror.org/039zvsn29" ],
    "materialType": "",
    "name": "Holorchis castex"
  }
}

```

## Adapt the above Complete JSON schema so that it's of type array
*I need to investigate whether the overall type should be object or array.*
*What would be the reasons for using an array rather than an object type at the top-level?*
*One possible reason to put sections into an array might be to force the order in which they appear.*

```json

{
  "$schema": "http://json-schema.org/draft/2019-09/schema#",
  "$id": "https://nsidr.org/schemas/opendsschema.json",
  "description": "v0.3, 11Feb2021. JSON schema for an open Digital Specimen.",
  "type": "object",
  "title": "openDigitalSpecimen",
  "properties": {
    "id": { "type": "string", "$comment": "doi name" },
    "title": { "type": "string", "enum": ["openDigitalSpecimen"] },
    "sections": { "type": "array",
                  "items": [{
                              "a_section": {
                                "$id": "#/properties/a_section",
                                "type": "object",
                                "properties": {

                                }
                              }
                            },
                            {
                              "b_section": {
                                "$id": "#/properties/b_section",
                                "type": "object",
                                "properties": {

                              }
                            }
                           }]  
    }
  },
  "required": [ "section", "modified", "midsLevel", "physicalSpecimenId", "institution" ]
    }
  },
  "required": [ "id", "title" ]
}



END.