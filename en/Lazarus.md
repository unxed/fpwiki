# Lazarus FAQ

│ **English (en)** │    
****

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** This FAQ may be **outdated** in certain parts.

## Contents

  * 1 General
    * 1.1 What is Lazarus?
    * 1.2 Why are the generated binaries so big?
    * 1.3 Do I need ppc386.cfg or fpc.cfg?
    * 1.4 How do I compile Lazarus?
    * 1.5 Is it possible to build Lazarus projects without the Lazarus IDE?
    * 1.6 How do I build other Free Pascal programs that use LCL without the Lazarus IDE?
    * 1.7 What version of FPC is required?
    * 1.8 I can't compile Lazarus
    * 1.9 How to embed a small file in the executable, without the need of a separate file? How to embed a resource?
    * 1.10 What is the meaning of the various file extensions used by Lazarus?
    * 1.11 Why are TForm.ClientWidth/ClientHeight the same as TForm.Width/Height
  * 2 Errors
    * 2.1 'Fatal: Circular unit reference between a and b'
    * 2.2 When I do _var mytext: text;_ to declare a text file, I get "Unit1.pas(32,15) Error: Error in type definition". How can I fix this?
    * 2.3 I get an error when using Printer.BeginDoc
    * 2.4 When I try to compile a project, I get an error message "Cannot find Unit ..."
    * 2.5 When I try to compile a project, I get an error message "Cannot find Unit interfaces"
    * 2.6 When I compile a project, that uses a LCL unit, I get a linker error
    * 2.7 At the line :{$R *.DFM} How can I solve this problem (Lazarus with FPC < 2.4.0)?
    * 2.8 'Identifier not found LazarusResources'
    * 2.9 When accessing events of objects, _e.g._ , the onclick event of a button, I get the following error. ERROR unit not found: stdCtrls
    * 2.10 'Fatal: Internal error XXXXYYZZW'
    * 2.11 Fatal: Cannot find [...] used by [...], incompatible ppu=[filename], package [package name]
    * 2.12 Lazarus cannot open a project if last character of the path to it is a space
    * 2.13 strToFloat - EConvertError: "0.nnn" is an invalid float
  * 3 Debugger
    * 3.1 How do I set up/configure for debugging
    * 3.2 How can I inspect properties?
    * 3.3 Why does the debugger not show some Variables/Structures ("no such symbol"/"incomplete type")
    * 3.4 How can I debug FCL components from packages with Lazarus
    * 3.5 Problems with GDB on Mac
    * 3.6 How can I use a log file for debugging?
    * 3.7 How can I use a Log-file for debugging the IDE itself?
  * 4 Contributing / Making Changes to Lazarus
    * 4.1 I created a Patch to dock the IDE Messages form on the "Source Code Editor" form (at bottom)
    * 4.2 I have fixed/improved Lazarus. How can I add my changes to the official Lazarus source?
    * 4.3 How can I install more than one copy of Lazarus?
    * 4.4 How can I become a Lazarus developer and access management in the SVN and bug-tracker?
  * 5 Where is ... defined
    * 5.1 Virtual key constants
  * 6 Using the IDE
    * 6.1 How can I use "identifier completion"?
    * 6.2 Can I change the code editor font and colours?
    * 6.3 How can I disable specific compiler warning messages?
    * 6.4 What are the IDE command line options?
  * 7 Linux
    * 7.1 Typing in edit fields generate duplicate letters
    * 7.2 How can I debug on Linux without the IDE?
    * 7.3 I can debug now but ddd does not find my sources or complains that they contain no code. What's that?
    * 7.4 I receive an error during the linking that states /usr/bin/ld: cannot find -l<some lib>
    * 7.5 I receive a warning during the linking that states: Warning: "crtbeginS.o" (or "crtendS.o") not found
    * 7.6 How can I convert a Kylix 2 project into a Lazarus project?
    * 7.7 When compiling lazarus the compiler can not find a unit. e.g.: gtkint.pp(17,16) Fatal: Can't find unit GLIB
    * 7.8 I have installed the binary version, but when compiling a simple project, Lazarus gives: Fatal: Can't find unit CONTROLS
    * 7.9 Lazarus compiles, but linking fails with: libgdk-pixbuf not found
    * 7.10 I have SuSE and I get /usr/bin/ld: cannot find -lgtk Error: Error while linking
    * 7.11 I have Kubuntu and I get /usr/bin/ld: cannot find -lgtk-x11-2.0
    * 7.12 Lazarus crashes with runtime error 211 after I installed a component
    * 7.13 When I run a program with threads I get runtime error 232
    * 7.14 I have Ubuntu Breezy/Mandriva KDE3 and my fonts in Lazarus IDE look too big
    * 7.15 How can my gtk programs use custom rc files?
    * 7.16 I have Ubuntu and I cannot compile for Gtk2 due to missing libraries
    * 7.17 Lazarus runs differently with Linux vs. Windows. What do I need to know?
  * 8 Windows
    * 8.1 When I cycle/rebuild the compiler, I get
      * 8.1.1 The name specified is not recognized as an internal or external command, operable program or batch file.>& was unexpected at this time
      * 8.1.2 make[3]: ./ppc1.exe: Command not found
    * 8.2 When I try to make Lazarus I get:
      * 8.2.1 make.exe: * * * interfaces: No such file or directory (ENOENT). Stop.make.exe: * * * [interfaces_all] Error 2
      * 8.2.2 makefile:27: *** You need the GNU utils package to use this Makefile. Stop.
    * 8.3 How can I give my program an XP look like Lazarus has?
    * 8.4 When I run Windows program created in Lazarus it starts with a DOS window
  * 9 macOS
    * 9.1 Why does compiling a project fail with 'unknown section attribute: no_dead_strip'?
    * 9.2 Xcode 3.1 and later
    * 9.3 Debugging
  * 10 Licensing
    * 10.1 Can I make commercial applications with Lazarus?
    * 10.2 Why are some components restricted from usage in commercial application?
    * 10.3 How do I know if a Component is part of the LCL?
    * 10.4 Can I make commercial plug-ins for Lazarus?
  * 11 Using the Forum
    * 11.1 What is the correct way to ask questions in the forum?
  * 12 Version numbering
  * 13 Problems/issues with old Lazarus/FPC versions
    * 13.1 Why is the linking so slow on Windows (Lazarus < 0.9.24)?
    * 13.2 Old Lazarus does not support Windows-style resources; converting lrs resources
    * 13.3 Antivirus
  * 14 See also



## General

### What is Lazarus?

Lazarus is a cross-platform integrated development environment (IDE) that lets you create visual (GUI) and non-visual Object [Pascal](<Pascal.md> "Pascal") programs, and uses the Free Pascal compiler to generate your executable. Its aim is _write once, compile anywhere_ : you should be able to just recompile your program source code with Lazarus running on another operating system (or a cross compiler) and get a program that runs on that operating system. 

