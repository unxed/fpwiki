# link

//process: {"A":{"field1":0, "field2": false},"B":{"field1":0, "field2": false}}
    
    uses
      Classes, SysUtils, FileUtil, Forms, Controls, Graphics, Dialogs, StdCtrls,
      typinfo,
      fpjson, jsonparser, jsonscanner; 
    
    procedure TForm1.DecodeString(AJSON: string);
    var
      jsData: TJSONData;
      jsItem: TJSONData;
      i, j: word;
      object_name, field_name, field_value, object_type, object_items: string;
    
    begin
      try
        jsData := GetJSON(AJSON);
    
        for i := 0 to jsData.Count - 1 do
        begin
          jsItem := jsData.Items[i];
    
          object_type := GetEnumName(TypeInfo(TJSONtype), Ord(jsItem.JSONType));
          object_name := TJSONObject(jsData).Names[i];
          object_items := IntToStr(jsItem.Count);
    
          memo1.Append('object type: ' + object_type + '|object name: ' + object_name + '|number of fields: ' + object_items);
    
          for j := 0 to jsItem.Count - 1 do
          begin
            field_name := TJSONObject(jsItem).Names[j];
            field_value := jsItem.FindPath(TJSONObject(jsItem).Names[j]).AsString;
    
            memo1.Append(field_name + '|' + field_value);
          end;
        end;
    
      finally
        FreeAndNil(jsData);
      end;
    end;

---

_Source: [https://wiki.freepascal.org/link](https://web.archive.org/web/20191017060856/https://wiki.freepascal.org/link)_
