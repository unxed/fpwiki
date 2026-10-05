# Creating a Backtrace with GDB

│ **[Deutsch (de)](</Creating_a_Backtrace_with_GDB/de> "Creating a Backtrace with GDB/de")** │  **English (en)** │  **[日本語 (ja)](</Creating_a_Backtrace_with_GDB/ja> "Creating a Backtrace with GDB/ja")** │  **[русский (ru)](<../ru/Creating_a_Backtrace_with_GDB.md> "Creating a Backtrace with GDB/ru")** │    
****

## Contents

  * 1 Reasons to create a backtrace
    * 1.1 Bug Reports
    * 1.2 Help yourself find a problem
    * 1.3 Just plain fun!!
  * 2 Creating a Backtrace
    * 2.1 Very short how-to
    * 2.2 Directly from the Call Stack window
    * 2.3 Windows Users
    * 2.4 Linux/BSD Users
    * 2.5 macOS
      * 2.5.1 macOS: Graphical application
      * 2.5.2 macOS: Non graphical application, command line program
    * 2.6 GDB ready to accept commands
  * 3 using lldb on Mac
  * 4 Getting the Backtrace out of the terminal and into an email
  * 5 Understanding a Backtrace
  * 6 See Also



## Reasons to create a backtrace

### Bug Reports

Backtraces can be very helpful when submitting bug reports. They will no doubt increase the chances of the bug getting fixed sooner. 

### Help yourself find a problem

A backtrace can really take the pain out of finding a problem with your own program. Often exposing the problem immediately. 

### Just plain fun!!

Okay this might be a stretch, but making backtraces is just plain fun! 

Seriously though, if you can think of another reason to create a backtrace add it here. 

## Creating a Backtrace

Note: **Mac users** should use _lldb_ instead of _gdb_. 