For more details see [Overview of Free Pascal and Lazarus](<Overview_of_Free_Pascal_and_Lazarus.md> "Overview of Free Pascal and Lazarus")

### Why are the generated binaries so big?

The binaries are big because they include a lot of debug information necessary for using gdb (GNU Debugger). A debugger is a program that is used to test your code. It uses the extra-information stored in your binary to test it. But once your program is working well, you may delete those infos and reduce the size of the binary. 

**Quick guide to Lazarus/FPC application size reduction**

  * 1\. Project|Compiler Options|Code|Smart Linkable (-CX) -> Checked
  * 2\. Project|Compiler Options|Linking|Debugging| Uncheck all except Strip Symbols From Executable (-Xs)
  * 3\. Project|Compiler Options|Linking|Link Style|Link Smart (-XX) -> Checked



![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** only do this if you don't need to run the debugger. For more details, see [Size Matters](<Size_Matters.md> "Size Matters")

Lazarus executable size starts big, but grows very slowly, because of the way the LCL is designed, and its use of certain Free Pascal features (RTTI). Projects that don't use the LCL are much smaller (this is similar to some non-GUI C++ frameworks). This typically requires more manual coding though. 

**Exclude image readers** See [Lazarus 1.10.0 release notes#LCL Changes](<Lazarus_1.10.md> "Lazarus 1.10.0 release notes") If your app does not use images compressed in one of those formats, then you can exclude it. 

### Do I need ppc386.cfg or fpc.cfg?

You only need fpc.cfg. This way the compiler knows where to find the libraries. 

### How do I compile Lazarus?

Do something like this: 
    
    
    cd lazarus
    make clean all
    

If you want to build Lazarus for different widgetset eg. Qt (supported on Linux, Windows and OSX) then use the LCL_PLATFORM argument: 
    
    
    cd lazarus
    make clean all LCL_PLATFORM=qt
    

### Is it possible to build Lazarus projects without the Lazarus IDE?

See [Using_the_LCL_without_Lazarus](<Using_the_LCL_without_Lazarus.md> "Using the LCL without Lazarus")

### How do I build other Free Pascal programs that use LCL without the Lazarus IDE?

See [Using_the_LCL_without_Lazarus](<Using_the_LCL_without_Lazarus.md> "Using the LCL without Lazarus")

### What version of FPC is required?

Any Lazarus version compiles and works with 2 latest FPC release versions. FPC release cycle is slow and 2 latest versions typically span over few years. 

Lazarus trunk also supports FPC trunk. 

  * Lazarus 1.0.8 requires at least FPC 2.6.2.
  * Lazarus 1.0 requires at least FPC 2.6.0.
  * Lazarus 2.x requires at least FPC 3.0.0
  * Lazarus 3.0 requires at least FPC 3.2.0



### I can't compile Lazarus

  1. Check if the compiler is the correct version
  2. Check if the (fpc) libraries are from the same version
  3. Check if the compiler installation path has spaces in it. Make sure it doesn't!
  4. Check if you have a fpc.cfg and no old ppc386.cfg
  5. Check also the OS-dependent FAQs
  6. If you're still stuck, ask on the forum or Lazarus mailing list



### How to embed a small file in the executable, without the need of a separate file? How to embed a resource?

Both Lazarus resource and Windows-type/FPC resources are supported. See [Lazarus Resources](<Lazarus_Resources.md> "Lazarus Resources"). 

### What is the meaning of the various file extensions used by Lazarus?

The [Lazarus Tutorial#The Lazarus files](<Lazarus_Tutorial.md> "Lazarus Tutorial") explains some extensions by an example. Here is a brief list: 

extension | filetype | description   
---|---|---  
`.lpi` | Lazarus Project Information | contains project-specific settings like compiler settings and needed packages. stored in XML   
`.lps` | Lazarus Program Session | Personal data like cursor positions, source editor files, personal build modes. stored in XML   
`.lpr` | Lazarus Program | Pascal source of main program.   
`.lfm` | Lazarus Form | Form configuration information for all objects on a form (stored in a Lazarus-specific textual format, similar to Delphi dfm; the actions are described by Pascal source code in a corresponding `*.pas` file)   
`.pas`   
`.pp`   
`.p` | Pascal code | Pascal code typically for a form stored in a corresponding `*.lfm` file   
`.lrs` | Lazarus Resource | Generated Lazarus Resource file; not to be confused with a Windows resource file. This file can be created with lazres tool (in directory Lazarus/Tools) using commandline: lazres myfile.lrs myfile.lfm   
`.ppu` | Compiled unit | Compiled source code created by the Free Pascal compiler for each unit and program.   
`.o` | Object file | created by the compiler, every ppu file has a corresponding o file, needed by the linker.   
`.lpk` | Lazarus package information | package-specific settings, like compiler settings and needed packages; stored in XML   
`.lrt` | Lazarus Resourcestring table | Lazarus Resourcestring table created when saving a lfm file and i18n is enabled. It contains the TTranslateString properties of the lfm. Do not edit them, they are overwritten.   
`.rst` | Resourcestring table | Resourcestring table created by the compiler for every unit with a resourcestring section. Do not edit them, they are overwritten.   
`.po` | gnu gettext messages | When [i18n](</index.php?title=i18n&action=edit&redlink=1> "i18n \(page does not exist\)") is enabled the IDE creates/updated the .po files with the resourcestrings from the rst and lrt files.   
  
### Why are TForm.ClientWidth/ClientHeight the same as TForm.Width/Height

The TForm.Width/Height do no include the frame, because there was no way to retrieve the frame size on all platforms. Without a reliable way, the LCL would move the forms around on the screen or resize them endlessly. 

Eventually it will be changed when there is a reliable way to get the size and position of a window with its frame on all platforms. To keep compatibility with older LCL forms, a version number and some extra methods will be added. 

## Errors

Platform specific errors/problems are also covered in sections below. 

### 'Fatal: Circular unit reference between a and b'

A common thing happened to a new user when one wants to create two forms referencing each other's properties. The error above only happens to uses clause of interface section, thus it's OK to put it in implementation section. Example: 

This one causes error: 
    
    
    unit a;
    
    interface
    
    uses b;
    
    implementation
    
    end.
    
    
    
    unit b;
    
    interface
    
    uses a;
    
    implementation
    
    end.
    

But this one doesn't: 
    
    
    unit a;
    
    interface
    
    implementation
    
    uses b;
    
    end.
    
    
    
    unit b;
    
    interface
    
    implementation
    
    uses a;
    
    end.
    

  
In some cases the **minimal base class** method might be suitable. 

Separate one class into a minimal base class (what the other needs) and derive it for the rest of the implementation. 

  
**unita** : uses unitb 
    
    
      type
        ClassA = class(BaseClassA)
        // rest of ClassA
        end;
    

**unitb** : 
    
    
      type
        ClassB = class;  //forward declaration
    
        BaseClassA = class
        // references ClassB
        end;
           
        ClassB = class
        // references BaseClassA
        end;
    

Discussion: <https://forum.lazarus.freepascal.org/index.php/topic,25667.msg156115.html#msg156115>

### When I do _var mytext: text;_ to declare a text file, I get "Unit1.pas(32,15) Error: Error in type definition". How can I fix this?

The TControl class has a [Text](<http://lazarus-ccr.sourceforge.net/docs/lcl/controls/tcontrol.text.html> "doc:lcl/controls/tcontrol.text.html") property. In a method of a form, that has higher visibility, the [Text](<http://lazarus-ccr.sourceforge.net/docs/rtl/system/text.html> "doc:rtl/system/text.html") type from the system unit. You can use the [TextFile](<http://lazarus-ccr.sourceforge.net/docs/rtl/system/textfile.html> "doc:rtl/system/textfile.html") type, which is just an alias for the Text type or you can add the unit to the type definition. 
    
    
    var
      MyTextFile: TextFile;
      MyText: System.Text;
    

A similar name clash exists with assigning and closing a text file. TForm has a _assign_ and a [Close](<http://lazarus-ccr.sourceforge.net/docs/lcl/forms/tcustomform.close.html> "doc:lcl/forms/tcustomform.close.html") method. You can use [AssignFile](<http://lazarus-ccr.sourceforge.net/docs/rtl/objpas/assignfile.html> "doc:rtl/objpas/assignfile.html") and [CloseFile](<http://lazarus-ccr.sourceforge.net/docs/rtl/objpas/closefile.html> "doc:rtl/objpas/closefile.html") or add the unit name _System_. 

### I get an error when using Printer.BeginDoc

The unit Printers must be added to the uses section and the _Printer4Lazarus_ package must be added to your project requirement in the IDE under: Project > Project Inspector > Add > New Requirement > Package Name. 

If the Printer4Lazarus package is not in the list when opening the dropdown box it must be installed. The package is part of the Lazarus installation and can be found in: [lazarus installed directory][/|\\]components[/|\\]printers. 

If you used the default installation directories [lazarus installed directory] is: 

Operating system | Lazarus installed directory   
---|---  
FreeBSD | /usr/local/share/lazarus   
Linux | /usr/lib/lazarus   
macOS | /Library/Lazarus/   
Windows | c:\lazarus   
  
The same solution also applies to the exception you can get when referencing _Printer.Printers_. 

### When I try to compile a project, I get an error message "Cannot find Unit ..."

Please see [Unit not found - How to find units](<Unit_not_found_-_How_to_find_units.md> "Unit not found - How to find units")

For information on the error "Cannot find unit interfaces", see the next section. 

### When I try to compile a project, I get an error message "Cannot find Unit interfaces"

This error means that the compiler can not find the file 'interfaces.ppu' **or** the file was found, but it is wrong or outdated (the .ppu file date code is older than the date code of the compiler itself). Testing the compiler option settings can help debug these issues, via Project: Compiler Options... (use the Test button at the bottom of the dialog). This test also lets you check that the {TargetCPU} and {TargetOS} variables are set properly. 

The interfaces unit can be found in {LazarusDir}\lcl\units\\{TargetCPU}-{TargetOS}\\{LCLWidgetSet}\interfaces.ppu. For example: /home/username/lazarus/lcl/units/i386-linux/gtk/interfaces.ppu. 

It is normal to have multiple versions of interfaces.ppu (in the proper directories) to allow compiling with different widgets. 

If the interface.ppu file is in the folder matching the OS and CPU you selected in the project preferences, and you get this error, you are using a different compiler / RTL for compiling your project than you used for compiling your Lazarus IDE, or the libraries need to be recompiled to give the .ppu files newer compilation dates. 

You can do one of the following: 

  1. Set the compiler in the Environment Options to the one you used to compile Lazarus. Also look carefully in the Environment Options to see if you are using the correct paths for the Lazarus directory and the FPC sources directory. Check that there is only one version of the compiler configuration file fpc.cfg - it should reside in /etc/ for Linux/Unix systems or in the same directory as the fpc compiler for Windows systems. Try to run "fpc -vt bogus" to check which fpc.cfg is being used in your system. Rogue copies often creep in if you have updated your compiler to a new version; they may be found in your home directory or in the same directory as the one in which you built your new compiler. DELETE THESE!
  2. Rebuild the LCL (or Lazarus completely) with the compiler selected in the Environmnent Options. You can do this with Tools -> Build Lazarus. Before doing this, check the current settings in Tools -> Configure Build Lazarus.
  3. You may also try to change the widgetset currently selected for the project. For example, the sample project "objectinspector" that comes with Lazarus is set to gtk by default. Compiling this project will surely give you "Can't find unit interfaces" in Windows platform. Changing widgetset to default(Win32) in [ Project Options | Compiler Options...](<IDE_Window__Compiler_Options.md> "IDE Window: Compiler Options") should fix this issue.



### When I compile a project, that uses a LCL unit, I get a linker error

Here is an example for such a linker error: 
    
    
    /path/to/lazarus/lcl/units/x86_64-linux/wsimglist.o: In function `REGISTERCUSTOMIMAGELIST':
    /path/to/lazarus/lcl//widgetset/wsimglist.pp:266: undefined reference to `WSRegisterCustomImageList'
    

  * Make sure yor project uses the package LCL. You can check this in the project inspector.
  * Make sure unit "interfaces" is used as one of the first units of your program.



![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** These functions are implemented by the LCL backends. By adding the unit interfaces, you link a LCL backend into your program.

### At the line :{$R *.DFM} How can I solve this problem (Lazarus with FPC < 2.4.0)?

Lazarus (or better Linux) doesn't know about resources, so you can't use them in the way Delphi/Win32 does. However Lazarus uses a method pretty compatible with this. You can still use your Delphi layouts (.dfm files) if you use the following steps: 

  * You need a textual version of the .dfm files. D5 and higher are doing this by default. If you have older files: `Alt`+`F12` to see the layout as text and paste/copy. When you have a text .dfm file, just copy it to a .lfm file.
  * Create a file with lazres (in lazarus/tools) lazres yourform.lrs yourform.lfm
  * Add the following initialization section to


    
    
    initialization
    {$I yourform.lrs}
    

Please keep in mind that not all properties in the dfm are supported yet by Lazarus, so you might get a crash. 

Edit: Since FPC 2.4.0, Delphi style resources are supported, so you don't need to change anything. Even Lazarus SVN already uses it as default projects. Note: it's still advised that you use Lazarus' Delphi project converter as there might still be unsupported properties. 

### 'Identifier not found LazarusResources'

When creating a form Lazarus automaticaly add some extra units to the uses section of your form unit. During the conversion of a delphi unit to a Lazarus unit this does not happen. So you need to add LResources to the Uses section of your form unit. 

### When accessing events of objects, _e.g._ , the onclick event of a button, I get the following error. ERROR unit not found: stdCtrls

Make sure, in the Project -> Project Inspector, that your project depends on the package "LCL" and that you have installed the FPC sources. 

Lazarus is the IDE and the visual components library LCL. All other stuff, like IO, Database, FCL and RTL are provided by FPC. The IDE needs the paths to all sources. 

The FPC source path can be set via: Environment -> Environment Options -> Files -> FPC source directory 

### 'Fatal: Internal error XXXXYYZZW'

An internal error is a compiler error that is not expected to happen (but is prepared to easily track down the problem if happens by giving code XXXXYYZZW). Every internal error is a bug, thus it's advised to report to the bugtracker (with small example that can demonstrate the internal error) when one gets it. 

### Fatal: Cannot find [...] used by [...], incompatible ppu=[filename], package [package name]

When the compiler gives the message "Cannot find A used by B", Lazarus checks what pas/pp/ppu files are in the search path, and if there is a ppu file it inserts "incompatible ppu=filename, package".__

Explanation: FPC cannot use this ppu for one of these reasons: 

  * it was compiled with another FPC version;
  * it was compiled with (depends on) some other ppu files that are not in the search path;
  * you misconfigured some search path(s).



Without knowing how you installed FPC + Lazarus, I'd be tempted to delete both and start again (make sure to delete the fpc.cfg file). 

### Lazarus cannot open a project if last character of the path to it is a space

If you try to open a project like `/home/bart/space /test.lpi` (note the space just before /test.lpi), the Lazarus IDE will give an error saying it cannot find `/home/bart/space /test.lpi`. 

This is a known limit of the IDE, which is caused by the fact that the IDE a.o. has to handle paths that users have supplied in edit controls, which have to be stripped from surrounding spaces. This limitation in no way affects user programs written in FPC/Lazarus. Avoid such pathnames for projects. 

### strToFloat - EConvertError: "0.nnn" is an invalid float

The "0.nnn" can be any floating point number. The problem is that your locale settings (eg DE Germany) are expecting a comma to be the decimal separator in numbers and not a decimal point. The preferred solution is to use the overlaoded version of strToFloat which accepts a TFormatSettings parameter. You could also add this to your code: `DefaultFormatSettings.decimalSeparator := '.';` to override your locale's decimal separator. Notice however that (at least on Windows) this could be set back to your locale's default in respons to a `WM_SETTINGCHANGE` message that is broadcasted. Alternatively, instead of strToFloat and floatToStr, use the Pascal intrinsics val and str - they always need a decimal point. 

## Debugger

_See also[GDB Debugger Tips](<GDB_Debugger_Tips.md> "GDB Debugger Tips")._

### How do I set up/configure for debugging

[Setup the debugger](<Debugger_Setup.md> "Debugger Setup")

### How can I inspect properties?

You have to use FPC 2.4 or newer. 

If you compile your application using -gw (dwarf debug info), you should be able to inspect properties. 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** This is only true for properties that map directly to a variable (the "read" declaration points to a member, not a function).

If your property returns the value of a function it is very dangerous to evaluate/inspect it. It would require this function to be called, and very often it would change the values of some of your variables. This would mean it would alter the state of your application in the debugger, and any further code-execution, debugging or inspections would return incorrect results. 

The optional ability to explicitly inspect the results of functions (with the risks described), and therefore calling code from the debugger is not yet implemented 

### Why does the debugger not show some Variables/Structures ("no such symbol"/"incomplete type")

For problems debugging: 

  * properties
  * Array of ... (Dynamic Array)
  * Variables in Nested Procedures
  * "no such symbol in context"
  * "incomplete type"



Please see [GDB_Debugger_Tips](<GDB_Debugger_Tips.md> "GDB Debugger Tips") and the and [FpDebug#Properties](<FpDebug.md> "FpDebug")

### How can I debug FCL components from packages with Lazarus

FCL components and classes are built without debug information by default and as a result, gdb cannot access component methods or properties. To build package components they must be rebuilt with a debug-line information "-gl" switch. 

This example assumes you have a Linux distribution with /usr/local/ installation prefix and that the Database package fcl-db is what is needed to contain debug-line information. While the fcl-db is used in this example, you may issue this make command from ANY of the included packages. 

Before you begin, you need to locate your FPC path by examining your FPC configuration file. The file (fpc.cfg) is located at /etc/fpc.cfg. Display the contents of fpc.cfg and find your fpc installation path. Look for a line starting with -Fu in the fpc.cfg: 
    
    
    -Fu/usr/local/lib/fpc/$fpcversion/units/$fpctarget/*
    

Make scripts are installing units into INSTALL_PATH/lib/fpc/$fpcversion/units/$fpctarget/, so you must be sure that /usr/local is the installation path, and should be assigned to INSTALL_PREFIX, otherwise the Make scripts will place units where they don't belong or the Make script will fail. 

  1. Open a shell terminal
  2. cd /user/local/share/src/fpc-2.3.1/fpc/fcl-db/
  3. sudo make clean all install INSTALL_PREFIX=/usr/local OPT=-gl



![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** INSTALL_PREFIX parameter should be properly configured for units to be installed.

In the sample below /usr/local is a default fpc path for Linux, but may vary on other OSes. 
    
    
    make clean all install INSTALL_PREFIX=/usr/local OPT=-gl
    

Finally, after rebuilding any FCL units you may want to rebuild LCL as well. 

### Problems with GDB on Mac

Lazarus has now support for lldb on Mac. See [Configuring the lldb debugger](<Installing_Lazarus_on_macOS.md> "Installing Lazarus on macOS"). 

If you want to use gdb, you can find some info here: [GDB tips for Mac](<GDB_Debugger_Tips.md> "GDB Debugger Tips")

### How can I use a log file for debugging?

Lazarus provides a basic log framework (style [`debugln(...)`](<DebugLn.md> "DebugLn") ) in the unit [LazLogger](<LazLogger.md> "LazLogger"). See that page for more details. 

### How can I use a Log-file for debugging the IDE itself?

Lazarus itself uses [LazLogger](<LazLogger.md> "LazLogger"). 

This means that you can run Lazarus with 
    
    
    --debug-log=lazarusdebuglog.txt
    

If this is not sufficient: in the IDE the extent of the information that is logged can be controlled with `--debug-enable`. See `--help` for a list of keywords, or install the package IdeLazLogger. 

If you want output on a console (most useful for Windows), add the option **-WC** in the Tools / Configure "Build Lazarus" options. Then rebuild Lazarus. 

See also [Console Mode Pascal#Run in IDE with redirected output](<Console_Mode_Pascal.md> "Console Mode Pascal")

## Contributing / Making Changes to Lazarus

### I created a Patch to dock the IDE Messages form on the "Source Code Editor" form (at bottom)

Such patches will not be applied, because they only implement a small part of the needed docking. The goal is to create a complete dock manager and use that. A complete dock manager can dock all IDE windows and it allows to let the user define how to dock. For example dock the messages window above or below the source editor — or not at all. For instance, 
    
    
    +-------------------++--+
    |menu               ||  |
    +-------------------+|  |
    +--++---------------+|  |
    |PI|| Source Editor ||CE|
    +--+|               ||  |
    +--+|               ||  |
    |  |+---------------++--+
    |OI|+-------------------+
    |  ||messages           |
    +--++-------------------+
    

The dock manager can store the layout and restore it on next load. Preferably the dock manager can dock in pages too. The dock manager does not need to use drag and drop. All patches implementing docking without a dock manager makes it harder to implement a real dock manager and will be rejected. 

An example for such a dock manager is the package [anchordockdsgn](</index.php?title=anchordockdsgn&action=edit&redlink=1> "anchordockdsgn \(page does not exist\)")

### I have fixed/improved Lazarus. How can I add my changes to the official Lazarus source?

Create a patch and send it to the developers. For details, see [Creating A Patch](<Creating_A_Patch.md> "Creating A Patch"). 

### How can I install more than one copy of Lazarus?

Please see [Multiple Lazarus](<Multiple_Lazarus.md> "Multiple Lazarus")

### How can I become a Lazarus developer and access management in the SVN and bug-tracker?

First of all, you must learn about Lazarus, to prove your knowledge and skill. Start by reading the [Lazarus Documentation](<Lazarus_Documentation.md> "Lazarus Documentation") wiki article, read the Lazarus source code, giving a look at the [Lazarus Bug-Tracker](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/>), fix some bugs, and if you think you are ready, contact the developers on the [mailing list](<https://www.mail-archive.com/lazarus@miraclec.com/>). 

## Where is ... defined

### Virtual key constants

Virtual key constants are defined in [LCLType](<http://lazarus-ccr.sourceforge.net/docs/lcl/lcltype> "doc:lcl/lcltype"). Add LCLtype to your **uses**. 

## Using the IDE

### How can I use "identifier completion"?

You can invoke identifier completion by pressing [ctrl][space]. Under the menu item _Environment - > Editor Options -> Code Tools -> Automatic Features_ you can set how quick this should happen automatically. 

### Can I change the code editor font and colours?

Yes, see the menu item _Tools - > Options_ and use the Editor/Display and Colors sections. On Windows you may find that the default Courier (and some other typefaces) are either too sharp or too soft, either selecting/deselecting 'Disable anti-aliasing'. TrueType fonts are an improvement, such as the free "Bitstream Vera Sans Mono" font available from [[1]](<https://download.gnome.org/sources/ttf-bitstream-vera/1.10/>). (To install a new font, unzip and copy the files into a suitable folder. In Windows XP you will need to use the Fonts section of the Control Panel. In Windows 7 you may select all the font files, right-click and find an Install option in the pop-up menu) 

### How can I disable specific compiler warning messages?

To disable specific compiler warning messages you can either insert a **{$WARN number OFF}** directive in your code or use FPC's option **-vm <x>**. You can find details for these in FPC's [Programmer's guide](<https://www.freepascal.org/docs.html>) and Appendix A of the [User's guide](<https://www.freepascal.org/docs.html>). 

### What are the IDE command line options?

See [Lazarus command line options](<Lazarus_command_line_options.md> "Lazarus command line options"). 

## Linux

### Typing in edit fields generate duplicate letters

This is more likely to happen under Ubuntu and Ubunto derived distros, and with GNOME Desktop but other distros/Desktop environments can be affected too. 

The problem is originated by bugs in the ibus Input Method System. Unfortunately Ibus is the default input method of GNOME desktop. 

If you don't know what an IM is, this means that you do not need it, and therefore the easiest way is to disable IM's at all. 

In order to do that you have a number of options, namely: 

  * 1 - Disable (or set to "none") the Input Method in System Settings->Preferences->Input Method


  * 2 - Add the three following lines in /etc/profile (system wide)


    
    
    export GTK_IM_MODULE=gtk-im-context-simple
    export QT_IM_MODULE=simple
    export XMODIFIERS=@im=none
    

  * 3 - Add a file named _input_method.sh_ containing just the 3 lines above in /etc/profile.d (system wide)


  * 4 - Add the 3 lines above in the home directory ~/.xprofile (per user). If there's no ~/.xprofile in your home directory, you should create one.


  * 5 - Create a Lazarus launching script containing the same 3 lines. (affects only Lazarus)



Methods 1 and 2 may not survive after a system update, while 3,4 and 5 will. 

Be aware that both GTK and QT applications may change dynamically the IM. If you experience the problem in an apparently random way, chances are good that some other application is enabling again an IM and fails to set to default when terminates. If such is the case method 5 is the only one suitable. 

You may find more information about Input Methods, and how to deal with them in: 

<https://fedoraproject.org/wiki/I18N/InputMethods> (This is Fedora specific but most informations apply in general) 

<http://www.pinyinjoe.com/linux/ubuntu-12-chinese-setup.htm> (This is Ubuntu specific, but again most informations apply in general. It explains how to **enable** IM but the info can be used also to **disable** it) 

### How can I debug on Linux without the IDE?

First of all you need a debugger. gdb is the standard debugger under linux and there are several GUI-frontends available. One common frontend is ddd, which is part of most common distributions. To compile lazarus/lcl with debug-information you should then use the following commands to start a debug session: 
    
    
    make clean; make OPT=-dDEBUG
    ddd lazarus
    

Be warned, however, that ddd is not as comfortable as, _e.g._ , the Lazarus debugger, especially if it comes to view the contents of a variable you have to take into account that ddd/gdb are case sensitive whereas Pascal is case-insensitive. Therefore, you have to type all variable names in uppercase to see their contents. For more information see the fpc-manuals. 

### I can debug now but ddd does not find my sources or complains that they contain no code. What's that?

This is a path-related problem with either gdb or ddd. You can avoid this by 

  * Use the "Change directory" command from the ddd menu and choose the directory where the sources are located. The drawback of this method is that you now can't use the source of the program you started with (e.g. lazarus). Thus it may be neccessary to change the directory multiple times.
  * In ddd goto [Edit] [gdb-settings] and set the search-path
  * Create a $(HOME)/.gdbinit file like:


    
    
         directory /your/path/to/lazarus
         directory /your/path/to/lazarus/lcl
         directory /your/path/to/lazarus/lcl/include
    

### I receive an error during the linking that states /usr/bin/ld: cannot find -l<some lib>

For example: 
    
    
     /usr/bin/ld: cannot find -lgdk
    

This means an external library was not found. In the above case under Linux it means the libgdk.so or libgdk.a was not found. Normally this means, you forgot to install the development package of this library. In the case of gdk the package is called under the Fedora Core distribution: 'gtk+-devel-1.2.10-33'. 

**Package Based Distributions**
    You need to install the package that provides the lib<somelib>.so or lib<somelib>.a files. Dynamic libs under linux have the extension .so, while static libs have the extension .a. On some Linux distro's you have installed the package (rpm, deb) <packagename> which provides <some lib>, but you also need the development package (rpm, deb), normally called <packagename>-dev, which contains the .a (static lib) and/or the .so (dynamic lib).

    Some distributions have commands to find which package contains a file:
    **Mandriva**
    
    
    urpmf lib<somelib>.so
    

    will list all packages containing the file named lib<somelib>.so, you'll have to install those ending in -devel

    **Debian**

    install the apt-file utility (apt-get install apt-file) then
    
    
    apt-file search lib<somelib>.so
    

    will list all packages containing the file named lib<somelib>.so, you'll have to install those ending in -dev

    **SuSE**

    SuSE installs the gtk devel libs under /opt/gnome/lib (or /opt/gnome/lib64 for 64 bits), which is not in the standard lib path. Simply add it to your /etc/fpc.cfg. (-Fl/opt/gnome/lib).

**Source Based Distributions and Manual Compilation (LFS)**
    Make sure that there is a lib<somelib>.a in the path, and that it contains the right version. To let the linker find the dynamic library, create a symlink called lib<some lib>.so to lib<some lib><version>-x,y.so if necessary (and/or for static lib; lib<some lib>.a to lib<some lib><version>-x,y.a).

    **FreeBSD** : As source based distro's, and also make sure you have -Fl/usr/local/lib in your fpc.cfg and/or Lazarus library path. Keep in mind that GTK1.2 has "gtk12" as package name under FreeBSD. (same for glib) NOTE: This has changed as of late. Newest ports have gtk-12 and glib-12 as well. You might stumble on this problem, since FPC requires the "-less" ones, you will need to symlink them like this:
    
    
    # as root
    cd /usr/local/lib && ln -s libglib-12.so libglib12.so
    cd /usr/X11R6/lib && ln -s libgtk-12.so libgtk12.so
    cd /usr/X11R6/lib && ln -s libgdk-12.so libgdk12.so
    

    **NetBSD** : As source based distro's, and also make sure you have -Fl/usr/pkg/lib in your fpc.cfg and/or Lazarus library path

    **Fedora** : In the "add/remove software" panel search for: "gtk2-devel", "glibc-devel" ([see here](<https://www.mail-archive.com/lazarus@lists.lazarus.freepascal.org/msg03637.html>)), "libx11-devel"

### I receive a warning during the linking that states: Warning: "crtbeginS.o" (or "crtendS.o") not found

The actual filename may also be "crtbegin.o" or "crtend.o"  
In most cases linking will not fail.  
Here's how to find where that file is (you need to have gcc installed, and gcc needs to target the same architecture as fpc), and tell the fpc (which tells the linker): 
    
    
    bart@simenon ~ $ gcc --print-file-name crtbeginS.o
    /usr/lib/gcc/x86_64-linux-gnu/5/crtbeginS.o
    

Add the path into fpc.cfg: 
    
    
    # path to the gcclib
    #ifdef cpui386
    #-Fl/usr/lib/gcc/x86_64-linux-gnu/5  if you have a 32-bit fpc: fill in the correct path here
    #endif
    #ifdef cpux86_64
    -Fl/usr/lib/gcc/x86_64-linux-gnu/5
    #endif
    

Note: On Linux the default (external) linker is ld. This is part of the linux distribution and not part of, or built with, FreePascal. 

### How can I convert a Kylix 2 project into a Lazarus project?

Nearly the same way as converting a Kylix project into a Delphi/VCL project. 

The LCL (Lazarus Component Library) tries to be compatible to Delphi's VCL. Kylix's CLX tries to be QT compatible. Here are some general hints: 

  * Rename all used CLX Q-units like QForms, QControls, QGraphics, ... into their VCL counterparts: Forms, Controls, Graphics, ...
  * Add LResources to the uses section of every form source
  * Rename or copy all .xfm files to .lfm files.
  * Rename or copy .dpr file to .lpr file.
  * Add "Interfaces" to the uses section in the .lpr file.
  * Remove {$R *.res} directive
  * Remove {$R *.xfm} directive
  * Add {$mode objfpc}{$H+} or {$mode delphi}{$H+} directive to .pas and .lpr files
  * Add an initialization section to the end of each form source and add an include directive for the .lrs file (Lazarus resource file):


    
    
    initialization
    {$I unit1.lrs}
    

    The .lrs files can be created via the lazres tool in: (lazarusdir)/tools/lazres.
    For example: ./lazres unit1.lrs unit1.lfm

  * Fix the differences. The LCL does not yet support every property of the VCL and the CLX is not fully VCL compatible.


  * To make it more platform independent, reduce unit libc (which is deprecated) references and substitute with native FPC units like baseunix/unix as much as possible. This will be necessary to support other targets than linux/x86 (including macOS, FreeBSD and Linux/x86_64)



### When compiling lazarus the compiler can not find a unit. e.g.: gtkint.pp(17,16) Fatal: Can't find unit GLIB

1\. Check a clean rebuild: do a 'make clean all' 

2\. Check if the compiler has the correct version (2.0.4 or higher) 

3\. Check if the compiler is using the right config file. The normal installation creates /etc/fpc.cfg. But fpc also searches for ~/.ppc386.cfg, ~/.fpc.cfg, /etc/ppc386.cfg and it uses only the first it finds. 

    **Hint:** You can see which config file is used with 'ppc386 -vt bogus'
    Remove any ppc386.cfg as it is really obsolete.

4\. Check if the config file (/etc/fpc.cfg) contains the right paths to your fpc libs. There must be three lines like this: 
    
    
       -Fu/usr/lib/fpc/$fpcversion/units/$fpctarget
       -Fu/usr/lib/fpc/$fpcversion/units/$fpctarget/rtl
       -Fu/usr/lib/fpc/$fpcversion/units/$fpctarget/*
    

    The first part of these paths (/usr/lib/fpc) depends on your system. On some systems this can be for example /usr/local/lib/fpc/... .
    **Hint:** You can see your searchpaths with 'ppc386 -vt bogus'

5\. Check that the config file (/etc/fpc.cfg) does not contain search paths to the lcl source files (.pp, .pas): 
    
    
     forbidden: -Fu(lazarus_source_directory)/lcl
     forbidden: -Fu(lazarus_source_directory)/lcl/interfaces/gtk
    

    If you want to add the lcl for all your fpc projects, make sure that the two paths look like the following and are placed after the above fpc lib paths:
    
    
     -Fu(lazarus_source_directory)/lcl/units/$fpctarget
     -Fu(lazarus_source_directory)/lcl/units/$fpctarget/gtk
    

6\. Check if the missing unit (glib.ppu) exists in your fpc lib directory. For example the gtk.ppu can be found in /usr/lib/fpc/$fpcversion/units/i386-linux/gtk/. If it does not exists, the fpc lib is corrupt and should be reinstalled. 

7\. Check if the sources are in a NFS mounted directory. In some cases the NFS updates created files incorrectly. Please, try to move the sources into a non NFS directory and compile there. 

8\. If you are still not succeeded try to use samplecfg script as follows: 

_#_ cd /usr/lib/fpc/_version_ / 

_#_ sudo ./samplecfg /usr/lib/fpc/_\$version_ /etc 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** Do not a slash ("/") after etc, because if you do, the system will create a folder `/etc/fpc.cfg/fpc.cfg`. In fact, we want samplecfg to make a file `/etc/fpc.cfg`, not the folder `/etc/fpc.cfg`.

### I have installed the binary version, but when compiling a simple project, Lazarus gives: Fatal: Can't find unit CONTROLS

Probably you are using a newer fpc package, than that used for building the lazarus binaries. The best solution is to download the sources and compile lazarus manually. You can download the source snapshot or get the source via svn: 
    
    
    bash
    svn checkout http://svn.freepascal.org/svn/lazarus/trunk lazarus
    cd lazarus
    make clean all
    

Make sure that Lazarus get the new source directory: Environment->General Options->Files->Lazarus Directory Top 

### Lazarus compiles, but linking fails with: libgdk-pixbuf not found

Install the gdk-pixbuf library for gtk1.x: 

Where to find the gdk-pixbuf library: 

RPMs: [https://rpmfind.net/linux/rpm2html/search.php?query=gdk-pixbuf&submit=Search+...&system=&arch=](<https://rpmfind.net/linux/rpm2html/search.php?query=gdk-pixbuf&submit=Search+...&system=&arch=>)

Debian packages: libgdk-pixbuf-dev 

Sources: <ftp://ftp.gnome.org/pub/gnome/unstable/sources/gdk-pixbuf/>

**Ubuntu 8.10:**

If you are compiling Lazarus with GTK 2.0 you'll get a "libgdk-pixbuf2.0" not found error. Just install libgtk2.0-dev using apt on this way: 
    
    
    apt-get install libgtk2.0-dev
    

### I have SuSE and I get /usr/bin/ld: cannot find -lgtk Error: Error while linking

Older SuSE versions (before SuSE 11) install the gtk devel libs under /opt/gnome/lib (or /opt/gnome/lib64 for 64 bits), which is not in the standard lib path. Simply add it to your /etc/fpc.cfg (-Fl/opt/gnome/lib). 

### I have Kubuntu and I get /usr/bin/ld: cannot find -lgtk-x11-2.0

You can either use the QT or the GTK2 backend. Default is GTK2. For GTK2 install the kubuntu package _libgtk2.0-dev_. 

### Lazarus crashes with runtime error 211 after I installed a component

After I installed a component, Lazarus crashes with the following message: 
    
    
    Threading has been used before cthreads was initialized.
    Make cthreads one of the first units in your uses clause.
    Runtime error 211 at $0066E188
    

How can I fix this? 

Your freshly installed component is using threads. FPC on *nix doesn't automatically include threading support, but it must be intialized. This initialization is done in the cthreads unit. Every application using the component needs to add this unit to the uses clause of the main program. Lazarus itself is no exception. This can be done in two ways: 

1) Open the package. In the package editor click on _Options_. Under page _Usage_ add to the _custom_ options **-dUseCThreads**. Then rebuild the IDE. This way the cthreads unit will be automatically used by the IDE under unix and the cthreads are initialized. 

2) In order to avoid modifying package, an FPC compiler option could be used directly. Open menu Tools->Configure "build Lazarus". "Configure build Lazarus" dialog will be shown; in field "Options:" type `-Facthreads` and then press "OK" button. The next step is to install the package. Lazarus will be built with option -Facthreads which means that it will treat main program as if unit cthreads where first in uses clause. 

_Hint:_ Maybe your old (non-crashing) Lazarus executable is stored as lazarus.old in the same directory as the crashing Lazarus executable. 

See also [Multithreaded Application Tutorial#Units needed for a multithreaded application](<Multithreaded_Application_Tutorial.md> "Multithreaded Application Tutorial")

### When I run a program with threads I get runtime error 232

The complete error message is: 
    
    
    This binary has no thread support compiled in.
    Recompile the application with a thread-driver in the program uses
    clause before other units using thread.
    Runtime error 232
    

**Solution** : Add cthreads as first unit to the uses clause of your main program, usually the .lpr-file. 

### I have Ubuntu Breezy/Mandriva KDE3 and my fonts in Lazarus IDE look too big

If Lazarus is compiled with Gtk1.2, the settings in Gnome Preferences/Font don't have any effect as they are related to Gtk2. You could try this solution: Create a file named .gtkrc.mine in your home directory (if it's not already there) and add these lines to it: 
    
    
    style "default-text" {
           fontset = "-*-arial-medium-r-normal--*-100-*-*-*-*-iso8859-1,\
                      -*-helvetica-medium-r-normal--*-100-*-*-*-*-*-*"
    }
    
    class "GtkWidget" style "default-text"
    

If this is not enough try and create also a .gtkrc symlink to .gtkrc.mine . It worked in this way under Xubuntu 7.10, Mandriva 2009.0 KDE3. 

### How can my gtk programs use custom rc files?

Option a) Name the rc file _yourprogram.gtkrc_ and put it in the same directory where the executable is. 

Option b) Use unit _GtkInt_ and call _GTKWidgetSet.SetRCFilename('your_preferred_rc_file');_ Best done before _Application.Initialize_ in the .lpr file with _{$IFDEF LCLGtk}_. 

Option c) Use unit _gtk2_ and call _gtk_rc_parse('your_rc_file'));_ and _gtk_rc_reparse_all;_. 

### I have Ubuntu and I cannot compile for Gtk2 due to missing libraries

Ubuntu has a problem with not creating all the symbolic links that you'll need even when the libraries are installed. Make sure that all missing libraries when trying to link for Gtk2 have their appropriate links. For instance, you might need to do: 
    
    
    cd /usr/lib
    sudo ln -s libgdk-x11-2.0.so.0 libgtk-x11-2.0.so
    

Make sure that the [whatever].so symbolic links are created and point to the actual libraries. 

### Lazarus runs differently with Linux vs. Windows. What do I need to know?

If you use readln(); and writeln(); for your input and output the console window (sometimes called terminal window) is not going to pop up as it does with Windows. You will need to access this window with View - Debug Windows - Terminal Output. Unlike with Windows you can not use backspace to correct typing errors. The 'Enter' key on the numeric keypad may not work either, depending on the widgetset the IDE was built for. Type carefully! 

Because Linux is case sensitive you will need to take care that you pay attention to case when referencing or accessing files. 

## Windows

### When I cycle/rebuild the compiler, I get

#### The name specified is not recognized as an internal or external command, operable program or batch file.>& was unexpected at this time

In the compiler directory there is an OS2 scriptfile named make.cmd. Different versions of Windows also see this as a script file, so remove it since what is needed for OS2 becomes a hindrance on Windows. 

#### make[3]: ./ppc1.exe: Command not found

Somehow make has lost its path (reason unknown). Try to cycle with a basedir set like: 
    
    
    make cycle BASEDIR=your_fpc_source_dir_herecompiler
    

### When I try to make Lazarus I get:

#### make.exe: * * * interfaces: No such file or directory (ENOENT). Stop.make.exe: * * * [interfaces_all] Error 2

You need to upgrade your make. 

#### makefile:27: *** You need the GNU utils package to use this Makefile. Stop.

Make sure you didn't install FPC in a path with spaces in the name. The Makefile doesn't support it. 

### How can I give my program an XP look like Lazarus has?

Project -> Project Options -> Check 'Use manifest to enables themes'. 

### When I run Windows program created in Lazarus it starts with a DOS window

Specify the -WG argument (Windows GUI) on the command line of the compiler or in the Lazarus IDE check the Windows GUI check box on the compiler options dialog box (Project menu -> Compiler Options -> Config and Target -> target OS Specific options. 

## macOS

### Why does compiling a project fail with 'unknown section attribute: no_dead_strip'?

Dead code stripping is not supported by the assembler and linker before Xcode 1.5 (available for macOS 10.3.9). Disable the compiler options: 

  * Code > Unit style > Smart linkable (-CX)
  * and Linking > Link Style > Link smart (-XX)



### Xcode 3.1 and later

The macOS linker in Xcode 3.1 (2007, Leopard 10.5) and later does not support stripping the symbol table attached to the output of the assembler and link editor. The linker supported stripping, for 32 bit executables only, up to Xcode 2.5 (2005, Tiger 10.4). 

You will need to manually run the strip command line utility instead after compiling your executable. 

### Debugging

[Please see here for debugger issues under macOS](<GDB_Debugger_Tips.md> "GDB Debugger Tips")

## Licensing

### Can I make commercial applications with Lazarus?

Yes, the LCL is licensed under the [LGPL with an exception](<FPC_modified_LGPL.md> "FPC modified LGPL"), which allows you to link to it statically without releasing the source of your application. Modifications and enhancements to the LCL must be distributed with source. Lazarus, the IDE, is licensed under the GPL. The LCL consists only of the code in the directory named "lcl", other code may not be covered by this statement. 

### Why are some components restricted from usage in commercial application?

Lazarus comes with additional components, that were developed by third parties. Those are under various other Licenses. If you wish to use them you need to see the License within the source files of those packages. Most of those 3rd party components are in the directory "components". 

### How do I know if a Component is part of the LCL?

All LCL units are in the directory "lcl". A list of units belonging to the LCL can be found [here](<https://lazarus-ccr.sourceforge.io/docs/lcl/>). If your code uses units not listed on this page, you may have used a component that is not part of the LCL. 

### Can I make commercial plug-ins for Lazarus?

Yes, the IDEIntf part of the IDE is licensed under the LGPL with the same exception, so that shared data structures in this part will not force you to license your plug-in or design-time package under the GPL. You are free to choose a plug-in of any license; we don't want to limit your choice. Therefore non-GPL compatible plug-ins are allowed. Note that it's not allowed to distribute a precompiled Lazarus with these non-GPL-compatible plugins included statically; however, we do not see this as a severe limitation, since recompiling Lazarus is easy. 

## Using the Forum

### What is the correct way to ask questions in the forum?

First and most important: **always mention which Lazarus version, Free Pascal version, Widgetset, CPU Architecture and Operating System (with full version) that you are using!** Don't just say that you are using "the latest version"; please state the exact version number. For snapshots, the revision number and/or date are also important. 

When asking a programming question, **always try to include some source code which demonstrates the problem**. Please surround your code with [code=pascal]...[/code] tags (or use the Insert code button in the forum post editor toolbar). You can attach complete programs as zip files if you want. Use Lazarus/Project/Publish Project to help with that. 

If you get an error on your code, **always specify what the error is**. Simply copying what the compiler / debugger / your program said should be enough in most cases. If the error happens at runtime, use -gl and disable function inlining when compiling so your program can generate a proper backtrace. 

Often an image is also very useful. You can host images in <https://imageshack.com/> and other similar websites and then post a link. 

Try to provide both a high-level overview of the problem (**what goal you want to achieve**) as well as a more detailed view of the problem (**how are you trying to achieve your goal'. Often, there is a much easier way of getting things done that you may not know about.**

Finally, make sure you have searched/read the relevant documentation (e.g. the [FPC documentation](<FPC_documentation.md> "FPC documentation") and Lazarus wiki, see e.g. [Lazarus Documentation](<Lazarus_Documentation.md> "Lazarus Documentation"). You should also [search the forums](<https://forum.lazarus.freepascal.org/index.php?action=search>): other people with may already have found solutions to similar problems. If you indicate you have already done a bit of homework, people will in general be much more willing to help. 

For more tips about asking questions the smart way, read this: <http://catb.org/esr/faqs/smart-questions.html>

See also: [How to use the forum](<Forum.md> "Forum")

## Version numbering

See the [Version Numbering](<Version_Numbering.md> "Version Numbering") page for an explanation of the branching and version numbering of Lazarus. 

## Problems/issues with old Lazarus/FPC versions

Issues with old Lazarus/FPC versions and notes about upgrading are covered below. 

Often, the best solution is to upgrade; please see the various [Release Notes](</Category:Release_Notes> "Category:Release Notes")

### Why is the linking so slow on Windows (Lazarus < 0.9.24)?

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** This problem was fixed in FPC 2.2 and Lazarus 0.9.24. Please update your Lazarus if you can. For older versions read text below.

Generally speaking, compilation on Windows takes more time then other platforms because the GNU Linker utilized by Free Pascal is slow on this platform. This problem only affects Windows, and is only bad on relatively old computers (less then 1Ghz) and computers with little RAM (128MB or less). 

Also, if you smartlink LCL the linking will be much slower. A study about this is located here: [File size and smartlinking](<File_size_and_smartlinking.md> "File size and smartlinking")

Starting from FPC 2.2, an internal linker is used which decreases the linking time dramatically. 

### Old Lazarus does not support Windows-style resources; converting lrs resources

Older versions of FPC (and Lazarus based on it) don't support Windows (.rc/.res) resources, but only LRS files (Lazarus Resource files). See [Lazarus Resources](<Lazarus_Resources.md> "Lazarus Resources") Since 0.9.30 Lazarus can use fpc resources. That means you can delete the lrs files for forms and replace the include directives `{$I unit1.lrs}` with `{$R *.lfm}`. 

### Antivirus

Excluding the Lazarus directory (and the directory/directories where your project/projects are) from on access virus scanning may help there. 

## See also

  * [Mac Installation FAQ](<Mac_Installation_FAQ.md> "Mac Installation FAQ") for macOS.
  * [Unit not found - How to find units](<Unit_not_found_-_How_to_find_units.md> "Unit not found - How to find units") information on what to do when you get "unit not found" messages.

---

_Source: [https://wiki.freepascal.org/Lazarus](https://web.archive.org/web/20250429210610/https://wiki.freepascal.org/Lazarus)_
