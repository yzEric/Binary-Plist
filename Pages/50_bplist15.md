# - bplist15 -



Binary plist file has only two sections:

1. The header (an identifier of plist file + CRC)

2. Data: elements serialized



Header
----
The first 8 bytes are an identifier, equal to 'bplist15'

	magic number ("bplist")
	file format version

byte length of plist incl. header, an encoded int number object (as below)

	32-bit CRC (ISO/IEC 8802-3:1989) of plist bytes w/o CRC, encoded always as "0x12 0x__ 0x__ 0x__ 0x__", big-endian, may be 0 to indicate no CRC




Data
----
Second is all of the elements in the plist, encoded and concatenated.


How to Read 
----
 * Read Header
     - check magic number
     - check version
     - CRC ???
 * Read Data
 

How to write 
----
 * Write magic number
 * Write version
 * Write No CRC
 * Write Elements
 



bplist15 Dictionnary  
----
Length is the number of pair of key;value store in dictonnary

    DictFlag length [size] PrimitifObjectKey1 PrimitifObjectKey2 . . . PrimitifObjectKeyN    Object1 Object2 . . . ObjectN
