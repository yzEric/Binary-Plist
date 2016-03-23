3.1 Singleton
---------

The most easy object to write and parse, slightly different from the other type.

Type number 0000 (0x0), the object length is actually the value of the object, they always use only 1 bytes.

  __3.1.1 null: 0000 0000__   
	I don't know what null is for.

  __3.1.2 False: 0000 1000__  
	Boolean value

  __3.1.3 True: 0000 1001__  
	Boolean value

  __3.1.2 fill: 0000 1110__   
	fill byte, I don't know what fill is for.


3.2 URL
-------
Apparition de la balise dans V10.8
Not implemented yet by apple in Core Foudation

_Extract of CF source file_

	0000 1100 -> URL with no base URL, recursive encoding of URL string
	0000 1101 -> URL with base URL, recursive encoding of base URL, then recursive encoding of URL string


 
  
  
3.3 Number
-----------
Integer, [size], UID, UUID, and real have an analogue structure:  



__3.3.1 Integer: 0001 0nnn__  
*4 bits for ID + 0nnn for legth + # of bytes is 2^nnn for integer, big-endian bytes data (big-endian bytes)*  



0nnn  | 10 base  |  Length in bytes  | Type
------|----------|-------------------|----------------------|
0000  |      0   |     1             |     8bits unsigned   |
0001  |      1   |     2             |    16bits unsigned   |
0010  |      2   |     4             |    32bits unsigned   |
0011  |      3   |     8             |    64bits unsigned   |
0100  |      4   |    16             |   128bits signed     |
0101  |      5   |    32             |   256bits signed     |
0110  |      6   |    64             |   512bits signed     |
0111  |      7   |   128             | 1 024bits signed     |


	1, 2, and 4-byte integers have to be interpreted as unsigned
	8-byte integers are signed (and 16-byte when available)
	negative 1, 2, 4-byte integers are always emitted as 8 bytes
	

Apple CF allways use power of two for write length but can read any length with previous limitations. (`sign or unsign???`)

To ensure maximum compatibility with non apple implementations, it is better at writing to use power of 2 lengths.

	

De même, les 3 bits sont suffisant pour indiquer un nombre suppérieur à 64bits, to insure compatibility all CF versions can read any length but `only the last 64 bits are significant currently`.


Et ca c'est pour quoi???  

```
    typedef struct { int64_t high;  uint64_t low;  } CFSInt128Struct;  
    enum {  kCFNumberSInt128Type = 17  };  
```  






Since Mac OS X.8 number of bytes that can be use for length has been reduce to 3, frist must be 0.

En effet, dans son implémentation apple se limite à toujours utilisé 1, 2, 4, 8-byte integers, donc 3 bits sont suffisant.(1)
(On pourait même se comptenter de 2 bits en changeant la règle des longueurs, 0000: 1 byte - 0001: 2 byte - 0010: 4 byte - 0011: 8: byte)


</br>
###3.3.2 [size]: 0001 0nnn  
This object have the same ID as integer, [size] is only use to express an element length superior to 15.
Encoding is as integers, except values are always unsigned. 
 
 
  

###3.3.5 Real: 0010 0nnn  
*4 bits for ID + 0nnn + # of bytes is 2^nnn for real data (big-endian bytes)*
Since Mac OS X.8 number of bytes that can be use has reduce to 3, frist must be 0.
	The object length to byte length conversion is the same as for integers. 
	The object length is 2 or 3, corresponding to a byte length of 4 or 8. 
    The encoding is as a big-endian, single-precision or a double-precision float,
    accordingly.

    `a vérifer dans les sources CF`


0nnn  | 10 base  |  Length in bytes  |   Name
------|----------|-------------------|---------------------|
0000  |      0   |     1             | 8bits Minifloat     |
0001  |      1   |     2             | Half (16bits)       |
0010  |      2   |     4             | Single (32bits)     |
0011  |      3   |     8             | Double (64bits)     |
0100  |      4   |    16             | Quadruple (128bits) |
0101  |      5   |    32             |            256bits  |
0110  |      6   |    64             |            512bits  |
0111  |      7   |   128             |          1 024bits  |
 



3.4 UID: 1000 0nnn  
-------------
*4 bits for ID + 0nnn + nnnn+1 is # of bytes for UID, big-endian bytes data (big-endian bytes)*  
Encoding is as integers, except values are always unsigned.  

 *UID = Unique ID*   
They are use to indentify and find objects in programmes. 



3.5 UUID: 0000 1110  
-------------
*4 bits for ID + 1110 (=16) + 16 bytes for UUID data (big-endian bytes)*

Apparition de la balise dans V10.8
Encoding is as integers, except values are always unsigned and length is fix (16 byte).
16-byte integer unsigned 


*Extract apple documentation[^1]*  

[^1]: https://developer.apple.com/library/mac/documentation/CoreFoundation/Reference/CFUUIDRef/Reference/reference.html

```
UUIDs (Universally Unique Identifiers), also known as GUIDs (Globally Unique Identifiers) or IIDs (Interface Identifiers), are 128-bit values designed to be unique.

```






###3.6 Date

0011 0011 + 8byte

Dates are stored as a float with a value of seconds since the epoch of 1 
January 2001, 0:00:00 GMT. Encoding is the same as the encoding for floats,
except that the object length is always 3, for a byte length of 8.



###3.7 Binary Data


0100 nnnn + [size] + ..

The byte length is the object length, and any value is valid. The bytes are 
not interpreted.



###3.8 String


####3.8.1 Single Byte String  

The byte length is the object length, and any value is valid.
Encoding is ASCII.


####3.8.2 Byte String  

The byte length is twice the object length, and any value is valid.
The encoding is utf-16 (big endian).

