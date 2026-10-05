# StringHashMap

## Contents

  * 1 About
  * 2 Author
  * 3 License
  * 4 Download
  * 5 Change Log
  * 6 Dependencies / System Requirements
  * 7 Installation
  * 8 The HashMapDemo1 Example Application
  * 9 See also



### About

StringHashMap is a string -> pointer associative map container. It is implemented in a fast and memory efficient way. The search time of a hash map is constant, it does not depend on the amount of data (assuming there is enough memory and no swapping). Millions of elements is no problem. 

The unit name is StrHashMap.pas and it was modified from unit JclStrHashMap.pas which is part of Jcl library. 

The purpose was to make it compile and work with FPC (Free Pascal Compiler). In the process lots of code with conditional compilation directives was removed and also many unit dependencies were removed. 

The download contains: 

  * Source unit file
  * Comprehensive demo program which builds a database of South Park episodes and then searches them swiftly using a hash map lookup :-)
  * README.txt file which basically tells you about this document and about the demo program.



### Author

[Juha Manninen](</User:JuhaManninen> "User:JuhaManninen") <juha dot manninen (at) phnet dot fi>

\+ the original authors of Jcl code. 

### License

This code is licensed under the same terms as Jcl library. 

### Download

The latest stable release can be found on GitHub: <http://github.com/JuhaManninen/Pascal>

There is a "Download Source" button available. 

### Change Log

  * Version 1.0 2009-11-25



### Dependencies / System Requirements

  * None. Pure Pascal.



Status: Stable 

Issues: It turned out there are about similar container classes in FPC's libraries so this one is not published in CCR. I published it in GitHub instead. 

### Installation

Just include unit StrHashMap in the "uses" section. Set path for it in project settings if needed. 

### The HashMapDemo1 Example Application

To install and run: 

  * From Example directory open HashMapDemo1.lpi
  * Build it
  * Run it at console so that you can see the output.
  * Inspect the source code based on the output.



The example application first builds a database of South Park episodes and then demonstrates the following features of StringHashMap: 

  * Search data records with lightning speed using hash keys.
  * Array syntax: Map[Key]
  * Find function: Map.Find(Key, Pointer)
  * FindData function: Map.FindData(Pointer, Key)
  * Iterating elements
  * Using the data pointer as integer, making a very efficient word counter.



### See also

  * [Data Structures, Containers, Collections](<Data_Structures,_Containers,_Collections.md> "Data Structures, Containers, Collections") Overview of other hash list/data structure implementations
  * [FreePascal hash maps comparsion](<http://www.benibela.de/fpc-map-benchmark_en.html>)

---

_Source: [https://wiki.freepascal.org/StringHashMap](https://web.archive.org/web/20250513074351/https://wiki.freepascal.org/StringHashMap)_
