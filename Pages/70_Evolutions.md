# - Versions -

A short history of the evolution of the format established by studying the source code published by Apple
    
10.2 - Jaguar
-----
*No sources on opensource.apple.com & no mac with this version at my disposal*
	

10.3 - Panther / CF299
-----
*My start point*

At write, keys of a dcitonnary can only be ASCIIString or Unicode16String, this limitation does not apply to reading.
  
 
10.4 - Tiger / CF368
----
Many changes in source file but it seems that there is in/out data remain unchanged

10.5 - Leopard / CF???
----
*No sources on opensource.apple.com & no mac with this version at my disposal*
    
10.5.1 - Leopard / CF???
----
*No sources on opensource.apple.com & no mac with this version at my disposal*
    
10.5.2 - Leopard / CF476.10
----
- Add of Trailer.offsetTable in file format description (this element was used in previous versions but was not documented)

- Keys of a dictionnary can now be any primitif object.

- Set  -> A new type is describe but is not implemented

- can read file with magic number bplist00 or bplist01 (seems to continue write bplist00, i found no version who write bplist01)


- Source code contains a better description of integers format:  
*Extract of CF source file*  
    
```
in format version '00', 1, 2, and 4-byte integers have to be interpreted as unsigned,
whereas 8-byte integers are signed (and 16-byte when available) 
negative 1, 2, 4-byte integers are always emitted as 8 bytes in format '00'
integers are not required to be in the most compact possible representation, but only the last 64 bits are significant currently
```

    typedef struct { int64_t high; uint64_t low; } CFSInt128Struct;  
    enum { kCFNumberSInt128Type = 17 };

       
10.6 - Snow Leopard
----
    // Tiger and earlier will parse "bplist00"
    // Leopard will parse "bplist00" or "bplist01"
    // SnowLeopard will parse "bplist0?" where ? is any one character

    // In Leopard, the unused bytes in the trailer must be 0 or the parse will fail
    // This check is not present in Tiger and earlier or after Leopard


10.7 - Lion
----
    No change in format and implementation
    

10.8 - Mountain Lion  
----    

* New binary format : v1.5  
The core foundation source code contains methodes for read and write in this new format but it doesn't seem to be public (no documentation and no command line tool for conversion)

* Another future binary format  
  Same format as v1.5 with minor change in header format:  
       - an integer object in header indicate length of plist incl. header
       - 32-bit CRC  

  <br/>
* Few change in types  

  Status   | Name   | 0000 1100  | Info  
---------- | ------ | ---------- | ---------------  
	New    |url     | 0000 1100  |  only for v1.5 or + (w: l.564 / r: l.)  
    New    |url     | 0000 1101  |  only for v1.5 or + (w: l.564 / r: l.) 
    New    |uuid    | 0000 1110  |  only for v1.5 or + (w: l.557 / r: l.) 
    New    |ordset  | 1011 nnnn  |  only for v1.5 or +, not implemented (for read or write ) in 10.8 (w: l.555 / r: l.) 
    Chg    |int     | 0001 0nnn  |  0001 nnnn -> 0001 0nnn (l.239) because first "n" byte wasn't use, no change in implementation (maybe to reserve 0001 1nnn to a future type)
    Chg    |real    | 0010 0nnn  |  0010 nnnn -> 0010 0nnn (l.240) because first "n" byte wasn't use, no change in implementation (maybe to reserve 0010 1nnn to a future type)
    Imp    |set     | xxxx xxxx  |  implementation for v1.5 format (w: l.528 / r: l.)
      	




10.9 - Mavericks  
----

All code for binary format : v1.5 has beem remove excepting an unsed test function (l.1584).  



  Status   | Name   | 0000 1100  | Info  
---------- | ------ | ---------- | ---------------  
	New    |string  | 0111 nnnn  |  only for v1.5 or +, not implemented (for read or write ) in 10.9
