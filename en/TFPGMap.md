# TFPGMap

TFPGMap is part of the `FGL` (Free Generics Library). 

## Example
    
    
    uses
      fgl;
     
    type
      TMyDict = specialize TFPGMap<string, integer>;
    var
      Dict: TMyDict;
      Value: Integer;
    begin
      Dict := TMyDict.Create;
      try
        Dict.Add('VarName', 99);
        ...
        Value := Dict['VarName'];
        ...
      finally
        Dict.Free;
      end;
    end;
    

## IndexOfData

By default `TFPGMap<>` does not provide a comparison for the Data type. So method IndexOfData will not find anything. You need to provide it by assigning the OnDataCompare event: 
    
    
    uses
      StrUtils, fgl;
    
    {$R *.lfm}
    
    type
      TMyTest = specialize TFPGMap<PtrInt, String>;
    ...
      private
        FMyTest: TMyTest;
      ...
      public
        property MyTest: TMyTest read FMyTest write SetMyTest;
      end;
    ...
     
    function CompareStringFunc(const aData1, aData2: String): Integer;
    begin
      Result := AnsiCompareStr(aData1, aData2);
    end;
     
    procedure TForm1.FormCreate(Sender: TObject);
    begin
      FMyTest:= TMyTest.Create;
      FMyTest.OnDataCompare := @CompareStringFunc;
    

## See also

  * [Free Pascal docs on TFPGMap](<https://www.freepascal.org/docs-html/current/rtl/fgl/tfpgmap.html>)

---

_Source: [https://wiki.freepascal.org/TFPGMap](https://web.archive.org/web/20240711002150/https://wiki.freepascal.org/TFPGMap)_
