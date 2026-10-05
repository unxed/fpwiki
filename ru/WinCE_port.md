# WinCE port

[![WinCE Logo.png](https://wiki.freepascal.org/images/8/86/WinCE_Logo.png)](</File:WinCE_Logo.png>)

Эта статья относится только к [Windows CE](</Category:WinCE> "Category:WinCE").

См. также: [Multiplatform Programming Guide](<../en/Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

│ **[English (en)](<../en/WinCE_port.md> "WinCE port")** │  **[italiano (it)](</WinCE_port/it> "WinCE port/it")** │  **[português (pt)](</WinCE_port/pt> "WinCE port/pt")** │  **русский (ru)** │  **[中文（臺灣） (zh_TW)](</WinCE_port/zh_TW> "WinCE port/zh TW")** │    
****

Портирование на WinCE вполне выполнимо. Начат и закреплён сей процесс был Юрием Сидоровым. Oliver (Oro06) портировал заголовочные файлы WinCE API. 

## Contents

  * 1 Статус
  * 2 Скачать кроссплатформенный компилятор
  * 3 Сборка WinCE cross compiler из исходников
  * 4 Компиляция тестового проекта
  * 5 Debugging WinCE applications
  * 6 Building FPC libraries
  * 7 Documentation
    * 7.1 some WinCE notes
    * 7.2 WinCE port notes
  * 8 See Also
  * 9 Links
  * 10 Contacts



## Статус

  * FPC 2.2.0 или позже поддерживает целевую платформу WinCE.
  * Процессоры: платформы WinCE: 
    * [ARM](<../en/ARM.md> "ARM") поддерживается.
    * [i386](</index.php?title=i386&action=edit&redlink=1> "i386 \(page does not exist\)") Поддержка недостаточно протестирована и содержит ошибки. Исправления приветствуются.
  * Поддерживаются следующие версии WinCE: 
    * Устройства на WinCE 3.0 или позднее
    * Pocket PC 2002 – версия WinCE: 3.0
    * Pocket PC 2003 – версия WinCE: 4.20
    * Pocket PC 2003 Second Edition – версия WinCE: 4.21
    * Windows Mobile 5 – версия WinCE : 5.0
    * Windows Mobile 6 – версия WinCE : 5.2
    * Windows Mobile 6.5 - версия WinCE : 5.2.1
  * юниты [RTL](<http://community.freepascal.org:10000/docs-html/rtl>) и [FCL](<http://community.freepascal.org:10000/docs-html/fcl>) работают.



## Скачать кроссплатформенный компилятор

Наиболее лёгкий способ начать разрабатывать для WinCE - загрузить свежую версию FPC под Win32 и arm-wince cross compiler for Win32. 

Скачать можно тут: <http://www.freepascal.org/download.var>

Сначала установите FPC for Win32, затем установите arm-wince cross compiler как add-on пакет. 

Если же нужен ещё и лазарус, то качайте вот отсюда оба: <http://sourceforge.net/projects/lazarus/files/Lazarus%20Windows%2032%20bits/Lazarus%200.9.28.2/>

Установите оба в одну папку, сначала win32 потом cross-compiler. 

## Сборка WinCE cross compiler из исходников

During the tutorial some paths will be suposed to demonstrate the build process. Just substitute those paths with the ones on your system. 

This tutorial demonstrates how to build **arm-wince** crosscompiler. **i386-wince** crosscompiler can be build similar way. Just substitute **arm** to **i386**. Also binutils are not needed for i386-wince target. 

  
**Step 1** \- Cross binutils 

These are the basic tools necessary to create executables, such as: Linker (ld), Assembler (as), Archiver (ar) (Creates smartlinking .a files), strip and some others. 

You need cross binutils for arm-wince, get them from <ftp://ftp.freepascal.org/pub/fpc/contrib/cross/arm-wince-binutils.zip> for Win32. 

Extract them to some dir in the path on your machine. We will supose that you extracted the cross binutils to: `C:\Programas\arm`

  
**Step 2** \- Cross compiler 

Now you need to build fpc cross compiler for ARM processor. To do that you need fpc 2.2.0 or later sources and working installation of fpc 2.2.0 for Win32. 

You can get the latest fpc source repository from Subversion: <http://www.freepascal.org/develop.html#svn>

We will supose that your Win32 fpc compiler is located here: `C:\Programas\fpc\bin\i386-win32`

And your Free Pascal source code is located here: `C:\Programas\fpc`

  
**Step 3** \- The Build process 

In order to build the cross compiler it is necessary to have a correct `PATH` environment variable. On Windows it is very easy to get a `PATH` crowded with information put by installers. To ensure that your path is correct, create a batch file with the following code: 
    
    
    PATH=C:\Programas\fpc\bin\i386-win32;C:\Programas\arm
    make cycle CPU_TARGET=arm OS_TARGET=wince
    

Put this file on `C:\Programas\fpc\compiler`, open the Windows command line, go to this folder and execute the batch file. 

On the end of the compile you should not see any errors. 

You should have a `ppccrossarm.exe` in `C:\Programas\fpc\compiler` and some `.o` and `.ppu` files in `C:\Programas\fpc\rtl\units\arm-wince`

Now copy those files to your Free Pascal installation. The cross compiler `ppccrossarm.exe` should go to `C:\Programas\fpc\bin\arm-wince` and the units to `C:\Programas\fpc\units\arm-wince`

  
**Step 4** \- Configuration file 

Now you need to create `fpc.cfg` configuration file in `C:\Programas\fpc\bin\arm-wince` folder in order to use `ppccrossarm.exe` easy. 

Create empty `fpc.cfg` file in `C:\Programas\fpc\bin\arm-wince` folder and add the following lines to it: 
    
    
    -Twince
    -FuC:\Programas\fpc\units\arm-wince
    -XParm-wince-
    -FDC:\Programas\arm
    

Finally add `C:\Programas\fpc\bin\arm-wince` and `C:\Programas\fpc\bin\i386-win32` to your `PATH` environment variable. 

**Remember to substitute the paths with the ones on your system.**

**Alternative Step 4** \- Configuration file for hybrid compilation 

If you want to compile your projects with the same "fpc" for both win32 and winCE platforms, you can do the following: 

  * copy ppcrossarm.exe into your fpc/bin/i386-win32 directory
  * edit your fpc.cfg in that directory and add:


    
    
    -FuC:\path_to_fpc\units\arm-wince
    -XParm-wince-
    -FDC:\path_to_arm_binutils
    

**Remember to substitute the paths with the ones on your system.**

Now when you want to compile for WinCE you just have to specify the OS and Arch targets to fpc. 

NOTE: if you use Lazarus this will work flawlessly, but make sure to point lazarus to "fpc.exe" not "ppc386.exe". 

## Компиляция тестового проекта

У меня есть компилятор! Что я могу делать теперь? Дальше следует инструкция по созданию программы hello world. 

**Шаг 1** \- Установкаи настройка эмулятора 

Вам понадобится Windows CE Emulator сконфигурированный с ActiveSync. The ActiveSync is necessary to install PocketCMD, a command line tool to run our hello world software. 

Download the free Pocket PC device emulator from Microsoft. It emulates ARM CPU. <http://msdn.microsoft.com/mobility/downloads/Emulator/default.aspx>

There is a tutorial on how to set up the Emulator for ActiveSync [here](<http://www.pocketpcfaq.com/faqs/5.0/emulator.htm>). 

**Step 2** \- Installing the command line 

Download and install **PocketCMD** by SymbolicTools. Get it [here](<http://www.symbolictools.de/public/pocketconsole/applications/PocketCMD>) (this site seems to be gone) but try here [[1]](<http://www.mobile2day.de/platform_wince/device/CASIO/Cassiopeia+E-115/download.html?file=c112516.exe&prod=14571>)

To enable the console on newer devices (I needed it in my 6.1CE pro), you may to set the following registry value [link](<http://forum.xda-developers.com/showthread.php?t=275019&highlight=console>)
    
    
    HKEY_LOCAL_MACHINE\Drivers\Console\OutputTo -> 0
    

Check the  WinCE port notes section bellow for more details on command lines on Pocket PC. 

**Step 3** \- Compiling 

A example file to compile: 
    
    
    program test;
    
    {$apptype console}
    
    var
      Str: string;
    begin
      WriteLn('Software Developed with:');
      WriteLn('The Free Pascal Compiler');
      WriteLn('');
      WriteLn('Please, enter your name:');
      ReadLn(Str);
      WriteLn('Your name is: ' + Str);
    end.
    

Sample command line to compile `test.pas`: 

`ppcrossarm test.pas`

You will get `test.exe` executable file. Copy it to your Windows CE device and run. 

[Here](<http://wiki.lazarus.freepascal.org/index.php/Windows_CE_Interface#Screenshots>) is a screenshot of a software created this way. 

## Debugging WinCE applications

GDB can be used to debug your WinCE applications remotely via ActiveSync. Download GDB 6.4 for Win32 host and arm-wince target here: <ftp://ftp.freepascal.org/pub/fpc/contrib/cross/gdb-6.4-win32-arm-wince.zip>

**Some hints:**

  * Pass `--tui` parameter to GDB to enable TUI interface which makes debugging more comfortable.
  * Use unix line endings (LF only) in your pascal source files. Otherwise GDB will show sources incorrctly.



**How to use:**

First, make ActiveSync connection to your Pocket PC device. 

Then launch gdb: 

`gdb --tui <your_executable_at_local_pc>`

On gdb prompt type: 

`run` or just `r`

GDB will copy your executable to the device in `\gdb` folder and run it. 

Here is a short list of most needed GDB commands: 

  * `r args` \- run program with args arguments.
  * `s` \- step into.
  * `n` \- step over.
  * `ni` \- step over instrument.step over assembly instruction.
  * `c` \- continue execution.
  * `br <function_name>` \- set a breakpoint at `function_name`. Use `PASCALMAIN` to set a breakpoint at program start.
  * `br <source_file>:<line_number>` \- set a breakpoint at specified source line.
  * `disas` \- show disassembly of current location.
  * `x/fmt address` \- dump memory at address with special format.use "help x" for more informations.
  * `bt` \- back trace.print back trace of the call stack.
  * `where` \- Display the current line and function and the stack of calls that got you there.
  * `q` \- Quit gdb.



To learn more how to use GDB read its documentation here: <http://www.gnu.org/software/gdb/documentation>

## Building FPC libraries

If you want to build libraries available with FPC just go to library folder and execute: 
    
    
    PATH=C:\Programas\fpc\bin\i386-win32;C:\Programas\fpc\compiler;C:\Programas\arm
    make OS_TARGET=wince CPU_TARGET=arm PP=ppcrossarm.exe
    

For example you want to build FCL. Go to `fpc\fcl` folder and execute the command above. You will get FCL compiled units in `fpc\fcl\units\arm-wince`. 

## Documentation

### some WinCE notes

  * If you are getting "Uknown error! Error code : 0xc1" and using dlls then the error might be becouse of using some export entries not found in dll file.Check the dll file export section and name of them and how you used them.



### WinCE port notes

  * `**chdir**` procedure always produces an error (WinCE does not support setting of current directory).
  * All file/dir paths must be absolute (started with \\).
  * WinCE is unicode OS. All string parameters to API calls must be PWideChar.
  * WinCE does not have support for environment strings.
  * WinCE does not have support for console applications by default. But you can install console support by yourself. Please note that FPC creates GUI applications for WinCE target by default. To create console application you should use `-WC` compiler switch or put `{$APPTYPE CONSOLE}` directive to source code.  
To enable console in WinCE install one of the following programs: 
    * **PocketCMD** by SymbolicTools. It is recommended solution. Get it [here](<http://www.symbolictools.de/public/pocketconsole/applications/PocketCMD>)
    * **PPC Command Shell** from Microsoft Windows Mobile Developer Power Toys. Get it [here](<http://www.microsoft.com/downloads/details.aspx?FamilyID=74473fd6-1dcc-47aa-ab28-6a2b006edfe9&displaylang=en>)  
PPC Command Shell have less features than PocketCMD. Also it have some issues. One of them - a new console window is opened even if an application is started from a console command prompt.
    * To enable console output in Windows Mobile 5 and 6 set "HKEY_LOCAL_MACHINE\Drivers\Console\OutputTo" to 0.



## See Also

  * [Tested Unstable Revisions](</index.php?title=Tested_Unstable_Revisions&action=edit&redlink=1> "Tested Unstable Revisions \(page does not exist\)")
  * [Windows CE interface for Lazarus](<http://wiki.lazarus.freepascal.org/Windows_CE_Interface>)
  * [Windows CE Development Notes](<http://wiki.lazarus.freepascal.org/Windows_CE_Development_Notes>)
  * [Приложения WinCE с исползованием KOL](<http://wiki.lazarus.freepascal.org/KOL-CE/ru>)



## Links

  * Useful WinCE info <http://www.rainer-keuchel.de/documents.html>
  * Standalone Pocket PC device emulator from Microsoft. It emulates ARM CPU. Get it [here](<http://www.microsoft.com/downloads/details.aspx?FamilyId=C62D54A5-183A-4A1E-A7E2-CC500ED1F19A&displaylang=en>)
  * Mamaich Pocket PC port of GCC <http://mamaich.uni.cc>
  * [Buildfaq](<http://www.stack.nl/~marcov/buildfaq.pdf>) is a general FAQ about how to build and configure FPC.



Here are some links related to ARM CPU Architecture 

  * [ARM Core Developers Forum](<http://www.armcorepro.com/>) Not that much active though.
  * [GCC ARM Improvement Project](<http://www.inf.u-szeged.hu/gcc-arm/>)
  * [ARM ASSEMBLER](<http://www.heyrick.co.uk/assembler/index.html>) Good information and codes related to arm assembly language.
  * [GNU ARM toolchain for Cygwin, Linux and MacOS](<http://www.gnuarm.com/>)
  * [Microsoft Windows CE .NET 4.2 ARM Guide](<http://msdn.microsoft.com/library/default.asp?url=/library/en-us/wcechp40/html/ccrefarmguide.asp>)
  * [ARM Instruction Sets & Programs](<http://soc.csie.ndhu.edu.tw/source/intro_embedded/ch2-arm-2.ppt>) Very good and consice information about arm architecture
  * [The ARM Instruction Set ](<http://web.njit.edu/~baltrush/arm_stuff/ARMInst.ppt>) Another fine power point file about arm



## Contacts

Write any questions regarding WinCE port to [Yury Sidorov](<mailto:yury_sidorov@mail.ru>)

---

_Source: [https://wiki.freepascal.org/WinCE_port/ru](https://web.archive.org/web/20240920213339/https://wiki.freepascal.org/WinCE_port/ru)_
