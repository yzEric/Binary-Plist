# - Main parts of a file -

Part         | Description
------------ | ------------------------------
Header       | 8 bytes: to identify plist file and version
True data    | Plist items
Offset table | Indicate the offset of each element
Trailer      | 32 bytes: give size of numbers (offset…)



</br>
Header
----
The first 8 bytes are an identifier, equal to 'bplistXX' where XX is two digits. 'bplist00' and 'bplist01' are known, I don't know the differences (it seems that all sources CFfiles write 00).

	magic number ("bplist")
	file format version

*Information from Core Foundation sources files:* [sources](#anchor1)

    Tiger and earlier will parse "bplist00"
    Leopard will parse "bplist00" or "bplist01"
    SnowLeopard will parse "bplist0?" where ? is any one character

*So for compatibility, if you try to wrire a plist file, it is probably better to always use bplist00*

</br>
Data
----
Second parts of the file is composed of all of the elements in the plist, encoded and concatenated.
Non primivite Elements like array and dictonnary that refer to other elements indicate the index of these elements in the offset table.


When writing a binary plist file, any values that repeat within the file can  be encoded only once and that single object referenced where ever that value repeats. It permit to reduce the size of file, this can be do applie to all type of object but it's not an obligation, Apple don't use it fot boolean.


it can be very slow for non primitif element, so apple use it at write only for primitif element.


</br>
Offset table
----
Third is the offset table, it's a concatenation of the offsets of all of the elements in the plist, each offset given as an unsigned integer in a fixed number of bytes. A non primitif object in the plist has a reference number that is based on the 0-based indexing of this table, e.g. object number 0 is the object at the offset given first in this table.

It also permite to skip part of document by jumping elements of a non primitive type item.


</br>
Trailer
----

The final 32 bytes of a binary plist file have the following format:


A simple table looks like this:

Data                                         | Offset | Length  | Data structure
:------------------------------------------- | ------ | --------| -------------
Unused                                       | 0      | 6       | 48-bit
byte size of offset ints in offset table     | 6      | 1       | 8-bit unsigned integer
byte size of object refs in arrays and dicts | 7      | 1       | 8-bit unsigned integer
Number of objects                            | 8      | 8       | 64-bit unsigned integer
Top level object index in offset table       | 16     | 8       | 64-bit unsigned big endian integer
Offset of offset table                       | 24     | 8       | 64-bit unsigned big endian integer
_Sum_                                        |        | _32_    | _256-bit_


 
- 6 bytes unsed  
*Extract apple documentation[^1]*  

      In Leopard, the unused bytes in the trailer must be 0 or the parse will fail  
      This check is not present in Tiger and earlier or after Leopard
  
- 1 byte integer which is the number of bytes for an offset value in offset table. 
     Offset values are encoded as unsigned, big endian integers. 
     Valid values are 1, 2, 3, or 4. -> A vérifier  
     Must be wide enough to encode the offset of the offset table, 
     not just the highest object offset. -> A vérifier, je vois pas bien pourquoi???
     
- 1 byte integer which is the number of bytes for an object reference number.
     Reference numbers are encoded as unsigned, big endian integers. 
     Valid values are 1 or 2.  -> A vérifier  
       
- 8 byte integer which is the number of objects in the plist
  
  
- 8 byte integer which is the reference (index) of the root object in offset table.
   This is usually zero. (Allways 0 in Core Foundation implementation)
     
- 8 byte integer which is the number of bytes for an offset value. 
     Offset values are encoded as unsigned big endian integers.
     Valid values are 1, 2, 3, or 4. 
     Must be wide enough to encode the offset of the offset table,  
     not just the highest object offset.-> A vérifier, je vois pas bien pourquoi???  
  
- a 4 byte integer which is the offset in the file of the start of the 
     offset table, named above as the third element in a binary plist.
     



