# SymbianOS

[![Warning-icon.png](https://wiki.freepascal.org/images/b/b2/Warning-icon.png)](</File:Warning-icon.png>)

**Warning:** The work was too large, the platform changed too fast and now the entire SymbianOS is deprecated, so the port was abandoned

## Contents

  * 1 Roadmap to port FPC to SymbianOS
  * 2 Versions Roadmap
  * 3 Compiling Free Pascal for the Emulator
  * 4 Compiling Free Pascal for the real device
  * 5 Building Symbian OS Applications
    * 5.1 Overview
    * 5.2 Guide for building applications for the Emulator
    * 5.3 Using mksymbian
  * 6 Screenshots
  * 7 See Also
  * 8 External Links



## Roadmap to port FPC to SymbianOS

  1. ~~Develop a Hello World application on c++ for SymbianOS UIQ 3~~ \- Felipe
  2. ~~Convert the Build software from perl to our own building system, and make it build the c++ software~~ \- Felipe
  3. ~~Effectively start to port the Free Pascal Runtime Library~~ \- Felipe
  4. Convert some c++ symbian applications to check for errors on the port - Felipe - 1 trivial done, more to go
  5. Complete the port of Free Pascal Runtime Library
  6. Start developing with UIQ 2 SDK, and make adjustments for everything to work with it
  7. Make sure everything works on the real Phone with UIQ 2 too



## Versions Roadmap

The first target will be UIQ 3.0 for the Symbian OS on x86 architecture (the emulator). 

Next will be the UIQ 2 for x86 emulator, and last the UIQ 2 real device. 

## Compiling Free Pascal for the Emulator

1 - Download the latest FPC from Subversion. Make sure you also have the latest stable FPC installed. 

2 - Next you will need a assembler compatible with the Code Warrior linker. Currently this is the GNU Assembler. It´s a good coincidence that the emulator uses normal win32 PE executables, but of course it could use any other format, so FPC will expect a cross-assembler with the correct name. Because of this we can simply copy the as.exe file that comes with FPC releases and rename it. 

Suppose your win32 gnu assembler is located at: C:\Programas\lazarus20\fpc\2.0.4\bin\i386-win32\as.exe 

You should make a copy of it with this name: C:\Programas\lazarus20\fpc\2.0.4\bin\i386-win32\i386-symbian-as.exe 

3 - Now, open a Windows Command Line session. The following batch script will execute a full compilation of FPC for the emulator. In this particular case lazarus was installed on C:\Programas\lazarus20 and the fpc 2.1 source code is on C:\Programas\fpc21 
    
    
    PATH=C:\Programas\lazarus20\fpc\2.0.4\bin\i386-win32
    cd c:\Programas\fpc21
    cd compiler
    make i386
    cd ..
    cd rtl
    cd symbian
    make FPC=C:\Programas\fpc21\compiler\ppc386.exe
    

4 - Building the RTL only is not enougth to compile a Symbian OS application. We also need the c++ bindings which connect our RTL to the Symbian libraries. 

4.1 - To build the bindings you will need the helper application called mksymbian. This application is included with the Free Pascal sources on the directory utils/mksymbian 

4.2 - Next compile mksymbian. There is a lazarus project to help building it, but directly calling the compiler from command line works just as well, like this: 
    
    
    fpc mksymbian.pas
    

4.3 - Copy the mksymbian executable to the fpc/rtl/symbian/bindings directory, open a console, go to the fpc/rtl/symbian/bindings directory and type this command: 
    
    
    mksymbian bindings
    

The resulting .o file(s) will be located on: C:\Programas\fpc21\rtl\units\i386-symbian 

5 - Next you can go to the session "Building Symbian OS Applications" below, to learn how to use your compiler to generate a Object Pascal software for Symbian. 

The Symbian OS RTL will be located at: C:\Programas\fpc21\rtl\units\i386-symbian 

## Compiling Free Pascal for the real device

Not yet implemented. 

  


## Building Symbian OS Applications

### Overview

Building an application on Symbian OS requires some learning, because there are several unique things about Symbian which must be utilized even the most simple applications. For example the UIDs, the need to register a application on the Emulator, etc. 

The UIQ SDK has a very complex build system composed of several ten thousends lines of Perl code, batch files and Makefiles. It induces the software to have a specific directory structure, which is very bad to port existing software, as well as to write cross-platform software. Because cross-platform is a strong point on Free Pascal, we decided to simplify and improve the process when writing our build system for Symbian. We created a external build utility, called mksymbian, written in Pascal that auxiliates the build process. 

### Guide for building applications for the Emulator

You need first to write a .ini file that will contain Symbian OS specific information. Here is a example file called QPasHello.ini 
    
    
    [Main]
    EXENAME=QPasHello.exe
    Language=Pascal
    ProjectType=EXE
    SDK=UIQ
    SDKVersion=3
    Emulator=1
    
    [FPC]
    CompilerPath=C:\Programas\fpc21\compiler\ppc386.exe
    AssemblerPath=C:\Programas\lazarus215\fpc\2.1.5\bin\i386-win32\as.exe
    RTLUnitsDir=C:\Programas\fpc21\rtl\units\i386-symbian\
    
    [UIDs]
    UID2=0x100039CE
    UID3=0xE1000002
    
    [Files]
    mainsource=QPasHello.pas
    mainresource=QPasHello_reg.rss
    
    [Objects]
    file0=qpashello.o
    

And the respective QPasHello.pas source file: 
    
    
    {
     QPasHello.pas
    
     *****************************************************************************
     *                                                                           *
     *  This demonstration program is public domain, which means no copyright,   *
     * but also no warranty!                                                     *
     *                                                                           *
     *  This program is distributed in the hope that it will be useful,          *
     *  but WITHOUT ANY WARRANTY; without even the implied warranty of           *
     *  MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.                     *
     *                                                                           *
     *****************************************************************************
    
     This application will create a popup message 'My Hello World' and then quit
    
     The message will last a few seconds.
    
     Author: Felipe Monteiro de Carvalho
    }
    program QPasHello;
    
    uses symbian;
    
    begin
      User_InfoPrint('My Hello World');
    end.
    

And also the main resource file QPasHello_reg.rss: 
    
    
    // QHelloWorld_reg.rss
    #include <AppInfo.rh>
    UID2 0x100039CE
    UID3 0xE1000002
    RESOURCE APP_REGISTRATION_INFO
    {
      // filename of app binary (minus extension)
      app_file = "QPasHello";
    }
    

Next use mksymbian to build your application, like this: 
    
    
    mksymbian build QPasHello.ini
    

If something goes wrong, check if mksymbian located your directories correctly: 
    
    
    mksymbian showpath
    

Also make sure to correct the paths according to your system on the ini file. 

### Using mksymbian

The syntax of this tool is: 

mksymbian [action] [project file] 

Action can be one of the following: 

  * build - Compiles a project
  * bindings - Compiles the pascal bindings for Symbian OS. It supposes that the necessary files are on the location where the command is executed.
  * showpath - Shows the paths found for the Symbian SDKs and Free Pascal. Utilized to check if the tool was able to find the tools.



The project file is a .ini file with many symbian os specific informations about a project, like the UIDs 

Target can be one of the following: 

  * WinEmulator - Builds for the x86 emulator
  * ArmDevice - Builds a binary for use on PDAs and Smartphones



Note: The tool parameters are not case-sensitive 

## Screenshots

First pascal symbian os UIQ 3 application: 

[![First pascal symbian app.PNG](https://wiki.freepascal.org/images/0/0a/First_pascal_symbian_app.PNG)](</File:First_pascal_symbian_app.PNG>)

## See Also

  * [Programming in Symbian OS](<Programming_in_Symbian_OS.md> "Programming in Symbian OS")
  * [Symbian OS Internals](<Symbian_OS_Internals.md> "Symbian OS Internals")



## External Links

  * <http://developer.uiq.com/> \- UIQ version 3 and superior developer community, with SDK download, Forum, Documentation and News.
  * [[1]](<http://www.symbian.com/developer/techlib/v70sdocs/doc_source/reference/cpp/libc/index.html>) docs for the POSIX layer
  * [[2]](<http://www.symbian.com/developer/techlib/v70sdocs/doc_source/devguides/cpp/base/CStandardLibrary/DesignSTDLIB.html>) a bit of description for the POSIX layer
  * [[3]](<http://search.cpan.org/~rgarcia/perl-5.9.4/README.symbian>) \- Compiling perl for symbian. They mainly use the POSIX libraries...
  * [[4]](<https://developer.uiq.com/forum/entry.jspa?externalID=31>) \- List of the UIQ Phones and the UIQ SDK version to use.
  * [[5]](<https://developer.uiq.com/forum/kbclick.jspa?categoryID=21&externalID=26&searchID=101700>) \- APIs changed from UIQ 2.1 to UIQ 3
  * [[6]](<https://developer.uiq.com/forum/kbclick.jspa?categoryID=21&externalID=20&searchID=101700>) \- How to port my application from UIQ 2.x to UIQ 3

---

_Source: [https://wiki.freepascal.org/SymbianOS](https://web.archive.org/web/20250324153953/https://wiki.freepascal.org/SymbianOS)_
