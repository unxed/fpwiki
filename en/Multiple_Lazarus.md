# Multiple Lazarus

│ **English (en)** │  **[русский (ru)](<../ru/Multiple_Lazarus.md>)** │

## Contents

  * 1 Multiple Lazarus instances
  * 2 Important
  * 3 Who needs multiple Lazarus instances and who does not?
  * 4 The Lazarus directory
  * 5 The config directory
    * 5.1 Solution for separating instances using multiple config dirs
    * 5.2 Using lazarus.cfg file
    * 5.3 Config directory files
  * 6 The Lazarus files
    * 6.1 Linux
      * 6.1.1 Example 1 : A normally installed Lazarus instance plus the git version
      * 6.1.2 Example 2 : A normally installed Lazarus instance plus a manual copy
    * 6.2 Windows
      * 6.2.1 Example 1: GIT and Release
  * 7 Installation of multiple Lazarus
    * 7.1 Windows - Using the installer
      * 7.1.1 Windows - Adding a cross compiler install



## Multiple Lazarus instances

You can have one directory with Lazarus in it on your system or more. These directories can contain the same or different versions of Lazarus and can be made to work independent of each other (see the section dealing with **\--primary-config-dir** below). Because you don't have to install Lazarus and the versions may be the same, the concept of having more than one Lazarus directory on your system will be called "multiple instances" in this article.   
Runthrough for Linux at [DualLaz](<DualLaz.md> "DualLaz"). 

## Important

  * Make Backups of 
    * Your primary config (e.g. ~/.lazarus under Unix)
    * Your projects *.lpi and *.lps



## Who needs multiple Lazarus instances and who does not?

You need multiple Lazarus instances... 

  * to have several versions of Lazarus
  * to have one Lazarus version compiled with several different versions of [FPC](<FPC.md> "FPC")



You don't need multiple instances... 

  * to test your project with different [widgetsets](<Widgetset.md> "Widgetset")
  * to [cross compile](<Cross_compiling.md> "Cross compiling")



## The Lazarus directory

The Lazarus directory (i.e. the directory where the Lazarus application is) is overridable using --lazarusdir, this is useful when running Lazarus from e.g. USB sticks with varying mountpoints or driveletters. 

## The config directory

All IDE options for an instance are saved to a config directory. Under Linux, macOS and BSD the default config directory is ~/.lazarus. Note the beginning dot, which makes it a hidden directory. 

On Windows, settings are saved to the User AppData folder, which maps to a different directory depending on the Windows version (and language, in Windows XP and earlier). See also information here: Windows. Note that you won't see the configuration directory in Windows unless you select to see hidden and system files. To view the directory, type: 
    
    
    %localappdata%
    

in the Start/Run box or in the address bar of Windows Explorer, which should bring you to the Local Application Data directory. For default installs, the settings are stored in the Lazarus directory shown there. 

Operating System | Default location for the config directory | Remarks   
---|---|---  
Linux, macOS, BSD, other UNIXes | ~/.lazarus | Hidden directory   
Windows XP | C:\Documents and Settings\<User name>\Local Settings\Application Data\lazarus | English Windows; others may have different paths   
Windows Vista and higher | C:\Users\<User name>\AppData\Local\lazarus\ | Regardless of selected language   
  
The config directory can **not** be shared between different Lazarus versions. A newer Lazarus can read older configs and will update them automatically. There is no guarantee that an older version can read config files of a newer version. It is always a good idea to make a backup of your configs from time to time for time you might’ve change something weird and not know what it was. 

### Solution for separating instances using multiple config dirs

The config directory can be given on the command line with the --primary-config-path parameter (shorter version: --pcp). For example: 
    
    
    lazarus --primary-config-path=~/.lazarus2
    

will start an IDE using _~/.lazarus2_ as config directory. This way you can have an arbitrary number of config directories and start several independent Lazarus instances. As Lazarus creates the settings files from default if they don't exist, you can specify empty directories if you want. 

If you use this often, then a small shell script can be created: 
    
    
    #!/bin/sh
    /path/to/lazarus --primary-config-path=~/.lazarus2
    

In Windows you cannot use ~ so use instead, for example, a shortcut on the desktop with the following parameters to call Lazarus: 
    
    
    \path\to\lazarus.exe --primary-config-path=lazarusconf
    

This will place the lazarus configuration directory inside the lazarus directory. 

### Using lazarus.cfg file

Another way for setting config directory is using a “lazarus.cfg" file and putting it into the folder with the Lazarus executable and putting this line into it : 
    
    
    --primary-config-path=\path\to\config\folder
    

You can make a config folder in the Lazarus installation path and set it's path to the cfg file. After that Lazarus will use this lazarus.cfg file and there is no need to make a shell script or use shortcut file with the —-pcp parameter. 

For example, for a Windows installation it will be like this : A config folder and a lazarus.cfg file near the Lazarus executable and put a value like this for example into the cfg file : 
    
    
    --primary-config-path=C:\Lazarus2\config\
    

And run Lazarus. It will ask for some config values and after the you can make a simple shortcut for later use without any parameters. 

### Config directory files

_lazarus.dci_ \- stores the Lazarus Code Templates 

_editoroptions.xml_ \- information about colors, fonts and other editor options 

_miscellaneousoptions.xml_ \- packages information 

_environmentoptions.xml_ \- stores the history of opened projects and files (Recent Opened Files and Projects) 

## The Lazarus files

The Lazarus source archive is contained in a single directory. You can simply compile Lazarus (see installation instructions) and a lot of files and subdirectories will be created, but and all will be below that single subdirectory. 

