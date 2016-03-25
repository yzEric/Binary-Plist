# - Non-pritif types -


Dictionary
----
Before v10.5, for write keys can only be string   
Since v10.5, keys can be any primitifs


The byte length is twice the object length times the number of bytes per 
object reference. The encoding is as the concatenation of two encoded 
arrays, the first of keys and the second of values. The first value in the 
list of keys corresponds to the first value in the list of values, and so 
on.

    DictFlag length [size] RefOfKey1 RefOfKey2 … RefOfKeyN   RefOfObject1 RefOfObject2 … RefOfObjectN


</br>
Array  
----
The byte length is the object length times the number of bytes per object 
reference for this plist file, i.e. either one or two times the object 
length. Any object length is valid. The encoding is the concatenation of 
object reference numbers as unsigned, big-endian integers each encoded in 
the number of bytes per object reference for this plist file.  


</br>

SET
----
The type appeared in Mac OS v10.5
Not implemented yet by apple in Core Foudation  

*Extract of CF source file*  

	1100 nnnn -> nnnn is count, unless '1111' then int count follows 

Set  -> la balise est implémentée uniquemement en v15 depuis V10.8



ORDER SET
----
The type appeared in Mac OS v10.8
Not implemented yet by apple in Core Foudation  

*Extract of CF source file*

	1011 nnnn -> nnnn is count, unless '1111' then int count follows






