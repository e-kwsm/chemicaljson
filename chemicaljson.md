# Introduction

The JSON format is a general container, and the work done in Chemical JSON was primarily aimed at developing a standard on top of the [JSON][JSON] (and [BSON][BSON]) format for chemical data. It was developed primarily to serve the needs of the Avogadro 2 application, but also to facilitate efficient storage in MongoDB (in the form of BSON), and to exchange data using [JSON-RPC 2.0][JSONRPC] and [RESTful APIs][REST]. It has since been used in web applications, and with 3DMol.js.

## Purpose

The purppose here is to document the format, provide an [open specification][openspec], establish what is required or optional, and to provide a living spefication as we extend the format. There is other work in the NWChem JSON project to develop a format that can be used to replace traditional log files in NWChem, and very early ideas on how to integrate JSON-LD into these JSON formats in order to offer a semantically rich format. This will ideally reuse some of the previous work done in the [CML format][CML] for XML.

## Motivation

The format was developed in a very pragmatic manner, and was primarily developed to serve the needs of the Open Chemistry projects. The format is intended to minimal, and easy to idenfity. It was also developed with the intent to allow extension, and the possibility of breaking changes.

# Basic Anatomy of a Chemical JSON file

Its layout and organization view a molecule as a set of arrays that describe various properties, for example the atomic numbers of the atoms are stored in an array of length N (where N is the number of atoms), and the '3d' coordinates are stored in an array of length 3N. All atom specific properties are stored in arrays in the 'atoms' object, and connectivity is stored in the 'bonds' object.

The key "chemical json" is expected with a value of 0 to represent the first version of the format. The reader and writer in Avogadro 2 expects some minimal content, but most keys are optional. Several examples are present in the repository, but at its core all that a Chemical JSON file is expected to contain is a list of atoms with coordinates.

## Minimal File

The exaample below shows the a minimal file. It has an atoms object with atomic numbers, and a coordinates block ("coords") that contains 3D coordinates.

    {
      "chemical json": 0,
      "atoms": {
        "elements": {
          "number": [  1,   6,   1,   1,   6,   1,   1,   1 ]
        },
        "coords": {
          "3d": [  1.185080, -0.003838,  0.987524,
                   0.751621, -0.022441, -0.020839,
                   1.166929,  0.833015, -0.569312,
                   1.115519, -0.932892, -0.514525,
                  -0.751587,  0.022496,  0.020891,
                  -1.166882, -0.833372,  0.568699,
                  -1.115691,  0.932608,  0.515082,
                  -1.184988,  0.004424, -0.987522 ]
        }
      }
    }

The coordinate block could use fractional coordinates with a unit cell instead. The most minimal file that Avogadro 2 can make use of contains the identifying key, the atomic numbers of the elements, and some coordinates. Most files also contain connectivity, but this is optional.

## Extended Example

The example below shows a typical example of the output of the Avogadro 2 program. In additional to the keys in the above file it contains bonds with connectivity, and order. It also contains a molecular name, and the InChI genreated for the molecule. The molecular formula is also present, and this example obviously has some duplication of data.

    {
      "chemical json": 0,
      "name": "Ethane",
      "inchi": "1/C2H6/c1-2/h1-2H3",
      "formula": "C 2 H 6",
      "atoms": {
        "elements": {
          "number": [  1,   6,   1,   1,   6,   1,   1,   1 ]
        },
        "coords": {
          "3d": [  1.185080, -0.003838,  0.987524,
                   0.751621, -0.022441, -0.020839,
                   1.166929,  0.833015, -0.569312,
                   1.115519, -0.932892, -0.514525,
                  -0.751587,  0.022496,  0.020891,
                  -1.166882, -0.833372,  0.568699,
                  -1.115691,  0.932608,  0.515082,
                  -1.184988,  0.004424, -0.987522 ]
        }
      },
      "bonds": {
        "connections": {
          "index": [ 0, 1,
                     1, 2,
                     1, 3,
                     1, 4,
                     4, 5,
                     4, 6,
                     4, 7 ]
        },
        "order": [ 1, 1, 1, 1, 1, 1, 1 ]
      },
      "properties": {
        "molecular mass": 30.0690,
        "melting point": -172,
        "boiling point": -88
      }
    }

[JSON]: http://www.json.org/ "JSON"
[BSON]: http://bsonspec.org/ "BSON"
[JSONRPC]: http://www.jsonrpc.org/specification "JSON-RPC 2.0"
[REST]: https://en.wikipedia.org/wiki/Representational_state_transfer "REST"
[openspec]: https://en.wikipedia.org/wiki/Open_specifications "Open specifications"
[CML]: http://www.xml-cml.org/ "Chemical Markup Language - CML"
