# Lazarus with FPC3.0 without UTF-8 mode

This page explains the problems and their possible solutions when using Lazarus with FPC 3.0+ but without the new UTF-8 mode. It may be needed when big parts of program code depend on system codepage. The UTF-8 mode is explained here: 

  * [Unicode Support in Lazarus](<Unicode_Support_in_Lazarus.md> "Unicode Support in Lazarus")



To disable the new UTF-8 mode, **-dDisableUTF8RTL** must be defined in project options, "Additions and Overrides" page. There is a button "Use system encoding" to help adding it with one click. This way it affects both the project and all its dependent packages, including LazUtils which contains the relevant code. If you use strings with codepoints > 127 in your project, you have to change the file encoding to the system encoding too. 

[Bart](</User:Bart> "User:Bart") has collected some reported issues under a meta-issue. Fortunately most of them are solved and Lazarus libraries should be robust also without the UTF-8 mode. There is an unsolved FPC issue about RawByteString though. 

  * <http://bugs.freepascal.org/view.php?id=28857>



## Contents

  * 1 Simple Example
    * 1.1 Problem System encoding and Console encoding (Windows)
  * 2 Problem applications with ACP strings and LCL controls
    * 2.1 Example



# Simple Example

  * create a _New Project_ -> _Simple Program_
  * activate the _-dDisableUTF8RTL_ mode with _Project Options ..._ -> _Compiler Options_ -> _Additions and Overrides_ -> click on _Use system encoding_
  * change the file encoding: _Source Editor_ -> mouse rightclick -> _File Settings_ -> _Encoding_ -> take your system encoding (for that example take 1252) and confirm



Now you are able to build a project without UTF8 dependences. 

Write something in your source editor: 
    
    
    program project1;
    
    {$mode objfpc}{$H+}
    {$codepage cp1252}
    
    var
      test: String;
    
    begin
      test := 'ÄÖÜ';
      WriteLn(test);
      ReadLn;
    end.
    

Start your program. 

## Problem System encoding and Console encoding (Windows)

If you start the simple example, you will maybe see a wrong content for that `writeln('ÄÖÜ')`. This is because you can have different codepages for the console and the file system. To check which codepage you have you can make a simple test: 
    
    
      Writeln('Console output codepage: ', GetTextCodePage(Output));
      Writeln('System codepage: ', DefaultSystemCodePage);
    

If your console codepage is different from your system codepage, you can change it in that way (original code from [forum](<http://forum.lazarus.freepascal.org/index.php/topic,30200.msg191843.html#msg191843>)) 
    
    
    unit setdefaultcodepages;
    
    interface
    
    uses
      Windows;
    
    implementation
    
    Const
      LF_FACESIZE = 32;
    
    Type
      CONSOLE_FONT_INFOEX = record
        cbSize     : ULONG;
        nFont      : DWORD;
        dwFontSize : COORD;
        FontFamily : UINT;
        FontWeight : UINT;
        FaceName   : array [0..LF_FACESIZE-1] of WCHAR;
      end;
    
    { Only supported in Vista and onwards!}
    
    function SetCurrentConsoleFontEx(hConsoleOutput: HANDLE; bMaximumWindow: BOOL; var CONSOLE_FONT_INFOEX): BOOL; stdcall; external kernel32;
    
    var
      New_CONSOLE_FONT_INFOEX: CONSOLE_FONT_INFOEX;
    
    initialization
      SetConsoleOutputCP(DefaultSystemCodePage);
      SetTextCodePage(Output, DefaultSystemCodePage);
    
      FillChar(New_CONSOLE_FONT_INFOEX, SizeOf(CONSOLE_FONT_INFOEX), 0);
      New_CONSOLE_FONT_INFOEX.cbSize := SizeOf(CONSOLE_FONT_INFOEX);
      New_CONSOLE_FONT_INFOEX.FaceName := 'Consolas';
    //  New_CONSOLE_FONT_INFOEX.FaceName := 'Lucida Console';  //use Lucida Console for Win XP
      New_CONSOLE_FONT_INFOEX.FontWeight := 400;
      New_CONSOLE_FONT_INFOEX.dwFontSize.Y := 16;
    
      SetCurrentConsoleFontEx(StdOutputHandle, False, New_CONSOLE_FONT_INFOEX);
    end.
    

If you now build a project and use that unit, all strings are correctly shown (Strings also Shortstrings). For example: 
    
    
    program project1;
    
    {$mode objfpc}{$H+}
    {$codepage cp1252}
    
    uses
      setdefaultcodepages;
    
    const
      StrCP1252 = #$80#$C4#$D6#$8C#$A5;
    
    var
      AStr : String;
      AShortString : ShortString;
    begin
      AStr := StrCP1252;
      AShortString := StrCP1252;
      WriteLn(StrCP1252);
      WriteLn('€ÄÖŒ¥');
      WriteLn(AStr);
      WriteLn(AShortString);
      ReadLn;
    end.
    

# Problem applications with ACP strings and LCL controls

If you want to use some LCL controls like TLabel, TEdit, TMemo etc., you have to convert the strings, cause the LCL controls use the UTF8 strings further. You can use the method SysToUTF8 for the conversion of a string to a LCL control string and UTF8ToSys for the conversion back. 

## Example

  * create a new Project _Application_ and change the encoding of your project file to _CP1252_ and activate the _-dDisableUTF8RTL_ mode
  * add a _TEdit_ and a _TButton_ on the form
  * create the event handler for the button click and write this code:


    
    
    procedure TForm1.Button1Click(Sender: TObject);
    var
      s: String;
    begin
      s := 'Läuse und Flöhe';
      Edit1.Text := SysToUTF8(s);
    end;

---

_Source: [https://wiki.freepascal.org/Lazarus_with_FPC3.0_without_UTF-8_mode](https://web.archive.org/web/20250126115436/https://wiki.freepascal.org/Lazarus_with_FPC3.0_without_UTF-8_mode)_
