# Streaming JSON

│ **[Deutsch (de)](</Streaming_JSON/de> "Streaming JSON/de")** │  **English (en)** │  **[polski (pl)](</Streaming_JSON/pl> "Streaming JSON/pl")** │  **[русский (ru)](<../ru/Streaming_JSON.md> "Streaming JSON/ru")** │  **[中文（中国大陆） (zh_CN)](</Streaming_JSON/zh_CN> "Streaming JSON/zh CN")** │    
****

[JSON](<JSON.md> "JSON") (JavaScript Object Notation) is a text-based, standardized data format. As the name implies, JSON documents are valid JavaScript code and can be directly converted into JavaScript objects. However, JSON can be used for data exchange regardless of the programming language used. 

This tutorial explains how to load JSON data into a Free Pascal program and process it there. It also explains how to convert data from the program into JSON (eg to send it to a web browser). 

## Contents

  * 1 General requirements
  * 2 Data Structure
  * 3 Load JSON
  * 4 Saving JSON
  * 5 Custom properties
  * 6 Conclusion
  * 7 See Also
  * 8 Notes & References



## General requirements

Loading and storing (streaming) objects is done with the fpjsonrtti unit. It also makes sense to use the Classes unit (see below for details). 

The uses statement should therefore contain at least these two units: 
    
    
    uses Classes, fpjsonrtti;
    

Currently (May 2014) there are some differences between Free Pascal's streaming system and JSON: 

  * JSON is a case-sensitive data format. It follows that the properties of the Free Pascal objects must be written in the same case as the JSON properties.
  * No properties can be defined with DefineProperties. No references to methods (event handlers) can be saved.2
  * [TCollection](<TCollection.md> "TCollection") and [TStrings](<TStrings.md> "TStrings") can used as a replacements for arrays.



Demo programs are provided with Free Pascal Compiler source code in the `packages/fcl-json/examples` directory. 

This JSON structure is used in the examples which follow: 
    
    
    {
      "id"     : 123,                                                // an integer
      "obj"    : { "name": "Hello world!" },                         // an object
      "coll"   : [ { "name": "Object 1" }, { "name": "Object 2" } ], // two objects in a TCollection
      "strings": [ "Hello 1", "Hello 2" ]                            // a string list
    }
    

It can be defined in your Free Pascal program using a constant assignment as follows: 
    
    
    const JSON_TESTDATA =
    '{'+LineEnding+
    '  "id": 123,'+LineEnding+
    '  "obj": { "name": "Hello world!" },'+LineEnding+
    '  "coll": [ { "name": "Object 1" }, { "name": "Object 2" } ],'+LineEnding+
    '  "strings": [ "Hello 1", "Hello 2" ]'+LineEnding+
    '}';
    

## Data Structure

The base class for the data is [TPersistent](</index.php?title=TPersistent&action=edit&redlink=1> "TPersistent \(page does not exist\)") from the Classes unit, [runtime type information (RTTI)](<Runtime_Type_Information_\(RTTI\).md> "Runtime Type Information \(RTTI\)") is created for it and all subclasses. These are essential for streaming. Since fpjsonrtti does not integrate into the streaming system, any other class translated with the compiler switch **{$M+}** can also be used. 

All properties to be read must be declared as a [property](</Property> "Property") in the [published](<Published.md> "Published") section of the class. As a rule, you can use read and write to refer directly to a data field (the variable). If you want, you can of course use getter and setter methods. 

The following class definition results from the JSON structure: 
    
    
    type
      TNameObject = class(TCollectionItem) // class for the 'obj' property and TCollection 
      private
        fName: String;
      published
        property name: String read fName write fName;
      end;  
    
      TBaseObject = class(TPersistent)  // class for the entire JSON structure
      private
        fid: Integer;
        fObj: TNameObject;
        fColl: TCollection;
        fStrings: TStrings;
      public
        constructor Create;
        destructor Destroy; override;
      published                         // all properties must be published 
        property id: Integer read fid write fid;
        property obj: TNameObject read fObj write fObj;
        property coll: TCollection read fColl;
        property strings: TStrings read fStrings;
      end;
    

The `TNameObject` class was derived from [TCollectionItem](</index.php?title=TCollectionItem&action=edit&redlink=1> "TCollectionItem \(page does not exist\)"). This means that it can be used both for the obj property and in the collection. If this is not desired, then two different classes must be defined here. 

The TCollection and the string list must be created in the constructor of the TBaseObject class and released in the destructor. 
    
    
    constructor TBaseObject.Create;
    begin
      // Create Collection and StringList
      fColl    := TCollection.Create(TNameObject);
      fStrings := TStringList.Create;
      fObj     := TNameObject.Create(nil);
    end;
    
    destructor TBaseObject.Destroy;
    begin
      // Release Collection and StringList
      fColl.Free;
      fStrings.Free;
      fObj.Free;
      inherited Destroy;
    end;
    

If you do not want any more functionality in the data classes, their definition is now complete. 

## Load JSON

With the method `Procedure JSONToObject(Const JSON : TJSONStringType; AObject : TObject);` in the [TJSONDeStreamer](</index.php?title=TJSONDeStreamer&action=edit&redlink=1> "TJSONDeStreamer \(page does not exist\)") class you can assign JSON data directly to an _existing_ object. Before you call the method, you must create TJSONDeStreamer and the target object. 

