# Widestrings

│ **English (en)** │

## Widestring

A widestring holds string data but the contents of that string are stored differently depending on operating system and FPC version. Non-Windows: 

  * (FPC 2.2-, 2.4, 2.6): Plain UTF16 (aka Kylix widestring).



Windows: 

  * FPC 2.2- plain UTF16 (aka Kylix widestring).
  * FPC 2.4+ COM compatible UTF16 (UCS2 on Windows 2000) encoded bytes.



See [LCL Unicode Support](<LCL_Unicode_Support.md> "LCL Unicode Support")

## Old content

_Is this still relevant?_ \--[BigChimp](</User:BigChimp> "User:BigChimp") 07:59, 23 June 2012 (UTC) the following code from rtl/inc/wstrings.inc is used for assignments between ansistrings and widestrings. 
    
    
    procedure Wide2AnsiMove(source:pwidechar;dest:pchar;len:SizeInt);
    var
      i : SizeInt;
    begin
      for i:=1 to len do
       begin
         if word(source^)<128 then
          dest^:=char(word(source^))
         else
          dest^:=' ';
         inc(dest);
         inc(source);
       end;
    end;
    
    
    procedure Ansi2WideMove(source:pchar;dest:pwidechar;len:SizeInt);
    var
      i : SizeInt;
    begin
      for i:=1 to len do
       begin
         if byte(source^)<128 then
          dest^:=widechar(byte(source^))
         else
          dest^:=' ';
         inc(dest);
         inc(source);
       end;
    end;
    
    Const
      Wide2AnsiMoveProc:TWide2AnsiMove=@Wide2AnsiMove;
      Ansi2WideMoveProc:TAnsi2WideMove=@Ansi2WideMove;
    

the procvars are supposed to allow you to replace theese with a converter more suited to the local charset. Unfortunately they do not allow for the "ansi" charset to be multibyte as they assume the number of ansichars in the ansistring will equal the number of widechars in the widestring. 

## See also

  * [Character and string types](<Character_and_string_types.md> "Character and string types")

---

_Source: [https://wiki.freepascal.org/Widestrings](https://web.archive.org/web/20250325163840/https://wiki.freepascal.org/Widestrings)_
