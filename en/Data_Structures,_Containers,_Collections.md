# Data Structures, Containers, Collections

│ **English (en)** │  **[français (fr)](</Data_Structures,_Containers,_Collections/fr> "Data Structures, Containers, Collections/fr")** │    
****

## Contents

  * 1 Introduction
  * 2 Run time library (RTL)
  * 3 Free Component Library (FCL)
    * 3.1 FCL-Base
    * 3.2 FCL-STL
  * 4 Lazarus/LCL
  * 5 3rd party
  * 6 See also



## Introduction

Most programs operate on data, either searching, sorting, iterating or simply insert and retrieve. Therefore, a means of **data structures, containers and collections** is required. 

Free Pascal ships with numerous data structures, at different levels (RTL, FCL) but there are also third party solutions offering such feature. 

## [Run time library (RTL)](<RTL.md> "RTL")

  * [Classes unit](<Classes_unit.md> "Classes unit")
    * [TList](<TList.md> "TList"): Manages a list of pointers, can search and sort the list, has event notification feature
    * [ TFPList](<TList.md> "TList"): Manages a list of pointers, can search and sort the list, without event notification feature, faster than TList
    * [TStrings](<TStrings.md> "TStrings"): Manages a list of strings, can search, split, save to / load from file, store objects, act like associative array, etc., is an abstract class
    * [TStringList](<TStringList.md> "TStringList"): TStrings descendant, implements abstract methods of TStrings, can sort, handle duplicates, has event notification feature, a concrete class
    * [TBits](</index.php?title=TBits&action=edit&redlink=1> "TBits \(page does not exist\)"): Manages a list of bits (0 or 1), useful for bit-map (not bitmap image format)
    * [TCollection](<TCollection.md> "TCollection") and TCollectionItem: Forms basic management of named items
  * [FGL (Free Generics Library)](<Generics.md> "Generics")
    * [TFPGList](</index.php?title=TFPGList&action=edit&redlink=1> "TFPGList \(page does not exist\)")
    * [TFPGObjectList](</index.php?title=TFPGObjectList&action=edit&redlink=1> "TFPGObjectList \(page does not exist\)")
    * [TFPGInterfacedObjectList](</index.php?title=TFPGInterfacedObjectList&action=edit&redlink=1> "TFPGInterfacedObjectList \(page does not exist\)")
    * [TFPGMap](<TFPGMap.md> "TFPGMap")
    * [TFPGMapInterfacedObjectData](</index.php?title=TFPGMapInterfacedObjectData&action=edit&redlink=1> "TFPGMapInterfacedObjectData \(page does not exist\)")
  * [Generics Collections (fully compatible with Delphi generics library)](<Generics.md> "Generics")
    * [TArray](</index.php?title=TArray&action=edit&redlink=1> "TArray \(page does not exist\)"): Static methods for searching and sorting
    * [TDictionary](</index.php?title=TDictionary&action=edit&redlink=1> "TDictionary \(page does not exist\)"): Key-value pairs, has event notification feature
    * [TObjectDictionary](</index.php?title=TObjectDictionary&action=edit&redlink=1> "TObjectDictionary \(page does not exist\)"): Key-value pairs with automatic freeing of objects if removed, has event notification feature
    * [TList](<TList.md> "TList"): List of items, can search, add, remove, reverse and sort the list, has event notification feature
    * [TObjectList](</index.php?title=TObjectList&action=edit&redlink=1> "TObjectList \(page does not exist\)"): List of objects with automatic freeing of objects if removed, can search, add, remove, reverse and sort the list, has event notification feature
    * [TObjectQueue](</index.php?title=TObjectQueue&action=edit&redlink=1> "TObjectQueue \(page does not exist\)"): Queue of objects with automatic freeing of objects if removed, has event notification feature
    * [TObjectStack](</index.php?title=TObjectStack&action=edit&redlink=1> "TObjectStack \(page does not exist\)"): LIFO (last in, first out) stack of objects with automatic freeing of objects if removed
    * [TQueue](</index.php?title=TQueue&action=edit&redlink=1> "TQueue \(page does not exist\)"): Queue, has event notification feature
    * [TStack](</index.php?title=TStack&action=edit&redlink=1> "TStack \(page does not exist\)"): LIFO (last in, first out) stack, has event notification feature
    * ~~TThreadedQueue~~ not yet implemented
    * [TThreadList](</index.php?title=TThreadList&action=edit&redlink=1> "TThreadList \(page does not exist\)"): Thread-safe list based on TList<T>
    * [TPair](</index.php?title=TPair&action=edit&redlink=1> "TPair \(page does not exist\)"): Record for key-value pair
    * Different comparers, enumerators and hashes available
    * Additional classes (unlike the Delphi Generics Collections) available 
      * [TAVLTree](</index.php?title=TAVLTree&action=edit&redlink=1> "TAVLTree \(page does not exist\)")
      * [TSortedSet](</index.php?title=TSortedSet&action=edit&redlink=1> "TSortedSet \(page does not exist\)")



## [Free Component Library (FCL)](<FCL.md> "FCL")

