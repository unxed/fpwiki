# TString

**!! Most of this can be achieved in modern versions using type handlers, without defining a separate class.**

The poster of this article doesn't motivate the reasons for doing this, so it is not clear if that fact obsoletes this article. 

## Contents

  * 1 TString
  * 2 Credits
  * 3 Unit
  * 4 Testing



## TString

The goal of this is create an object that holds a string and useful string methods in one class. (Why?) 

## Credits

If you improve it please add you to the contributors list. 
    
    
    {
     
    Contributors: (lainz -007-)
     
    A TString stores a string with should have at least all the methods
    listed in the String object of JavaScript
    https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/
     
    Version 0.1
    (lainz -007-)
    * getLength, getLengthStr, toLowerCase, toUpperCase, trim, trimLeft, trimRight, valueOf
     
    }

## Unit

This is the unit that holds TString object. 
    
    
    unit ustring;
     
    {$mode objfpc}{$H+}
     
    interface
     
    uses
      Classes, SysUtils,
      Dialogs; //<-- just for testing
     
    type
     
      { TString }
     
      TString = class
      private
        FValue: string;
        procedure SetFValue(AValue: string);
      public
        constructor Create(AValue: string);
        destructor Destroy; override;
      public
        function getLength: integer; // length
        function getLengthStr: string;
     
        function toLowerCase: string;
        function toUpperCase: string;
     
        function trim: string;
        function trimLeft: string;
        function trimRight: string;
     
        function valueOf: string;
      public
        property Value: string read FValue write SetFValue;
      end;
     
    implementation
     
    { TString }
     
    procedure TString.SetFValue(AValue: string);
    begin
      if FValue = AValue then
        Exit;
      FValue := AValue;
    end;
     
    constructor TString.Create(AValue: string);
    begin
      Value := AValue;
    end;
     
    destructor TString.Destroy;
    begin
      inherited Destroy;
    end;
     
    function TString.getLength: integer;
    begin
      Result := length(Value);
    end;
     
    function TString.getLengthStr: string;
    begin
      Result := IntToStr(GetLength);
    end;
     
    function TString.toUpperCase: string;
    begin
      Result := AnsiUpperCase(Value);
    end;
     
    function TString.toLowerCase: string;
    begin
      Result := AnsiLowerCase(Value);
    end;
     
    function TString.trim: string;
    begin
      Result := SysUtils.Trim(Value);
    end;
     
    function TString.trimLeft: string;
    begin
      Result := SysUtils.TrimLeft(Value);
    end;
     
    function TString.trimRight: string;
    begin
      Result := SysUtils.TrimRight(Value);
    end;
     
    function TString.valueOf: string;
    begin
      Result := Value;
    end;
     
    end.

## Testing

This is a test of each method. 
    
    
    ...
     
    { Testing TString }
     
    var
      myString: TString;
      l: integer;
     
    initialization
     
      {* Basic Structure *}
     
      // create
      myString := TString.Create('Hello');
     
      // assign
      myString.Value := 'Hello there!';
      ShowMessage(myString.Value); // Hello there!
     
      {* Get properties *}
     
      // getLength
      l := myString.GetLength; // 12
     
      // getLengthStr
      ShowMessage(myString.GetLengthStr); // '12'
     
      {* Case *}
     
      // toLowerCase
      ShowMessage(myString.toLowerCase); // 'hello there!'
     
      // toUpperCase
      ShowMessage(myString.toUpperCase); // 'HELLO THERE!'
     
      {* Trimming whitespace *}
     
      // trim
      myString.Value := '  abc  ';
      ShowMessage('"' + myString.trim + '"'); // 'abc'
     
      // trimLeft
      ShowMessage('"' + myString.trimLeft + '"'); // 'abc  '
     
      // trimRight
      ShowMessage('"' + myString.trimRight + '"'); // '  abc'
     
      {* Misc *}
     
      // valueOf
      ShowMessage(myString.valueOf); // same as reading myString.Value
     
     
    finalization
      myString.Free;
     
    end.

---

_Source: [https://wiki.freepascal.org/TString](https://web.archive.org/web/20250301000000/https://wiki.freepascal.org/TString)_