To create a backtrace with [GDB](<GDB.md> "GDB") (Gnu DeBugger): 

  1. You must have the program [GDB](<http://www.gnu.org/software/gdb/gdb.html>). It is available on most *nix installations. If you downloaded the Windows version of fpc or Lazarus, GDB is already included in the installation. In Lazarus it is set up as the default debugger. Look in Tools -> Options -> Debugger -> General Tab. Default path for GDB on Windows is c:\lazarus\mingw\bin\gdb.exe.
  2. GDB must be in the PATH environment variable. If it's not under windows, from the command prompt type (example): set PATH=%PATH%;c:\lazarus\mingw\bin\
  3. The program you wish to debug MUST have debugging info included in the executable. If you have used "strip", "upx", compiled with "Xs" or used some other exe shrinker/compressor this won't work!



### Very short how-to

Linux terminal: 
    
    
     $ cd program_folder
     $ gdb program_name
     run
    

Then AV happens. Type: 
    
    
     bt
    

### Directly from the Call Stack window

To capture a Backtrace from within Lazarus, make sure you compile the program including both options "Generate debug info for GDB" and "Display line numbers in run-time error backtraces (-gl)" and you have properly setup GDB in Debugger Options. 

  1. Run your application until the problem appear
  2. If a dialog should appear alerting about a problem showing a couple of buttons: "Break" and "Continue", click the "Break" button.
  3. Open the Call Stack window (Menu View->Debug Windows->Call Stack
  4. Look a the list of Calls, if necessary press "(+)More"
  5. Press the "Copy All" button.
  6. The Backtrace is now in your clipboard and ready for pasting it.



### Windows Users

1\. Open a MS-DOS prompt 

2\. Change to the drive you have your exe on: 
    
    
    C: {press enter}
    

3\. Change to the directory containing the exe: 
    
    
    cd \myprograms\Project1 {press enter}
    

4\. Load your program with GDB: 
    
    
    gdb Project1.exe {press enter}
    

  


### Linux/BSD Users

1\. Open your favorite Terminal program 

2\. Change to the directory containing the executable: 
    
    
    cd $HOME/myprograms/project1 {press enter}
    

3\. Load your program with GDB: 
    
    
    gdb project1 {press enter}
    

### macOS

GDB is not available by default on macOS. You should use the macOS default [lldb debugger](<Creating_a_Backtrace_with_lldb.md> "Creating a Backtrace with lldb")

#### macOS: Graphical application

1\. If not already done: create the application bundle (Project / Project Options / Application / Create Application Bundle) 

2\. Open your favorite Terminal program 

3\. Change to the directory containing the application bundle / executable: 
    
    
    cd $HOME/myprograms/project1 {press enter}
    

4\. Load your program with GDB: 
    
    
    gdb project1.app/Contents/MacOS/project1 {press enter}
    

#### macOS: Non graphical application, command line program

1\. Open your favorite Terminal program 

2\. Change to the directory containing the executable: 
    
    
    cd $HOME/myprograms/project1 {press enter}
    

3\. Load your program with GDB: 
    
    
    gdb project1 {press enter}
    

### GDB ready to accept commands

You should now see a prompt that looks something like this: 
    
    
    GNU gdb 6.3.50.20050217-cvs
    Copyright 2004 Free Software Foundation, Inc.
    GDB is free software, covered by the GNU General Public License, and you are
    welcome to change it and/or distribute copies of it under certain conditions.
    Type "show copying" to see the conditions.
    There is absolutely no warranty for GDB.  Type "show warranty" for details.
    This GDB was configured as "i686-pc-linux-gnu".
    (gdb) _
    

**Let GDB stop when an exception is raised**
    
    
    (gdb) break fpc_raiseexception {press enter}
    

**Now, run your program:**
    
    
    (gdb) run {press enter}
    

Your program will run normally, until it crashes or causes an error. 

When your program crashes instead of the usual error "Access Violation" or "Division by Zero" your program will just seem to stop, not respond and not repaint itself. This is because gdb has caught the error and is waiting for you to tell it what to do. 

Switch over to your MS-DOS window or Terminal and you should have a screen something like this: 
    
    
    Program received signal SIGFPE, Arithmetic exception.
    0x080733aa in TFORM1__BUTTON1CLICK (SENDER=0x403d8bd8, this=0x403d77d8) at unit1.pas:36
    Current language:  auto; currently pascal
    (gdb) _
    

To create a backtrace now type: 
    
    
    backtrace {press enter}
    

or 
    
    
    bt {press enter}
    

You should now have a screen that looks something like this: 
    
    
    #0  0x080733aa in TFORM1__BUTTON1CLICK (SENDER=0x403d8bd8, this=0x403d77d8) at unit1.pas:36
    #1  0x080ecd78 in TCONTROL__CLICK (this=0x403d8bd8) at control.inc:1768
    #2  0x081274bd in TBUTTONCONTROL__CLICK (this=0x403d8bd8) at buttoncontrol.inc:57
    #3  0x08129171 in TCUSTOMBUTTON__CLICK (this=0x403d8bd8) at buttons.inc:186
    #4  0x08129198 in TCUSTOMBUTTON__WMDEFAULTCLICKED (MESSAGE={MSG = 1031, WPARAM = 
            136821192, LPARAM = -1073747812, RESULT = 1075866658, WPARAMLO = 47560, 
            WPARAMHI = 2087, LPARAMLO = 59548, LPARAMHI = 49151, RESULTLO = 27682, RESULTHI = 16416}, 
            this=0x403d8bd8) at buttons.inc:198
    #5  0x0805bbf7 in SYSTEM_TOBJECT_$__DISPATCH$formal ()
    #6  0x080ec6e0 in TCONTROL__WNDPROC (THEMESSAGE={MSG = 1031, WPARAM = 136821192, 
            LPARAM = -1073747812, RESULT = 1075866658, WPARAMLO = 47560, WPARAMHI = 2087, 
            LPARAMLO = 59548,  PARAMHI = 49151, RESULTLO = 27682, RESULTHI = 16416}, 
            this=0x403d8bd8) at control.inc:1447 
    #7  0x080e4ed9 in TWINCONTROL__WNDPROC (MESSAGE={MSG = 1031, WPARAM = 136821192, 
            LPARAM = -1073747812, RESULT = 1075866658, WPARAMLO = 47560, WPARAMHI = 2087, 
            LPARAMLO = 59548,  PARAMHI = 49151, RESULTLO = 27682, RESULTHI = 16416}, 
            this=0x403d8bd8) at wincontrol.inc:2154
    #8  0x08161955 in DELIVERMESSAGE (TARGET=0x403d8bd8, AMESSAGE=void) at gtkproc.inc:3242
    #9  0x0818897c in GTKWSBUTTON_CLICKED (AWIDGET=0x8285380, AINFO=0x40453794) at gtkwsbuttons.pp:112
    ... { Truncated for simplicity} Complete listing [here](<Creating_a_Backtrace_with_GDB_Backtrace.md> "Creating a Backtrace with GDB\\Backtrace")
    (gdb) _
    

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** If the backtrace is too long you will be prompted to press enter for the next page of the backtrace until it is complete

