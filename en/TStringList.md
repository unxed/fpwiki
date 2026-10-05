# TStringList

│ **English (en)** │

A **TStringList** is a [datatype](<Data_type.md> "Data type") that can hold an arbitrary length list of [strings](<String.md> "String"). The strings in a TStringList are accessible as concatenated plain text or as a series of strings. Functionality is also provided for key-value pair access. 

Inheritance

  * [TObject](<TObject.md> "TObject") \- Base [class](<Class.md> "Class") of all classes. 
    * [TPersistent](</index.php?title=TPersistent&action=edit&redlink=1> "TPersistent \(page does not exist\)"), [IFPObserved](</index.php?title=IFPObserved&action=edit&redlink=1> "IFPObserved \(page does not exist\)") \- Base class for streaming system and persistent properties - Interface implemented by an object that can be observed. 
      * [TStrings](<TStrings.md> "TStrings") \- Class to manage arrays or collections of strings 
        * **TStringList** \- Standard implementation of the TStrings class.



TStringList adds sorting functionality to [TStrings](<TStrings.md> "TStrings") by adding properties `Sorted`, `Duplicates` and [`CaseSensitive`](<case-sensitive.md> "case-sensitive") and [methods](<Method.md> "Method") like `Find` to facilitate speeded search within a list. 

Example
    
    
      // get a value from file FILNAM filled with key=value pairs 
    function GetValueFromFile( filnam: string, key: string ): string;
    var
      lst: TStringList;
      v: String;
    begin
      lst := TStringList.Create();
      lst.CaseSensitive := false;
      lst.Duplicates := dupIgnore; // do not add duplicates
      lst.Sorted := true;
      lst.LoadFromFile( filnam );
      v := lst.Values[ key ];
      lst.Free();
      result := v;
    end;
    

TStringlist also has a 'builtin' data dictionary mode, here is an example provided by forum user Remy Lebeau 
    
    
       uses
         ..., Classes;
        
       var
         SL: TStringList;
         Value: Integer;
       begin
         SL := TStringList.Create;
         try
           SL.Add('VarName=99');
           ...
           Value := StrToInt(SL.Values['VarName']);
           ...
         finally
           SL.Free;
         end;
       end;
    

  


## See also

  * [TStringList doc](<http://lazarus-ccr.sourceforge.net/docs/rtl/classes/tstringlist.html> "doc:rtl/classes/tstringlist.html")
  * [TStringList-TStrings Tutorial](<TStringList-TStrings_Tutorial.md> "TStringList-TStrings Tutorial")
  * [ example of saving a TStringList to disk faster than the SaveToFile method can](<TBufStream.md> "TBufStream")

---

_Source: [https://wiki.freepascal.org/TStringList](https://web.archive.org/web/20250319000113/https://wiki.freepascal.org/TStringList)_
