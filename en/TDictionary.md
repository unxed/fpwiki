# TDictionary

## Contents

  * 1 TDictionary
  * 2 Basic example
  * 3 Usage:
    * 3.1 Add
      * 3.1.1 Add
      * 3.1.2 AddOrSetValue
    * 3.2 Remove
    * 3.3 Clear
    * 3.4 Get Value
    * 3.5 Item Count
    * 3.6 Loops
      * 3.6.1 Loop Keys
      * 3.6.2 Loop Values
      * 3.6.3 Loop Key-Value-Pairs
    * 3.7 Check If Key/Value Exists
    * 3.8 Trying
      * 3.8.1 TryAdd
      * 3.8.2 TryGetValue
    * 3.9 ToArray
  * 4 Event notifications
  * 5 Using Classes
  * 6 Exceptions
  * 7 Example in Delphi mode



## TDictionary

TDictionary is a generic data-container for key-value pairs (associative array). 

It was introduced with Generics.Collections (rtl-generics) in FPC 3.1.1+ and published in major release 3.2 (June 2020). It's part of the FPC Runtime Library and fully Delphi compatible. 

(Like in Delphi) The TDictionary is implemented as a hash table. This means the key-value pairs are not sorted in a defined order in memory and are not iterable by index. However you can iterate over the Key- and Value- collections. Or you can turn them in an array and iterate over that. 

Both key and value can be of primitive type (e.g. integer, string, boolean) or rather complex (e.g. classes, records, arrays). 

It is recommended to name the specialized dictionary-type with abbreviations of key and value types e.g "TStringIntegerDict" when the key is a of type string and the value is of type integer. 

## Basic example
    
    
    {$mode objfpc} {$H+}
    program Example;
    
    uses
      Generics.Collections;
    
    type
      TStrStrDictionary = specialize TDictionary<string, string>;
    
    var
      MyDictionary: TStrStrDictionary;
    begin
      MyDictionary := TStrStrDictionary.Create;
      try
        //do stuff with your dictionary..
        MyDictionary.AddOrSetValue('Key1', 'Value1');
        
        if MyDictionary.ContainsKey('Key1') then
            WriteLn(MyDictionary['Key1']);
    
      finally
        MyDictionary.Free;
      end;
    end.
    

See also #Example in Delphi mode. 

## Usage:

### Add

**AddOrSetValue()** is recommended, because **Add()** will fail, if the key already exists: 'Dublicate not allowed in Dictionary'.  


#### Add
    
    
    MyDictionary.Add('Key1', 'Value1');
    

#### AddOrSetValue

recommended 
    
    
    MyDictionary.AddOrSetValue('Key1', 'Value1');
    

### Remove
    
    
    MyDictionary.Remove('Key2');
    

If the Key does not exist, no Error is thrown. 

### Clear

Remove all entries. 
    
    
    MyDictionary.Clear;
    

### Get Value
    
    
    WriteLn(MyDictionary['Key1']);
    
    //WriteLn(MyDictionary.Items['Key1']); works aswell
    

If the provided key does not exist, this will throw an Error: 'Dictionary Key does not Exist'. You could check with .ContainsKey() beforehand or use TryGetValue(). 

### Item Count

Total number of Items in dictionary (count) 
    
    
    WriteLn('Items in Dictionary: ', MyDictionary.Count);
    

### Loops

#### Loop Keys
    
    
    for KeyStr in MyDictionary.Keys do
      WriteLn('Found key:', KeyStr);
    

#### Loop Values
    
    
    for ValueStr in MyDictionary.Values do
      WriteLn('Found value: ', ValueStr);
    

#### Loop Key-Value-Pairs
    
    
    //var KeyValuePair : TStrStrDictionary.TDictionaryPair;
    for KeyValuePair in MyDictionary do
      WriteLn('Found a Pair: Key:' , KeyValuePair.Key , ' Value:' , KeyValuePair.Value);
    

### Check If Key/Value Exists
    
    
    //Check if Key exists
    if MyDictionary.ContainsKey('Key1') then
      WriteLn('key exists')
    else
      WriteLn('key not found');
    
    
    //Check if Value exists
    if MyDictionary.ContainsValue('Searched value') then
      writeln('value exists')
    else
      writeln('value not found');
    

### Trying

These functions return a boolean.  
True if sucessfull, False if not. No error is thrown, so they are a good alternative to Get Value or Add(). 

#### TryAdd
    
    
    if MyDictionary.TryAdd('Key','TestValue') then
      writeln('added successfully')
    else
      writeln('not successfull');
    

Limitation: TryAdd does **not** update an existing keys value. Use AddOrSetValue() instead. (Even tho there is no feedback provided) 

#### TryGetValue

TryGetValue takes two Arguments ->(key, value) and returns a boolean.  
If found, it returns **True** and the provided 'value' variable becomes the value.  
If not found, the function returns **False** and the provided 'value'-variable is **nil**. 
    
    
    //var SearchedValue: string;
    if MyDictionary.TryGetValue('Key 1', SearchedValue) then
      WriteLn('Key found. Its value is: ' + SearchedValue)
    else
      WriteLn('Could not get value');
    
    //writeln(BoolToStr(MyDictionary.TryGetValue('Key 1', SearchedValue), true));
    

### ToArray

ToArray method returns an array of type TDictionaryPair: 
    
    
    var 
      myArray: array of TStrStrDictionary.TDictionaryPair;
    begin
      myArray := MyDictionary.ToArray()
      WriteLn(length(myArray));
      WriteLn(myArray[0].Key)
      WriteLn(myArray[0].Value)
    

  


