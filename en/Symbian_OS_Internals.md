# Symbian OS Internals

## Contents

  * 1 Build System Reengineering
    * 1.1 Default SDK Build Process for UIQ 3
    * 1.2 Default SDK Build Process for UIQ 2.1
    * 1.3 Modifyed Build Process
    * 1.4 Registering a application on the UIQ Emulator
  * 2 Comments about the Build system
  * 3 Comments about File formats
  * 4 Misc info from old SymbianOS Port page
  * 5 See Also



## Build System Reengineering

This section is a documentation on the attempt to re-engineer the UIQ 3 build system to a format more friendly to Free Pascal and cross-platform development in general. 

### Default SDK Build Process for UIQ 3

Two commands are executed when building UIQ 3 software for the emulator. Suppose your UIQ SDK is installed on c:\Symbian\UIQ3SDK and your software C++ source code is on C:\QHelloWorld\src. 

On C++ you should enter the directory C:\QHelloWorld\group where the build file is located (bld.inf), and type: 
    
    
    bldmake bldfiles
    abld build winscw udeb
    

The first command will: 

  * Generate the directory: C:\Symbian\UIQ3SDK\epoc32\BUILD\Programas\SymbianOS\QHelloWorld\group


  * Fill it with many Makefiles


  * Create a ABLD.BAT script


  * Perl files involved: bldmake.pl



The second command will: 

  * Generate the directory: C:\Symbian\UIQ3SDK\epoc32\BUILD\Programas\SymbianOS\QHelloWorld\group\QHELLOWORLD\WINSCW


  * There it will put the source code, a QHELLOWORLD.WINSCW file and a UDEB directory with compiled .o files for the software


  * It will also create a executable (QHelloWorld.exe) for the software on the directory: C:\Symbian\UIQ3SDK\epoc32\release\winscw\udeb


  * Perl files involved: abld.pl makmake.pl



### Default SDK Build Process for UIQ 2.1

The build process for UIQ 2.1 is very similar to UIQ 3, but there are some differences. 

There commands to build a software are: 
    
    
    cd C:\Programas\UIQ21\UIQExamples\HelloWorld
    devices -setdefault @UIQ_21:com.symbian.UI
    bldmake bldfiles
    makmake HelloWorld.mmp THUMB
    abld build thumb urel
    

This will: 

  * Generate the directory: C:\Programas\UIQ21\epoc32\BUILD\PROGRAMAS\UIQ21\UIQEXAMPLES\HELLOWORLD\HELLOWORLD\THUMB\UREL


  * Fill the directory C:\Programas\UIQ21\epoc32\BUILD\PROGRAMAS\UIQ21\UIQEXAMPLES\HELLOWORLD it with many Makefiles


  * Create a ABLD.BAT script


  * Fill the first UREL directory with .o object files and a .in file


  * The THUMB directory will have a .THUMB a .rsg and a .def library definition file


  * It will also create a executable (QHelloWorld.app) for the software on the directory: /epoc32/release/thumb/urel


  * Perl files involved: bldmake.pl abld.pl makmake.pl



The main difference is that all applications are actually DLLs on UIQ 2, so we need to use tools like dlltool and .def files. 

### Modifyed Build Process

To aid on this complex build process, a command line tool called "mksymbian" will be created. 

Default UIQ build process expects that the source be on a rather peculiar directory format. This is unsuitable for cross-platform development. Just imagine if each OS required a different directory tree, cross-platform would be impossible. Instead we will simply require a .ini file specifying the target options. 

On the folder of the project you should have source code and also one or more project information files. This files should be in INI format, and specifies information for our tool to build the project. 

To compile a project with it you can do: 
    
    
    mksymbian build myproject.ini
    

One important task of this build tool is identifying correctly where Symbian SDKs and Free Pascal are installed. You can verify this using the command: 
    
    
    mksymbian showpath
    

### Registering a application on the UIQ Emulator

Each application should provide a non-localizable resource file with information necessary to register it on the emulator (or later on a real phone). This file ends with the .rss extension and should look similar to this example one: 
    
    
    // QHelloWorld_reg.rss
    #include <AppInfo.rh>
    
    UID2 KUidAppRegistrationResourceFile
    UID3 0x01000001
    
    RESOURCE APP_REGISTRATION_INFO
    {
    	// filename of application binary (minus extension)
    	app_file = "QHelloWorld";
    }
    

During the build process this file will be compiled into a .RCS resource file. On the emulator, all registration files should be located on the directory \private\10003a3f\apps 

Mksymbian tool will take care of compiling the resource and placing it on the correct position. There next paragraphs on this sections are technical details of how this is done. 

Looking at the UIQ 3 SDK directory structure you will find 3 directories that match \private\10003a3f: 

  * C:\Programas\UIQ3SDK\epoc32\data\Z\private\10003a3f\apps



