# Chelper

│ **English (en)** │  **[français (fr)](</Chelper/fr> "Chelper/fr")** │    
****

## Contents

  * 1 About
  * 2 Author
  * 3 License
  * 4 Download
  * 5 Change Log
  * 6 Dependencies / System Requirements
  * 7 Issues
  * 8 Installation
  * 9 How to use
    * 9.1 Shortkeys
    * 9.2 General
    * 9.3 Defines
    * 9.4 Configuration File



### About

Chelper is Lazarus extension tool, that helps converting C headers to Pascal. Yes, it's yet another C-to-Pascal converting tool like lot of similar ([H2Pas](<H2Pas.md> "H2Pas"), ToPas), but one might find it more useful. 

The extension doesn't try to convert a header for the user, instead it's a tool to help with manual translation, doing almost all of "dirty work" for the user. 

Chelper also supports converting Objective-C declarations into Objective-P. 

### Author

Dmitry 'skalogryz' Boyarintsev 

### License

[modified](<http://svn.freepascal.org/svn/lazarus/trunk/COPYING.modifiedLGPL>) [LGPL](<http://svn.freepascal.org/svn/lazarus/trunk/COPYING.LGPL>) (same as the FPC RTL and the Lazarus LCL). You can contact the author if the modified LGPL doesn't work with your project licensing. 

### Download

There's no "downloadable" package yet, you can access the tool from SVN: 

<https://sourceforge.net/p/lazarus-ccr/svn/HEAD/tree/components/chelper/>
    
    
    svn checkout <https://svn.code.sf.net/p/lazarus-ccr/svn/components/chelper/>
    

### Change Log

  * Version 0.8, 8 Aug 2010



### Dependencies / System Requirements

  * Lazarus 0.9.29 or Higher



Status: _Beta_

### Issues

  * No complete expression parser to construct the expression tree.



If you find a bug in the converter please report it to [bugs.freepascal.org](<http://bugs.freepascal.org>) for Lazarus-CCR project. Please attach the C-header text and the explanation what doesn't work as expected. 

### Installation

  * Get the package from SVN
  * Install Chelper.lpk package to the Lazarus.
  * Go to _Tools- >C to Pascal Options->Converter_, uncheck _Use external converter_ if you want to use the package built-in converter.



Using built-in converter has an advantage of the performance. However, if you'll need to update converter, you'll need to rebuild the whole IDE. It's also possible that converter might cause crashes or hangs. You should bug report it, but you're risking to loose all of your data. 

It's much safer to use the external cconvert tool. It's also easier to update the converter if a new features are added or bugs fixed. All you need is to rebuild the cconverter executable a lot of faster than rebuilding Lazarus. 

To use the external tool, you need to do the following: 

  * Open and build cconvert.lpi project (located at the package directory). You should get "cconvert" executable file.
  * Open Tools->C to Pascal Options->Converter, press Select and choose cconvert executable file.
  * Check _Use external converter_ at _Tools- >C to Pascal Options->Converter_



### How to use

#### Shortkeys

  * ctrl+b - convert a single C declaration to Pascal
  * ctrl+shift+b - convert all C declarations to Pascal from the current cursor position to the end of the file.



#### General

  * Open a file with C declarations in Lazarus IDE or start a new file copying declarations there
  * Place the cursor at the line where C declaration and press Ctrl+B (the shortcut would be modifiable in future).



for example: 
    
    
    int c;
    int b; 
    

if the cursor placed at 'inc c;' line and ctrl+b pressed the result would be: 
    
    
    var
      c : Integer;
    int b;
    

exactly one definition is converted. The cursor would be place right after the converted definition. You can press ctrl+b again, and the result would be: 
    
    
    var
      c : Integer;
    var 
      b : Integer;
    

Parsing definitions one-by-one will get your C headers ready with a very short period of time, as well as keeping it clean and correct from Pascal point of view. 

  * If you're sure your defines (explained below) are configured well enough. You can try to convert the rest of the file in one call by pressing ctrl+shift+b



i.e. you have 2 declarations in the file: 
    
    
    int fn(int, float);
    extern int v;
    

placing the cursor before them and pressing ctrl+shift+b, would generate. 
    
    
    function fn(par0: Integer; par1: Single): Integer; cdecl; external;
    
    var
      v : Integer; external;
    

**You must be careful, because it's common to use macros in C headers. Leaving any of macros undefined might produce wrong pascal declaration**. The next section explains how to define a macro for the chelper. 

#### Defines

In most cases parameterless macro definitions, like 
    
    
    #define VALUE 1 << 2;
    

are used like constant values. Chelper translates them as constants trying to convert the define expression. 
    
    
    const
      VALUE = 1 shr 2;
    

In other cases defines are used for some "special" markings or additional information for external tools. 
    
    
    #define LIB_DEPRECATED 
    LIB_DEPRECATED int somefunc(int p);
    

without the knowledge of LIB_DEPRECATED being the macro (of nothing), it's impossible to parse function declaration correctly. You can specify macros used by Chelper in the special defines file. 

Select Tools->C to Pascal options->Main select the defines file and press Edit. 

This would open the custom defines file in Lazarus IDE. All macros declared at this file are used by Chelper. After adding/editing macro defines save the file and you can return to the C header converting. 

  


Chelper handles parameter macros as well. 
    
    
    int somefunc SAFE_PARAMS( (int i))
    

adding the the following declaration into defines file 
    
    
    #define SAFE_PARAMS(x) x
    

will allow to parse the declaration correctly 
    
    
    function somefunc(i: Integer): Integer;
    

#### Configuration File

Configuration file represents itself a simple .ini file. Following section are used in the file: 

  * Main - for general declarations, that effects


  * Types - custom type conversion section. Name=Value, where Name is the c-type name, Value is resulting pascal type



example: 
    
    
    [Types]
    int=integer
    int*=PInteger
    float=single
    float*=PSingle

---

_Source: [https://wiki.freepascal.org/Chelper](https://web.archive.org/web/20240807132528/https://wiki.freepascal.org/Chelper)_
