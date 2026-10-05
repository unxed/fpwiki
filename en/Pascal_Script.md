# Pascal Script

│ **[Deutsch (de)](</Pascal_Script/de> "Pascal Script/de")** │  **English (en)** │  **[español (es)](</Pascal_Script/es> "Pascal Script/es")** │  **[日本語 (ja)](</Pascal_Script/ja> "Pascal Script/ja")** │  **[русский (ru)](<../ru/Pascal_Script.md> "Pascal Script/ru")** │    
****

**Pascal Script** is an [Object Pascal](<Object_Pascal.md> "Object Pascal")/[Delphi](<Delphi.md> "Delphi")/[Lazarus](<Lazarus.md> "Lazarus")-compatible interpreter with bytecode compiler that delivers a [scripting](<PascalScript.md> "PascalScript") environment for application programs. 

It currently works in [macOS](<Portal_Mac.md> "Portal:Mac"), [Windows](<Portal_Windows.md> "Portal:Windows") and [Linux on 32-bit and 64-bit](<Portal_Linux.md> "Portal:Linux") [x86](</Category:x86> "Category:x86"), [PowerPC](<PowerPC.md> "PowerPC") and [ARM](<ARM.md> "ARM") processors. 

It was created and is maintained by Carlo Kok and is copyrighted by [RemObjects software](<http://www.remobjects.com>) as freeware with full source available. 

The fixing of a few incompatibilities between ROPS (RemObjects Pascal Script) and Free Pascal 2.0.1 was made by Bogusław Brandys with a great help of many developers from #fpc and #lazarus-ide IRC channels. Thank You. 

Its main characteristics are : 

  * almost all Object Pascal syntax supported
  * Delphi/Lazarus classes supported (however cannot be declared inside of script)
  * can create fully workable GUI forms with components
  * easily import new classes into script engine



The download contains the components package for Delphi (various versions) and Lazarus + a few samples for Delphi (which may or may not work under Free Pascal+ Lazarus). It is work in progress... 

This component is now designed for cross-platform applications, however limited to 32-bit Intel platform only. I'd like to make it work on PowerPC and 64-bit architectures someday. (Note: The current version seems to support 64-bit machines, according to RemObjects.) 

## Contents

  * 1 Screenshots
  * 2 License
  * 3 Download
  * 4 Change Log
    * 4.1 Dependencies / System Requirements
    * 4.2 Installation
    * 4.3 Compilation errors
  * 5 Usage
  * 6 Example application
  * 7 History
  * 8 See also



## Screenshots

Here are some screenshots how it looks under Lazarus: 

  * [![under Linux](https://wiki.freepascal.org/images/0/0f/Rops_linux.png)](</File:Rops_linux.png> "under Linux")

under Linux 

  * [![under Windows](https://wiki.freepascal.org/images/6/65/Rops_windows.png)](</File:Rops_windows.png> "under Windows")

under Windows 

  * [![under Windows](https://wiki.freepascal.org/images/e/e1/maXbox_mini_LAZARUS.png)](</File:maXbox_mini_LAZARUS.png> "under Windows")

under Windows 

  * [![macOS Mojave](https://wiki.freepascal.org/images/6/6d/Pascal_Script_for_Lazarus_2_on_Mac.png)](</File:Pascal_Script_for_Lazarus_2_on_Mac.png> "macOS Mojave")

macOS Mojave 




## License

BSD like, see [ full text](<Pascal_Script/License.md> "Pascal Script/License"). 

## Download

  * From RemObjects (FPC + Lazarus is supported)



    This is the main page of RemObjects [Pascal Script distribution](<http://www.remobjects.com/ps.aspx>). There are download links for binary packages.

  * New repository: <https://github.com/remobjects/pascalscript> <git://github.com/remobjects/pascalscript.git>
  * **In newer Lazarus versions Pascal Script is included as a standard component.**



## Change Log

  * Version 1.0 2005/10/21
  * ("Official" support of FPC, as seen on 2006/07/21)



### Dependencies / System Requirements

  * None
  * Status: Beta (ToDo: update info)
  * Issues: (ToDo: update info)
  * Needs testing on Windows.
  * Needs testing on Linux.
  * Almost working ;-)



### Installation

  * Create the directory lazarus\components\pascalscript
  * Unzip files into the directory
  * Open lazarus
  * Open the package pascalscript.lpk with Component/Open package file (.lpk)
  * Click on Compile
  * Click on Install



### Compilation errors

When compiling to install the package, the compiler will stumble on two lines in uPSR_forms.pas: 
    
    
    RegisterMethod(@TAPPLICATION.HELPCOMMAND, 'HELPCOMMAND'); // <-- this one
    RegisterMethod(@TAPPLICATION.HELPCONTEXT, 'HELPCONTEXT');
    RegisterMethod(@TAPPLICATION.HELPJUMP, 'HELPJUMP');       // <-- and that one
    

Simply comment out the lines. These methods are not yet implemented in the LCL. 

## Usage

Drop the PascalScript component on a form and a few plugins. (TODO:finish) 

If you get the error "Fatal: Can't find unit uPSCompiler used by ...", open up the pascalscript package, and under the "more" options, select "add to project". 

See the example projects. 

See also these [articles](<https://github.com/remobjects/pascalscript/wiki>) from RemObjects. 

## Example application

Sample small console mode interpreter application: [Pascal Script Examples (psce)](<Pascal_Script_Examples.md> "Pascal Script Examples")

Sample components Demo with Lazarus GUI: [[[1]](<http://sourceforge.net/projects/maxbox/files/Lazarus/PASCALSCRIPT_LAZARUS.zip/download>)] 

## History

Pascal Script started out in 2001 with CajScript 1.0, which was soon superseded by CajScript 2.0 (later called Innerfuse Pascal Script 2.0). Version 2.0 interpreted scripts while it ran them, which had the disadvantage that every piece of code had to be reparsed every time the script engine went over it. With Pascal Script 3.0, this was changed to a new model, where the compiler and runtime were completely separated from each other and used a custom byte code format to represent the compiled script. This compiled script only contained the bare minimum that was required to execute the code. Later, when Carlo Kok joined RemObjects, it was renamed RemObjects Pascal Script and is now being maintained by RemObjects Software. 

One prominent use of Pascal Script is the Open Source InnoSetup project. InnoSetup is a widely used setup engine that uses Pascal Script as scripting engine to provide advanced scripting abilities during installation and uninstallation. Using Pascal Script, users can customize almost all parts of the setup, add new wizard pages, call into dlls to add advanced features and provide custom behavior and install conditions. 

## See also

  * [RemObjects Pascal Script page](<https://www.remobjects.com/ps.aspx>)
  * [Pascal Script tab](<Pascal_Script_tab.md> "Pascal Script tab")

---

_Source: [https://wiki.freepascal.org/Pascal_Script](https://web.archive.org/web/20250301000000/https://wiki.freepascal.org/Pascal_Script)_
