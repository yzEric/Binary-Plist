3. Object encoding
===============


**ID:**  

The first four bits are an id number of the object type.



**SIZE:**  

_- For singletons:_   
seconds four bites directly express the value of element. 


_- For other types:_  
second four bits are the size of the object excepting.
If the given size is equal to 15, this means that the true object size is greater than 
can be expressed in four bits. In this case, the next byte is the start of the true size,
encoded like the `integer` objects, except the value is unsigned in this documentation
i call it `[size]` for disambiguitize.  

Convertion from size to number of bytes or objects depende of type.
for exemple: nnn, nnnn+1, 2^nnn …



**VALUE:**  

_- For primitif types:_  
Next bytes are the value of the element

_- For non-primitif types:_  
Next bytes gives the index of sub items in the offset tables, number of bytes per index is given in the trailler.

   

The following table gives few informations about each data type, details are given after: 

Name        | Format                   | Description                                            
------------|--------------------------|-----------------------------
null        | 0000 0000	                | null object
bool false  | 0000 1000	                | False                                                   
bool true   | 0000 1001	                | True                                    
url         | 0000 1100	                | URL with no base URL, recursive encoding of URL string
url         | 0000 1101	                | URL with base URL, recursive encoding of base URL, then recursive encoding of URL string
uuid        | 0000 1110	                | 16-byte UUID
fill        | 0000 1110	                | fill byte                                               
int         | 0001 0nnn + …	            | # of bytes is 2^nnn, big-endian bytes
[size]      | 0001 0nnn + …	            | # of bytes is 2^nnn, big-endian bytes
real        | 0010 0nnn + …	            | # of bytes is 2^nnn, big-endian bytes
date        | 0011 0011 + 8byte        | 8 byte float follows, big-endian bytes
data        | 0100 nnnn + [size] + ... | nnnn is number of bytes unless 1111 then int count follows (see [size]), followed by bytes
string      | 0101 nnnn + [size] + ... | ASCII string, nnnn is # of chars, else 1111 then int count (see [size]), then bytes
string      | 0110 nnnn + [size] + ... | Unicode string, nnnn is # of chars, else 1111 then int count (see [size]), then big-endian 2-byte uint16_t
'           | 0111 xxxx                | _unused_                                       
uid         | 1000 nnnn + ...          | nnnn+1 is # of bytes                                    
'           | 1001 xxxx                | _unused_                                             
array       | 1010 nnnn + [size] + ... | nnnn is count, unless '1111' then int count follows (see [size])                  
order set   | 1011 nnnn + [size] + ... | nnnn is count, unless '1111' then int count follows (see [size])                  
set         | 1100 nnnn + [size] + ... | nnnn is count, unless '1111' then int count follows (see [size])                  
dict        | 1101 nnnn + [size] + ... | nnnn is count, unless '1111' then int count follows (see [size])                  
'           | 1110 xxxx                | _unused_                                             
'           | 1111 xxxx                | _unused_                                             


( * ) -> minor change since this version  
( ~ ) -> pas totalement sur, pas de code source pour confirmer la version exact.

