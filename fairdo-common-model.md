---
# FAIR Digital Object (FAIR DO) Common Model
---

| <!-- --> | <!-- --> |
| ---- | ---- |
| **Namespace** | https://fairdo.org/models# |
| **Namespace prefix** | fairdo: |
| **Description**| Type definitions (ontology) for the FAIR Digital Object Common Model, intended to underlie a wide array of digital object applications in the Internet of FAIR Data and Services (IFDS) |
| **Version**| 1.0.0 |
| **Last modified**| 2021-01-27 |
| **Prior version**| None |
| **Published by**| Initial draft prepared by [DiSSCo Coordination and Support Office](https://www.dissco.eu/contact-2/) |
| ***Notes***| *Derived from and builds on work of the [RDA Group of European Data Experts (GEDE)](https://www.rd-alliance.org/groups/gede-group-european-data-experts-rda#:~:text=The%20aim%20of%20the%20Group,on%20a%20bottom%2Dup%20process.) on the [FAIR Digital Object Framework](https://github.com/GEDE-RDA-Europe/GEDE/tree/master/FAIR%20Digital%20Objects/FDOF) and ideas from [Luiz Bonino further developing that](https://fairdigitalobjectframework.org/); especially the term definitions and latterly the work of the new [FDO Forum](https://fairdo.org/), that is likely to assume ownership of this work.* |

- [Entity definitions](#entity-definitions)
  - [Classes](#classes)
  - [Properties](#properties)
  - [Relations](#relations)


## Entity definitions

### Classes

| **Class** | **FairDigitalObject** |
| ---- | ---- |
| **Label** | FairDO |
| **URI** | https://fairdo.org/models#FairDO |
| **Description** | An object represented in the digital space that follows the FAIR principles according to the [Digital Object Implementation](https://github.com/GEDE-RDA-Europe/GEDE/blob/master/FAIR%20Digital%20Objects/FDOF/DO-Implementation-FDO.docx) of the guidelines and requirements of the [FAIR Digital Object Framework (FDOF)](https://github.com/GEDE-RDA-Europe/GEDE/blob/master/FAIR%20Digital%20Objects/FDOF/FAIR%20Digital%20Object%20Framework-v1-02.docx). |
| **Type definition** | <...a PID...> |
| **Subclass of** | None |

| **Class** | **MetadataDigitalObject** |
| ---- | ---- |
| **Label** | MetadataDO |
| **URI** | https://fairdo.org/models#MetadataDO |
| **Description** | An object, itself FAIR containing information (metadata) about another digital object. Inherits properties from class FairDigitalObject. |
| **Type definition** | <...a PID...> |
| **Subclass of** | FairDO |




| **Type Name** | FairDigitalObjectType |
| ---- | ---- |
| **Identifier** | *Handle to be assigned* |
| **Description** | An object represented in the digital space that follows the FAIR principles according to the [Digital Object Implementation](https://github.com/GEDE-RDA-Europe/GEDE/blob/master/FAIR%20Digital%20Objects/FDOF/DO-Implementation-FDO.docx) of the guidelines and requirements of the [FAIR Digital Object Framework (FDOF)](https://github.com/GEDE-RDA-Europe/GEDE/blob/master/FAIR%20Digital%20Objects/FDOF/FAIR%20Digital%20Object%20Framework-v1-02.docx). |
| **Provenance (including contributors/source, creation date, modification date)
| **Related Standards and Recommendations** |  |
| **Expected Uses** |  |
| **Representations and Semantics** | isRepresentedBy Bitsequence |
| **Properties Specific to this Type** |  |
| **Relationships to Other Types** | Superclass for all FAIR Digital Objects |









GUPRI	Class	Globally unique, persistent and resolvable identifier.
Metadata Schema	Class	A schema to define the structure and semantics of Metadata Records.
PersistencePolicy	Class	A document defining the conditions under which objects will be persisted.
hasMetadata	Relation	Defines the relation between the the object and the Metadata Record(s) that describes it. This relation is present in the FDOF Identifier Record. For some types of objects, e.g., structured data, this relation may also be present in the object itself.
hasMetadataSchema	Relation	Defines the relation between the Metadata Record and the schema(s) it conforms to. Multiple schemas may be present as different parts of the Metadata Record may conform to different Metadata Schemas.
hasPersistencePolicy	Relation	Defines the relation between an object and the document defining its persistence policy.
identifier	Relation	Defines the identity relation between a GUPRI and the object it identifies.
isMetadataOf	Relation	Defines the relation between a Metadata Record and the object it identifies.
isMetadataSchemaOf	Relation	Defines the relation between a Metadata Schema and Metadata Record(s) conforming to the schema.

Identifier
hasIdentifier
Type
hasType





### Properties


### Relations
