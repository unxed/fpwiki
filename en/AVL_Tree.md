# AVL Tree

## Contents

  * 1 TAVLTree
  * 2 TAvgLvlTree
    * 2.1 Creating a tree
    * 2.2 Enumerating all items of a tree
      * 2.2.1 Enumerate from lowest to highest
      * 2.2.2 Enumerate from highest to lowest
      * 2.2.3 Delete all nodes that fits a condition
    * 2.3 Searching items
    * 2.4 Resorting, changing the order
  * 3 Associative arrays using AVL trees
  * 4 Indexed AVL Tree
  * 5 See also



# TAVLTree

Implemented in the FCL unit **avl_tree**. This is the predecessor of TAvgLvlTree. 

# TAvgLvlTree

Where to get: Unit AvgLvlTree of Lazarus package **LazUtils** of the Lazarus sources. 

The AVL trees - Average Level Trees are sorted self balancing binary trees. Similar to a list like TPFList a TAvgLvlTree can store arbitrary data (Pointer), but contrary to TFPList the tree is always sorted and balanced. Therefore searching is very fast. Features: 

  * You can define your own compare function or method.
  * Searching an item or key takes O(log(n))
  * Inserting an item takes O(log(n))
  * Deleting an item takes O(log(n))
  * Finding the lowest or highest item takes log(n)
  * Finding the sucessor or predecessor item takes O(log(n))
  * Iterating through all items in order takes O(n)
  * Enumerators for running from lowest to highest and highest to lowest
  * Supports duplicates
  * Keeps duplicates order stable.
  * It is not thread safe, but it does not use any global variables. So it can be used in threads just like TFPList.



## Creating a tree

By default the tree is sorted for the pointers. That means similar to TFPList you can add any pointers or objects: 
    
    
    uses AvgLvlTree; // in package lazutils
    
    Tree:=TAvgLvlTree.Create;
    Tree.Add(AnObject1);
    Tree.Add(AnObject2);
    ...
    // If you want to know if an item is already in the tree use Find:
    if Tree.Find(AnObject1)<>nil then
      writeln('AnObject is in the tree');
    ...
    // To remove an item from the tree use:
    Tree.Remove(AnObject1);
    

To create a tree for your custom data you only need a compare function. The following example demonstrates how to sort TMyData objects via their Filenames: 
    
    
    uses AvgLvlTree; // in package lazutils
    type
      TMyData = class
      public
        Filename: string;
        Content: string;
      end;
    
    function CompareMyData(Data1, Data2: Pointer): integer;
    begin
      Result:=CompareFilenames(TMyData(d1).Filename,TMyData(d2).Filename);
    end;
    
    ...
    var
      MyData1: TMyData;
      Tree: TAvgLvlTree;
    begin
      Tree:=TAvgLvlTree.Create(@CompareMyData);
    
      MyData1:=TMyData.Create;
      MyData1.Filename:='SomeFile';
      Tree.Add(MyData1);
    end;
    

## Enumerating all items of a tree

### Enumerate from lowest to highest
    
    
    var
      MyData: TMyData;
    begin
      for Node in Tree do begin
        MyData:=TMyData(Node.Data);
        writeln(MyData.Filename);
      end;
    end;
    

Notes: 

  * You can insert new nodes while enumerating with this enumerator.
  * You must not delete the Node, nor change the Node.Data using this enumerator.



### Enumerate from highest to lowest
    
    
    var
      MyData: TMyData;
    begin
      for Node in Tree.GetEnumeratorHighToLow do begin
        MyData:=TMyData(Node.Data);
        writeln(MyData.Filename);
      end;
    end;
    

### Delete all nodes that fits a condition

If you need full control over the enumeration you can use Node.**Sucessor** and Node.**Precessor**. Here is an example how to delete all nodes that fits a filter: 
    
    
    var
      Node: TAvgLvlTreeNode;
    begin
      Node:=Tree.FindLowest;
      while Node<>nil do begin
        NextNode:=Node.Successor;
        if TMyData(Node.Data).Flag=true then
          Tree.Delete(Node); // Note: this frees the Node, not the Node.Data. Use Tree.FreeAndDelete(Node) to free the Data.
        Node:=NextNode;
      end;
    end;
    

Note: 

  * TMyData.Flag is just an example property. You can check whatever you want.



## Searching items

You can search an item with the same key: 
    
    
    var
      Node: TAvgLvlTreeNode;
    begin
      MyData.Filename:='test';
      Node:=Tree.Find(MyData); // finds a node with a Filename = 'test'
                     // Note: The above function CompareFilenames works case insensitive under Windows
                     // so under Windows it can find a node with Filename 'TEST'
    end;
    

**Note:** **Find** will find a node where your compare function returns 0. 

You can also search directly with a filename, without creating a TMyData. The function FindKey takes as arguments a key (here: the filename) and a special compare function: 
    
    
    function CompareFilenameWithMyData(Filename, MyData: Pointer): integer;
    begin
      Result:=CompareFilenames(String(Filename),TMyData(MyData).Filename);
    end;
    
    ...
    var
      Filename: string;
      Node: TAvgLvlTreeNode;
    begin
      Filename:='Test';
      Node:=Tree.FindKey(Pointer(Filename),@CompareFilenameWithMyData);
    
    end;
    

