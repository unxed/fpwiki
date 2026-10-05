# International Bank Account Number

│ **English (en)** │  **[suomi (fi)](</International_Bank_Account_Number/fi> "International Bank Account Number/fi")** │  **[français (fr)](</International_Bank_Account_Number/fr> "International Bank Account Number/fr")** │    
****

The International Bank Account Number (IBAN) is an international standard for numbering bank accounts. It is an ISO 13616 standard. The IBAN consists of up to 34 alphanumeric characters, comprising a country code, two check digits and a bank account number. 

Country  | IBAN formatting example   
---|---  
Belgium  | `BE71 0961 2345 6769`  
Brazil  | `BR15 0000 0000 0000 1093 2840 814 P2`  
Costa Rica  | `CR99 0000 0000 0000 8888 88`  
France  | `FR76 3000 6000 0112 3456 7890 189`  
Ireland  | `IE12 BOFI 9000 0112 3456 78`  
Germany  | `DE91 1000 0000 0123 4567 89`  
Greece  | `GR96 0810 0010 0000 0123 4567 890`  
Mauritius  | `MU43 BOMM 0101 1234 5678 9101 000 MUR`  
Pakistan  | `PK70 BANK 0000 1234 5678 9000`  
Poland  | `PL10 1050 0099 7603 1234 5678 9123`  
Romania  | `RO09 BCYP 0000 0012 3456 7890`  
Saint Lucia  | `LC14 BOSL 1234 5678 9012 3456 7890 1234`  
Saudi Arabia  | `SA44 2000 0001 2345 6789 1234`  
Spain  | `ES79 2100 0813 6101 2345 6789`  
Sweden  | `SE87 3000 0000 0101 2345 6789`  
Switzerland  | `CH56 0483 5012 3456 7800 9`  
United Kingdom  | `GB98 MIDL 0700 9312 3456 78`  
  
Permitted IBAN characters are the digits _0_ to _9_ and the 26 Latin alphabetic characters _A_ to _Z_. This applies even in countries where these characters are not used in the national language (e.g., Thailand). 

The country code uses ISO 3166-1 alpha-2. The check digits are calculated according to [ISO 7064](<ISO_7064.md> "ISO 7064") (MOD97-10) using [StrMOD97](<ISO_7064.md> "ISO 7064"). 

## Function IsValidIBAN

Function IsValidIBAN() checks if an IBAN is valid. Code from forum member **wp**. 
    
    
    Program iban;
     
    {$mode objFPC} {$H+}
     
    Uses
     StrUtils, SysUtils;
     
    // Calculate Modulo 97-10
    function StrMOD97(s: String): integer;
    const
      modu = 97;
    var
      sx: String;
      isx, ic, p: Integer;
    begin
       p := Length(s);
       while (p > 9) do
         begin
           sx := Copy(s, 1, 9);
           Delete(s, 1, 9);
           isx := StrToInt(sx);
           ic := isx mod modu;
           s := IntToStr(ic) + s;
           p := Length(s);
         end;
       isx := StrToInt(s);
       if isx >= modu
         then ic := isx mod modu
       else ic := isx;
      result := ic;
    end;
     
    function ProcessChar(ch: char):string;
    begin
      if (ch in ['0'..'9']) then
        Result := ch
      else
      // IBAN letter to number algorithm
      if (ch in ['A'..'Z', 'a'..'z']) then
        result := IntToStr(ord(upCase(ch)) - ord('A') + 10)
      else
        result := '';
    end;  
     
    // Check IBAN validity
    function IsValidIBAN(s: string): boolean;
    var
      s1: string = '';
      ch: String;
      i: Integer;
    begin
      Result := false;
     
      // Delete any spaces
      s := DelSpace(s);
     
      // Obvious tests
      if Length(s) < 4 then
        exit;
      if not ((s[1] in ['A'..'Z']) and (s[2] in ['A'..'Z'])) then
        exit;
      if not ((s[3] in ['0'..'9']) and (s[4] in ['0'..'9'])) then
        exit;
     
      // Process number block first
      for i := 5 to Length(s) do
      begin
        ch := ProcessChar(s[i]);    // Convert letters to numbers, keep numbers
        if ch = '' then
          exit;
        s1 := s1 + ch;
      end;
      // Move country code and check sum to the end of the test string
      for i := 1 to 4 do
        s1 := s1 + ProcessChar(s[i]);
     
      result := (StrMOD97(s1) = 1);
    end;
     
    type
      TTestRec = record
        TestIBAN: String;
        Expected: Boolean;
      end;
     
    const
      // Expected results validated by https://www.ibancalculator.com/iban_validieren.html
      Tests: array[0..10] of TTestRec = (
        (TestIBAN:'GB82 WEST 1234 5698 7654 32';             Expected: true),
        (TestIBAN:'GB82 TEST 1234 5698 7654 32';             Expected: false),
        (TestIBAN:'GB82 WEST12345698765432';                 Expected: true),
        (TestIBAN:'GB 82WEST12345698765432';                 Expected: true),
        (TestIBAN:'GB82 WEST123 456 987 654 32';             Expected: true),
        (TestIBAN:'CH93 0076 2011 6238 5295 7';              Expected: true),
        (TestIBAN:'IL62 0108 0000 0009 9999 999';            Expected: true),
        (TestIBAN:'GR16 0110 1250 0000 0001 2300 695';       Expected: true),
        (TestIBAN:'US12 3456 7890 0987 6543 210';            Expected: false),
        (TestIBAN:'LC14 BOSL 1234 5678 9012 3456 7890 1234'; Expected: true),
        (TestIBAN:'BR15 0000 0000 0000 1093 2840 814 P2';    Expected: true )
      );
      W = 40;
    var
      i: Integer;
      s: String;
      ok: Boolean;
    begin
      for i := 0 to High(Tests) do
      begin
        s := Tests[i].TestIBAN;
        ok := IsValidIBAN(s);
        if ok = Tests[i].Expected then
        begin
          if ok then
            WriteLn(s:W, ': Correct IBAN correctly detected')
          else
            WriteLn(s:W, ': Incorrect IBAN correctly detected.');
        end else
        if Tests[i].Expected then
          WriteLn(s:W, ': ERROR (Correct IBAN not detected)')
        else
          WriteLn(s:W, ': ERROR (Incorrect IBAN not detected)');
      end;
     
      ReadLn;
    end.
    

## See Also

  * [IBAN](<https://www.rosettacode.org/wiki/IBAN#Free_Pascal>) on RosettaCode.org

---

_Source: [https://wiki.freepascal.org/International_Bank_Account_Number](https://web.archive.org/web/20230529013000/https://wiki.freepascal.org/International_Bank_Account_Number)_