Get an array of all keys, i.e. turn the Keys collection into an array: 
    
    
    var 
      keyArray: array of string; //needs to be the correct type
      i: integer;
    begin
      keyArray := MyDictionary.Keys.ToArray()
      WriteLn(length(keyArray));
      WriteLn(keyArray[0])
      for i := 0 to Length(keyArray)-1 do
        writeln(keyArray[i]);
    

Get an array of all values, i.e. turn the Values collection into an array: 
    
    
    var 
      valueArray: array of string; //needs to be the correct type
      i: integer;
    begin
      valueArray := MyDictionary.Values.ToArray()
      WriteLn(length(valueArray));
      WriteLn(valueArray[0])
      for i := 0 to Length(valueArray)-1 do
        writeln(valueArray[i]);
    

## Event notifications

You can set up custom functions/procedures which are triggered if an Key or Value got added/removed. To do this, we add 2 procedures to the class we specialize from TDictionary. The TDictionary class provides the events OnKeyNotify and OnValueNotify. We assign them to our procedures. 

Hint: If you update a existing value, there is no "updated" event. Instead "removed" followed by "added" is triggered. 

TODO: get the Key name inside the ValueNotify event context. 
    
    
    {$mode objfpc} {$H+}
    program Example;
    
    uses
      Generics.Collections;
    
    type
      TStrStrDictionary = class(specialize TDictionary<ShortString, ShortString>)
      private
        procedure DoKeyNotify(ASender: TObject; constref AItem: ShortString; AAction: TCollectionNotification);
        procedure DoValueNotify(ASender: TObject; constref AItem: ShortString; AAction: TCollectionNotification);
      end;
    
    procedure TStrStrDictionary.DoKeyNotify(ASender: TObject; constref AItem: ShortString; AAction: TCollectionNotification);
    begin
      //WriteLn(AAction);
      //AAction can be cnRemoved or cnAdded
      if AAction = cnAdded then
        writeln('A Key got added, the Key is:', AItem)
      else if AAction = cnRemoved then
        writeln('A Key got removed, the Key was:', AItem)
    end;
    
    procedure TStrStrDictionary.DoValueNotify(ASender: TObject; constref AItem: ShortString; AAction: TCollectionNotification);
    begin
      //how can we get here the key this value was added to?
    
      //Writeln(AAction);
      //can be cnRemoved or cnAdded
      if AAction = cnAdded then
        writeln('A Value got added, the value is:', AItem)
      else if AAction = cnRemoved then
        writeln('A Value got removed, the value was:', AItem)
    end;
    
    
    var
      MyDictionary: TStrStrDictionary;
      TestArr: array of TStrStrDictionary.TDictionaryPair;
    begin
      MyDictionary := TStrStrDictionary.Create;
    
      //Event notifications:
      MyDictionary.OnKeyNotify := @MyDictionary.DoKeyNotify;
      MyDictionary.OnValueNotify := @MyDictionary.DoValueNotify;
    
      try
        MyDictionary.AddOrSetValue('Key1', 'Value1'); //triggers 'Add' Event
        MyDictionary.AddOrSetValue('Key1', 'New Value'); //triggers 'Remove' and 'Add' Event
        MyDictionary.Remove('Key1'); //triggers 'Remove' Event
        Readln();
      finally
        MyDictionary.Free;
      end;
    end.
    

## Using Classes

You can use classes for Keys and/or Values. Because of the fact that all created classes must be manually freed, it is strongly recommended to use a [TObjectDictionary](</index.php?title=TObjectDictionary&action=edit&redlink=1> "TObjectDictionary \(page does not exist\)") instead.  
A [TObjectDictionary](</index.php?title=TObjectDictionary&action=edit&redlink=1> "TObjectDictionary \(page does not exist\)") takes ownership of its contained classes and frees them automaticly if its freed.  
However here is an example where manual freeing is required: 
    
    
    {$mode objfpc} {H+}
    program example;
    
    uses
      Generics.Collections;
    
    type
      TMyTestClass = class
        Name: string;
      end;
    
      TIntClassDictionary = specialize TDictionary<Integer, TMyTestClass>;
    
    var
      MyDictionary: TIntClassDictionary;
      MyClass: TMyTestClass;
      i: integer;
    begin
      MyDictionary := TIntClassDictionary.Create;
      try
        for i:= 0 to 5 do
          begin
             MyClass := TMyTestClass.Create;
             MyClass.Name := 'Something';
             MyDictionary.Add(i, MyClass);
          end;
    
         //...
    
      finally
        //manual freeing
        for MyClass in MyDictionary.Values do
          MyClass.Free;
    
        MyDictionary.Free;
      end;
    end.
    

## Exceptions

To handle exceptions, import the SysUtils unit. 
    
    
    {$mode objfpc} {H+}
    program Example;
    
    uses
      ..., SysUtils;
    
      try          
        WriteLn(MyDictionary.Items['KeyXY']); // Accessing a non-existent key 
      except
        on E: Exception do begin
          WriteLn('An exception was raised: ', E.Message);
          WriteLn(E.ClassName);
        end;
      end;
    
    //Output:
    // An exception was raised: Dictionary key does not exist
    //EListError
    

## Example in Delphi mode
    
    
    {$mode delphi}
    uses generics.collections;
     
    type
      TStrIntDict = TDictionary<string, integer>;
     
    var
      MyDict: TStrIntDict;
    begin
      MyDict := TStrIntDict.create;
      MyDict.AddOrSetValue('foo', 42);
      if MyDict.ContainsKey('foo') then
        WriteLn(MyDict['foo']);
    end.

---

_Source: [https://wiki.freepascal.org/TDictionary](https://web.archive.org/web/20250214015423/https://wiki.freepascal.org/TDictionary)_
