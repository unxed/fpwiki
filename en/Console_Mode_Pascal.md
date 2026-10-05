# Console Mode Pascal

│ **English (en)** │  **[русский (ru)](<../ru/Console_Mode_Pascal.md>)** │

## Contents

  * 1 Console-Mode Pascal Programming
    * 1.1 Console mode programming without Lazarus
    * 1.2 Console mode programming with Lazarus
      * 1.2.1 Project type: Console Application
      * 1.2.2 Project type: Program
    * 1.3 Pascal scripts
  * 2 Running a Program
    * 2.1 Compile
    * 2.2 Run in Console
    * 2.3 Run in IDE
    * 2.4 Run in IDE with redirected output
  * 3 Unicode (UTF8) output
    * 3.1 Output CJK text in Windows console
  * 4 Examples
    * 4.1 Execute shell command
    * 4.2 Example: update FPC and Lazarus
  * 5 See also



## Console-Mode Pascal Programming

Many of us were writing [Pascal](<Pascal.md> "Pascal") [programs](<Program.md> "Program") long before [Graphical User Interfaces](<Graphical_User_Interface.md> "Graphical User Interface") (GUIs) and [Integrated Development Environments (IDEs)](<IDE.md> "IDE") became fashionable. Many others are beginners at Pascal programming and need to be able to try out the basic tools of the language. Still others need to write [console](<Command-line_interface.md> "Command-line interface")\- or [text-mode](<Text_User_Interface.md> "Text User Interface") applications in order to perform complex system control tasks. 

### Console mode programming without Lazarus

Even though many prefer to use the [Lazarus IDE](<Lazarus_IDE.md> "Lazarus IDE") to write console mode programs, you can also write them with any text editor and compile them yourself by calling the [FPC](<FPC.md> "FPC") [compiler](<Compiler.md> "Compiler"), e.g. this for a program in example.pas: 
    
    
    fpc example.pas
    

Also, the text mode _fp_ IDE (looks a bit like old [Turbo Pascal](<Turbo_Pascal.md> "Turbo Pascal")) is available. In the next sections we will focus on programming with [Lazarus](<Lazarus.md> "Lazarus"). 

### Console mode programming with Lazarus

Lazarus provides an ideal environment for learning Pascal, and for developing text-mode programs. All the functionality of the Integrated Development Environment can be used, including the Source Editor with its [syntax highlighting](<Syntax_highlighting.md> "Syntax highlighting"), access to the libraries, complex searching and code completion tools, and syntax checking. If you do not want a [Form](<TForm.md> "TForm") with its visual components, you do not need one, but the Lazarus Source Editor is still a great environment for program development. You can compile and run your program during development, without ever leaving the Editor. 

To start a console-mode program, go to the Main Menu and select Project -> New Project and then select either **Program** or **Console Application**. The IDE will not generate all the extra files associated with a full graphic application, and will not open an Object Inspector box, but will open the Source Editor with a skeleton program structure and await your programming input. 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** Under Windows a GUI application has no console and thus can not **writeln** or **readln**. You will get _File not open_ errors. Disable under _Project Options / Compiler Options / Linking / Target specific options / Win32 GUI application_ to create a console application. In Lazarus IDE 1.4, this is under _Project(menu) / Project Options / Compiler Options / Config and Target / Target specific options / Win32 GUI application_.

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** Lazarus has some functionality for command line programs, such as [copyfile](<CopyFile.md> "CopyFile"). To use these, add a project requirement for LazUtils, which will not pull in the entire LCL. Then add the relevant unit to your uses clause.

#### Project type: Console Application

Selecting this project type in Lazarus creates a program with a new class derived from TCustomApplication. TCustomApplication provides a lot of the common things and makes programming command line utilities easy. For example checking command line options, writing help, checking [environment variables](<Command_line_parameters_and_environment_variables.md> "Command line parameters and environment variables") and [exception](<Exceptions.md> "Exceptions") handling. All [LCL](<LCL.md> "LCL") programs use this automatically. 

#### Project type: Program

As a demonstration, we will write a very minimalistic Pascal program. In the IDE, select the 'Program' project type and let the IDE helps you a bit. For example when you add another [unit](<Unit.md> "Unit") the IDE will automatically add the unitname to the program [uses](<Uses.md> "Uses") section. This behavior is defined in the project options. So you can go between 'Program' and 'Custom Program' at any time. 

An example for complete beginners: 
    
    
    program Project1;
    {$mode objfpc}{$H+}
    uses
      {$IFDEF UNIX}{$IFDEF UseCThreads}
      cthreads,
      {$ENDIF}{$ENDIF}
      Classes;
    begin
      WriteLn('Hello World!');
      ReadLn;
    end.
    

### Pascal scripts

Additionally, it is possible to write scripts that are dynamically compiled by [InstantFPC](<InstantFPC.md> "InstantFPC"), a cross-platform solution to run (small) Pascal programs as scripts. The shebang 
    
    
    #!/usr/bin/env instantfpc

