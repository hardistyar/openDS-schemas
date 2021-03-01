---
# PID Information Types for an open Digital Specimen
---

## Introduction
This paper, "[Automated Schema Extraction for PID Information Types](https://doi.org/10.1109/BigData.2016.7840957)" by Ulrich Schwardmann, GWDG gives the background information necessary for understanding how to define and use Basic PITs and PITs for open Digital Specimens.

A PID Information Type (PIT) is the type definition of a data construct that aids in processing that data. Basically, PITs are [JSON Schemas](https://json-schema.org/) against which [JSON](https://www.json.org/json-en.html) instances/fragments can be validated and processed. 

> *Note: In the context of digital objects, the link between the JSON instance/fragment and the schema needed to support processing of it is provided by a reference in the PID record of the object to its type definition (PIT) in a data type registry. This is the equivalent, in the Web/HTTP context of including a "describedBy" link relation in the JSON instance (ref. section 9.5.1.1 of [JSON Schema Core](https://json-schema.org/draft/2020-12/json-schema-core.html) specification and this [Stackoverflow question](https://stackoverflow.com/questions/45190382/how-to-specify-that-a-json-instance-is-defined-by-a-specific-json-schema)).*

A PIT is built from a finite combination of (other) PITs and Basic PITs. The difference between PITs and Basic PITs is that PITs are built based principally on JSON objects and arrays whereas Basic PITs use only the primitive JSON types i.e., null, boolean, number, string, integer.

The [ePIC (test) data type registry](https://dtr-test.pidconsortium.eu/#) is the DTR where these examples can be found. There is also a [registry of live, approved type definitions](http://dtr.pidconsortium.eu/#) but this does not presently contain any for openDS.

Here, we will use the terminlogy of the above-mentioned article for brevity i.e., PIT and Basic PIT. In the ePIC data type registries, these are named as "PID-InfoType" and "PID-BasicInfoType" respectively. 

## PITs and Basic PITs to make complex type definitions.
PIT definitions are hierarchical. More complex PITs are made up of simpler PITs. The above-mentioned paper gives an example of how a PIT for `geographic-coordinate` is made up of PITs for latitude, longitude and altitude. In the case of latitude/longitude there is the possibility that semidecimal, decimal, and sexagesimal representations are all valid alternatives. Thus, each of the latitude and longitude PITs is made up of Basic PITs, one for each alternative. There is no alternative representation for altitude, so this is defined as a Basic PIT.

This [`geographic-coordinate` example](https://dtr-test.pidconsortium.eu/#objects/21.T11148/ada87a981a168d9a4ccf) is a useful starting point for learning how to build an open DS PIT.

We can image a top-level PIT that is the complete type definition of a DS. This can be built from several 'section PITs' corresponding to the overall structure i.e., one PIT per section type - a_section, i_section, s_section, etc. These section PITs can themselves be built from combinations of other PITs, such as the `geographic-coordinate` example given and Basic PITs.

Taking this approach allows the structure of an openDS to remain flexible and adaptive to new needs. PITs can evolve independently of one another and fragments of data corresponding to PITs, especially the section PITs can be processed separately and independently of one another, as needs of applications dictate.

This approach also corresponds well with the overall approach of the [openDS data model](https://github.com/DiSSCo/openDS/blob/master/data-model/data-model-intro.md) with PITs corresponding to objects and properties. Thus, for example the GeographicOrigin object class could have `geographic-coordinate` as one of its properties.

## Example to begin with
We take the simple first example DS object outlined in the [basic-structure](basic-structure.md) description of an open DS.

```JSON
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
## Basic PITs
From the example, we can see that we need to define Basic PITs corresponding to each of the atomic information elements we expect to see in a DS at MIDS level 1 i.e., `modified`, `midsLevel`, `physicalSpecimenId`, `institution`, `materialType` and `name`. These are mostly straightforward Basic PITs, each have a single property - with the exception of `institution`, which needs two properties.

| **Basic PIT (type name)** | **Identifier** |
| --- | --- |
| `modified` | <a href="https://hdl.handle.net/21.T11148/99db324742823c55d975?locatt=view:ui" target="_blank">21.T11148/99db324742823c55d975</a> |
| `midsLevel` | <a href="https://hdl.handle.net/21.T11148/05d74b79e4d5139afe23?locatt=view:ui" target="_blank">21.T11148/05d74b79e4d5139afe23</a> |
| `physicalSpecimenId` | <a href="https://hdl.handle.net/21.T11148/4ac7431c2616a213481e?locatt=view:ui" target="_blank">21.T11148/4ac7431c2616a213481e</a> |
| `institution` | <a href="https://hdl.handle.net/21.T11148/197d0d589583abf80bc0?locatt=view:ui" target="_blank">21.T11148/197d0d589583abf80bc0</a> |
| `materialType` | <a href="https://hdl.handle.net/21.T11148/e99cf30f33c4a6397352?locatt=view:ui" target="_blank">21.T11148/e99cf30f33c4a6397352</a> |
| `name` | <a href="https://hdl.handle.net/21.T11148/6ae999552a0d2dca14d6?locatt=view:ui" target="_blank">21.T11148/6ae999552a0d2dca14d6</a> |

## (Compound) PITs
Then we need a PIT defining the a_section, made up from these Basic PITs. We also need a PIT defining the open DS as a whole (but at this moment only comprising the header information, the a_section PIT and the i_section PIT). In the table below, this PIT for the whole DS has the type name `ODStype1803`.

| **PIT (type name)** | **Identifier** |  |
| --- | --- | --- |
| `a_section` | <a href="https://hdl.handle.net/21.T11148/10d897b59e4da36ec0ad?locatt=view:ui" target="_blank">21.T11148/10d897b59e4da36ec0ad</a> |  |
| `i_section` | <a href="https://hdl.handle.net/21.T11148/833de75dcd35a82bb240?locatt=view:ui" target="_blank">21.T11148/833de75dcd35a82bb240</a> |  |
| `ODStype1803` | <a href="https://hdl.handle.net/21.T11148/627a5b7620b1471aa945?locatt=view:ui" target="_blank">21.T11148/627a5b7620b1471aa945</a> | **<< Look at this one and then click through each property to get to the lowest level Basic PITs** |

That's as far as I've got so far.

*One issue that comes up during discussion is the question of whether type definitions should be versioned. Needs to be further investigated.*




END.