## Resorting, changing the order

You can change the compare function. The tree will automatically rebuild itself. The Nodes will be destroyed and all Data will be added into new nodes. For example you can switch from comparing case sensitive to case insensitive: 
    
    
    type
      TMyData = class
      public
        Name: string;
        constructor Create(aName: string);
      end;
    
    constructor TMyData.Create(aName: string);
    begin
      Name:=aName;
    end;
    
    function CompareMyDataCaseSensitive(Data1, Data2: Pointer): integer;
    begin
      Result:=CompareStr(TMyData(Data1).Name,TMyData(Data2).Name);
    end;
    
    function CompareMyDataCaseInsensitive(Data1, Data2: Pointer): integer;
    begin
      Result:=AnsiCompareText(TMyData(Data1).Name,TMyData(Data2).Name);
    end;
    
    procedure Demo;
    var
      Tree: TAvgLvlTree;
      Node: TAvgLvlTreeNode;
    begin
      Tree:=TAvgLvlTree.Create(@CompareMyDataCaseSensitive);
      Tree.Add(TMyData.Create('ABC'));
      Tree.Add(TMyData.Create('Test'));
      Tree.Add(TMyData.Create('abc'));
      Tree.Add(TMyData.Create('test'));
      writeln('sorted case sensitive:');
      for Node in Tree do
        writeln('  ',TMyData(Node.Data).Name);
      // sort case insensitivie
      Tree.OnCompare:=@CompareMyDataCaseInsensitive;
      writeln('sorted case insensitive:');
      for Node in Tree do
        writeln('  ',TMyData(Node.Data).Name);
    end;
    

Output: 
    
    
    sorted case sensitive:
      ABC
      Test
      abc
      test
    sorted case insensitive:
      ABC
      abc
      Test
      test
    

# Associative arrays using AVL trees

An associative array is an array where instead of a the position an arbitrary key is used to find an item. For example a string. The unit AvgLvlTree implements several associative arrays using AVL trees: 

  * TStringToStringTree: string to string, you can choose case sensitive or case insensitive or provide your own string comparison function
  * TStringToPointerTree: As TStringToStringTree but to arbitrary pointers, which might be objects



Here is an example demonstrating how to use a TStringToStringTree: 
    
    
    uses
      AvgLvlTree;
    var
      NameToValue: TStringToStringTree;
      S2SItem: PStringToStringItem;
    begin
      // create
      NameToValue:=TStringToStringTree.Create(false); // false = Names are compared case insensitive via ''AnsiCompareText''
    
      // set values
      NameToValue['First'] := 'One';
      NameToValue['Second'] := 'Two';
    
      // get values
      writeln('First=',NameToValue['first']); // writes One
      writeln('Second=',NameToValue['second']); // writes Two
      writeln('Third=',NameToValue['third']); // third is not yet set, it writes the empty string
    
      // check if a key is defined
      writeln('First exists: ',NameToValue.Contains('first')); // gives true
      writeln('Third exists: ',NameToValue.Contains('third')); // gives false
    
      // enumerate all values
      for S2SItem in NameToValue do 
        writeln('Name=',S2SItem^.Name,' Value=',S2SItem^.Value);
    
      // free
      NameToValue.Free;
    end;
    

# Indexed AVL Tree

TIndexedAVLTree extends TAvgLvlTree with the ability to access the nodes via their index (0=lowest, Count-1=highest) similar to an array. In other words it is like an array that is always sorted. 
    
    
    uses
      AvgLvlTree;
    type
      TMyData = class
      public
        Name: string;
        constructor Create(aName: string);
      end;
     
    constructor TMyData.Create(aName: string);
    begin
      Name:=aName;
    end;
     
    function CompareMyDataCaseInsensitive(Data1, Data2: Pointer): integer;
    begin
      Result:=AnsiCompareText(TMyData(Data1).Name,TMyData(Data2).Name);
    end;
    
    var
      SortedArray: TIndexedAVLTree;
      Node: TIndexedAVLTreeNode;
    begin
      // create
      SortedArray:=TIndexedAVLTree.Create(@CompareMyDataCaseInsensitive);
      SortedArray.Add(TMyData.Create('Mars'));
      SortedArray.Add(TMyData.Create('Earth'));
      SortedArray.Add(TMyData.Create('Venus'));
    
      // access items via index
      for i:=0 to SortedArray.Count-1 do
        writeln(i,': ',TMyData(SortedArray[i]).Name);
    
      // access nodes via GetNodeAtIndex
      for i:=0 to SortedArray.Count-1 do
        writeln(i,': ',TMyData(SortedArray.GetNodeAtIndex(i).Data).Name);
    
      // get the index of a node
      i:=2;
      Node:=SortedArray.GetNodeAtIndex(i);
      writeln(i,'=',SortedArray.NodeToIndex(Node));
    
      SortedArray.FreeAndClear; // free nodes and objects
      SortedArray.Free; // free the tree
    end;
    

# See also

  * [Data Structures, Containers, Collections](<Data_Structures,_Containers,_Collections.md> "Data Structures, Containers, Collections") Alternative data structures

---

_Source: [https://wiki.freepascal.org/AVL_Tree](https://web.archive.org/web/20241206065750/https://wiki.freepascal.org/AVL_Tree)_