Here a copy of your RCS resource file will be put 

  * C:\Programas\UIQ3SDK\epoc32\winscw\c\Private\10003a3f



This directory contains only some binary files. 

  * C:\Programas\UIQ3SDK\epoc32\release\winscw\udeb\Z\private\10003a3f\apps



Here a copy of your RCS resource file will be put, and you can also see RCS files for all other software installed on the emulator. 

## Comments about the Build system

there is also an replacement for the Symbian build system which uses Makefiles: <http://www.koeniglich.de/sdk2unix/symbian_sdk_on_unix.html> \-- [User:Johanneswi](</index.php?title=User:Johanneswi&action=edit&redlink=1> "User:Johanneswi \(page does not exist\)")

    That build system only targets the ARM device, not the Emulator, and isn´t available for UIQ 3. So it doesn´t help much --[Sekelsenmat](</User:Sekelsenmat> "User:Sekelsenmat") 16:11, 26 October 2006 (CEST)

A simpler way would be to concentrate on the POSIX compatible API that Symbian OS has... that would be much faster to do.... And it would not be fixed to UIQ only ... 

We could probably reuse existing bindings (for example the Unix rtl or something like that...) -- [User:Johanneswi](</index.php?title=User:Johanneswi&action=edit&redlink=1> "User:Johanneswi \(page does not exist\)")

## Comments about File formats

The emulator does not use the E32 fileformat used by the real devices but makes an normal PE exe that bind against the emulator libraries so thath should be much easier to do than to really build ARM binaries... (I don't have Windows right now so I can't test anything yet...) -- [User:Johanneswi](</index.php?title=User:Johanneswi&action=edit&redlink=1> "User:Johanneswi \(page does not exist\)")

    Yes, and that´s why the first target will be the emulator =) --[Sekelsenmat](</User:Sekelsenmat> "User:Sekelsenmat") 16:11, 26 October 2006 (CEST)

  


## Misc info from old SymbianOS Port page

1\. Compiler seems to be complete for arm, but THUMB and interworking is something good for symbian. 

2\. It seems there are some ABI incompatibilities, at least people from symbian irc insist only symbian 9 and greater is ok with arm abi, find out what they are and how to implement them. 

3\. Everything on symbian is in C++, and fpc cannot connect to C++ object files (yet). The only solution is to create bindings in c and then link fpc to c instead. 

These tools seems to be helpful in some way: 
    
    
      smoke, swig, doxygen
    

And some links to look at in this field: 

[rvelthuis.de/articles/articles-cppobjs.html](<http://rvelthuis.de/articles/articles-cppobjs.html>)

[qtcsharp.sourceforge.net/background.php](<http://qtcsharp.sourceforge.net/background.php>)

[dot.kde.org](<http://dot.kde.org/1095964115/1095969821/1095971862/1095972037/1095972799/1096037901/1096041117/1096057255/>)

[wiki.kde.org/tiki-index.php?page=Language+Bindings+Talk](<http://wiki.kde.org/tiki-index.php?page=Language+Bindings+Talk>)

[wiki.dotgnu.info/Smoke/Refract](<http://wiki.dotgnu.info/Smoke/Refract>)

4\. Symbian has a very complicated build/make process. How to do that? 

5\. Symbian has different kinds of applications, some new application modes have to be introduced. 

6\. Investigate how other languages, not C++, deal with these problems. 

7\. Neither Symbain OS nor its C++ have exceptions, static data and some strange other things. why? Does it mean we cannot have them on fpc, too? How to deal with this? 

8\. Someone with good knowledge and background of symbian is defenitely needed here. I just know how to code simple programs on symbian. 

9\. The source code of gcc might be useful too. 

Some usefull links: 

[Symbian programming tutorial, recommended!](<http://www.sciabarra.com/symbian-tutorial/?Symbian_C%2B%2B_Programming_Tutorial>)

[Tutorial](<http://symbian.compactbyte.com/>)

[Pythong for series60](<http://www.postneo.com/postwiki/moin.cgi/PythonForSeries60>)

[SDKS](<http://www.newlc.com/-SDKs->)

[Symbian OS design faults, interesting](<http://www.codeproject.com/ce/Symbian_OS_design_faults.asp>)

Good books: 

Wiley, Programming for the Series 60 Platform and Symbian OS (2003) 

Wiley, Symbian OS Internals Real time Kernel Programming Dec 2005 

## See Also

  * [SymbianOS](<SymbianOS.md> "SymbianOS")

---

_Source: [https://wiki.freepascal.org/Symbian_OS_Internals](https://web.archive.org/web/20240920204100/https://wiki.freepascal.org/Symbian_OS_Internals)_
