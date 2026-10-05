# FPC message: Can not find unit

│ **English (en)** │

## Contents

  * 1 General hints
  * 2 Can't find unit interfaces
  * 3 Can't find unit LResources
  * 4 Can't find unit System
  * 5 Detailed description about this topic
  * 6 See also



## General hints

Example: 
    
    
     unit1.pas(40,42) Fatal: Can't find unit forms
    

The compiler didn't find a compatible forms.ppu file nor a source file (forms.pp or forms.pas or forms.p). 

The compiler searches in the unit path. The unit path is the sum of: 

  * the fpc.cfg file (Linux, macOS, BSD: /etc/fpc.cfg and ~/.fpc.cfg)
  * the Lazarus compiler options. See [Compiler Options](<IDE_Window__Compiler_Options.md> "IDE Window: Compiler Options")
    * -> Paths -> Other unit files
    * -> Inherited -> All inherited options -> unit path (if available)



![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** Even if there is a forms.ppu file in the unit path, the above message can appear, because FPC checks if the ppu file is valid (up to date). The .ppu file is valid, if all files it depends on are valid. If you install another FPC all .ppu files on your system become invalid and must be recompiled. It may be useful to run Lazarus in a terminal (cmd window) to see the actual error FPC shows.

If using FPC, you can show where it searches for units by adding the -vut options, e.g. 
    
    
    fpc -vut myprogram.pas
    

## Can't find unit interfaces

The 'interfaces' unit is the part of the LCL. You must use it at least once in your program to link in the LCL interface (the platform dependent part of the LCL. For instance gtk or win32). If you see this error: 

  * Make sure the project uses the LCL package. See Project -> Project Inspector -> Required Packages. If there is no LCL click on the 'Add' button and then 'New Requirement', select the LCL and press Ok;
  * Make sure the unit is part of the project. Use Project -> Add editor file to project;
  * Make sure the 'interfaces.ppu' is in the unit path, and the 'interfaces.pp' is **not** in the unit path. And that you only have one 'interfaces.ppu' in the unit path. You can see the unit path in Source Editor -> Right mouse click -> File Settings ... -> Unit Info -> Unit Paths;
  * Make sure your compiled LCL fits to the installed compiler. For example if you installed a new compiler, then the LCL must be rebuilt (for example the interfaces.ppu must be rebuilt (You must be loged in as admin to rebuild) );
  * Make sure the project compiles the correct (current) widgetset. See Project -> Compiler Options -> Path -> LCL Widget Type (various). Change it to "default (*****)" to use default widgetset. If you don't want compiles with default widgetset, then change this value to any value, and recompile LCL to respective widgetset (win32/win64, gtk, gtk2, etc).
  * Make sure the project compiles the correct "Target CPU". See "Project -> Compiler Options -> Code -> Target CPU (-P)" and change them to (Default) (or in blank).



## Can't find unit LResources

Can be solved by doing "Tools/Build Lazarus" 

## Can't find unit System

This often [happens](<https://gitlab.com/freepascal.org/fpc/source/-/issues/39586>) with console IDE on Ubuntu systems. To fix it, follow [this solution](<https://stackoverflow.com/a/64637024/1020550>). 

Also maybe you're trying compile your project to other "Target OS". See "Project -> Project Options -> Compiler Options -> Code generation -> Target OS (-T)" and change them to (Default) (or in blank). 

## Detailed description about this topic

There is a detailed page about searching units here: [Unit not found - How to find units](<Unit_not_found_-_How_to_find_units.md> "Unit not found - How to find units")

## See also

  * [Lazarus Can't find unit system used by fcllaz](<Lazarus_Can't_find_unit_system_used_by_fcllaz.md> "Lazarus Can't find unit system used by fcllaz")
  * [Lazarus FAQ](<Lazarus_FAQ.md> "Lazarus FAQ")
  * [Mac Installation FAQ](<Mac_Installation_FAQ.md> "Mac Installation FAQ") \- issues during (and after) installation

---

_Source: [https://wiki.freepascal.org/FPC_message%3A_Can_not_find_unit](https://web.archive.org/web/20240410064750/https://wiki.freepascal.org/FPC_message%3A_Can_not_find_unit)_
