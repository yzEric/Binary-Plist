# - Example -

This part illustrate how read and write a plist file

Content of the demo file
----

```
  Root
    Dictionnary
      Key: ID   / Value: 100
      Key: Name / Value: abcde
```


</br>
_If you are familar to xml Plist:_  

```
<plist version="1.0">
<dict>
	<key>ID</key>
	<integer>100</integer>
	<key>name</key>
	<string>abcde</string>
</dict>
</plist>
```



</br>

Binary file in a hex editor:
----
The figure below shows the file data in a hexadecimal editor.


(https://raw.githubusercontent.com/yzEric/Binary-Plist/master/Ressources/exemple-0b.png)


Fisrt bytes (bplist00) show that it is a Plist binary file, the text is aissement identifiable, however the rest is relatively difficult to identify.

| _Figure 1: Hex Data_     |
| :----------------------: |
| ![exemple-b]             |



[exemple-b]: https://raw.githubusercontent.com/yzEric/Binary-Plist/master/Ressources/exemple-0b.png



</br>
Binary file in a hex editor with data highlight:
----
The figures below show the file data in Synalyze It! Pro with a plist grammar.   

| _Figure 2: Hex Data in Synalyze It_     |
| :-------------------------------------: |
| ![exemple-a]                            |
| ![exemple-c]                            |

[exemple-a]: https://raw.githubusercontent.com/yzEric/Binary-Plist/master/Ressources/exemple-0a.png

[exemple-c]: https://raw.githubusercontent.com/yzEric/Binary-Plist/master/Ressources/exemple-0c.png

</br>
Content of files:
----
The table below gives details of each file elements

Offset | Hex               | Value     | Description
------:|-----------------: |----------:|-------------------
     0 | 62 70 6C 69 73 74 |    bplist | Magic Number
     6 |             30 30 |        00 | Version 
     8 |                D2 | 1101 0010 | Dictionnary with 2 items
     9 |                01 |         1 |  _index of key A_
    10 |                02 |         2 |  _index of key B_
    11 |                03 |         3 |  _index of item for key-A_
    12 |                04 |         4 |  _index of item for key-B_
    13 |                54 | 0101 0100 | String, 4 chars
    14 |       6E 61 6D 65 |      name |  _value_
    18 |                52 | 0101 0010 | String, 2 chars
    19 |             49 44 |        ID |  _value_
    21 |                55 | 0101 0101 | String, 5 chars 
    22 |    61 62 63 64 65 |      abcd |  _value_
    27 |                10 | 0001 0000 | Interer, 2^ Hx0000 = 1
    28 |                64 |       100 |  _value_
    29 |                08 |         8 | Offset of item index 0
    30 |                0D |        13 | Offset of item index 1
    31 |                12 |        18 | Offset of item index 2
    32 |                15 |        21 | Offset of item index 3
    33 |                1B |        27 | Offset of item index 4
    34 |     0000 00000000 |           | Unused
    35 |                01 |         1 | Size of integer for item index
    36 |                01 |         1 | Size of integer for offsets      
    44 | 00000000 00000005 |         5 | Number of items
    50 | 00000000 00000000 |         0 | Index of the top level item
    58 | 00000000 0000001D |        29 | Offset of the offset table 






## How to Read 

 * Read header : check magic number and version
 * Jump to trailer an reade it
 * Move to offset table at index of top level  element indicate in trailler
 * Read offset of top level element move to this position
 * Read of top level element can strat, it must be a non primitif element


## How to write 
In input, the master element that contain data structure to store in plist file

 * Write the header  
 * Give a unique index to each element of the structure, element who have same value must have the same index excepting singletons  
 * Write data of elements  
 * Write offset table  
 * Write trailler



