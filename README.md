Chemical JSON
=============

The JSON format is extremely simple, and there are good, fast parsers available
for a number of programming languages. We first started developing Chemical
JSON as a simple format for the Open Chemistry project, The development was
initially specified in an ad-hoc fashion [on the wiki][Chemical_JSON]. The
versions currently in use, and documented there were developed to facilitate
addition of property arrays mapped to atoms and/or bonds, they map quite
directly to Avogadro 2's in-memory representations, and are also stored
efficiently in BSON (the binary JSON representation used by MongoDB).

In parallel to this there has been development of JSON representations for
computational chemistry output from the NWChem code, continuing on from early
work to extend CML. We have started to explore the extension of Chemical JSON
for a number of applications, and methods for improving it by bringing in
JSON-LD concepts.

This repository is being set up to facilitate collaboration on the use of JSON
represntations for chemistry, and to document those representations more
formally than has been done in the past.

[Chemical_JSON]: http://wiki.openchemistry.org/Chemical_JSON
