# UTF8 Tools

│ **English (en)** │  **[русский (ru)](<../ru/UTF8_Tools.md> "UTF8 Tools/ru")** │ 

## Contents

  * 1 About
  * 2 Units
    * 2.1 Using streams
    * 2.2 Character info
    * 2.3 Access UTF-8 by code index
  * 3 Download



## About

This code allows to process Unicode text and determine for unicode chars: 

  * if char "letter"
  * if char "digit"
  * if char upper-case, lower-case
  * if char "white space"
  * if char "punctuation"
  * etc.



Also it has class to read/write Unicode from/to TStream. 

# Units

## Using streams

Unit "charencstreams": load/save data from/to almost any text source: 

  * ANSI, UTF8, UTF16, UTF32
  * big-endian, little-endian
  * with/without BOM



Demo: 
    
    
     f := TCharEncStream.Create;
     f.LoadFromFile(OpenDialog1.FileName);
     Memo1.Text := f.UTF8Text;  
     f.Free;
    

## Character info

Unit "character": get information about code points using the TCharacter class. Demo: 
    
    
     if TCharacter.IsLetter(s[i]) then
        s[i] := TCharacter.toLower(s[i]);
    

  


## Access UTF-8 by code index

Unit "utf8scanner": access UTF-8 strings by code index, use case statements on UTF-8 strings and more. Demo: 
    
    
     s := TUTF8Scanner.Create(Memo1.Text);
     for i := 1 to s.Length do
       if TCharacter.IsLetter(s[i]) then
         s[i] := TCharacter.toLower(s[i]);
     Memo1.Text := s.UTF8String;
     s.Free;
    

Case demo: 
    
    
      s := TUTF8Scanner.Create(Memo1.Text);
      s.FindChars := 'öäü';
      repeat
        case s.FindIndex(s.Next) of
      {ö} 0: s.Replace('oe');
      {ä} 1: s.Replace('ae');
      {ü} 2: s.Replace('ue');
        end;
      until s.Done;
      Memo1.Text := s.UTF8String;
      s.Free;
    

# Download

[Download utf8tools.zip](<http://www.theo.ch/lazarus/utf8tools.zip>)

---

_Source: [https://wiki.freepascal.org/UTF8_Tools](https://web.archive.org/web/20250422003841/https://wiki.freepascal.org/UTF8_Tools)_