The information displayed here has the sequence of events up to where the error occured. [ Understanding a Backtrace](<Creating_a_Backtrace_with_GDB.md> "Creating a Backtrace with GDB")

To exit GDB (and your program) you may now type: 
    
    
    quit {press enter}
    

Or to continue running until the next exception type: 
    
    
    cont {press enter}
    

_Hint: At any time while your program is running you may press Ctrl+C (from the Terminal or MS-DOS prompt) to get a "(gdb) _" prompt_

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** The use of an english keyboard layout can prevent problems with gdb (e.g. if you type special characters like ' \ ').

## using lldb on Mac

start lldb 

    lldb

load your project 

    target create project1.app/MacOs/Contents/project1

set a breakpoint You may skip this, if the project crashes, rather than raising an fpc exception) If there are expected exceptions, that will be handled by try except, then use "c" to continue 

    breakpoint set --func fpc_raiseexception

run it 

    r

when it stops, get the backtrace 

    bt

show locals for different frames (replace 1 in frame select, with different frames, e.g. top 3 or top 5) 

    frame select 1
    frame variable -P 1

## Getting the Backtrace out of the terminal and into an email

Currently we have made a backtrace and seen it in the Terminal(or MS-DOS prompt) but how do I get it into an email? There are two ways that I know of: 

  1. Use the copy/paste functionality of your Terminal(or MS-DOS prompt)
  2. Redirect the output of gdb to a file like so:


    
    
    gdb project1 > output.txt
    

Now you can open the file and copy/paste it into your email. 

Caveat: The second method requires that you type blindly. So you would type "run" to start the program and "backtrace" to make a backtrace like normal but nothing would show up on the screen. 

Exception: On Linux you can get around not being able to see with the following command: 
    
    
    touch output.txt & tail output.txt -f & gdb project1 > output.txt
    

## Understanding a Backtrace