permits to start the script like a stand-alone program. 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** Though it is used to run Pascal scripts, InstantFPC uses regular Object Pascal, not the PascalScript script language. So "**PascalScript** " and **Pascal script** are not the same.

## Running a Program

#### Compile

When you have finished your program (or program fragment) you can compile and run it by selecting Run -> Build (or Quick Compile) from the Main Menu or clicking on the Green (Run) triangle symbol [![Start.png](https://wiki.freepascal.org/images/b/b8/Start.png)](</File:Start.png>) in the Button Bar. Any compiler messages (warnings, progress reports or error messages) will appear in the Message Box, and hopefully there will eventually be a message to say 

'Project "Project1" successfully built.:)'. 

But where is the program??!! 

If you have _not_ yet saved the program, then the IDE put it into the _temporary_ directory (e.g. /tmp under linux, C:\temp under windows, see Environment Options / Files / Directory for building test projects). 

If you already saved the project, then the program was created in the same directory, where you saved the project1.lpi file. 

#### Run in Console

You can execute the program by going to a console (terminal) window, then use **cd** to change to the directory and typing the name of the program. On Unix/Linux, if it is in the current directory you will have to start with ./ as the program will probably not be found in the PATH. 

Example for Linux/Unix where a program is stored in /tmp: 
    
    
    cd /tmp 
    ./Project1
    

However, it can be very inconvenient to keep skipping out of the Lazarus Editor and into a terminal window and back again. Fortunately, there is a mechanism that allows a terminal window to be opened from within the Lazarus environment. 

#### Run in IDE

In internal console: show in the terminal output window. 

In redirect output. 

From the Main Menu, select Run -> Run Parameters, then check the box for "Use launching application". The first time you do this and try the Compile/Run sequence, you will probably get a rude message saying: 
    
    
    "xterm: Can't execvp /usr/share/lazarus//tools/runwait.sh: Permission denied".  
    

If this happens, you need to change the permissions on the appropriate file (for example using chmod +x filename, or using the Windows utility for changing permissions); you might have to do this as root. After this, each time you launch your program, a console box will appear and all your text I/O (readln, writeln etc) will appear in it. 

After your program has finished execution, a message "Press enter" appears on the screen. Thus any output your program generated will remain on the screen until you have had a chance to read it; after you press 'enter' the console window closes. 

On Linux, if the above message "xterm: Can't execvp /usr/share/lazarus//tools/runwait.sh: Permission denied" does not appear after checking the box for "Use launching application", type `/usr/bin/xterm $(TargetCmdLine)` and check `Use launching application` (xterm is installed on almost all Linux systems). 

Unfortunately, this method does not allow you to use the integrated debugger. 

#### Run in IDE with redirected output

If you want to see what is written to stdout and want to use the integrated debugger as well, stdout can be redirected to a file with the following code: 
    
    
    uses
      baseunix; 
    
    var
      OutputFile: text;
    
    begin     
      Assign(OutputFile, 'Output.txt');
      if FileExists(Filename) then begin
        Append(OutputFile);
      end
      else begin
        Rewrite(OutputFile);   { open file for writing, destroying contents, if any }
      end;
    
      ResultCode := fpdup(OutputFile, output);
    
      if ResultCode < 0 then begin
        raise Exception.CreateFmt('dup failed: %s', [ResultCode]);
      end;
      Close(OutputFile);
    end.
    

Output.txt can then be viewed with 'tail -f output.txt' or viewed with an editor if 'tail' is not available on your [os](<Operating_System.md> "Operating System"). 

Alternatively, using Lazarus 0.9.31 or up (on Linux only): In the "View" menu, under "Debug Windows" there is an entry for a "console output" to view stdout 

## Unicode (UTF8) output

If you want your console mode program to show Unicode ([UTF8](<UTF-8.md> "UTF-8")) output on Windows Vista and higher (and perhaps on earlier versions, too), you can use the **SetConsoleOutputCP** command to set the console to the UTF8 character set. 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** you will need to make sure the font of the console can show the letters you want to output (e.g. Greek, Cyrillic, Korean).

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** you will need to include the Windows unit.

See [LCL Unicode Support](<LCL_Unicode_Support.md> "LCL Unicode Support") for more details on Unicode support in Lazarus and FPC. 

Example program: 
    
    
    program uniconsole;
    
    {$mode objfpc}{$H+}
    {$APPTYPE CONSOLE}
    
    uses
      {$IFDEF UNIX}
        {$IFDEF UseCThreads}
        cthreads,
        {$ENDIF}
      {Widestring manager needed for widestring support}
      cwstring,
      {$ENDIF}
      {$IFDEF WINDOWS}
      Windows, {for setconsoleoutputcp}
      {$ENDIF}
      Classes
      ;
    
    var
    UTF8TestString: string;
    
    begin
    {$IFDEF WINDOWS}
    SetConsoleOutputCP(CP_UTF8);
    {$ENDIF}
    UTF8TestString:= 'rosé, водка and ούζο';
    writeln ('plain: ' + UTF8TestString);
    {Apparently we do not need UTF8ToConsole for this
    UTF8ToConsole did not do anything for me in fact.}
    end.
    

### Output CJK text in Windows console

This example from forum member **Thaddy** shows how to properly setup Windows console to support CJK text. 
    
    
    {$mode delphiunicode}{$H+}
    {$codepage utf8}
    uses Windows, SysUtils;
    var
      s3, s1, s2: string;
      var
      cfi: CONSOLE_FONT_INFOEX;
    begin
      cfi.cbSize := SizeOf(cfi);
      cfi.nFont := 0;
      cfi.dwFontSize.X := 0;
      cfi.dwFontSize.Y := 20;
      cfi.FontFamily := FF_DONTCARE;
      cfi.FontWeight := FW_NORMAL;
      // two fonts that support Chinese etc.
      cfi.Facename := 'NSimSun';
      // cfi.FaceName := 'SimSun-extB'; // this is a raster font.
    
      SetTextCodePage(Output, CP_UTF8);
      SetConsoleOutputCP(CP_UTF8);
      SetCurrentConsoleFontEx(GetStdHandle(STD_OUTPUT_HANDLE), False, @cfi);
    
      s1 := '春夏';
      s2 := '秋冬';
      s3 := s1+s2;
      Writeln('s1=', s1,'  s2=', s2, '  s3=', s3);
      Readln;
    end.
    

## Examples

You can use the Lazarus editor to try out all the examples in the standard Pascal textbooks, or you can write your own. Some of the most useful procedures are those for executing system commands or for running other programs (whether written in Pascal, C or Perl, or shell or batch scripts). 

#### Execute shell command

Here is an example for a _Custom Program_ or _Program_... that is Linux/Unix/macOS specific though: 
    
    
    program TryShell;
    uses 
      Classes, Unix;
    var 
      S: LongInt;
    begin
      S := fpsystem('/bin/ls -la *.p*'); //lists .pp, .pas, .php, .png etc in current directory
      WriteLn('Program exited with status : ', S)
    end.
    

#### Example: update FPC and Lazarus

Rather more complex commands can be executed. For example, if you have already checked out the SVN repositories for FPC and Lazarus (see [buildfaq](<buildfaq.md> "buildfaq")) you could keep your FPC and Lazarus source files up-to-date by retrieval from the SVN repository with the following sequence of calls: 
    
    
    program LazUpdate;
    uses
      Classes, Unix;
    var 
      S: LongInt;
    begin 
      S := fpsystem('cd /usr/local/src/fpc/devel/fpc        ; make clean');
      S := fpsystem('cd /usr/local/src/fpc/devel/lazarus  ; make clean');
      S := fpsystem('cd /usr/local/src/fpc/devel             ; svn update fpc       >& ~/cvscheckout.log');
      S := fpsystem('cd /usr/local/src/fpc/devel             ; svn update lazarus >& ~/cvslaz.log'        );
    end.
    

Note that issuing the command 
    
    
    fpsystem('cd /somedirectory');
    

followed by 
    
    
    fpsystem ('do something in that subdirectory');
    

does not work, because after each **fpsystem()** [function](<Function.md> "Function") call the execution of the program returns to the directory it started in; so we need to include multiple statements per line within our calls to shell via **fpsystem()**. [Rather: it always starts in . ] 

One does not have to enter every command as a separate line of Pascal; one could create a **bash** script file like this (from [buildfaq](<buildfaq.md> "buildfaq")): 
    
    
    #!/bin/sh
    cd /usr/local/src/fpc/devel
    cd fpc
    make clean
    cd ..
    cd lazarus
    make clean
    cd ..
    svn up fpc >& ~/cvscheckout.log
    svn up lazarus >& ~/cvslaz.log
    

Name it **updatelaz.sh** , and then instead of using **bash** to execute it directly, call it from a Pascal program thus: 
    
    
    (* This can be compiled or used as a Pascal script *)
    program LazUpdate1;
    uses 
      Classes, Unix;
    var 
      S: LongInt; 
    begin
      S := fpsystem('updatelaz.sh')
    end.
    

## See also

  * [Command line parameters and environment variables](<Command_line_parameters_and_environment_variables.md> "Command line parameters and environment variables")
  * [Executing External Programs](<Executing_External_Programs.md> "Executing External Programs")
  * [ Debugging console applications](<Debugger_Console_App.md> "Debugger Console App")
  * [InstantFPC](<InstantFPC.md> "InstantFPC")

---

_Source: [https://wiki.freepascal.org/Console_Mode_Pascal](https://web.archive.org/web/20250122172501/https://wiki.freepascal.org/Console_Mode_Pascal)_
