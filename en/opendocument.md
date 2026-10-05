# opendocument

## Contents

  * 1 Definition
  * 2 Description
  * 3 Example
  * 4 Win NT "File names with spaces" workaround



## Definition

Unit: Lazarus **lclintf**
    
    
    function OpenDocument(APath: String): Boolean;

Official documentation: [[1]](<http://lazarus-ccr.sourceforge.net/docs/lcl/lclintf/opendocument.html>)

## Description

**opendocument** opens a document/file with the default viewer/editor registered with the operating system for that file/file extension. E.g. on Windows, the code will use the registry to look up the file association for the extension. 

[![Warning-icon.png](https://wiki.freepascal.org/images/b/b2/Warning-icon.png)](</File:Warning-icon.png>)

**Warning:** On Windows at least in stable version (currently 1.0.6), trying to open documents with spaces in it will not work. This has been fixed in trunk (r36934) and it will probably be merged to 1.0.8. (See [Issue #21659](<http://bugs.freepascal.org/view.php?id=21659>) in the bugtracker)

## Example
    
    
    uses 
    ...
    lclintf
    ...
    OpenDocument('readme.pdf');

## Win NT "File names with spaces" workaround

The following is a simple function you can experiment with to do a similar job for file names with spaces. It is derived directly from OpenURL in <your lazarus install>\lcl\include\sysenvapis_win.inc, and has variations for wince and win9x removed - if you need those, see the original file. **Experiment with** is emphasised - it has only been tested for my particular use-case on windows 7. It does not seem to need the filename quoted. 
    
    
    uses windows;
    
    function OpenFileDoc(Doc: String): Boolean;
    var
      ws: WideString;
    begin
      Result := False;
      if Doc = '' then Exit;
      ws := UTF8Decode(doc);
      Result := ShellExecuteW(0, 'open', PWideChar(ws), nil, nil, SW_SHOWNORMAL) > 32;
    end;

---

_Source: [https://wiki.freepascal.org/opendocument](https://web.archive.org/web/20200806130304/https://wiki.freepascal.org/opendocument)_
