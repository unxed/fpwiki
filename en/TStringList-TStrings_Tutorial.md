# TStringList-TStrings Tutorial

│ **[Deutsch (de)](</TStringList-TStrings_Tutorial/de> "TStringList-TStrings Tutorial/de")** │  **English (en)** │  **[español (es)](</TStringList-TStrings_Tutorial/es> "TStringList-TStrings Tutorial/es")** │  **[suomi (fi)](</TStringList-TStrings_Tutorial/fi> "TStringList-TStrings Tutorial/fi")** │  **[français (fr)](</TStringList-TStrings_Tutorial/fr> "TStringList-TStrings Tutorial/fr")** │  **[polski (pl)](</TStringList-TStrings_Tutorial/pl> "TStringList-TStrings Tutorial/pl")** │  **[русский (ru)](<../ru/TStringList-TStrings_Tutorial.md> "TStringList-TStrings Tutorial/ru")** │    
****

## Contents

  * 1 TStringList
    * 1.1 Simple example
    * 1.2 Expanded Example
    * 1.3 Conversion to and from delimited strings
  * 2 File Handling
  * 3 Sorting
  * 4 Dynamic string array comparison
  * 5 Keep Learning
  * 6 See also



## TStringList

The [TStringList](<http://lazarus-ccr.sourceforge.net/docs/rtl/classes/tstringlist.html> "doc:rtl/classes/tstringlist.html") (or its parent [TStrings](<http://lazarus-ccr.sourceforge.net/docs/rtl/classes/tstrings.html> "doc:rtl/classes/tstrings.html")) is much like a fancy [dynamic array](<Dynamic_array.md> "Dynamic array") or [Set](<Set.md> "Set") of Strings (a set of strings is not possible in FPC). It will come in handy a lot when programming and I'm going to teach you basic TStringList usage! 

### Simple example
    
    
    program StrList;
    {$mode objfpc}
    uses
     Classes, SysUtils;
    var
      Str: TStringList;
    begin
      Str := TStringList.Create; // This is needed when using this class(or most classes)
      Str.Add('Some String!');
      writeln('The stringlist now has ' + IntToStr(Str.Count) + ' string(s).');
      Readln;
      Str.Free; //Release the memory used by this stringlist instance
    end.
    

This is a simple [console program](<Command-line_interface.md> "Command-line interface") that will create and add one string to a stringlist. Now here's some things you should know: 

**Create** \- Will create the string list for modifying. If you use [`constructor`](<Constructor.md> "Constructor") **Create** , you have to later **Free**. Calling `free` causes [`destructor`](<Destructor.md> "Destructor") a `Destroy` invitation and release the memory it takes. If not, you program will not crash, but it will not release all the memory it occupied: a memory leak. 

**Count** \- This [property](</Property> "Property") is a counter for the number of strings in the List. 

**Add** \- This [method](<Method.md> "Method") allows you to add one string to the stringlist. It is a function that will return the **Index** of the String. This is where the counter comes in handy. 

**Delete** \- Will delete a string from the stringlist. Just know that you do not simply input the string, you have to input the index of the string. Like I said: it's like a fancy Dynamic Array. 

**IndexOf** \- Will return the index of the string in the list. If it is not found it returns -1. 

**Clear** \- Will clear the list. 

### Expanded Example

How about a more juicy example, eh? 
    
    
    program StrList2;
    {$mode ObjFPC}
    uses
    Classes, SysUtils;
    
    var
      Str: TStringList;
      S: String;
      Counter: Integer;
    begin
      Str := TStringList.Create;
      Writeln('String List Test');
      repeat
        Writeln('Enter a string to add (type EXIT to stop adding strings)');
        Readln(S); 
        if (S = 'EXIT') then 
          Break; // exit the loop
        
        if (S <> '') then
        begin
          Counter := Str.Add(S);
          Writeln('String: ' + S + ' was Added!');
          Writeln('Index is: ' + IntToStr(Counter)); // The counter will always become the index of the last thing added
        end 
        else 
        begin
          Writeln('No data entered...');
        end;
      until (S = 'EXIT');
      writeln('Contents of the TStringList: '+ Str.CommaText);
      Str.Free; //release the memory again
    end.
    

However, to avoid possible memory leaks you should always use a [Try](<Try.md> "Try") \- [Finally](<Finally.md> "Finally") block where possible for this, so you get something like: 
    
    
    var
      slist: TStringList;
    
    ...
    
    slist := TStringList.Create;
    try
      ...
      // do things with your stringlist  
      ...
    finally
      if Assigned(slist) then
        FreeAndNil(slist);
    end;
    
    
    
    // This works perfect, no double creation of stringlist... comments free to send to edgarrod71@gmail.com
    function theStringList: TStringList;
    var
     J: integer;
    begin
      result := TStringList.Create;
      for J:=0 to 10 do
        result.add(intToStr(J));
    end;
    
    procedure Caller;
    var
      SL: TStringList;
      K: integer;
    begin
      SL := theStringList;
      for K:=0 to pred(SL.Count) do
        writeln(SL[K]);
      if assigned(SL) then
        SL.Free;
    end;
    

To iterate through the list it is better to use the **while-do** loop instead of the **for-to** loop. The reason is that the list can be empty, then the for-to loop does not work correctly. In this example, SubscriberDel procedure is called for each ID in the List and the list of the IDs is passed to the procedure as a string. 
    
    
    procedure SubscribersDelete(const Subscriber_ID: string);
    var
      List : TStringList; 
      i : Integer;
    begin
      List := TStringList.Create;
      List.Text := Subscriber_ID;
      i := 0;
      while i < List.Count do
        begin
          SubscriberDel(List.Strings[i]);
          inc(i);
        end;
      List.Free;
    end;
    

### Conversion to and from delimited strings

Code below will result in a stringlist, containing 4 elements ('1', '2', '3' and '4'); 
    
    
    procedure Sample;
    var
        MyStringList: TStringList=nil;
    begin
         MyStringList:= TStringList.create;
         MyStringList.Delimiter := ';';
         MyStringList.DelimitedText:='1;2;3;4';
         MyStringList.free;
    end;
    

  
Respectively next code will assemble a stringlist into a delimited string ('1;2;3;4'): 
    
    
    function Sample2 : string;
    var
        MyStringList: TStringList=nil;
    begin
         MyStringList:= TStringList.create;
         MyStringList.Delimiter := ';';
         MyStringList.Add('1');
         MyStringList.Add('2');
         MyStringList.Add('3');
         MyStringList.Add('4');
         Result :=MyStringList.DelimitedText;
         MyStringList.free;
    end;
    

  
Note that Delimter is a character, **not** a string! If your separator is a string (for example „\n‟), you could use code below, to get a stringlist, containing 4 elements ('1', '2', '3' and '4'): 
    
    
    procedure Sample;
    var
        MyStringList: TStringList=nil;
    begin
         MyStringList:= TStringList.create;
         MyStringList.text:=StringReplace('1\n2\n3\n4','\n',Lineending,[rfReplaceAll, rfIgnoreCase]);
         MyStringList.free;
    end;
    

Vice versa, next [function](<Function.md> "Function") will return „1\n2\n3‟: 
    
    
    Function Sample : string;
    var
        MyStringList: TStringList=nil;
    begin
         MyStringList:= TStringList.create;
         MyStringList.SkipLastLineBreak := True;
         MyStringList.add('1');
         MyStringList.add('2');
         MyStringList.add('3');
         result := StringReplace(MyStringList.Text,Lineending,'\n', [rfReplaceAll, rfIgnoreCase]);
         MyStringList.free;
    end;
    

## File Handling

When using the TStringList you have 2 file handling [procedures](<Procedure.md> "Procedure"): **SaveToFile** and **LoadFromFile**. SavetoFile will save all strings in the list to a file. LoadFromFile will open the file and add the file data to the list string by string. 
    
    
    program StrListFile;
    {$mode objfpc}
    uses
     Classes, SysUtils;
    
    var
      Str: TStringList;
    begin
      Str := TStringList.Create;
      try
        Str.LoadFromFile('SomeFile.txt');
        Str.Add('Hello');
        Str.SaveToFile('SomeFile.txt');
      finally
        Str.Free;
      end;
    end.
    

You just opened a file, edited it and saved it right back to were it was! 

## Sorting

The Find procedure requires that the list be sorted. The easiest approach is to set its Sorted property to True, then new items added to the list will be inserted in the correct position. This has a few extra side effects, firstly, you cannot use the Insert() procedure, use only Add(). Secondly, it sets the Duplicates property, its default setting is "dupIgnore" which means attempts to Add identical entries will be ignored. However, calls to AddObject() that have an identical string and a new Object, the new Object replaces the existing one. 

## Dynamic string array comparison

TStringList is simply an [object-oriented](<object-oriented_programming.md> "object-oriented programming") version of a dynamic string array. Some methods have analogs: 

Operation | array of string | TStringList   
---|---|---  
Variable declaration | StringList: array of string; | StringList: TStringList;   
Initialization | implicit constructor | StringList := TStringList.Create   
Set size | SetLength(StringList, X); | StringList.Size := X;   
Get size | X := Length(StringList); | X := StringList.Count;   
Add item | SetLength(StringList, Length(StringList) + 1); StringList[Length(StringList) - 1] := X; | StringList.Add(X);   
Delete item | for I := Index to Length(StringList) - 2 do StringList[I] := StringList[I + 1]; SetLength(StringList, Length(StringList) - 1); | StringList.Delete(Index);   
Remove all items | SetLength(StringList, 0); | StringList.Clear;   
Finalization | implicit destructor | StringList.Free;   
  
However, TStringList offers much more functionality than a basic structure such as a dynamic array. 

## Keep Learning

TStringList has many other interesting features: 

  1. It allows you to sort the strings
  2. It allows you to limit the list to only unique strings
  3. You can get the text of all strings as a single string using the **Text** property.
  4. You can store an object or other data next to the string



You can learn all the different procedures, functions and properties. See [TStringList documentation](<http://lazarus-ccr.sourceforge.net/docs/rtl/classes/tstringlist.html> "doc:rtl/classes/tstringlist.html")... or the help in Lazarus. 

... and you might like to extend this tutorial if you feel like it. 

## See also

  * [Character and string types](<Character_and_string_types.md> "Character and string types")

---

_Source: [https://wiki.freepascal.org/TStringList-TStrings_Tutorial](https://web.archive.org/web/20240101000000/https://wiki.freepascal.org/TStringList-TStrings_Tutorial)_