The stack contains all calls at the moment when the division by zero error occured. At position #0 is the latest call "button1-click()". subproc-call #37 is the first call of main() of the whole app. 

  
TODO: Write Me :) 
    
    
    (gdb) bt
    #0  0x080733aa in TFORM1__BUTTON1CLICK (SENDER=0x403d8bd8, this=0x403d77d8) at unit1.pas:36
    #1  0x080ecd78 in TCONTROL__CLICK (this=0x403d8bd8) at control.inc:1768
    #2  0x081274bd in TBUTTONCONTROL__CLICK (this=0x403d8bd8) at buttoncontrol.inc:57
    #3  0x08129171 in TCUSTOMBUTTON__CLICK (this=0x403d8bd8) at buttons.inc:186
    #4  0x08129198 in TCUSTOMBUTTON__WMDEFAULTCLICKED (MESSAGE={MSG = 1031, WPARAM = 136821192, 
            LPARAM = -1073747812, RESULT = 1075866658, WPARAMLO = 47560, WPARAMHI = 2087, 
            LPARAMLO = 59548, LPARAMHI = 49151, RESULTLO = 27682, RESULTHI = 16416}, 
            this=0x403d8bd8) at buttons.inc:198
    #5  0x0805bbf7 in SYSTEM_TOBJECT_$__DISPATCH$formal ()
    #6  0x080ec6e0 in TCONTROL__WNDPROC (THEMESSAGE={MSG = 1031, WPARAM = 136821192, 
            LPARAM = -1073747812, RESULT = 1075866658, WPARAMLO = 47560, WPARAMHI = 2087, 
            LPARAMLO = 59548, LPARAMHI = 49151, RESULTLO = 27682, RESULTHI = 16416}, 
            this=0x403d8bd8) at control.inc:1447
    #7  0x080e4ed9 in TWINCONTROL__WNDPROC (MESSAGE={MSG = 1031, WPARAM = 136821192, 
            LPARAM = -1073747812, RESULT = 1075866658, WPARAMLO = 47560, WPARAMHI = 2087, 
            LPARAMLO = 59548, LPARAMHI = 49151, RESULTLO = 27682, RESULTHI =   16416},
            this=0x403d8bd8) at wincontrol.inc:2154
    #8  0x08161955 in DELIVERMESSAGE (TARGET=0x403d8bd8, AMESSAGE=void) at gtkproc.inc:3242
    #9  0x0818897c in GTKWSBUTTON_CLICKED (AWIDGET=0x8285380, AINFO=0x40453794) at gtkwsbuttons.pp:112
    #10 0x401d7877 in gtk_marshal_NONE__NONE () from /usr/lib/libgtk-1.2.so.0
    #11 0x40206e5a in gtk_signal_remove_emission_hook () from /usr/lib/libgtk-1.2.so.0
    #12 0x4020616a in gtk_signal_set_funcs () from /usr/lib/libgtk-1.2.so.0
    #13 0x40204234 in gtk_signal_emit () from /usr/lib/libgtk-1.2.so.0
    #14 0x40176e1c in gtk_button_clicked () from /usr/lib/libgtk-1.2.so.0
    #15 0x4017838a in gtk_button_get_relief () from /usr/lib/libgtk-1.2.so.0
    #16 0x401d7877 in gtk_marshal_NONE__NONE () from /usr/lib/libgtk-1.2.so.0
    #17 0x4020607b in gtk_signal_set_funcs () from /usr/lib/libgtk-1.2.so.0
    #18 0x40204234 in gtk_signal_emit () from /usr/lib/libgtk-1.2.so.0
    #19 0x40176d60 in gtk_button_released () from /usr/lib/libgtk-1.2.so.0
    #20 0x40177d26 in gtk_button_get_relief () from /usr/lib/libgtk-1.2.so.0
    #21 0x401d764d in gtk_marshal_BOOL__POINTER () from /usr/lib/libgtk-1.2.so.0
    #22 0x4020619f in gtk_signal_set_funcs () from /usr/lib/libgtk-1.2.so.0
    #23 0x40204234 in gtk_signal_emit () from /usr/lib/libgtk-1.2.so.0
    #24 0x40239fd9 in gtk_widget_event () from /usr/lib/libgtk-1.2.so.0
    #25 0x401d74d0 in gtk_propagate_event () from /usr/lib/libgtk-1.2.so.0
    #26 0x401d65c1 in gtk_main_do_event () from /usr/lib/libgtk-1.2.so.0
    #27 0x40065b44 in gdk_wm_protocols_filter () from /usr/lib/libgdk-1.2.so.0
    #28 0x4003de75 in g_get_current_time () from /usr/lib/libglib-1.2.so.0
    #29 0x4003e32c in g_get_current_time () from /usr/lib/libglib-1.2.so.0
    #30 0x4003e4f5 in g_main_iteration () from /usr/lib/libglib-1.2.so.0
    #31 0x401d6388 in gtk_main_iteration_do () from /usr/lib/libgtk-1.2.so.0
    #32 0x080b4d26 in TGTKWIDGETSET__WAITMESSAGE (this=0x40473014) at gtkobject.inc:1427
    #33 0x08070073 in TAPPLICATION__IDLE (this=0x4045b014) at application.inc:319
    #34 0x08070ffb in TAPPLICATION__HANDLEMESSAGE (this=0x4045b014) at application.inc:827
    #35 0x08071380 in RUNMESSAGE (parentfp=0xbffff5a4) at application.inc:933
    #36 0x080712d7 in TAPPLICATION__RUN (this=0x4045b014) at application.inc:944
    #37 0x080531c3 in main () at project1.lpr:13
    (gdb)
    

This Backtrace was produced using this code: 
    
    
    30. procedure TForm1.Button1Click(Sender: TObject);
    31. var
    32.  X, Y, Z: Integer;
    33. begin
    34.   X := 0;
    35.   Y := 0;
    36.   Z := X div Y;   //   division by zero error
    37. end;
    

## See Also

  * [Creating a Backtrace with lldb](<Creating_a_Backtrace_with_lldb.md> "Creating a Backtrace with lldb")

---

_Source: [https://wiki.freepascal.org/Creating_a_Backtrace_with_GDB](https://web.archive.org/web/20240909150109/https://wiki.freepascal.org/Creating_a_Backtrace_with_GDB)_
