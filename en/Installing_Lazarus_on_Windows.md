# Installing Lazarus on Windows

[![Windows logo - 2012.svg](https://upload.wikimedia.org/wikipedia/commons/thumb/5/5f/Windows_logo_-_2012.svg/50px-Windows_logo_-_2012.svg.png)](</File:Windows_logo_-_2012.svg>)

This article applies to [Windows](</Category:Windows> "Category:Windows") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

## Contents

  * 1 Using the Windows Installer
  * 2 Installing Lazarus on a Portable USB device
  * 3 Installing Lazarus from sources on Windows
    * 3.1 Getting the Lazarus sources by using TortoiseSVN
    * 3.2 Getting the Lazarus sources from the command line
      * 3.2.1 Downloading Lazarus "fixes" version source
      * 3.2.2 Downloading Lazarus development version source
    * 3.3 Getting the Lazarus sources from a snapshot
    * 3.4 Building Lazarus
    * 3.5 Preparation for the first start
    * 3.6 First start of the IDE
    * 3.7 Updating the installed version
      * 3.7.1 svn
      * 3.7.2 git
    * 3.8 Help: The new IDE does not start
    * 3.9 What does the _bigide_ make argument do?
  * 4 Multiple Lazarus installs
  * 5 Troubleshooting
  * 6 Lazarus FAQ
  * 7 Installing old versions
  * 8 See also



## Using the Windows Installer

By far the easiest and most common way to install Lazarus on Windows is to go to the official [Lazarus SourceForge download site](<https://sourceforge.net/projects/lazarus/files/>) select an appropriate combined FPC/Lazarus installation package, download the latest release and launch the installer. 

The current releases of the Windows Lazarus installer packages install very easily, and should work 'out-of-the-box'. 

Do not install into the standard program folder `c:\Program Files` because access to it is restricted. Moreover, it is recommended to avoid spaces in the folder name because it may confuse some tools used by Lazarus. 

If you already have another Lazarus version on your system it is recommended to keep the old version and to keep it separate from the new installation. For this purpose Lazarus offers to write its configuration files into a specific directory so that both versions can coexist. See section Preparation for the first start below. 

Upgrading can be as simple as downloading the new installer and running it. You will be taken through a typical Windows installation wizard to install the FPC compiler and FPC source within the same directory structure as Lazarus IDE, and Lazarus should launch and operate without significant problems, provided you have **uninstalled** any previous version of Lazarus and/or FPC which was not installed with a recent version fo the Windows installer (older installations can often be found in the C:\pp directory). 

  


[![Note-icon.png](https://wiki.freepascal.org/images/b/be/Note-icon.png)](</File:Note-icon.png>)

**Tip:** It's perhaps a good idea to reboot your Windows system after you have installed Lazarus and before you try to install additional Lazarus components such as zeoslib for example.

## Installing Lazarus on a Portable USB device

It is even possible to install the whole Lazarus/FPC package on a portable USB drive or memory stick (capacity at least 256 MB), for use in environments where you are not allowed to install software on your Windows workstation or where you do not have administrator privileges. You do have to be very careful about adjusting the paths in the compiler and environment options and the `fpc.cfg` file. 

Helpfully there is a third-party Lazarus application [startLazarusPortable](<https://github.com/JoeJoeTV/startLazarusPortable>) available on GitHub in both source and compiled form to take care of this for you. startLazarusPortable is a launcher application that starts Lazarus from a folder/portable USB device and makes all the necessary configuration changes for you. 

See also [this forum post](<https://forum.lazarus.freepascal.org/index.php/topic,46436.msg393696.html#msg393696>) which (as of early February 2020) contains a fix for when you have added some packages to the Lazarus IDE from the online repository. 

## Installing Lazarus from sources on Windows

Installing form sources on Windows basically is a process of getting the sources, put those in an appropriate folder and start the build process (see below).  
This will create the Lazarus IDE in the same folder as where the sources are located.  
If so desired, you can then create a Windows shortcut for the IDE for example on your desktop.  
  
The use of `make install`, whic is a common practise on e.g. Linux, is discouraged.  
If however you insist on using `make install` then notice that you must use `LAZARUS_INSTALL_DIR=C:/somepath` instead of `INSTALL_PREFIX=C:/somepath`, also you must use forward slashes (/) as a directoryseparator, or use double backslash (\\\\\\\ e.g. C:\\\\\\\somepath). See [Issue #38010](<https://gitlab.com/freepascal.org/lazarus/lazarus/-/issues/38010>).  
  


The following instructions are for both SubVersioN and Git repository of the Lazarus IDE. If you are not planning to follow the development source, just want a one-off, you can download a zip file of the Lazarus development source. 

It is assumed that the following installations will go into folders appropriately named `c:\Lazarus-fixes22` or `c:\Lazarus-dev`. Of course any other folder names can be used provided that they do not contain spaces or non-ASCII characters, and that the folders are fully writeable by the user. Do NOT install into `c:\Program files` which is not fully writeable by a user. 

### Getting the Lazarus sources by using TortoiseSVN

The easiest way to work with svn is by using the TortoiseSVN application (<https://tortoisesvn.net/index.de.html>). Install this application if you do not have it already. It adds short-cuts to the Explorer context menu from which you can work with svn easily. 

To download the Lazarus fixes 2.2 sources create the folder `c:\Lazarus-fixes22` and right-click on it in Explorer. In the context menu open the command _SVN Checkout..._. Enter the _URL of the SVN repository_ in the corresponding box. This is: 

  * https://github.com/fpc/Lazarus/branches/fixes_2_2 for the fixes version (adjust the version number in the `fixes22` folder name for the current version).



Make sure that the _Checkout directory_ box contains the directory in which you want to have the new Lazarus version (`c:\Lazarus-fixes22` in our example). After pressing OK the source files are downloaded which will take some time. 

Note: svn checkout of the development version (aka trunk in svn or or main in git) is problematical due to an svn bug. See below using command line svn. 

### Getting the Lazarus sources from the command line

Alternatively you can also download the sources from the command line if you have the command line version of SVN (<https://subversion.apache.org/packages.html#windows>) or GIT (<https://gitforwindows.org/>) on your system. 

#### Downloading Lazarus "fixes" version source

[Details of the fixes](<Lazarus_2.md> "Lazarus 2.2 fixes branch") included in the Lazarus 2.2 fixes version. Here are the steps to follow to download the fixes source: 

  * Open a command prompt window


  * Create a folder for the new Lazarus:


    
    
     md c:\Lazarus-fixes22
    

  * Change into this folder:


    
    
     cd c:\Lazarus-fixes22
    

Using git: 
    
    
     git clone -b fixes_2_2 https://gitlab.com/freepascal.org/lazarus/lazarus.git .
    

Using svn: 
    
    
     svn checkout https://github.com/fpc/Lazarus/branches/fixes_2_2 .
    

  * The download will take some time. Be patient.



#### Downloading Lazarus development version source

Here are the steps to follow to download the development (was known as "trunk" in SVN; now known as "main" in GIT) version source of the Lazarus IDE using svn or git: 

  * Open a command prompt window


  * Create a folder for the new Lazarus:


    
    
     md c:\Lazarus-dev
    

  * Change into this folder:


    
    
     cd c:\Lazarus-dev
    

Using git: 
    
    
     git clone -b main https://gitlab.com/freepascal.org/lazarus/lazarus.git .
    

Using svn (due to an svn bug, this is a two step process): 
    
    
     svn checkout --depth files https://github.com/fpc/Lazarus/branches .
     svn update --set-depth infinity main
    

  * The download will take some time. Be patient.



### Getting the Lazarus sources from a snapshot

A snapshot of the Lazarus development version source is available from the official GitLab repository. You can download it through the Gitlab web interface at <https://gitlab.com/freepascal.org/lazarus/lazarus>. Alternatively, you can fetch it from <https://gitlab.com/freepascal.org/lazarus/lazarus/-/archive/main/lazarus-main.zip>. 

Other, previously available snapshot 'servers', are mentioned here [snapshot servers](<Lazarus_Snapshots_Downloads.md> "Lazarus Snapshots Downloads"). Not recommended. 

Unzip the downloaded archive to the folder in which you want Lazarus to be installed, eg `c:\lazarus-dev`. 

### Building Lazarus

Prerequisite for building Lazarus is that a recent FPC installation must be available. "Recent version" means: 

  * the current FPC release;
  * the previous release version;
  * the FPC fixes version; or
  * the FPC development version.



If you already have a Lazarus release installation on your disk - here you can find the FPC in the equally named folder of the installation directory. Make sure that the FPC version number is adequate. If not, install a newer Lazarus release (which is always recommended). Or, of course, you can also install FPC separately. You are also strongly recommended to install the FPC source as well, not absolutely essential but very handy. 

You should also be aware that the bitness of the FPC determines the bitness of the Lazarus IDE and the bitness of the applications that you will create. So, when your fpc.exe resides in a folder `i386_win32` the IDE (and your applications) will be 32-bit. When the folder is `x86_64-win64` the IDE (and your applications) will be 64-bit. 

  * Remember the full path to the folder containing `fpc.exe` and use it in the following batch file.
  * Change to the folder into which you downloaded the source files and create a batch file with the following content


    
    
    set path=c:\freepascal\bin\x86_64-win64
    :: Of course change this path variable to the path of your FPC compiler
    make bigide
    

  * Executing this batch file will build a Lazarus containing all the basic packages, like in a release version. (Of course you must install any other third-party packages by yourself later.)
  * if using Powershell, try these commands (this time assuming your FPC is in c:\FPC\3.2.2 - typical of a recent install) -


    
    
    $env:PATH="c:\FPC\3.2.2\bin\i386-win32"
    make bigide
    

  * Since the Lazarus development version (trunk in svn, main in git) is not "stable" it may happen that the "bigide" version cannot be build due to an error in one of the standard packages. In this case it may be helpful to know that the basic IDE (with LCL only) can be built by using the `make` command without the `bigide` parameter:


    
    
    set path=c:\freepascal\bin\x86_64-win64
    :: Of course change this path variable to the path of your FPC compiler
    make
    

  * Because the Lazarus IDE by default links to a dll-call "CreateToolhelp32Snapshot", which does not exist on the Win9x platform, the IDE will not run on Win9x out of the box. In order to make it run you have to rebuild the IDE with `make -dWIN9XPLATFORM bigide`.
  * Compilation will take some time again. At the end, the new IDE is ready, but you should not start it yet -- read the next section first.



### Preparation for the first start

Before you start the new IDE you should make sure that it does not interfere with your current other Lazarus installation(s). For this purpose you must have a separate directory for the Lazarus configuration files, maybe `c:\LazConfigs\trunk`. Lazarus writes is configuration settings to this folder if you open the `lazarus.exe` with the argument `--primary-config-path=c:\LazConfigs\dev`, or shorter, `--pcp=c:\LazConfigs\dev`. When this parameter is missing the configuration files are read from and written to the default folder in `c:\users\<your_name>\appdata\lazarus` which will very probably change settings of the other installation. 

Since it is easy to forget this command line parameter it is highly recommended to add a file `lazarus.cfg` to the lazarus installation folder (the folder containing `lazarus.exe`) in which the pcp parameter is listed. This is also how the Lazarus release versions handle secondary installations. 

`echo "--pcp=c:\LazConfigs\trunk" > lazarus.cfg``↵ Enter`

Starting Lazarus in the directory containing that `lazarus.cfg` file will ensure Lazarus keeps its config files in the nominated directory. 

Alternatively you can also start the IDE from the desktop short-cut which has the `--pcp=...` as an argument. 

### First start of the IDE

When you start the new IDE for the first time probably there will be an error message saying that the FPC and the debugger cannot be found. In this error dialog, navigate to the folder which you had used as the path to build the IDE, and select the `fpc.exe`. As for the debugger, navigate to the `mingw` folder of your Lazarus release installation (or install gdb separately) and select the `gdb.exe`. There may also be a message about an issue with the fppkg configuration - click "Restore fppkg configuration" to resolve this. If subsequent error messages appear they can be ignored. 

Now the IDE will be happy and you can start using your new Lazarus trunk (or fixes) IDE. 

### Updating the installed version

It is the advantage of a version-control system such as git and svn that it is very easy to update the installed source version with any subsequent changes. 

#### svn

  * If you used TortoiseSVN right-click on the installation folder of this Lazarus and select _SVN Update_ from the context menu.
  * If you prefer the console change directory (cd) to the installation folder and type `svn update`, or `svn up`



#### git

If you used git: 

  * Open a command prompt
  * cd to the installation folder and type `git pull`



The download now is very fast because only the differences to the local installation must be fetched. 

Then start the IDE, go to _Tools_ > _Build Lazarus with profile..._ and recompile the IDE. It may happen that compilation aborts with some error; in this case go to _Tools_ > _Configure Build Lazarus_ , select _Clean all_ and _Switch after building to automatically_ in the "Clean up" box, and click _Build_. 

### Help: The new IDE does not start

In very rare cases the new sources contain a fatal error so that the IDE does not start successfully. 

For this case you must know that the IDE always keeps a backup copy, called `lazarus.exe.old`. Simply delete the newly created `lazarus.exe` and rename `lazarus.exe.old` to `lazarus.exe`. Now you are back to work, at least with the old version. 

Try the installation later again, usually such catastrophic bugs are soon detected, reported and fixed. 

If you used TortoiseSVN you may want to right-click on the Lazarus installation folder and select _SVN Show Log_ from the context menu to see the commit notes of the recent changes and to learn whether the bug already has been fixed. 

If you used command line git, you can change to the source folder and type `git log `|` more` to see the commit notes of the recent changes and to learn whether the bug already has been fixed. 

### What does the _bigide_ `make` argument do?

The _bigide_ `make` argument adds a bunch of packages to Lazarus that many find useful and cannot do without. The packages that are added are: 

  * cairocanvas
  * chmhelp
  * datetimectrls
  * externhelp
  * fpcunit
  * fpdebug
  * instantfpc
  * jcf2
  * lazcontrols
  * lazdebuggers

| 

  * lclextensions
  * leakview
  * macroscript
  * memds
  * onlinepackagemanager
  * pas2js
  * PascalScript
  * printers
  * projecttemplates
  * rtticontrols

| 

  * sdf
  * sqldb
  * synedit
  * tachart
  * tdbf
  * todolist
  * turbopower_ipro
  * virtualtreeview

  
---|---|---  
  
The above list is sourced from the _`[Lazarus source directory]/IDE/Makefile.fpc`_ and may be subject to change. 

Note that if you have not compiled your own Lazarus IDE with the _bigide_ argument, you can install any of these packages yourself using the Lazarus IDE [_Package > Install/Uninstall Packages..._](<IDE_Window__Installed_Packages.md> "IDE Window: Installed Packages") dialog. 

## Multiple Lazarus installs

Please see [Multiple Lazarus](<Multiple_Lazarus.md> "Multiple Lazarus") for details on having more than one Lazarus version installed on one system. We cover issues that can arise due to multiple Lazarus installs here, because they can also happen when installing over a previous version. 

## Troubleshooting

Troubleshooting details that should (hopefully) be applicable across platforms may be found in the article [Installation Troubleshooting](<Installation_Troubleshooting.md> "Installation Troubleshooting"). 

## Lazarus FAQ

The Lazarus FAQ - Frequently Asked Questions - page is available [here](<Lazarus_Faq.md> "Lazarus Faq"). 

## Installing old versions

See [Installation hints for old versions](<Installation_hints_for_old_versions.md> "Installation hints for old versions")

## See also

  * [Creating your own Lazarus Windows installer](<Lazarus_Installer.md> "Lazarus Installer")

---

_Source: [https://wiki.freepascal.org/Installing_Lazarus_on_Windows](https://web.archive.org/web/20241206075928/https://wiki.freepascal.org/Installing_Lazarus_on_Windows)_
