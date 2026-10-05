# Lazarus Hacks

Add here your hacks/modifications to the Lazarus IDE or LCL widgets. If a good idea arises and is good enough it may become part of the official distribution; if not it can be used by other people that need more handy functionality than standards. 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** Please post a patch to the bugtracker to get improvements into the Lazarus code. Just posting them here will not work.

## Exposing/Extending TCustomEdit

In some applications we need to constantly convert the Text property of TCustomEdit descendants from or to numeric values: 
    
    
      TCustomEditFilters = (cefNone, cefInteger, cefUnsigned, cefCurrency, cefFloat);
    
      { TCustomEdit }
    
      TCustomEdit = class(TWinControl)
      private
        ...
        FTmpModified : Boolean;
        FceFilter : TCustomEditFilters;
        procedure SetceFilter(aCeFilter: TCustomEditFilters);
        function GetAsInteger:Integer;
        procedure SetAsInteger(aVal: Integer);
        function GetAsFloat:Extended;
        procedure SetAsFloat(aVal: Extended);
        function GetAsCurrency:Currency;
        procedure SetAsCurrency(aVal: Currency);
    
      public
        ...
        property TmpModified: Boolean read FTmpModified write FTmpModified;
        property AsInteger: Integer read getAsInteger write setAsInteger;
        property AsFloat: Extended read getAsFloat write setAsFloat;
        property AsCurrency: Currency read getAsCurrency write setAsCurrency;
      published
        property Alignment: TAlignment read FAlignment write SetAlignment default taLeftJustify;
        property ceFilter : TCustomEditFilters read FceFilter write SetceFilter default cefNone;
      end;
    
    //On the implementation part tcustomedit.inc:
    
      resourcestring
        rsInvalidNumbe = 'Invalid number !';
    
      procedure TCustomEdit.Change;
      begin
        FTmpModified := True;
        if Assigned(FOnChange) then FOnChange(Self);
      end;
    
      procedure TCustomEdit.DoExit;
      begin
        if FCeFilter <> cefNone then
        begin
          case FCeFilter of
            cefInteger, cefUnsigned: AsInteger;
            cefFloat: AsFloat;
            cefCurrency: AsCurrency;
          end;
        end;
        FAutoSelected := False;
        inherited DoExit;
      end;
    
      procedure TCustomEdit.SetceFilter(aCeFilter: TCustomEditFilters);
      begin
        FCeFilter := aCeFilter;
        if FCeFilter = cefNone then Alignment := taLeftJustify
        else Alignment := taRightJustify;
      end;
    
      function TCustomEdit.GetAsInteger:Integer;
      begin
        TryStrToInt(Text, result);
      end;
    
      procedure TCustomEdit.SetAsInteger(aVal: Integer);
      begin
         Text := IntToStr(aVal);
      end;
    
      function TCustomEdit.GetAsFloat:Extended;
      begin
        if Text = '' then result := 0
        else if not TryStrToFloat(Text, result) then
          Raise Exception.Create(rsInvalidNumbe);
      end;
    
      procedure TCustomEdit.SetAsFloat(aVal: Extended);
      begin
         Text := FloatToStr(aVal);
      end;
    
      function TCustomEdit.GetAsCurrency:Currency;
      begin
        if Text = '' then result := 0
        else if not TryStrToCurr(Text, result) then
          raise Exception.Create(rsInvalidNumbe);
      end;
    
      procedure TCustomEdit.SetAsCurrency(aVal: Currency);
      begin
         Text := CurrToStr(aVal);
      end;
    

## Replacing VK_DECIMAL keypad dot by comma

When writing Lazarus applications to non-US locales there is a problem with the keypad "." (dot) when the locale uses "," (comma) as decimal separator. I've been looking for a solution and ended up with this: 

Form.KeyPreview := True; Form.OnKeyDown := @FormKeyDown; Form.OnKeyPress := @FormKeyPress; 
    
    
      { TFormBase }
    
      TFormBase = class(TForm)
        ...
      protected
        { protected declarations }
        fVK_Decimal : Boolean;
        ...
      end;
    
    procedure TFormBase.FormKeyPress(Sender: TObject; var Key: char);
    begin
       if fVK_Decimal and (key = '.') then key := DecimalSeparator;
    end;
    
    procedure TFormBase.FormKeyDown(Sender: TObject; var Key: word; Shift: TShiftState);
    begin
      if (Shift = []) and (key = VK_DECIMAL) then
        fVK_Decimal := True; 
    end;
    

## See also

  * [Creating A Patch](<Creating_A_Patch.md> "Creating A Patch") \- How to get your modifications into the Lazarus codebase

---

_Source: [https://wiki.freepascal.org/Lazarus_Hacks](https://web.archive.org/web/20240308084623/https://wiki.freepascal.org/Lazarus_Hacks)_
