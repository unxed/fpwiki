# Old LCL Unicode Support

**For historical reference only.**  
For current information about Unicode support in Lazarus see [Unicode Support in Lazarus](<Unicode_Support_in_Lazarus.md> "Unicode Support in Lazarus"). 

  


## Contents

  * 1 Introduction
  * 2 Instructions for users
    * 2.1 FPC is not Unicode aware
    * 2.2 Converting between ANSI and Unicode
    * 2.3 Widestrings and Ansistrings
    * 2.4 UTF8 strings and characters
      * 2.4.1 UTF-8 String Copy, Length, LowerCase, etc
    * 2.5 Dealing with directory and filenames
    * 2.6 East Asian languages on Windows
    * 2.7 Usage of UTF-8
  * 3 Free Pascal Particularities
    * 3.1 UTF8 and source files - the missing BOM
  * 4 Unicode essentials
  * 5 Implementation Details
    * 5.1 Lazarus/LCL generally uses only UTF-8
    * 5.2 Unicode-enabling the win32 interface
      * 5.2.1 Overview
      * 5.2.2 No Unicode support on Win9x
        * 5.2.2.1 Wide functions present on Windows 9x
        * 5.2.2.2 Functions that need Ansi and Wide versions
      * 5.2.3 Screenshots
  * 6 FPC codepages
  * 7 See Also



## Introduction

As of 0.9.25, Lazarus has full Unicode support in all platforms except Gtk 1. This page covers Unicode in Lazarus up to version 1.4 which still uses FPC 2.6.4. 

Starting from 2.0 Lazarus will have improved Unicode support using features of FPC 3.0+. See details here: [Unicode Support in Lazarus](<Unicode_Support_in_Lazarus.md> "Unicode Support in Lazarus")

## Instructions for users

Even though Lazarus has Unicode widgetsets, it's important to note that not everything is Unicode. It's the responsibility of the developer to know what is the encoding of their strings and do the proper conversion between libraries which expect different encodings. 

Usually the encoding is per library (e.g. a dynamic library DLL or a Lazarus package). Each library will uniformly expect 1 kind of encoding, which will usually either be Unicode (UTF-8 for Lazarus) or ANSI (which actually means the system encoding, and may be UTF-8 or not). The RTL and the FCL of FPC <= 2.6 expect ANSI strings. 

You can convert between Unicode and ANSI using 

  * the **UTF8ToAnsi** and **AnsiToUTF8** functions from the (FPC) System unit
  * or the **UTF8ToSys** and **SysToUTF8** from the (Lazarus) FileUtil unit.



The latter two are smarter (faster) but pull more code into your program. 

### FPC is not Unicode aware

The Free Pascal Runtime Library (RTL), and the Free Pascal Free Component Library (FCL) in current FPC versions (<= 2.6.x) are ANSI, so you will need to convert strings coming from Unicode libraries or going to Unicode libraries (e.g. the LCL). 

There are significant improvements to development branches of FPC 2.7.1 with regard to Strings. See RawByteString and UTF8String in [FPC Unicode support](<FPC_Unicode_support.md> "FPC Unicode support"). 

### Converting between ANSI and Unicode