### FCL-Base

  * AVL_Tree unit 
    * TAVLTree and TAVLTreeNode; see [AVL Tree](<AVL_Tree.md> "AVL Tree")
  * Contnrs unit 
    * TFPObjectList
    * TObjectList
    * TComponentList
    * TClassList
    * TOrderedList
    * TStack
    * TObjectStack
    * TQueue
    * TObjectQueue
    * TFPHashList
    * TFPHashObjectList
    * TFPDataHashTable
    * TFPStringHashTable
    * TFPObjectHashTable
    * TBucketList
    * TObjectBucketList



### FCL-STL

  * GVector unit 
    * TVector: Implements dynamically self-resizing array, item order is based on insertion order
  * [GSet](<GSet.md> "GSet") unit 
    * TSet: Implements red-black tree backed set, sorted in a manner of your choosing
  * GHashSet unit 
    * THashSet: Implements hashtable backed set, no order is guaranteed
  * GStack unit 
    * TStack: Implements stack, LIFO (last in first out) order
  * GQueue unit 
    * TQueue: Implements stack, items are pushed from front, FIFO (first in first out) order
  * GPriorityQueue unit 
    * TPriorityQueue: Implements heap backed priority queue, order is based on given compare function
  * GDeQue unit 
    * TDeque: Implements double ended queue, items can be pushed and popped from either front or back
  * GMap unit 
    * TMap: Implements TSet (and therefore, red-black tree) backed map
  * GHashMap unit 
    * THashMap: Implements hashtable backed map
  * GTree unit 
    * TTree: Implements k-ary tree, supports depth first and breadth first traversal



## Lazarus/LCL

  * AvgLvlTree: extended versions of the FPC/FCL TAVLTree and TAVLTreeNode. See [AVL Tree](<AVL_Tree.md> "AVL Tree").



## 3rd party

project | license | generic | types | notes   
---|---|---|---|---  
[CL4L](<https://github.com/CynicRus/CL4L>) | Public Domain | no  | al, as, ll, v, hm, hs, q, s | Conversion of _The Delphi Container Library_. Provides algorithms like in STL (Apply, Found, CountObject, Copy, Generate, Fill, Reverse, Sort...)   
[heContnrs](<http://code.google.com/p/fprb/wiki/heContnrs>) | BSD3 | yes  | ... | Collection of generic Free Pascal containers: BTree, lists, vectors.   
[Fundamentals](<https://github.com/fundamentalslib/>) | BSD | no  | ... | GitHub account contains 2 versions of library: 4 and 5.   
[RBS AntiDOT](<http://sourceforge.net/projects/adot/>) | GPLv2 | ??  | ... | Library of containers and data structures for Pascal: Vectors, Maps, Sets, Lists and others.   
[LightContainers](<http://www.stack.nl/~marcov/lightcontainers.zip>), [GitHub mirror](<https://github.com/Alexey-T/FPC_light_containers>) | FPC | no+yes  | aasl | Generic and non-generic variants. Quite low-level.   
[StringHashMap](<StringHashMap.md> "StringHashMap") | MPL1.0 | ??  | ... | Port of code from JCL.   
[GContnrs](<http://yann.merignac.free.fr/unit-gcontnrs.html>) | LGPLv2 + linking exception | yes  | v, hm, hs, ll, q, dq, s, ts, tm, bs | Generic containers: doubly linked lists, dequeues, hash maps, hash sets, priority queues, queues, stacks, tree maps, tree sets, vectors and bitsets.   
[CL4fpc](<https://sourceforge.net/projects/cl4fpc/>) | GPLv2 | yes  | ... | Red-black tree, AVL tree, Decart tree, weight-balanced tree, hash-maps, FIFO, ResPool and others.   
[HAMT](<https://www.benibela.de/sources_en.html#hamt>) | LGPLv2 + linking exception | yes  | tm, ts | Immutable hash array mapped tree.   
[LGenerics](<LGenerics.md> "LGenerics") | Apache 2.0 | yes  | gr, s, q, dq, v, rb, av, hs, hm, tm, ts, ms, mm, bm, bs... | Collection of generic algorithms and data structures for FPC and Lazarus.   
[PascalContainer](<https://github.com/terrylao/PascalContainer>) | BSD | yes  | rb, q, dq, hm, tm | Generic data structure with B-Tree, B+Tree, B*Tree, T-Tree, HashMap, priority queue, red-black-Tree, AVL-tree, Quad-Tree, SkipList, LockFreeQueue.   
code | data type   
---|---  
al | array list   
as | array set   
rb | red-black tree   
av | AVL tree   
hm | hashmap   
hs | hashset   
ll | linked list   
q | queue   
dq | deque   
s | stack   
v | vector   
tm | treemap   
ts | treeset   
ms | multiset   
mm | multimap   
bm | bijective map   
bs | bitset   
gr | graph   
aasl | "array of array of T" based sorted list.   
  
## See also

  * [FreePascal hash maps comparsion](<http://www.benibela.de/fpc-map-benchmark_en.html>)
  * [Tomes of Delphi: Algorithms and Data Structures](<https://secondboyet.com/FixedArticles/DADSBook.html>)
  * [EZDSL: Easy Data Structures Library for Delphi](<https://github.com/jmbucknall/EZDSL>)

---

_Source: [https://wiki.freepascal.org/Data_Structures%2C_Containers%2C_Collections](https://web.archive.org/web/20240913224850/https://wiki.freepascal.org/Data_Structures%2C_Containers%2C_Collections)_
