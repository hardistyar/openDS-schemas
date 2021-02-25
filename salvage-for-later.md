---
# Older version of Structure of openDS json schemas
# 25 Feb. Kept so can salvage useful stuff from it later.
---

## Introduction
Using examples, this page explains the structure of an [open Digital Specimen](https://github.com/DiSSCo/openDS) and declares the [JSON schema](https://json-schema.org/) to support those.

In this form it serves as a basis for discussion and understanding.

Important starting points for understanding are the following:

- The structure of an openDS corresponds to the Minimum Information about a Digital Specimen (MIDS), level by level.
- The definition of an openDS here corresponds to the Digital Specimen class in the [object model](https://github.com/DiSSCo/openDS/blob/master/data-model/data-model-intro.md).

## Making a DS semantically meanful - context definition
In a first phase, while we aim to stabilise the structure, we consider only the JSON representation of the DS. Later, to make the DS machine-readable we will extend this to a JSON-LD representation that links the key names to terms in relevant vocabularies. In doing so, there are some open questions:

- Is it the DS itself that must be expressed as JSON-LD or its schema?
- Do we need compliance with [Schema.org](https://schema.org/) and relevant types and properties from the [bioschemas.org](https://bioschemas.org/) extensions to provide structured data on the Internet that can be discovered by the major search engines? Does this have to be in the DS itself, it's schema (see above question) or just the HTML landing pages on the Web where the specimens can be seen by humans?
- Other schemas we may need to take into account include  [geoschemas.org](https://geoschemas.org/about) and [science-on-schema.org](https://github.com/ESIPFed/science-on-schema.org).
- *At the bottom of all this is whether we consider an ODS to be of resourceType Dataset from the Schema.org perspective. If not, then do we need to do it? In a sense, an ODS is a dataset because we describe it as an authoritative package of links to other data. It is self-contained, and describable.*

In any case, and for when we might need it here's the beginnings of a [context definition](context-definition.md).

## Example 1: MIDS level 1, authoritative information only
An example DS for a physical specimen in the collection of the Muséum National d'Histoire Naturelle, Paris (MNHN) with the identifier "MNHN-IM-2013-8488"; compliant with MIDS level 1 and having no images or supplementary information.

Source of data: https://science.mnhn.fr/institution/mnhn/collection/im/item/2013-8488

Location of DS: https://nsidr.org/objects/20.5000.1025/64ae0cf0dacb7bd20ba5?pretty

```json
{
  "id": "20.5000.1025/64ae0cf0dacb7bd20ba5",
  "typeName": "ODStype1802",
  "authoritative": {
    "modified": "2021-02-22T14:10:20.824Z",
    "midsLevel": 1,
    "physicalSpecimenId": "MNHN-IM-2013-8488",
    "institution": [
      "MNHN",
      "https://ror.org/03wkt5x30"
    ],
    "materialType": "Alcohol, 95%",
    "name": "Pygmaepterys pointieri Garrigues & Merle, 2014"
  }
}
```

## Structure of the schema for the above example
The JSON schema for the above example begins with the definition of the schema itself - the first five lines of the JSON fragment below. It states the version of the [JSON schema specification](https://json-schema.org/) that applies and gives an identifier, description, type and title to the schema.

The schema then specifies in its properties what is being defined i.e., an object of type "ODStype1802" with its included subsets of specimen related properties. The first subset of specimen related properties - the authoritative properties - appear next. At the end of the authoritative (a_section) a list of those property keys that are mandatory appears as the `required` validation array. It's empty in the fragment below. The object's  `id`, `typeName` and `authoritative` section are all required for an open Digital Specimen to be a valid construct. The object's `id` must be a valid Handle.

*Note to self: Constrain the allowed values of `id` to the permitted pattern of a Handle. Uses the `pattern` validation keyword with (ECMAScript (ECMA-262)](http://json-schema.org/understanding-json-schema/reference/regular_expressions.html) regular expression dialect to specify the pattern.*

```json
{
  "$schema": "http://json-schema.org/draft/2019-09/schema#",
  "$id": "https://nsidr.org/schemas/opendsschema.json",
  "description": "v0.4, 22 February 2021. JSON schema for an open Digital Specimen.",
  "type": "object",
  "title": "ODStype1802",
  "properties": {
    "id": { "type": "string" },
    "typeName": { "type": "string", "const": [ "ODStype1802" ] },
    "authoritative": {
      "$id": "#/properties/a_section",
      "type": "object",
      "properties": {
        "$comment": "authoritative properties here; list mandatory ones as required (below)"
      },
      "required": [ ]  
    },
    "$comment": " ... several more optional sections of the schema appear here, see below ... "
  },
  "required": [ "id", "typeName", "authoritative" ]
}
```


*Notes to self about the fragment above:*

- *We need to constrain the `id` to be a valid Handle. How to do that?*
- *do we need to pull in the $context here? Unresolved issue as to how we exploit links to semantics*
- *the deployed schema at http://nsidr.org/objects/20.5000.1025/abdc0e4c69d372a68cec, which is eventually the one that will be pointed to by the `$id` uri does not match what is written here. That out-dated one (v0.2) needs to be replaced at some moment with this one. The `id` will need to be changed to that URL eventually, or maybe to the type definition in a type registry, more likely?*
- *it may well be that the json description of the ***type definition*** of an openDS is different from this schema definition. Logically they will be the same but how they are used is different.*
- *how do we process type definitions? What general purspose software exists for dealing with them? Out of date now, but gives some hints: https://dlib.org/dlib/may07/saidis/05saidis.html.*
- *The schema references the current draft 2019-09 of the JSON-schema specification. However, some schema validator tools work only with earlier versions. e.g., the https://extendsclass.com/json-schema-validator.html works with /draft-07/schema#.*
- *Consider that we might want to enforce a convention (pattern) for any specific propertyNames that are not already in the schema but which might be added to a JSON instance.*
- *Consider that we might want to use the additionalProperties keyword to control how the extensions (E) section works.*

## Overall structure (sections of the schema)
At the highest level of structure, the properties of an open Digital Specimen are divided into several sections, each represented in the schema as a nested JSON object. The first of these, the authoritative or a_section has already been introduced above. It together with the other section types are explained as follows:

- **authoritative a_section**, contains essential data about a specimen. The a_section is represented by an a_section object containing all the key/value pair properties that are deemed to be authoritative information about a physical specimen. The complete schema definition of the a_section corresponds to the information elements expected to be present at MIDS level 2. The a_section is mandatory.

- **images i_section**, containing references to images or images themselves of the specimen and/or its labels. The images section is represented by an i_section object containing metadata about the available images.

- **supplementary s_section**, containing links to supplementary data derived from a specimen. The supplementary section is represented by an s_section object, which itself may contain multiple objects grouping specific kinds of supplementary information (such as external links) together. The definition of the s_section corresponds to the additional information elements expected to be present at MIDS level 3 and more.

- **tertiary t_section**, containing links to related data associated with a specimen but not directly derived from the physical specimen.
  
Expanding the basic schema construct introduced above, the JSON schema fragment describing this overall structure is the following:

```json
{
  "$schema": "http://json-schema.org/draft/2019-09/schema#",
  "$id": "https://nsidr.org/schemas/opendsschema.json",
  "description": "v0.4, 22 February 2021. JSON schema for an open Digital Specimen.",
  "type": "object",
  "title": "ODStype1802",
  "properties": {
    "id": { "type": "string" },
    "typeName": { "type": "string", "const": [ "ODStype1802" ] },
    "authoritative": {
      "$id": "#/properties/a_section",
      "type": "object",
      "properties": {
        "$comment": "authoritative properties here; list mandatory ones as required (below)"
      },
      "required": [ ]  
    },
    "images": {
      "$id": "#/properties/i_section",
      "type": "object",
      "properties": {
        "$comment": "image properties here; list mandatory ones as required (below)"
      },  
      "required": [  ]
    },
    "supplementary": {
      "$id": "#/properties/s_section",
      "type": "object",
      "properties": {
        "$comment": "supplementary properties here; list mandatory ones as required (below)"
      },  
      "required": [  ]
    },
    "tertiary": {
      "$id": "#/properties/t_section",
      "type": "object",
      "properties": {
        "$comment": "tertiary properties here; list mandatory ones as required (below)"
      },  
      "required": [  ]
    },
    "$comment": " ... further optional sections of the schema can appear here ... "
  },
  "required": [ "id", "typeName", "authoritative" ]
}
```

### Other section types
*For further study.*

There are several additional section types for further study, including:

- **extension e_section**, *for further study. This is likely to be proprietary to specific organisations/systems.*

- **operations/methods o_section**, *for further study.*
  
- **payloads (contents) p_section**, *for further study.* *thumbnail images perhaps?*
  
- ***Others ...***

## Authoritative a_section information elements

>>>>Clean up here

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
    "authoritative": {
      "$id": "#/properties/a_section",
      "type": "object",
      "properties": {
        "modified": { "type": "string", "title": "Modified (date:time)", "format": "date-time" },
        "midsLevel": { "type": "integer", "minimum": 0, "maximum": 3 },
        "physicalSpecimenId": { "type": "string", "title": "Physical specimen identifier" },
        "institution": {
          "type": "array", "items": [ {"type": "string", "title": "Code (GRSciColl)" },
                                      {"type": "string", "format": "uri", "title": "Referent" } ] 
        },
        "materialType": { "type": "string", "title": "Material type" },
        "name": { "type": "string", "maxLength": 128, "title": "Name" }
      },  
      "required": [ "modified", "midsLevel", "physicalSpecimenId", "institution", "materialType", "name" ]
    }
```

## Conditional subschemas for different values of midsLevel
- *Consider that, depending on the declared MIDS level, we'd expect to see a certain set of properties present in the ODS. See 'applying subschemas conditionally, using IfThenElse constructs. Need also to look at subschemas and how to structure these as separate schema fragments / type definitions i.e., the type definition of an ODS a_section is different depending on the declared/calculated MIDSLevel.*

*Something along the lines of (which still needs to be checked and tested and optimised):*

>>>Needs cleaning up still to align with other parts

```json
    "authoritative": {
      "$id": "#/properties/a_section",
      "type": "object",
      "properties": {
        "modified": { "type": "string", "title": "Modified (date:time)", "format": "date-time" },
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

## S section
>>>what do we need from below?

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








## Complete JSON schema for validation testing

```json

update this >>>>

{
  "$schema": "http://json-schema.org/draft/2019-09/schema#",
  "$id": "https://nsidr.org/schemas/opendsschema.json",
  "description": "v0.4, 22 February 2021. JSON schema for an open Digital Specimen.",
  "type": "object",
  "title": "ODStype1802",
  "properties": {
    "id": { "type": "string" },
    "typeName": { "type": "string", "const": [ "ODStype1802" ] },
    "authoritative": {
      "$id": "#/properties/a_section",
      "type": "object",
      "properties": {
        "modified": { "type": "string", "title": "Modified (date:time)", "format": "date-time" },
        "midsLevel": { "type": "integer", "minimum": 0, "maximum": 3 },
        "physicalSpecimenId": { "type": "string", "title": "Physical specimen identifier" },
        "institution": {
          "type": "array", "items": [ {"type": "string", "title": "Code (GRSciColl)" },
                                      {"type": "string", "format": "uri", "title": "Referent" } ] 
        },
        "materialType": { "type": "string", "title": "Material type" },
        "name": { "type": "string", "maxLength": 128, "title": "Name" }
      },  
      "required": [ "modified", "midsLevel", "physicalSpecimenId", "institution", "materialType", "name" ]
    }
  },
  "required": [ "id", "title", "authoritative" ]
}
```




Note, deployed (adapted) into nsidr.org as schema ODStype1802 on 18th February 2021.

Institution array is open-ended, allowing as many items as you like to be added. In CORDRA How do I allow additional referents to be added as format=uri rather than just as uncontrolled strings? Within an array, is it possible in the schema to constrain the number of elements and their order?

Material Type is not specified yet. At the moment in CORDRA, just a string type.




END.