[![Note-icon.png](https://wiki.freepascal.org/images/b/be/Note-icon.png)](</File:Note-icon.png>)

**Note:** AnsiToUTF8 and UTF8ToAnsi require a widestring manager under Linux, BSD and Mac OS X. You can use the SysToUTF8 and UTF8ToSys functions (unit FileUtil) or add the widestring manager by adding cwstring as one of the first units to your program's uses section.

Examples: 

Say you get a string from a TEdit and you want to give it to some RTL file routine: 
    
    
    var
      MyString: string; // utf-8 encoded
    begin
      MyString := MyTEdit.Text;
      SomeRTLRoutine(UTF8ToAnsi(MyString));
    end;
    

And for the opposite direction: 
    
    
    var
      MyString: string; // ANSI encoded
    begin
      MyString := SomeRTLRoutine;
      MyTEdit.Text := AnsiToUTF8(MyString);
    end;
    

### Widestrings and Ansistrings

A widestring is a string type whose basic data holding elements have a size of 2 bytes. Widestrings almost always hold data in the UTF-16 encoding. See [Widestrings](<Widestrings.md> "Widestrings")

Note that while each data point accessible as an array of a widestring has 2 bytes, in UTF-16 a character may have 1 or 2 data points, which would then occupy 2 or 4 bytes. This means that accessing a Widestring as an array and expecting to obtain UTF-16 characters this way is **completely wrong** and will fail when a 4 byte character is present in the string. Note also that UTF-16, like UTF-8, may have decomposed characters. The character "Á" for example might be encoded as a single character or as 2 characters: "A" + a modifying accent. Thus in Unicode a text which involves accented letters can often be encoded in multiple ways and Lazarus and FPC do not handle this automatically. 

When passing Ansistrings to Widestrings you have to convert the encoding. 
    
    
    var 
      w: widestring;
    begin
      w:='Über'; // wrong, because FPC will convert system codepage to UTF16
      w:=UTF8ToUTF16('Über'); // correct
      Button1.Caption:=UTF16ToUTF8(w);
    end;
    

### UTF8 strings and characters

Until Lazarus 0.9.30 the UTF-8 handling routines were in the LCL in the unit LCLProc. In Lazarus 0.9.31+ the routines in LCLProc are still available for backwards compatibility but the real code to deal with UTF-8 is located in the lazutils package in the unit lazutf8. 

To execute operations on UTF-8 strings please use routines from the unit lazutf8 instead of routines from the SysUtils routine from Free Pascal, because SysUtils is not yet prepared to deal with Unicode, while lazutf8 is. Simply substitute the routines from SysUtils with their lazutf8 equivalent, which always has the same name except for an added "UTF8" prefix. 

#### UTF-8 String Copy, Length, LowerCase, etc

Nearly all operations which one might want to execute with UTF-8 strings are covered by the routines in the unit lazutf8 (unit LCLProc for Lazarus 0.9.30 or lower). See the following list of routines taken from lazutf8.pas: 
    
    
    function UTF8CharacterLength(p: PChar): integer;
    function UTF8Length(const s: string): PtrInt;
    function UTF8Length(p: PChar; ByteCount: PtrInt): PtrInt;
    function UTF8CharacterToUnicode(p: PChar; out CharLen: integer): Cardinal;
    function UnicodeToUTF8(u: cardinal; Buf: PChar): integer; inline;
    function UnicodeToUTF8SkipErrors(u: cardinal; Buf: PChar): integer;
    function UnicodeToUTF8(u: cardinal): shortstring; inline;
    function UTF8ToDoubleByteString(const s: string): string;
    function UTF8ToDoubleByte(UTF8Str: PChar; Len: PtrInt; DBStr: PByte): PtrInt;
    function UTF8FindNearestCharStart(UTF8Str: PChar; Len: integer;
                                      BytePos: integer): integer;
    // find the n-th UTF8 character, ignoring BIDI
    function UTF8CharStart(UTF8Str: PChar; Len, CharIndex: PtrInt): PChar;
    // find the byte index of the n-th UTF8 character, ignoring BIDI (byte len of substr)
    function UTF8CharToByteIndex(UTF8Str: PChar; Len, CharIndex: PtrInt): PtrInt;
    procedure UTF8FixBroken(P: PChar);
    function UTF8CharacterStrictLength(P: PChar): integer;
    function UTF8CStringToUTF8String(SourceStart: PChar; SourceLen: PtrInt) : string;
    function UTF8Pos(const SearchForText, SearchInText: string): PtrInt;
    function UTF8Copy(const s: string; StartCharIndex, CharCount: PtrInt): string;
    procedure UTF8Delete(var s: String; StartCharIndex, CharCount: PtrInt);
    procedure UTF8Insert(const source: String; var s: string; StartCharIndex: PtrInt);
    
    function UTF8LowerCase(const AInStr: string; ALanguage: string=''): string;
    function UTF8UpperCase(const AInStr: string; ALanguage: string=''): string;
    function FindInvalidUTF8Character(p: PChar; Count: PtrInt;
                                      StopOnNonASCII: Boolean = false): PtrInt;
    function ValidUTF8String(const s: String): String;
    
    procedure AssignUTF8ListToAnsi(UTF8List, AnsiList: TStrings);
    
    //compare functions
    
    function UTF8CompareStr(const S1, S2: string): Integer;
    function UTF8CompareText(const S1, S2: string): Integer;
    

### Dealing with directory and filenames

Lazarus controls and functions expect filenames and directory names in UTF-8 encoding, but the RTL uses ANSI strings for directories and filenames. 

For example, consider a button which sets the Directory property of the TFileListBox to the current directory. The RTL Function [GetCurrentDir](<http://lazarus-ccr.sourceforge.net/docs/rtl/sysutils/getcurrentdir.html> "doc:rtl/sysutils/getcurrentdir.html") is ANSI, not Unicode, so conversion is needed: 
    
    
    procedure TForm1.Button1Click(Sender: TObject);
    begin
      FileListBox1.Directory:=SysToUTF8(GetCurrentDir);
      // or use the functions from the FileUtil unit
      FileListBox1.Directory:=GetCurrentDirUTF8;
    end;
    

The unit FileUtil defines common file functions with UTF-8 strings: 
    
    
    // basic functions similar to the RTL but working with UTF-8 instead of the
    // system encoding
    
    // AnsiToUTF8 and UTF8ToAnsi need a widestring manager under Linux, BSD, Mac OS X
    // but normally these OS use UTF-8 as system encoding so the widestringmanager
    // is not needed.
    function NeedRTLAnsi: boolean;// true if system encoding is not UTF-8
    procedure SetNeedRTLAnsi(NewValue: boolean);
    function UTF8ToSys(const s: string): string;// as UTF8ToAnsi but more independent of widestringmanager
    function SysToUTF8(const s: string): string;// as AnsiToUTF8 but more independent of widestringmanager
    
    // file operations
    function FileExistsUTF8(const Filename: string): boolean;
    function FileAgeUTF8(const FileName: string): Longint;
    function DirectoryExistsUTF8(const Directory: string): Boolean;
    function ExpandFileNameUTF8(const FileName: string): string;
    function ExpandUNCFileNameUTF8(const FileName: string): string;
    {$IFNDEF VER2_2_0}
    function ExtractShortPathNameUTF8(Const FileName : String) : String;
    {$ENDIF}
    function FindFirstUTF8(const Path: string; Attr: Longint; out Rslt: TSearchRec): Longint;
    function FindNextUTF8(var Rslt: TSearchRec): Longint;
    procedure FindCloseUTF8(var F: TSearchrec);
    function FileSetDateUTF8(const FileName: String; Age: Longint): Longint;
    function FileGetAttrUTF8(const FileName: String): Longint;
    function FileSetAttrUTF8(const Filename: String; Attr: longint): Longint;
    function DeleteFileUTF8(const FileName: String): Boolean;
    function RenameFileUTF8(const OldName, NewName: String): Boolean;
    function FileSearchUTF8(const Name, DirList : String): String;
    function FileIsReadOnlyUTF8(const FileName: String): Boolean;
    function GetCurrentDirUTF8: String;
    function SetCurrentDirUTF8(const NewDir: String): Boolean;
    function CreateDirUTF8(const NewDir: String): Boolean;
    function RemoveDirUTF8(const Dir: String): Boolean;
    function ForceDirectoriesUTF8(const Dir: string): Boolean;
    
    // environment
    function ParamStrUTF8(Param: Integer): string;
    function GetEnvironmentStringUTF8(Index : Integer): String;
    function GetEnvironmentVariableUTF8(const EnvVar: String): String;
    function GetAppConfigDirUTF8(Global: Boolean): string;
    

### East Asian languages on Windows

The default font (Tahoma) for user interface controls under Windows XP is capable of correctly displaying several scripts/alphabets/languages, including Arabic, Russian (Cyrillic alphabet) and Western languages (Latin/Greek alphabets), but not East Asian languages, like Chinese, Japanese and Korean. 

Simply by going to the Control Panel, choosing Regional Settings, clicking on the Languages Tab and installing the East Asia Language Pack, the standard user interface font will start showing those languages correctly. Obviously Windows XP versions localized for those languages will already have this language pack installed. Extended instructions [here](<http://newton.uor.edu/Departments&Programs/AsianStudiesDept/Language/asianlanguageinstallation_XP.html>). 

Later Windows versions presumably have support for these languages out of the box. 

### Usage of UTF-8

See details in [UTF8 strings and characters](<UTF8_strings_and_characters.md> "UTF8 strings and characters")

## Free Pascal Particularities

### UTF8 and source files - the missing BOM

When you create source files with Lazarus and type some non-ASCII characters the file is saved in UTF8. It does **not** use a **BOM** (Byte Order Mark). You can change the encoding via right click on source editor / File Settings / Encoding. Apart from the fact that UTF-8 files are not supposed to have BOMs, the reason for the lacking BOM is how FPC treats Ansistrings. For compatibility the LCL uses Ansistrings and for portability the LCL uses UTF8. 

Note: Some MS Windows text editors might treat the files as encoded with the system codepage (OEM codepage) and show them as invalid characters. **Do not add the BOM.** If you add the BOM you have to change all string assignments. 

For example: 
    
    
    Button1.Caption := 'Über';
    

When no BOM is given (and no codepage parameter was passed) the compiler treats the string as system encoding and copies each byte unconverted to the string. This is how the LCL expects strings. 
    
    
    // source file saved as UTF without BOM
    if FileExists('Über.txt') then ; // wrong, because FileExists expects system encoding
    if FileExistsUTF8('Über.txt') then ; // correct
    

## Unicode essentials

The Unicode standard maps integers from 0 to 10FFFF(h) to characters. Each such mapping is called a code point. In other words, Unicode characters are in principle defined for code points from U+000000 to U+10FFFF (0 to 1 114 111). 

There are three major schemes for representing Unicode code points as unique byte sequences. These schemes are called Unicode transformation formats: UTF-8, UTF-16 and UTF-32. Conversions between all of them are possible. Here are their basic properties: 
    
    
                               UTF-8 UTF-16 UTF-32
    Smallest code point [hex] 000000 000000 000000
    Largest code point  [hex] 10FFFF 10FFFF 10FFFF
    Code unit size [bits]          8     16     32
    Minimal bytes/character        1      2      4
    Maximal bytes/character        4      4      4
    

**UTF-8** has several important and useful properties: It is interpreted as a sequence of bytes, so that the concept of lo- and hi-order byte does not exist. Unicode characters U+0000 to U+007F (ASCII) are encoded simply as bytes 00h to 7Fh (ASCII compatibility). This means that files and strings which contain only 7-bit ASCII characters have the same encoding under both ASCII and UTF-8. All characters >U+007F are encoded as a sequence of several bytes, each of which has the two most significant bits set. No byte sequence of one character is contained within a longer byte sequence of another character. This allows easy searching for substrings. The first byte of a multibyte sequence (representing a non-ASCII character) is always in the range C0h to FDh and it indicates how many bytes follow for this character. All further bytes in a multibyte sequence are in the range 80h to BFh. This allows easy resynchronization and robustness. 

**UTF-16** has the following most important properties: It uses a single 16-bit word to encode characters from U+0000 to U+d7ff, and a pair of 16-bit words to encode any of the remaining Unicode characters. 

Finally, any Unicode character can be represented as a single 4 byte/32-bit unit in **UTF-32**. 

For more, see: [Unicode FAQ - Basic questions](<http://www.unicode.org/faq/basic_q.html>), [Unicode FAQ - UTF-8, UTF-16, UTF-32 & BOM](<http://www.unicode.org/faq/utf_bom.html>), [Wikipedia: UTF-8](<http://en.wikipedia.org/wiki/UTF-8>) [[1]](<http://en.wikipedia.org/wiki/ISO-8859>)

# Implementation Details

## Lazarus/LCL generally uses only UTF-8

Since the GTK1 interface was declared obsolete in Lazarus 0.9.31, all LCL interfaces are Unicode capable and the LCL uses and accepts only UTF-8 encoded strings, unless in routines explicitly marked as accepting other encodings. 

## Unicode-enabling the win32 interface

### Overview

First, and most importantly, all Unicode patches for the Win32 interface must be enclosed by IFDEF WindowsUnicodeSupport, to avoid breaking the existing ANSI interface. After this stabilizes, all ifdefs will be removed and only the Unicode part will remain. At his moment all existing programs that use ANSI characters will need migration to Unicode. 

### No Unicode support on Win9x

Windows platforms <=Win9x are based on ISO code page standards and only partially support Unicode. Windows platforms starting with Windows NT (e.g. Windows 2000, XP, Vista, 7, 8) and Windows CE fully support Unicode. 

Win 9x and NT offer two parallel sets of API functions: the old ANSI enabled *A and the new, Unicode enabled *W. *W functions accept wide strings - UTF-16 encoded strings - as parameters. 

Windows 9x has all *W functions but they mostly have empty implementations, so they do nothing. Only some some *W functions are fully implemented in 9x; these are listed below in the section "Wide functions present on Windows 9x". This property is relevant as it allows to have one single application for both Win9x and WinNT and detect at runtime which set of APIs to use. 

Windows CE only uses Wide API functions. 

#### Wide functions present on Windows 9x

Some Wide API functions are present on Windows 9x. Here is a list of such functions: <http://support.microsoft.com/kb/210341>

Conversion example: 
    
    
    GetTextExtentPoint32(hdcNewBitmap, LPSTR(ButtonCaption),
    Length(ButtonCaption), TextSize);
    

Becomes: 
    
    
    {$ifdef WindowsUnicodeSupport}
      GetTextExtentPoint32W(hdcNewBitmap, PWideChar(Utf8Decode(ButtonCaption)), Length(WideCaption), TextSize);
    {$else}
      GetTextExtentPoint32(hdcNewBitmap, LPSTR(ButtonCaption), Length(ButtonCaption), TextSize);
    {$endif}
    

#### Functions that need Ansi and Wide versions

First Conversion example: 
    
    
    function TGDIWindow.GetTitle: String;
    var
     l: Integer;
    begin
       l := Windows.GetWindowTextLength(Handle);
       SetLength(Result, l);
       Windows.GetWindowText(Handle, @Result[1], l);
    end;
    

Becomes: 
    
    
    function TGDIWindow.GetTitle: String;
    var
      l: Integer;
      AnsiBuffer: string;
      WideBuffer: WideString;
    begin
    
    {$ifdef WindowsUnicodeSupport}
    
    if UnicodeEnabledOS then
    begin
      l := Windows.GetWindowTextLengthW(Handle);
      SetLength(WideBuffer, l);
      l := Windows.GetWindowTextW(Handle, @WideBuffer[1], l);
      SetLength(WideBuffer, l);
      Result := Utf8Encode(WideBuffer);
    end
    else
    begin
      l := Windows.GetWindowTextLength(Handle);
      SetLength(AnsiBuffer, l);
      l := Windows.GetWindowText(Handle, @AnsiBuffer[1], l);
      SetLength(AnsiBuffer, l);
      Result := AnsiToUtf8(AnsiBuffer);
    end;
    
    {$else}
    
       l := Windows.GetWindowTextLength(Handle);
       SetLength(Result, l);
       Windows.GetWindowText(Handle, @Result[1], l);
    
    {$endif}
    
    end;
    

### Screenshots

[![Lazarus Unicode Test.png](https://wiki.freepascal.org/images/4/49/Lazarus_Unicode_Test.png)](</File:Lazarus_Unicode_Test.png>)

# FPC codepages

The compiler (FPC) supports specifying the code page in which the source code has been written via the command option _-Fc_ (e.g. _-Fcutf8_) and the equivalent _codepage_ directive (e.g. _{$codepage utf8}_). In this case, rather than literally copying the bytes that represent the string constants in your program, the compiler will interpret all character data according to that codepage. There are two things to watch out for though: 

  * on Unix platforms, make sure you include a widestring manager by adding the _cwstring_ unit to your uses-clause. When using unit LazUtf8, this is already done. Without it, the program will not be able to convert all character data correctly when running. It's not included by default because this unit makes your program dependent on libc, which makes cross-compilation harder.
  * When using _-Fc_ or _$codepage_ the compiler converts all string constants that contain non-ASCII characters to widestring constants. These are automatically converted back to ansistring (either at compile time or at run time), but this can cause one caveat if you try to mix both characters and ordinal values in a single string constant:



For example: 
    
    
    program project1;
    {$codepage utf8}
    {$mode objfpc}{$H+}
    {$ifdef unix}
    uses cwstring;
    {$endif}
    var
      a,b,c: string;
    begin
      a:='ä';
      b:='='#$C3#$A4; // #$C3#$A4 is UTF-8 for ä
      c:='ä='#$C3#$A4;
      writeln(a,b); // writes ä=ä
      writeln(c);   // writes ä=Ã¤
    end.
    

When compiled and executed, this will write: 
    
    
    ä=ä
    ä=Ã¤
    

The reason is once the _ä_ is encountered, as mentioned above the rest of the constant string assigned to 'c' will be parsed as a widestring. As a result the #$C3 and #$A4 are interpreted as widechar(#$C3) and widechar(#$A4), rather than as ansichars. 

# See Also

  * [Character and string types](<Character_and_string_types.md> "Character and string types")
  * [UTF-8](<UTF-8.md> "UTF-8") \- Description of UTF-8 strings
  * [FPC Unicode support](<FPC_Unicode_support.md> "FPC Unicode support")
  * [for-in_loop#Traversing_UTF-8_strings](<for-in_loop.md> "for-in loop") \- Code snippet that shows how to traverse UTF8 strings
  * [unicode use cases](<unicode_use_cases.md> "unicode use cases")

---

_Source: [https://wiki.freepascal.org/Old_LCL_Unicode_Support](https://web.archive.org/web/20200616040100/https://wiki.freepascal.org/Old_LCL_Unicode_Support)_
