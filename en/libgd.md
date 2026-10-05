# libgd

The **gd** unit is the interface unit for the gd library. The gd library is a library to produce graphics files (jpeg,png and wbmp), available from this site. 

It was converted from the header files for version 1.8.4., but other versions may work as well. 

It has some additional calls which make it more pascal like: 
    
    
    function fopen(a,b:string):pFile;
    procedure gdImageChar(im:gdImagePtr; f:gdFontPtr; x:longint; y:longint; c:char; color:longint);
    procedure gdImageCharUp(im:gdImagePtr; f:gdFontPtr; x:longint; y:longint; c:char; color:longint);
    procedure gdImageString(im:gdImagePtr; f:gdFontPtr; x:longint; y:longint; s:string;  color:longint);
    procedure gdImageStringUp(im:gdImagePtr; f:gdFontPtr; x:longint; y:longint; s:string; color:longint);
    procedure gdImageString16(im:gdImagePtr; f:gdFontPtr; x:longint; y:longint; s:string; color:longint);
    procedure gdImageStringUp16(im:gdImagePtr; f:gdFontPtr; x:longint; y:longint; s:string; color:longint);
    {$ifdef hasttf}
    function  gdImageStringTTF(im:PgdImage; brect:Plongint; fg:longint; fontlist:string; ptsize:double; angle:double; x:longint; y:longint; astring:string): string;
    function  gdImageStringFT(im:PgdImage; brect:Plongint; fg:longint; fontlist:string; ptsize:double; angle:double; x:longint; y:longint; astring:string):string;
    {$endif}
    

These functions will work with both ansistrings and shortstrings; The unit can be compiled in both the {$H+} as the {$H-} state. 

The HASTTF define should be defined for libraries which have TTF2 lib support compiled in. By default, this is not included in the unit, so if you need TrueType Font support, you will need to recomlpile the unit. 

The **gdtest** example is adapted from the example of the gd unit by Mike Bradbery. It shows how to use the pascal-like functions in the gd unit instead of the raw C like functions which use pchars. 

the **gdtestcgi** example shows how to output an image to standard output, this can be used for CGI scripts. 

Go to back [Packages List](<Package_List.md> "Package List")

---

_Source: [https://wiki.freepascal.org/libgd](https://web.archive.org/web/20240121085819/https://wiki.freepascal.org/libgd)_
