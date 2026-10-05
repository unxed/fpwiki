# IDE Window: Run parameters

│ **[Deutsch (de)](</IDE_Window:_Run_parameters/de> "IDE Window: Run parameters/de")** │  **English (en)** │  **[français (fr)](</IDE_Window:_Run_parameters/fr> "IDE Window: Run parameters/fr")** │  **[日本語 (ja)](</IDE_Window:_Run_parameters/ja> "IDE Window: Run parameters/ja")** │    
****  
****

## Contents

  * 1 Local
    * 1.1 Host Application
    * 1.2 Command line parameters
    * 1.3 Launching application
    * 1.4 Working directory
    * 1.5 Display
    * 1.6 Default Window/Console position and size
  * 2 Environment
    * 2.1 System variables
    * 2.2 User overrides



## Local

[![IDE Window - Run Parameters.png](https://wiki.freepascal.org/images/7/77/IDE_Window_-_Run_Parameters.png)](</File:IDE_Window_-_Run_Parameters.png>)

### Host Application

If your project executable and the executable to debug is different, you can specify here the other executable. 

Default: leave empty 

### Command line parameters

Add here the command line parameters, with which your project should be started. 

Default: leave empty 

Note: Not used when using Lauching Application. 

### Launching application

If you want to start your project not directly, but via a script on in a terminal or a special profiler or a special debugger specify here the command line and enable the checkbox. Without this you can view output in [internal console](<IDE_Window__Terminal_Output.md> "IDE Window: Terminal Output")

On Linux if you run a console program from Lazarus, it is executed but no console is showing. To see the console program ran from Lazarus similarly to Windows, add `/usr/bin/xterm $(TargetCmdLine)` and check `Use launching application` (xterm is installed on almost all Linux systems). 

### Working directory

Normally the working directory of a debugged project is the executable directory. That means all relative file names are expanded by the working directory. You can override this directory here. 

Note: On OSX, the system forces the current directory to '/' for all GUI applications, so this doesn't do anything 

### Display

To show the application under X (gnome, kde, xfce) on another desktop, specify the Display command line variable. For example to show the application on the second desktop use :1. To show on another computer use 192.168.1.17:0 or hydra:1. 

### Default Window/Console position and size

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** * Lazarus 3.99

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** * Windows Only

Set bounds for the console window (console app). For GUI apps set bounds for first window with Position = poDefault. 

  * Console Window take Left/Top and Columns/Rows.
  * GUI apps take Left/Top and Width/Height.



If Width/Height is set without Left/Top then the OS may assume a Left/Top=0/0. 

## Environment

[![IDE Window - Run Parameters - Environment.png](https://wiki.freepascal.org/images/4/41/IDE_Window_-_Run_Parameters_-_Environment.png)](</File:IDE_Window_-_Run_Parameters_-_Environment.png>)

Normally a debugged program starts with the same environment variables as the IDE itself. 

### System variables

Here you can see, what system variables are currently defined. You cannot change/override them. 

### User overrides

Add environment variable overrides for the debugged program. Do not confuse them with command line parameters or config variables. These variables can be retrieved by the program via such functions as GetEnvironmentVariable, GetEnvironmentVariableCount, Application.GetEnvironmentList.

---

_Source: [https://wiki.freepascal.org/IDE_Window%3A_Run_parameters](https://web.archive.org/web/20250420074834/https://wiki.freepascal.org/IDE_Window%3A_Run_parameters)_
