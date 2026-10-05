# fcl-json

│ **English (en)** │  **[polski (pl)](</fcl-json/pl> "fcl-json/pl")** │  **[русский (ru)](<../ru/fcl-json.md> "fcl-json/ru")** │  **[中文（中国大陆） (zh_CN)](</fcl-json/zh_CN> "fcl-json/zh CN")** │    
****

## Contents

  * 1 General info
  * 2 Streaming
  * 3 Examples
    * 3.1 Getting Started
    * 3.2 Traversing Items
    * 3.3 Save/load form position to/from file
    * 3.4 Save/load TStringList
    * 3.5 From JsonViewer
    * 3.6 Change formatting of float numbers
    * 3.7 See also



# General info

fcl-json is a [JSON](<JSON.md> "JSON") implementation. 

It contains units such as: 

  * fpjson: base unit which implements TJsonData and its children, e.g. TJsonObject
  * jsonParser: implements TJsonParser, used in the From JsonViewer example below
  * jsonConf: implements TJsonConfig which is handy to read/write application data from/to files
  * jsonScanner: json source lexical analyzer



Note: In fpjson, accessing e.g. 
    
    
    SomeJSONObject.Integers['price']
    

may give a SIGSEGV/Access Violation if that integer variable does not exist. This is apparently intentional, see [[1]](<http://bugs.freepascal.org/view.php?id=22273>). You'd have to use the Find method (available since FPC 2.6.2) to first check if the element ('price' in this example) exists. 

# Streaming

fcl-json contains the unit "fpjsonrtti" which is used to load objects (TObject instances) from or save them to JSON format. 

See [Streaming JSON](<Streaming_JSON.md> "Streaming JSON") for a short example. 

# Examples

## Getting Started
    
    
    uses
      fpjson, jsonparser;
    
    procedure JSONTest;
    var
      jData : TJSONData;
      jObject : TJSONObject;
      jArray : TJSONArray;
      s : String;
    begin
      // this is only a minimal sampling of what can be done with this API
    
      // create from string
      jData := GetJSON('{"Fld1" : "Hello", "Fld2" : 42, "Colors" : ["Red", "Green", "Blue"]}');
    
      // output as a flat string
      s := jData.AsJSON;
    
      // output as nicely formatted JSON
      s := jData.FormatJSON;
    
      // cast as TJSONObject to make access easier
      jObject := jData as TJSONObject;
    
      // retrieve value of Fld1
      s := jObject.Get('Fld1');
    
      // change value of Fld2
      jObject.Integers['Fld2'] := 123;
    
      // retrieve the second color
      s := jData.FindPath('Colors[1]').AsString;
    
      // add a new element
      jObject.Add('Happy', True);
    
      // add a new sub-array
      jArray := TJSONArray.Create;
      jArray.Add('North');
      jArray.Add('South');
      jArray.Add('East');
      jArray.Add('West');
      jObject.Add('Directions', jArray);
    
    end;
    

Note: it's necessary to free jData when you have finished with it. Otherwise a memory leak is created. 

## Traversing Items
    
    
    uses
      Classes, TypInfo, fpjson, jsonparser;
    
    procedure JSONItems(Info: TStrings);
    var
      jData : TJSONData;
      jItem : TJSONData;
      i, j: Integer;
      object_name, field_name, field_value, object_type, object_items: String;
    begin
      jData := GetJSON('{"A":{"field1":0, "field2": false},"B":{"field1":0, "field2": false}}');
    
      for i := 0 to jData.Count - 1 do
      begin
        jItem := jData.Items[i];
     
        object_type := GetEnumName(TypeInfo(TJSONtype), Ord(jItem.JSONType));
        object_name := TJSONObject(jData).Names[i];
        WriteStr(object_items, jItem.Count);
     
        Info.Append('object type: ' + object_type + '|object name: ' + object_name + '|number of fields: ' + object_items);
     
        for j := 0 to jItem.Count - 1 do
        begin
          field_name := TJSONObject(jItem).Names[j];
          field_value := jItem.FindPath(TJSONObject(jItem).Names[j]).AsString;
     
          Info.Append(field_name + '|' + field_value);
        end;
      end;
     
      jData.Free;
    end;
    

## Save/load form position to/from file
    
    
    uses
      jsonConf;
    
    procedure SaveFormPos(AForm: TForm; const AFilename: string);
    var
      c: TJSONConfig;
    begin
      c:= TJSONConfig.Create(Nil);
      try
        //try/except to handle broken json file
        try
          c.Formatted:= true;
          c.Filename:= AFilename;
        except
          exit;
        end;
    
        c.SetValue('/dialog/max', AForm.WindowState=wsMaximized);
        if AForm.WindowState<>wsMaximized then
        begin
          c.SetValue('/dialog/posx', AForm.Left);
          c.SetValue('/dialog/posy', AForm.Top);
          c.SetValue('/dialog/sizex', AForm.Width);
          c.SetValue('/dialog/sizey', AForm.Height);
        end;
      finally
        c.Free;
      end;
    end;
    
    procedure LoadFormPos(AForm: TForm; const AFilename: string);
    var
      nLeft, nTop, nW, nH: Integer;
      c: TJSONConfig;
    begin
      c:= TJSONConfig.Create(Nil);
      try
        //try/except to handle broken json file
        try
          c.Formatted:= true;
          c.Filename:= AFilename;
        except
          exit;
        end;
    
        nLeft:= c.GetValue('/dialog/posx', AForm.Left);
        nTop:= c.GetValue('/dialog/posy', AForm.Top);
        nW:= c.GetValue('/dialog/sizex', AForm.Width);
        nH:= c.GetValue('/dialog/sizey', AForm.Height);
        AForm.SetBounds(nLeft, nTop, nW, nH);
    
        if c.GetValue('/dialog/max', false) then
          AForm.WindowState:= wsMaximized;
      finally
        c.Free;
      end;
    end;
    

## Save/load TStringList
    
    
    uses
      jsonConf;
    var
      cfg: TJSONConfig;
      List: TStringList;
      path: string;
    begin
      List:= TStringList.Create;
      cfg:= TJSONConfig.Create(nil);
      try
        //try/except to handle broken json file
        try
          cfg.Formatted:= true;
          cfg.Filename:= AJsonFilename;
        except
          exit;
        end;
    
        cfg.GetValue('/mylist', List, '');
        List.Add('some_value');
        cfg.SetValue('/mylist', List);
      finally
        cfg.Free;
        List.Free;
      end;
    

## From JsonViewer

Example usage can be found in the Lazarus jsonviewer tool (located in lazarus/tools/jsonviewer). In particular, this part of the tool shows how to use json: 
    
    
    procedure TMainForm.OpenFile(Const AFileName : String);
    var
      S : TFileStream;
      P : TJSONParser;
      D : TJSONData;
    begin
      S:=TFileStream.Create(AFileName,fmOpenRead);
      try
        P:=TJSONParser.Create(S);
        try
          P.Strict:=FStrict;
          D:=P.Parse;
        finally
          P.Free;
        end;
      finally
        S.Free;
      end;
      FFileName:=AFileName;
      SetCaption;
      FreeAndNil(FRoot);
      FRoot:=D;
      ShowJSONDocument;
    end;
    
    procedure TMainForm.ShowJSONDocument;
    begin
      with TVJSON.Items do
      begin
        BeginUpdate;
        try
          TVJSON.Items.Clear;
          SHowJSONData(Nil,FRoot);
          with TVJSON do
            if (Items.Count>0) and Assigned(Items[0]) then
            begin
              Items[0].Expand(False);
              Selected:=Items[0];
            end;
        finally
          EndUpdate;
        end;
      end;
    end;
    
    procedure TMainForm.ShowJSONData(AParent : TTreeNode; Data : TJSONData);
    var
      N,N2 : TTreeNode;
      I : Integer;
      D : TJSONData;
      C : String;
      S : TStringList;
    begin
      N:=Nil;
      if Assigned(Data) then
      begin
        case Data.JSONType of
          jtArray,
          jtObject:
            begin
              if (Data.JSONType=jtArray) then
                C:=SArray
              else
                C:=SObject;
              N:=TVJSON.Items.AddChild(AParent,Format(C,[Data.Count]));
              S:=TstringList.Create;
              try
                for I:=0 to Data.Count-1 do
                  if Data.JSONtype=jtArray then
                    S.AddObject(IntToStr(I),Data.items[i])
                  else
                    S.AddObject(TJSONObject(Data).Names[i],Data.items[i]);
                if FSortObjectMembers and (Data.JSONType=jtObject) then
                  S.Sort;
                for I:=0 to S.Count-1 do
                begin
                  N2:=TVJSON.Items.AddChild(N,S[i]);
                  D:=TJSONData(S.Objects[i]);
                  N2.ImageIndex:=ImageTypeMap[D.JSONType];
                  N2.SelectedIndex:=ImageTypeMap[D.JSONType];
                  ShowJSONData(N2,D);
                end
              finally
                S.Free;
              end;
            end;
          jtNull:
            N:=TVJSON.Items.AddChild(AParent,SNull);
          else
            N:=TVJSON.Items.AddChild(AParent,Data.AsString);
        end;
        if Assigned(N) then
        begin
          N.ImageIndex:=ImageTypeMap[Data.JSONType];
          N.SelectedIndex:=ImageTypeMap[Data.JSONType];
          N.Data:=Data;
        end;
      end;
    end;
    

## Change formatting of float numbers

Q: My program generates and writes data to JSON file. I use FormatJSON() method to make output more readable. I'm not quite satisfied how numbers with float point look: 
    
    
    "coordinates" : [
         5.5978631048365003E+001,
         2.2100000000000000E+002
    ]
    

I want to see normal form: 
    
    
    "coordinates" : [
         55.978631048365003,
         221.0
    ]
    

A (forum members **y.ivanov** , **rvk** , **wp**): 
    
    
    {$mode objfpc}{$h+}
    uses
      fpjson,
      jsonparser,
      SysUtils;
     
    type
      TJSONFloat4Number = class(TJSONFloatNumber)
      protected
        function GetAsString: TJSONStringType; override;
      end;
     
    function TJSONFloat4Number.GetAsString: TJSONStringType;
    var
      F: TJSONFloat;
      fs: TFormatSettings;
    begin
      fs := DefaultFormatSettings;
      fs.DecimalSeparator := '.';
      F := GetAsFloat;
      Result := FormatFloat('0.0###############', F, fs); // format with your preferences
    end;
    
    procedure JSONTest;
    var
      jData: TJSONData;
    begin
      jData := GetJSON('{"coordinates": [5.5978631048365003E+001, 2.2100000000000000E+002]}');
      writeln(jData.FormatJSON);
      jData.Free;
    end;
     
    begin
      SetJSONInstanceType(jitNumberFloat, TJSONFloat4Number);
      JSONTest;
      Readln;
    end.
    

## See also

  * The FpcTwit example in the [Components and Code examples](<Components_and_Code_examples.md> "Components and Code examples") library makes use of JSON to send/receive data.
  * [FCL Reference](<https://lazarus-ccr.sourceforge.io/fpcdoc/fcl/fpjson/index.html>) for unit fpjson.
  * [An article (PDF)](<http://www.freepascal.org/~michael/articles/webdata/webdata.pdf>) covering use of XML and JSON in FreePascal.
  * [Package List](<Package_List.md> "Package List")

---

_Source: [https://wiki.freepascal.org/fcl-json](https://web.archive.org/web/20241124151858/https://wiki.freepascal.org/fcl-json)_
