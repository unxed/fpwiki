# TString List-TString Tutorial

From Free Pascal wiki

****English (en)**** | [**Español (es)**](</TString_List-TString_Tutorial/es> "TString List-TString Tutorial/es") | [**Русский (ru)**](<../ru/TString_List-TString_Tutorial.md> "TString List-TString Tutorial/ru")

## Contents

  * 1 TStringList
    * 1.1 Simple example
    * 1.2 Expanded Example
  * 2 File Handling
  * 3 Dynamic string array comparison
  * 4 Keep Learning

  
---  
  
##  TStringList

The TStringList (or its parent, TStrings) is much like a fancy dynamic array or Set of Strings (a set of strings is not possible in FPC). It will come in handy a lot when programming and I'm going to teach you basic TStringList usage! 

###  Simple example
    
    
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

This is a simple console program that will create and add one string to a stringlist. Now here's some things you should know: 

**Create** \- Will create the string list for modifying. If you use **Create** , you have to later **Free** it and release the memory it takes. If not, you program will not crash, but it will not release all the memory it occupied: a memory leak. 

**Count** \- This property is a counter for the number of strings in the List. 

**Add** \- This method allows you to add one string to the stringlist. It is a function that will return the **Index** of the String. This is where the counter comes in handy. 

**Delete** \- Will delete a string from the stringlist. Just know that you do not simply input the string, you have to input the index of the string. Like I said: it's like a fancy Dynamic Array. 

**IndexOf** \- Will return the index of the string in the list. If it is not found it returns -1. 

**Clear** \- Will clear the list. 

###  Expanded Example

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
        Writeln('Enter a string to add');
        Readln(S); 
        if (S = 'EXIT') then Halt; //Halt immediately stops execution
        //If you look closely, you will see this leads to a memory leak.
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
      Str.Free; //release the memory again
    end.

However, to avoid possible memory leaks you should always use a try - finally block where possible for this, so you get something like: 
    
    
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

##  File Handling

When using the TStringList you have 2 file handling procedures: **SaveToFile** and **LoadFromFile**. SavetoFile will save all strings in the list to a file. LoadFromFile will open the file and add the file data to the list string by string. 
    
    
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

##  Dynamic string array comparison

TStringList is simply an advanced object version of dynamic string array. Some methods have analogs: 

Operation  |  array of string  |  TStringList   
---|---|---  
Variable declaration  |  StringList: array of string;  |  StringList: TStringList;   
Initialization  |  implicit constructor  |  StringList := TStringList.Create   
Set size  |  SetLength(StringList, X);  |  StringList.Size := X;   
Get size  |  X := Length(StringList);  |  X := StringList.Count;   
Add item  |  SetLength(StringList, Length(StringList) + 1); StringList[Length(StringList) - 1] := X;  |  StringList.Add(X);   
Delete item  |  for I := Index to Length(StringList) - 2 do StringList[I] := StringList[I + 1]; SetLength(StringList, Length(StringList) - 1);  |  StringList.Delete(Index);   
Remove all items  |  SetLength(StringList, 0);  |  StringList.Clear;   
Finalization  |  implicit destructor  |  StringList.Free;   
  
But TStringList offers much more functionality than a basic structure such as a dynamic array. 

##  Keep Learning

TStringList has many other interesting features: 

  1. It allows you to sort the strings 
  2. It allows you to limit the list to only unique strings 
  3. You can get the text of all strings as a single string using the **Text** property. 
  4. You can store an object or other data next to the string 



You can learn all the different procedures, functions and properties. See [TStringList documentation](<http://www.freepascal.org/docs-html/rtl/classes/tstringlist.html>)... or the help in Lazarus. 

... and you might like to extend this tutorial if you feel like it.

---

_Source: [https://wiki.freepascal.org/TString_List-TString_Tutorial](https://web.archive.org/web/20140131022136/https://wiki.freepascal.org/TString_List-TString_Tutorial)_
