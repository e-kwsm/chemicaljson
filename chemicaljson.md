# Introduction

The JSON format is a general container, and the work done in Chemical JSON was primarily aimed at developing a standard on top of the [JSON][JSON] (and [BSON][BSON]) format for chemical data. It was developed primarily to serve the needs of the Avogadro 2 application, but also to facilitate efficient storage in MongoDB (in the form of BSON), and to exchange data using [JSON-RPC 2.0][JSONRPC] and [RESTful APIs][REST]. It has since been used in web applications, and with 3DMol.js. The early format was documented on the [Chemical JSON][CJSONWiki] wiki page.

## Purpose

The purpose here is to document the format, provide an [open specification][openspec], establish what is required or optional, and to provide a living specification as we extend the format. There is other work in the [NWChem JSON][nwchemjson] project to develop a format that can be used to replace traditional log files in NWChem, and very early ideas on how to integrate JSON-LD into these JSON formats in order to offer a semantically rich format. This will ideally reuse some of the previous work done in the [CML format][CML] for XML.

## Motivation

The format was developed in a very pragmatic manner, and was primarily developed to serve the needs of the Open Chemistry projects. The format is intended to minimal, and easy to identify. It was also developed with the intent to allow extension, and the possibility of breaking changes.

# Basic Anatomy of a Chemical JSON file

Its layout and organization view a molecule as a set of arrays that describe various properties, for example the atomic numbers of the atoms are stored in an array of length N (where N is the number of atoms), and the '3d' coordinates are stored in an array of length 3N. All atom specific properties are stored in arrays in the 'atoms' object, and connectivity is stored in the 'bonds' object.

The key 'chemicalJson' is expected with a value of 1 to represent the second version of the format (the first version used the key of 'chemical json' and a value of 0). The major revision from version 0 to 1 was a switch from the use of spaces to camel case. The reader and writer in Avogadro 2 expects some minimal content, but most keys are optional. Several examples are present in the repository, but at its core all that a Chemical JSON file is expected to contain is a list of atoms with coordinates.

## Top Level Keys

The 'chemicalJson' key and 'atoms' object are considered required. Other top-level keys/objects are optional. Common top level keys include 'name' that specifies a name for the molecule, 'inchi' that contains the InChI of the molecule, 'inchikey' that contains the InChI key, and 'formula' that contains the space separated molecular formula.

## Atoms Object

The 'atoms' object is at the root level, and is minimally expected to have an 'elements' object with an array called 'number', and a 'coords' object with an array called '3d'. If a 'unitCell' object is present at the root level than the 'coords' object is expected to contain an array called '3dFractional' that contains the fractional coordinates rather than the 3D Cartesian coordinates.

Coordinates are expected to be in Angstrom units, and the atomic numbers are expected to correspond to the proton number of the atom represented. The optional array 'selected' specifies which atoms are considered selected. An optional array in elements named 'symbols' may contain the symbols, e.g. 'C' for carbon, and an array named 'ids' contains unique string identifiers for each atom.

## Bonds Object

The 'bonds' object is at the root level, and if present it is expected to contain a 'connections' object with an 'index' array of length 2N specifying the index of the start and end atom of each bond, and an 'order' array of length N that specifies the order of the bond. The connections object may optionally contain an 'ids' array that specifies the start and end unique text identifier. Bonds should only be specified once. If atom 0 is bonded to atom 1 it is up to the program to account for the implicit bond from atom 1 to atom 0.

## Unit Cell

The 'unitCell' object is at the root level, and if present specifies the unit cell. The keys 'a', 'b', 'c', 'alpha'
, 'beta', and 'gamma' are expected to specify the unit cell. It a unit cell is present then the atoms are expected to have their coordinates supplied as fractional coordinates.

## Properties Object

They 'properties' object is at the top level, and if present contains key value pairs corresponding to molecule properties, such as the molecular mass, melting point, boiling point, charge, etc.

## Molecular Orbitals

The 'orbitals' object contains some electronic structure for the molecule if present. The 'energies' array has an energy for each orbital, and the 'occupations' array specifies how many electrons occupy the orbital, and 'moCoefficients' specifies the coefficients for the molecular orbital.

## Vibrations

The 'vibrations' object contains a 'frequencies' array with the frequency of each mode, an 'intensities' array with the intensity of each mode, a 'modes' array with a numeric identifier for the mode, and an 'eigenvectors' array that contains an array of eigenvectors for each vibration.

## Minimal File

The example below shows the a minimal file. It has an atoms object with atomic numbers, and a coordinates block ("coords") that contains 3D coordinates.

    {
      "chemicalJson": 1,
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

The example below shows a typical example of the output of the Avogadro 2 program. In additional to the keys in the above file it contains bonds with connectivity, and order. It also contains a molecular name, and the InChI generated for the molecule. The molecular formula is also present, and this example obviously has some duplication of data.

    {
      "chemicalJson": 0,
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
        "molecularMass": 30.0690,
        "meltingPoint": -172,
        "boilingPoint": -88
      }
    }

[CJSONWiki]: http://wiki.openchemistry.org/Chemical_JSON "Chemical JSON"
[JSON]: http://www.json.org/ "JSON"
[BSON]: https://bsonspec.org/ "BSON"
[JSONRPC]: https://www.jsonrpc.org/specification "JSON-RPC 2.0"
[REST]: https://en.wikipedia.org/wiki/Representational_state_transfer "REST"
[openspec]: https://en.wikipedia.org/wiki/Open_specifications "Open specifications"
[CML]: http://www.xml-cml.org/ "Chemical Markup Language - CML"
[nwchemjson]: https://github.com/wadejong/NWChem-Json
