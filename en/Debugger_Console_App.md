# Debugger Console App

## Contents

  * 1 Introduction
  * 2 Windows
  * 3 Linux
    * 3.1 Lazarus 0.9.30 / 0.9.30.2
    * 3.2 Lazarus 0.9.31 / 1.0 and higher
  * 4 Mac & Linux
    * 4.1 Redirect output to file
    * 4.2 Redirect input and output to a pty (terminal)
      * 4.2.1 Debugger startup options
    * 4.3 gdbserver
    * 4.4 Lazarus 0.9.31
    * 4.5 Lazarus 2.x with LLDB on macOS aarch64
  * 5 See also



## Introduction

This page contains information on how to debug console applications in Lazarus. That is where to find the output (writeln) of your app, and where to type the input (readln). 

This page was written for Lazarus 0.9.30 and above. 

## Windows

On Windows, the application will open its own console window, just as it would outside the debugger. There is no need to do anything special apart from adding a `ReadLn;` before the application terminates so that the console window does not automatically close until you press `↵ Enter`. 

## Linux

### Lazarus 0.9.30 / 0.9.30.2

Please see the notes in the#Mac section. 

### Lazarus 0.9.31 / 1.0 and higher

You can open a console window from the menu: "View" => "Debug Windows" => "Terminal Output" 

It shows the raw output of your application. Note it has no support for any escape sequences, as some terminals may have. Anything you type into this window will be sent to your application. 

  


## Mac & Linux

Unfortunately there is currently no built-in support for debugging console applications. 

Console applications do not have their own terminal, but use the terminal they were started from (the terminal of the shell they were started from). When started from within Lazarus, there is no such terminal. You can specify a "starter application" (like xterm) under menu "Run" => "Run parameters". however this only works if you start your application without a debugger. **This does not work for debugging.**

There are a couple of workarounds. 

### Redirect output to file

In menu "Run" => "Run parameters": specify in "command line parameters": 
    
    
     >/somedir/somefile
    

Then outside Lazarus open a shell and run 
    
    
    tail -f /somedir/somefile
    

### Redirect input and output to a pty (terminal)

Open a terminal outside of Lazarus and run 
    
    
    pty
    

On Ubuntu 18, the command is 
    
    
    tty
    

It should return something like 
    
    
     /dev/pty/2
    

The exact value depends on the OS and the number of terminals already open. 

Note
    The returned value is only valid as long as the terminal stays open, so if you open a new terminal next time you debug in Lazarus, you need to adapt all settings to the new value.

In menu "Run" => "Run parameters": specify in "command line parameters": 
    
    
     >/dev/pty/2 </dev/pty/2
    

If you want to use the terminal to send input to your application, then you may need to stop the shell in it from reacting to the input (unless you started a terminal without a shell). Run something like 
    
    
    tail -f /some/empty/file/that/does/not/change
    

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** This means you still can't use ctrl-c in the input.

#### Debugger startup options

In the global options dialog, on the debugger page, you can specify parameters given to **gdb** (do _**not**_ use the in/output redirect **>** or **<** here, which is meant for interactive **bash** but use something like **\--args**). 

It may be possible to specify the **pty** here, but there is currently no further info. 

### gdbserver

Use the gdbserver based debugger. You can start gdbserver on the same computer as the IDE. Run gdbserver inside the terminal that you want to use for your stdout/stdin 

### Lazarus 0.9.31

In the global options dialog, on the debugger page is an entry "ConsoleTty". You can enter the result of "pty" here. Then you do not need to specify any **<** or **>** redirects. 

### Lazarus 2.x with LLDB on macOS aarch64

In the global options dialog, on the debugger page there is a "LaunchNewTerminal" option when **LLDB** is selected (currently the only option for this platform). 

However if you tick that without making the following prerequisites, **lldb** (and thus Lazarus) will freeze. 

1, First of all, check that your **terminal shell** is listed in the **/etc/shells** file. 

You can check your current shell by this command: 
    
    
    echo $SHELL
    

And list the current contents of this file by this command: 
    
    
    cat /etc/shells
    

If you installed your shell e.g. via Homebrew, your shell is probably not on the list. In this case you can add it manually, to make the following commands will work. 

If you have to edit this file, **be extremely cautious** , and do it **on your own risk**! You have been noticed! 

  
2, (Optional step) If you use different terminal emulator than the default one, you can test the following command. 
    
    
    osascript -e "tell application \"Terminal\" to do script \"echo lazarus\""
    

The system protection dialog should pop-up, about do you allow xy application to control the terminal. You should answer yes, and check that the freshly popped-up built-in terminal app writes "lazarus". 

  
3, Open Lazarus. Define a new external tool in the Tools menu, with the program of **/bin/sh** and the following parameters. 
    
    
    -c 'osascript -e "tell application \"Terminal\" to do script \"echo lazarus\""'
    

After you've added the tool, restart Lazarus, and click on it. The same system protection dialog will pop up, but for Lazarus now. Say yes, to allow it. 

  
After doing these prerequsities, you can safely tick the "LaunchNewTerminal" option in the global settings, and every time you start a debugging, you can control stdin/stdout of the debugged program in a separate terminal window. 

  
Note: Debugging will be really, really, really slow, unless you add the dedicated Debug/Release modes to your project, and use the Debug one for debugging. Use Dwarf2 symbols at the moment when possible. 

## See also

  * [ General tips/info on debugging](<GDB_Debugger_Tips.md> "GDB Debugger Tips")

---

_Source: [https://wiki.freepascal.org/Debugger_Console_App](https://web.archive.org/web/20250317191957/https://wiki.freepascal.org/Debugger_Console_App)_