A Lazarus instance (sources and resources) is a single subdirectory. Just copy this directory and you get a whole new Lazarus instance. 

You don't need to _install_ Lazarus to be able to run it. But if you do indeed _install_ Lazarus, you get some nice shortcuts on your desktop and some file types (e.g. lpk, lpi, pas) are associated with that Lazarus installation. These things are platform dependent. There is no more than one single Lazarus indeed _installed_ , secondary copies are just "there". 

Newer installers support _installing_ Lazarus multiple times. Alternatively, You can also add another Lazarus instance **manually** quite easily. Of course, file associations etc. can only be linked to one single Lazarus instance. 

### Linux

Find a 2015 run-through for Linux 64 bit at [DualLaz](<DualLaz.md> "DualLaz"). 

#### Example 1 : A normally installed Lazarus instance plus the git version

Download and install the Lazarus debian- & .rpm - packages as normal. 

Then download the Lazarus git archive to a directory of your choice, preferably somewhere in your home directory (e.g. ~/lazarus). See here [Getting Lazarus from the GitLab server](<Getting_Lazarus.md> "Getting Lazarus"). 

Start your **secondary** Lazarus with: 
    
    
    /home/you/lazarus/startlazarus --pcp=~/.lazarus2nd
    

#### Example 2 : A normally installed Lazarus instance plus a manual copy

i.e. you obtain two subdirectories: 
    
    
    /usr/lib/lazarus     original  copy     via debian install
    /home/you/lazarus    secondary copy 
    

or you may look at: 
    
    
    /home/you/development/lazarus    original copy      via fpcUP install
    /home/you/lazarus                secondary copy 
    

Download and install the Lazarus debian-packages as usual. 

Then copy the Lazarus directory to your home directory and change the ownership of the files: 
    
    
    cp -a /usr/lib/lazarus $(HOME)/
    chown $USER:$USER -R $(HOME)/lazarus
    

(Note: The chown line assumes that your group has the same name as your username. If not, replace $USER:$USER with your username:groupname) 

Now change your Lazarus desktop/menu shortcut to start the **secondary** copy 
    
    
    /home/you/lazarus/startlazarus --pcp=~/.lazarus2nd
    

Then start the IDE and change the environment options: Tools -> Options -> Environment / Lazarus directory to the **secondary** subdir, namely /home/you/lazarus 

  
In recent Lazarus versions, you are also given a screen to name the other important paths, so you would name the secondary sudirs in the 2ndary config. 

Also, inside the /home/you/development/fpc/bin/x86_64-linux/**fpc.cfg** text file, all paths should rather point to your secondary copy. To make sure everything is fine, run **startlazarus** from within xterm and watch the errormessages. make sure all paths are found (very difficult currently). 

  
You now have a secondary copy that which you can edit and recompile. Don't forget to close source files of the primary /usr/lib/lazarus. 

### Windows

#### Example 1: GIT and Release

This is to have both GIT and last release version of Lazarus. 

1) Install Lazarus last release and check setup option 'create desktop shortcut', you will get 'Lazarus.lnk' on the desktop. 

2) Get Lazarus from GIT and compile, then create a shortcut on your desktop for 'lazarus.exe' and rename it to 'Lazarus GIT.lnk'. 

Then you can set the config dir for each version editing the shortcut path to the executable: 

This is for release, because you don't change the primary config path parameter, the config will be saved in the default path: 
    
    
    C:\lazarus\startlazarus.exe
    

For GIT, store the settings e.g. in the folder 'C:\lazgitcfg\': 
    
    
    C:\FPC\laz\startlazarus.exe --primary-config-path=C:\lazgitcfg
    

## Installation of multiple Lazarus

### Windows - Using the installer

On Windows the installer (Version 1.1) can create a secondary installation. And it will ask for a folder to store the config. 

  * Install: 
    * Make sure you back up ALL your existing configurations BEFORE you install.
    * AGAIN: Make sure you back up ALL your existing configurations BEFORE you install.
    * After install check that the correct config is used  
See the file "lazarus.cfg" in the installation directory. And Menu "View" > "IDE Internals" > "About IDE"
    * If you see a message about upgrading/downgrading the config, then the installation is incorrect. Press "Cancel"
  * Usage 
    * Do not share packages between installations; rather you must have several independent copies of each package
    * Backup lpi and lps files (if you share projects). Ideally store sessions (lps) in the "IDE config directory"  
Menu: "Project" > "Project Options" > Session
  * Uninstall: 
    * If you plan to uninstall: back up everything again
  * Update: 
    * If you plan to update, back up everything again
    * After updating, check that the correct config is still being used  
See the file "lazarus.cfg" in the installation directory. And Menu "View" > "IDE Internals" > "About IDE"



The Windows Installer is able to create secondary installations, which allows you to have different versions installed that do not interfere with each other. 

All you need to do is tick the checkbox "secondary installation" (when choosing the install directory), and follow the instructions. You will then be asked for a new empty folder where the config will be stored (this can be a subfolder in the install directory). 

If you update an installation using the Windows Installer, it will detect when the selected directory contains a secondary install. 

In order to always be able to quickly see which IDE you are using, and also to check that the correct config is loaded, it is advisable to make changes to some visual attributes. A possibility is to change the gutter color in the editor. 

#### Windows - Adding a cross compiler install

The installation packages for cross compilers can be added to secondary installations, too. However, they will currently give a warning that the folder is not empty. This warning can be ignored.

---

_Source: [https://wiki.freepascal.org/Multiple_Lazarus](https://web.archive.org/web/20221209024011/https://wiki.freepascal.org/Multiple_Lazarus)_