The following method loads the data from the JSON structure `JSON_TESTDATA` in the object o and then outputs the current values ​​of the properties to the console. 
    
    
    procedure DeStreamTest;
    var
      DeStreamer: TJSONDeStreamer;
      o: TBaseObject;
      no: TNameObject;
      s: String;
    begin
      WriteLn('DeStream test');
      WriteLn('======================================');
    
      // Create the DeStreamer object and target object
      DeStreamer := TJSONDeStreamer.Create(nil);
      o := TBaseObject.Create;
      try
        // Load JSON data into object o
        DeStreamer.JSONToObject(JSON_TESTDATA, o);
        // output ID
        WriteLn(o.id);
        // output object name
        WriteLn(o.obj.name); 
        // output the names of all objects
        for TCollectionItem(no) in o.coll do
          Writeln(no.name);
        // output all strings
        for s in o.strings do
          WriteLn(s);
    
      // Cleanup
      finally
        o.Destroy;
        DeStreamer.Destroy;
      end;
    end;
    

## Saving JSON

The class [TJSONStreamer](</index.php?title=TJSONStreamer&action=edit&redlink=1> "TJSONStreamer \(page does not exist\)") is used to convert an object into JSON text. Here the method `Function ObjectToJSONString(AObject : TObject) : TJSONStringType;` is used. 

In the following procedure, an object is created, filled with the test data, and then converted to JSON. The JSON text is output on the console. The order in which the properties are output cannot be specified. 
    
    
    procedure StreamTest;
    var
      Streamer: TJSONStreamer;
      o: TBaseObject;
      JSONString: String;
    begin
      WriteLn('Stream test');
      WriteLn('======================================');
    
      Streamer := TJSONStreamer.Create(nil);
      o := TBaseObject.Create;
      try
        // Setup data
        o.id := 123;
        o.obj.name := 'Hello world!';
        TNameObject(o.coll.Add).name := 'Object 1';
        TNameObject(o.coll.Add).name := 'Object 2';
        o.strings.Add('Hello 1');
        o.strings.Add('Hello 2');
    
        Streamer.Options := Streamer.Options + [jsoTStringsAsArray]; // Save strings as JSON array
        // convert to JSON and output to console
        JSONString := Streamer.ObjectToJSONString(o);
        WriteLn(JSONString);
    
      // Cleanup
      finally
        o.Destroy;
        Streamer.Destroy;
      end;
    end;
    

## Custom properties

Now that we can save and load to JSON, we can as well customize how the property is saved. By default a TColor (TGraphicsColor) is saved as a number, but for example we want to make it a string, so is more human readable. 

This example uses BGRABitmap library, that has a function to convert from string to color easily. The streamed control in this case is a button control from BGRAControls package. 

This customizes how TColor is saved and loaded. 

Saving code: 
    
    
    procedure TForm1.Button1Click(Sender: TObject);
    var
      Streamer: TJSONStreamer;
      JSONString: String;
    begin
      Streamer := TJSONStreamer.Create(nil);
      try
        Streamer.OnStreamProperty:=@OnStreamProperty;
        JSONString := Streamer.ObjectToJSONString(BCButton1.StateNormal);
        Memo1.Lines.Text := JSONString;
      finally
        Streamer.Destroy;
      end;
    end;
    
    
    
    procedure TForm1.OnStreamProperty(Sender: TObject; AObject: TObject;
      Info: PPropInfo; var Res: TJSONData);
    var
      bgracolor: TBGRAPixel;
    begin
      if (Info^.PropType^.Name = 'TGraphicsColor') then
      begin
        bgracolor := ColorToBGRA(TColor(GetPropValue(AObject, Info, False)));
        Res.Free;
        Res := TJSONString.Create('rgb('+IntToStr(bgracolor.red)+','+IntToStr(bgracolor.green)+','+IntToStr(bgracolor.blue)+')');
      end;
    end;
    

Loading code: 
    
    
    procedure TForm1.Button2Click(Sender: TObject);
    var
      DeStreamer: TJSONDeStreamer;
      s: String;
    begin
      DeStreamer := TJSONDeStreamer.Create(nil);
      try
        DeStreamer.OnRestoreProperty:=@OnRestoreProperty;
        DeStreamer.JSONToObject(Memo1.Lines.Text, BCButton1.StateNormal);
      finally
        DeStreamer.Destroy;
      end;
    end;
    
    
    
    procedure TForm1.OnRestoreProperty(Sender: TObject; AObject: TObject;
      Info: PPropInfo; AValue: TJSONData; var Handled: Boolean);
    var
      bgracolor: TBGRAPixel;
    begin
      Handled := False;
      if (Info^.PropType^.Name = 'TGraphicsColor') then
      begin
        Handled := True;
        bgracolor := StrToBGRA(AValue.AsString);
        SetPropValue(AObject, Info, BGRAToColor(bgracolor));
      end;
    end;
    

## Conclusion

With the knowledge presented, simple and complex JSON data structures can be loaded into Free Pascal programs. Should any pre- or post-processing of the JSON data be necessary, the text data can be first loaded from the [jsonparser](</index.php?title=jsonparser&action=edit&redlink=1> "jsonparser \(page does not exist\)") unit into a JSON data structure using the [TJSONParser](</index.php?title=TJSONParser&action=edit&redlink=1> "TJSONParser \(page does not exist\)") class and then manipulated as desired with the [fpJSON](</index.php?title=fpJSON&action=edit&redlink=1> "fpJSON \(page does not exist\)") unit. 

The Options property of the TJSONStreamer class can be used to control how the output maps its own data structures in JSON. 

## See Also

  * [JSON](<JSON.md> "JSON")
  * [fcl-json](<fcl-json.md> "fcl-json")
  * [Streaming components](<Streaming_components.md> "Streaming components")



## Notes & References

  1. <http://lists.freepascal.org/fpc-pascal/2013-January/036254.html>

  2. <http://lists.lazarus.freepascal.org/pipermail/lazarus/2011-January/058878.html>

---

_Source: [https://wiki.freepascal.org/Streaming_JSON](https://web.archive.org/web/20250114085401/https://wiki.freepascal.org/Streaming_JSON)_
