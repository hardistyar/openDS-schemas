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
- *the deployed schema pointed to by the `$id` uri does not match what is written here. That out-dated one needs to be replaced at some moment with this one.*
- *`$id` needs to be changed to a registered Handle for the type definition based on this schema in a type registry",*
- *it may well be that the json description of the ***type definition*** of an openDS is different from this schema definition. Logically they will be the same but how they are used is different.*
- *how do we process type definitions? What general purspose software exists for dealing with them? Out of date now, but gives some hints: https://dlib.org/dlib/may07/saidis/05saidis.html.*

```json
{
  "$schema": "https://json-schema.org/draft/2019-09/schema",
  "$id": "https://nsidr.org/schemas/opendsschema.json",
  "description": "JSON schema for an open Digital Specimen, version 0.1",
  "type": "object",
  "title": "OpenDigitalSpecimen",
  "required": [
   "id",
   "midsLevel",
   "modified",
   "physicalSpecimenId",
   "institution"
  ],
  "properties": {
    "id": {
      "type": "string", "$comment": "doi name"
    },
    ... remainder of the schema ... 
  }
}
```
As the fragment indicates with the `required` array, a very small number of information elements are needed for an open digital specimen to be created. An `id`, which is a registered DOI and the calculated `midslevel` for the information present are mandatory. The remaining required information elements `modified`, `physicalSpecimenId` and `institution` correspond to the information elements required at [MIDS level 0](https://github.com/tdwg/mids/issues?q=is%3Aopen+is%3Aissue+label%3AMIDS-0+label%3A%22MIDS+Element%22).

The '*remainder of the schema*' referred to in the above fragment continues with definition of the overall structure.

## Overall structure
At the highest level of structure, the properties of an open Digital Specimen are divided into several sections, as follows:

- **id**, containing the registered DOI name for the open digital specimen. 
  
- **Authoritative (A) section**, containing essential data about a specimen. The A section is represented by an a_section object containing all the key/value pair properties that are deemed to be authoritative information about a physical specimen. The definition of the A section corresponds to the information elements expected to be present at MIDS level 2.
  
- **Images (I) section**, containing references to images or images themselves of the specimen and/or its labels. The I section is represented by an i_section object containing metadata about the available images.

- **Supplementary (S) section**, containing links to supplementary data derived from a specimen. The S section is represented by an s_section object, which itself may contain multiple objects grouping specific kinds of supplementary information (such as external links) together. The definition of the S section corresponds to the additional information elements expected to be present at MIDS level 3 and more.

- **Tertiary (T) section**, containing links to related data associated with a specimen but not directly dervied from the physical specimen.
  
The json schema fragment describing this overall structure is the following:

```json
{
   / ... beginning of the schema ... ],
  "properties": {
    "id": {
      "type": "string", "$comment": "doi name"
    },

    "a_section": {
      "$id": "#/properties/a_section",
      "title": "Authoritative information section",
        <a_section name/value pairs>,
      "$comment": "a_section information elements follow"
    }, 
    
    "i_section": {
      "$id": "#/properties/i_section",
      "title": "Images information section",
      "type": "array",
      "imagelinks": [
        <object of type array for links to images> ],
      "$comment": "more i_section information elements follow"
    }, 
    
    "s_section": {
      "$id": "#/properties/s_section",
      "title": "Supplementary information section",
      "type": "array",
      "externallinks": {
        <object of type array for links> },
      "$comment": "insert more s_section objects/variables here",
    },  
    
    "t_section": {
      "$id": "#/properties/t_section",
      "title": "Tertiary information section",
      "type": "array",
      "externallinks": {
        <object of type array for links> },
      "$comment": "insert more t_section objects/variables here",
    }

    … <other similar section definitions, if needed e.g., Proprietary extension (E) section> …
  }
}

```

There are several additional section types for further study, including:

- **Extension (E) section**, *for further study. This is likely to be proprietary to specific organisations/systems.*

- **Operations/methods (O) section**, *for further study.*
  
- **Content/payload (C) section**, *for further study.*
  
- ***Others ...***

## Authoritative a_section information elements
Definitions of individual information elements belonging to the authoritative a_section follow. Each definition is a key/value pair.


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
      "title": "Authoritative information section",
        "modified": { "type": "string", "title": "Modified (date:time)" },
        "midsLevel": { "type": "integer", "minimum": 0, "maximum": 3 },
        "physicalSpecimenId": { "type": "string", "title": "Physical specimen identifier" },
        "institution": {
          "type": "array", "items": [ {"type": "string", "title": "Code (GRSciColl)" },
                                      {"type": "string", "title": "Referent" } ] 
        },
        "materialType": {},
        "name": { "type": "string", "maxLength": 128, "title": "Name" },  
        "Comment": "other a_section information elements follow, when MIDS-2 defined/stabilised."
    }, 
```

## Supplementary s_section information elements
Definitions of individual information elements belonging to the supplementary s_section follow. Each definition is a key/value pair.

*To be completed.*



## Complete JSON schema for validation testing

```json
{
  "$schema": "https://json-schema.org/draft/2019-09/schema",
  "$id": "https://nsidr.org/schemas/opendsschema.json",
  "description": "JSON schema for an open Digital Specimen, version 0.1",
  "type": "object",
  "title": "OpenDigitalSpecimen",
  "required": [
   "id",
   "midsLevel",
   "modified",
   "physicalSpecimenId",
   "institution"
  ],
  "properties": {
    "id": {
      "type": "string", "$comment": "doi name"
    },
    "a_section": {
      "$id": "#/properties/a_section",
      "title": "Authoritative information section",
        "modified": { "type": "string", "title": "Modified (date:time)" },
        "midsLevel": { "type": "integer", "minimum": 0, "maximum": 3 },
        "physicalSpecimenId": { "type": "string", "title": "Physical specimen identifier" },
        "institution": {
          "type": "array", "items": [ {"type": "string", "title": "Code (GRSciColl)" },
                                      {"type": "string", "title": "Referent" } ] 
        },
        "materialType": {},
        "name": { "type": "string", "maxLength": 128, "title": "Name" },  
        "Comment": "other a_section information elements follow, when MIDS-2 defined/stabilised."
    }
  }
}

```


## Example DS coded according to schema
This is an example DS coded according to the above schema definition.

```
{
  "id": "20.5000.1025/123456",
  "Authoritative information section": {
    "modified": "2021-02-09T16:40:18Z",
    "midsLevel": 1,
    "physicalSpecimenId": "013258549",
    "institution": ["NHMUK", "https://ror.org/039zvsn29" ],
    "materialType": "",
    "name": "Holorchis castex"
  }
}
```

*09Feb: Got to here>>> The above JSON instance doesn't validate agains the schema immediately above it, for two reasons.*
- *The validator I've been using (https://extendsclass.com/json-schema-validator.html) complains it can't find the json-schema specification schema at json-schema.org. Am I referencing it wrongly?*
- *The a_section object isn't getting recognised as a sub-structure. Do I need to use a nested schema here?*.