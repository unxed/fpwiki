# IDE Window: External Tool

│ **English (en)** │

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** External tools are global, not project specific

[![IDE Window - External Tool.png](https://wiki.freepascal.org/images/e/eb/IDE_Window_-_External_Tool.png)](</File:IDE_Window_-_External_Tool.png>)

## Contents

  * 1 Title
  * 2 Program Filename
  * 3 Parameters
  * 4 Working Directory
  * 5 Options
    * 5.1 Scan output for FPC messages
    * 5.2 Show console
    * 5.3 Hide window
    * 5.4 Scan output for make messages
  * 6 Key
  * 7 Macros
  * 8 Examples
    * 8.1 Run cmd.exe
    * 8.2 Run Tortoisesvn to checkout or update your local svn repository



### Title

This name is is shown in the IDE menu. 

### Program Filename

The full path to the tool. For example: 
    
    
     /usr/bin/ppc386
    

### Parameters

The command line parameters. For example: 
    
    
     -l test.pas
    

### Working Directory

The directory, where to start the tool. All relative paths will be relative to this. 

### Options

#### Scan output for FPC messages

Parse the output for FPC messages and jump to errors. 

#### Show console

Only available on MS Windows. Creates a console. Default is false. Since 1.7. 

#### Hide window

Only available on MS Windows. Do not show the application window. Default is true. Since 1.7. 

#### Scan output for make messages

Parse the output for make messages and jump to errors. 

### Key

Define the shortcut for this tool. This is optional. 

### Macros

You can use macros in the programfilename, the parameters and the working directory. 

See [IDE Macros in paths and filenames](<IDE_Macros_in_paths_and_filenames.md> "IDE Macros in paths and filenames"). 

### Examples

#### Run cmd.exe

Choose <Tools><Configure External Tools> from the lazarus main menu and setup the following. 

  * Title: cmd.exe
  * Program Filename: C:\windows\system32\cmd.exe
  * Working Directory: $(ProjPath)
  * Disable all scanners
  * Show console: enable
  * Hide window: disable



#### Run Tortoisesvn to checkout or update your local svn repository

Add a function to checkout svn sources from the repository. This example shows how to do this if you are on windows and have tortoisesvn installed. First create a batch-file in the folder lazarus\tools. (e.g lazarus\tools\checkout_lazarus_win.bat) 

_tortoiseproc /command:checkout /url:"<http://svn.freepascal.org/svn/lazarus/trunk/>" /path:"..\"_

Then choose <Tools><Configure External Tools> from the lazarus main menu and setup the following. 

  * Title: Checkout Lazarus
  * Program Filename: $LazarusDir()\tools\checkout_lazarus_win.bat
  * Working Directory: $LazarusDir()\tools\



If you want to add an additional function to update the Lazarus sources from repository, then you can create another batch-file with the following content. 

_tortoiseproc /command:update /path:"..\"_

---

_Source: [https://wiki.freepascal.org/IDE_Window%3A_External_Tool](https://web.archive.org/web/20250419145709/https://wiki.freepascal.org/IDE_Window%3A_External_Tool)_
