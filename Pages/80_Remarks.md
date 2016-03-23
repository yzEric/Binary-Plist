5. Remarks
=============

Binary plist use a system of references to minimize size of file. Indeed two élémets which has of the same subelements value can point a single subelement in binary file.

It should be possible to create a loop with elements elementA -> elementB -> elementA
