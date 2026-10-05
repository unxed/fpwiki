# GSet

## Contents

  * 1 TSet
    * 1.1 A few notes
    * 1.2 Performance
    * 1.3 See Also



## TSet

TSet provides a data structure that will store, sorted, your data using a Red / Black Tree. This approach is said to be a little faster when making many changes to the data in the tree compared to, for example, an [AVL Tree](<AVL_Tree.md> "AVL Tree"). 

The TSet (and several other data structures) were contributed to FPC in 2012 and appears to have had little attention since then. Documentation is scant, see below. 

The idea with TSet (and many similar data 'frameworks') is that you provide a sorting function that is aware of the nature of your data and, as you add or search for data to the structure, it uses you function to navigate around the tree. This approach is much faster than iterating over a linear list for example. 

In use, you may do something like this - 
    
    
    program TSetDemo;
    {$mode objfpc}{$H+}
    {$modeswitch advancedrecords} 
    uses
        SysUtils, gvector, gset;        
        
    type
        PNote=^TNote;
        TNote = record		// This is our data
            Title : string;
            NoteSize   : integer;
        end;   
          
    type
        TLess = record
            class function c(L, R: PNote): Boolean; static;
        end;
    
    type
        TNoteSet = specialize TSet<PNote, TLess>;    
             
    
    // ------------------------
        
    var
        NoteSet : TNoteSet;
    	    
        
    class function TLess.c(L, R: PNote): Boolean;       // Our sorting function
    begin
        Result := L^.Title < R^.Title;
    end;              
        
        
    procedure AddItemToSet(Title : string; NoteSize : integer);
    var
        SomeData: PNote;
    begin
        new(SomeData);
        SomeData^.NoteSize := NoteSize;
        SomeData^.Title := Title;
        NoteSet.Insert(SomeData);
    end;  
    
                                     
    procedure TestTSet();
    var
        it : TNoteSet.TIterator;
        pMin: TNoteSet.PNode;
    begin
        NoteSet := TNoteSet.Create;
        AddItemToSet('abc', 22);   
        AddItemToSet('abc', 100);     // Note, a duplicate !
        AddItemToSet('xyz', 700);
        AddItemToSet('ghi', 3125);
        
        it := NoteSet.Min;                  
        if it <> nil then begin
            pMin := it.FNode;             // So we can come back to it.
            repeat
                writeln(it.GetData^.Title + ' ' + inttostr(it.GetData^.NoteSize));
            until not it.Next;
            
            // OK, here we Free or Dispose as appropriate.
            it.FNode := pMin;
            repeat
                dispose(it.GetData);
            until not it.Next;
        end;
        it.Free;
        NoteSet.free;
    end;                                   
    
    begin
        TestTSet();
    end.
    

### A few notes

  * Obviously, you can store what ever you want in the data record. Its up to you to create it and, importantly, free it.
  * In this example we create an iterator and, for convenience, we set it back to first item and use it again. We could free and recreate it as an alternative and, if the data has changed, we must do that. Important, that call to NoteSet.Min allocates memory, you must free it before recalling or finishing up.



### Performance

While a Red Black Tree is reported to be faster when altering data, my tests do not show an appreciable improvement over the TAvgLvlTree. Your results may vary and if its important, do some tests of your own. I, personally, consider the TAvgLvlTree is just a little more comfortable in the FPC setting, it uses a number of conventions that we tend to expect and is likely to be better supported by the FPC community. 

Thanks to AVK in the forum, without who's help I would have walked away from this topic. 

### See Also

  * [Data_Structures,_Containers,_Collections](<Data_Structures,_Containers,_Collections.md> "Data Structures, Containers, Collections")
  * <https://forum.lazarus.freepascal.org/index.php/topic,56580.0.html>
  * <http://history.ioinformatics.org/conf/c6_boza.pdf>
  * <https://ioinformatics.org/journal/INFOL101.pdf>
  * <https://en.wikipedia.org/wiki/Red_black_tree>

---

_Source: [https://wiki.freepascal.org/GSet](https://web.archive.org/web/20250121223853/https://wiki.freepascal.org/GSet)_
