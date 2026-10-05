# iPhone headers translator

[![Apple iOS new.svg](https://upload.wikimedia.org/wikipedia/commons/thumb/4/48/Apple_iOS_new.svg/50px-Apple_iOS_new.svg.png)](</File:Apple_iOS_new.svg>)

This article applies to [iOS](</Category:iOS> "Category:iOS") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

## Contents

  * 1 Pascal iPhone header translator
    * 1.1 Author
    * 1.2 Introduction
    * 1.3 File structure
    * 1.4 Features (main goals of project)
    * 1.5 Version review
    * 1.6 How to parse
    * 1.7 Testing
    * 1.8 Download
  * 2 See Also



## Pascal iPhone header translator

### Author

Anton e-mail: ast DOT a_s AT mail DOT ru 

### Introduction

Because no one can redistribute Pascal units for iOS SDK we need a tool that can produce such units from official C headers. Of course everyone can take original headers and manually translate them but it is not the simplest work. Indeed this process can be (at least partially) automated. I'd like to present iPhone headers translator that is written in FPC. 

### File structure

There are several files zip available fro download page: 

  * parser.pas - main program;
  * htranslator.pas - contains main class that perform translation (indeed it only manage this process);
  * hscanner.pas - contains "c-header scanner" class;
  * hparser.pas - contains common c-header parser, that translates c declarations (without helper functions with implementations (bodies));
  * bparser.pas - extends header parser to handle functions with bodies too;
  * ocparser.pas - extends parser to handle objective-c declarations;
  * decltypes.pas - contains class to manage translated types;
  * declids.pas (since 20120410) - contains class to manage globally declared symbols (constants, variables and functions).



### Features (main goals of project)

  * do not require any additional (input) files, only iPhone SDK C-headers;
  * produced Pascal units must not need any manual fixes to be compiled;
  * translate also implementations of helper functions



### Version review

  * Initial version from 2012-03-19: can translate UIKit and related frameworks.



    Translation process was tested for the following SDKs:

    

    **iPhoneSimulator** 3.0, 3.2, 4.0, 4.1, 4.2, 4.3, 5.0;

    

    **iPhoneOS** 5.0.

    Compilation of produced units was tested only for iPhoneSimulator 5.0.
    Test project can be found on download page (ipt.zip contains Lazarus project, notes about its compilation can be found in next section).

  * ip_pars20120410: can translate almost all frameworks from SDK 5.0.



    Some frameworks (applicationservices, coreservices, accelerate, carbon) for now are translated to empty units because translation of "nested frameworks" is not implemented yet. Parsing process was tested for iPhoneSimulator 5.0 and iPhoneOS 5.0, compilation was tested only for simulator.

### How to parse

To build parser "fpc parser" is enough. Of course you need to install iPhone SDK. Cause I have only simulator, I will use its SDK. Typically it is installed to /Developer/Platforms/iPhoneSimulator/Developer/SDKs/iPhoneSimulator5.0.sdk (for 5.0 version). The path to SDK will be used as a parameter of parser. Indeed, it's time to start it. Parser contains a little help, so it can be used (--help or without parameters). 

Parser parameters: 

  1. one of parameter must be sdk="path_to_sdk" (mandatory);
  2. there can be parameter dest="output_path" (to place Pascal units, if not specified "src" is used);
  3. all other parameters are regarded as names of frameworks to parse.



Example: 
    
    
    $ ./parser sdk=/Developer/Platforms/iPhoneSimulator/Developer/SDKs/iPhoneSimulator5.0.sdk UIKit
    

will translate UIKit and all dependant frameworks (cfnetwork, corefoundation, coregraphics, coreimage, corevideo, foundation, opengles, security). I've tested it for SDKs 3.0, 3.2, 4.0, 4.1, 4.2, 4.3, 5.0. Although now it works good there are some "todo's" and unimplemented parts. 

_ip_pars20120410.zip_ contains two scripts that can be used to start translation process: 

  1. **start_ios.sh** for iPhoneOS 5.0 SDK;
  2. **start_sim.sh** for iPhoneSimulator 5.0 SDK.



Notes: 

  * Before starting these scripts you will need execute _chmod +x_ for them.
  * These scripts cannot be used for SDKs of different versions because trying to parse missing framework leads to fatal error.



### Testing

My test program is also attached (_ipt.zip_ contains Lazarus project). Project output directory must be changed. In order to run it in Simulator I have created empty XCode project "iphone_test" and run it. After that I've closed XCode and run only simulator - it has contained my new program "iphone_test". After that I've simply replaced binary /Users/macuser/Library/Application Support/iPhone Simulator/5.0/Applications/C93F7A57-E612-4A40-B155-DB106CA860D6/iphone_test.app/iphone_test 

### Download

From here: <http://sourceforge.net/projects/p-tools/files/iPhone%20headers%20translator/>

<https://github.com/odisey1245/ip_parse>

## See Also

  * [iPhone/iPod development](<iPhone/iPod_development.md> "iPhone/iPod development")

---

_Source: [https://wiki.freepascal.org/iPhone_headers_translator](https://web.archive.org/web/20240914044659/https://wiki.freepascal.org/iPhone_headers_translator)_
