# ObjCParser

│ **English (en)** │ 

## Contents

  * 1 Overview
  * 2 Installation
  * 3 Usage
    * 3.1 Configuration File Sections
    * 3.2 Common flags
    * 3.3 Type replacement
    * 3.4 Type name definition
    * 3.5 Token Replacement



## Overview

ObjCparser is the tool that helps to convert Objective-C headers to FPC headers as described at [PasCocoa](<PasCocoa.md> "PasCocoa")

Currently, the ObjCParser supports the Objective-C 1.0 language specification. (Objective-C 2.0 properties are not yet supported). 

## Installation

ObjCParser is available from the PasCocoa/Parser folder: 

SVN: <https://sourceforge.net/p/lazarus-ccr/svn/475/tree/bindings/pascocoa/parser/>

## Usage

The command line usage of the Parser is the following: 

objcparser [switches] [headername.h] 

Supported switches: 

-all (any file named *.h is considered a header file and being parsed); 

-ini (setting file) 

-uini (setting file to write to updated files. Type definition information [TypeDefs] is written to the specified file ) 

-noout (.inc files are not generated. However, if specified, the config file is still written) 

### Configuration File Sections

The configuration file uses the simple .INI file format. So, it can be easily edited by any editor available. 

### Common flags

Following parsing settings are defined in the [Common] section 

  * mainunit=mainunitname



defines the main unit specifier to be added to the generated pascal files 
    
    
     [Common]
     mainunit=HelloWorld.pas
    

As a result, each .inc generated file contains 
    
    
    {$mainunit HelloWorld.pas}
    

  * ignoreinclude



specifies, what file #include or #import must be ignored, otherwise file would be also $include-d. The full file name must be specified, or if the file name ends with back slash (CoreFoundation/), all files that belongs to the path would also be ignored (like: CoreFoundation/SomeFile.h, CoreFoundation/SomeFile2.h) 

Files specified as ignoreinclude value should be space separated. 

It is possible to define any number of ignoreinclude values, but they must be prefixed whith 'ignoreinclude'. 

Example: 
    
    
     [Common]
     ignoreinclude = "ioctrl.h"
     ignoreinclude1 = "iostd.h conio.h SomeLib/"
     ignoreinclude2 = "Foundation/ MyLib/hello2.h"
    

in this case: #include or #import definition containing: "ioctrl.h", "iostd.h", "conio.h" or "MyLib/hello2.h" or any file that is a sub-file to "Foundation" or "SomeLib" folders is ignored. 

There is no difference whether <> or "" is used to #include or #import a file. 

### Type replacement

[TypeReplace] Replaces type name (at method params, results, and type structures) with the specified type name. type replacement can also be specified for type references, for this case type the name must end with '*' (asterisk) 

Example: 
    
    
    [TypeReplace]
    NSUInteger = Longword;
    NSUInteger* = PLongWord;
    

This generates the following code: objc: <objc> -(NSUInteger) doSomethingWithNumber: (NSUInteger *) num </objc>

pascal: 
    
    
    function SomeClass.doSomethingWithNumber(num: PLongword): LongWord;
    

### Type name definition

[TypeDefs] Specifies the type name usage. 

There are 4 types available: 

  * int (any type that can be cast to a 4-byte integer)


  * float (floating point value: single or double)


  * struct (any type or structure that cannot be cast to a 4 byte integer or larger than 4 bytes)


  * objcclass (objective-c class)



if no type definition is specified, it is considered to be int-typed. 

this information is critical to select the proper objc_msgSend function (implementation section). objcclass types are also replaced with the 'objc.id' type to avoid type naming conflicts between pascal and objc class names. 

### Token Replacement

Token replacement (currently REMOVE ONLY) should be used to get rid of C pre-compiler defined values. 
    
    
    [TokenReplace]
    AVAILABLE_MAC_OS_X_VERSION_10_5_AND_LATER=""
    

AVAILABLE_MAC_OS_X_VERSION_10_5_AND_LATER token is usually met by the method, function or type declaration. It should be removed or the file cannot be parsed correctly. Remember, that double quotes "" (empty string) must be specified or the parser ignores TokenReplacement

---

_Source: [https://wiki.freepascal.org/ObjCParser](https://web.archive.org/web/20250616134235/https://wiki.freepascal.org/ObjCParser)_
