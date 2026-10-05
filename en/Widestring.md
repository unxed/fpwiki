# Widestring

│ **English (en)** │

  
Back to [data types](<Data_type.md> "Data type"). 

  
The [data type](<Data_type.md> "Data type") WideString has no size limit and comprises of an array of type [WideChar](<WideChar.md> "WideChar"). The functions of the LazUTF8 unit are there to facilitate the conversion of types from [AnsiString](<Ansistring.md> "Ansistring") to WideString and from WideString to [AnsiString](<Ansistring.md> "Ansistring"). 

The examples below are for Windows operating systems! 
    
    
    Uses
      ...,
      LazUTF8,
      ...;
      
    ...
    
    { Definition of the WideString and AnsiString data types }
    
    Var 
      w: WideString;
      a: AnsiString; 
    
    Begin
      { Examples of correct assignment of AnsiString to WideString }
    
      w: = UTF8ToUTF16 ('0123ABCabc456AöU!, .-');
      w: = w + UTF8ToUTF16 (IntToString (45)); 
    
      { Example of correct assignment of a WideString to an AnsiString }
    
      a: = UTF16ToUTF8 (w);
    
      ...
    end;

---

_Source: [https://wiki.freepascal.org/Widestring](https://web.archive.org/web/20221007121359/https://wiki.freepascal.org/Widestring)_
