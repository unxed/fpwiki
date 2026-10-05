# fppkg

│ **English (en)** │  **[русский (ru)](<../ru/fppkg.md>)** │

## Contents

  * 1 Introduction
  * 2 Fppkg walk-through
    * 2.1 Step 1, install package
    * 2.2 Step 2, list installed packages
    * 2.3 Step 3, install package with a dependency
    * 2.4 Step 4, re-install an already installed package
    * 2.5 Step 5, install a package from source
    * 2.6 Step 6, uninstall a package
    * 2.7 Step 7, fix broken dependencies
    * 2.8 Step 8, fix broken dependencies manually
  * 3 Quick-start guide
    * 3.1 Setting up
      * 3.1.1 Configuring the compiler
    * 3.2 Installing a package
  * 4 Commandline arguments
  * 5 Actions
    * 5.1 Update
    * 5.2 List
    * 5.3 Build
    * 5.4 Install
    * 5.5 Download
    * 5.6 Convertmk
    * 5.7 Config
  * 6 Configuration
    * 6.1 General configuration
    * 6.2 The global configuration file
      * 6.2.1 Configuration for use with Lazarus
    * 6.3 Compiler configuration files
    * 6.4 Using macros in the configuration files
  * 7 Upgrade



## Introduction

Fppkg is the Free Pascal package manager. It is used to compile and install Free Pascal packages which are based on [FPMake](<FPMake.md> "FPMake"). It also resolves dependencies between packages and can fetch the packages from a central repository. 

## Fppkg walk-through

### Step 1, install package

To install a package, just run 'fppkg install <packagename>'. This will look like this: 
    
    
    $ fppkg install cnocasn1
    Downloading package cnocASN1 version 0.9.0
    Start compiling package cnocASN1 for target x86_64-linux.
           Compiling cnocasn1.pas
           Compiling cnocasn1decoder.pas
           Compiling cnocasn1tagfactory.pas
           Compiling cnocasn1tagfactory.pas
           Compiling cnocasn1encoder.pas
           Compiling cnocasn1element.pas
           Compiling cnocasn1decoder.pas
    [100%] Compiled package cnocASN1
    Installing package cnocASN1
    Installation package cnocASN1 for target x86_64-linux succeeded
    

First the cnocasn1 package is downloaded from a central repository. Then it is compiled and installed. Now with the package installed it is possible to use it's units (like cnocasn1, cnocasn1encoder etc) into any Free Pascal application. 

When installing the package does not work for you, take a look at the troubleshooting section. 

### Step 2, list installed packages

Fppkg can give a list of all installed packages, and the packages which are available for installation. This could be that they could be downloaded, or the source might be available locally. Below a (cropped) list of all packages after the installation of cnocasn1. 
    
    
    $ fppkg list
    Name                 Installed      Available     
    a52                  3.3.1          -             
    aspell               3.3.1          -             
    bfd                  3.3.1          -             
    bzip2                3.3.1          -             
    cairo                3.3.1          -             
    cdrom                3.3.1          -             
    chm                  3.3.1          -             
    cnocASN1             0.9.0          0.9.0         
    codetools            1.0.1          -             
    dblib                3.3.1          -             
    

The packages which are part of fpc itself, like chm, are installed but not (online) available. This is because fpc version 3.3.1 is a moving target. When an official version of fpc is being used, all the packages are also available in the online repository. 

### Step 3, install package with a dependency

All (maybe except the RTL) packages have dependencies on other packages. When fppkg is used to install a package, it will check all dependencies before compilation. When a dependency is not available locally, fppkg will lookup the package in the central repository and, if available, download and install them. See for example: 
    
    
    $ fppkg install cnocoidc
    Downloading package cnocOIDC version 0.9.0
    Install dependencies
    Downloading package cnocRSA version 0.9.0
    Start compiling package cnocRSA for target x86_64-linux.
           Compiling cnocrsa.pas
    [100%] Compiled package cnocRSA
    Installing package cnocRSA
    Installation package cnocRSA for target x86_64-linux succeeded
    Dependencies installed
    Start compiling package cnocOIDC for target x86_64-linux.
           Compiling cnocopenidconnect.pas
           Compiling cnocoidcidtoken.pas
           Compiling cnocoidcidtoken.pas
    [100%] Compiled package cnocOIDC
    Installing package cnocOIDC
    Installation package cnocOIDC for target x86_64-linux succeeded
    

Before installing cnocOIDC, the missing dependency cnocRSA is downloaded and installed. Note that the cnocRSA package depends on the cnocasn1 package, but this package is already installed during step 1 and is therefore not installed again. 

### Step 4, re-install an already installed package

Fppkg will not only install dependencies, but will also re-compile all packages that depend on a newly installed package. Suppose you want to have debug-information for the cnocasn1 package. This is possible as follows: 
    
    
    $ fppkg install -o -gl cnocasn1
    Start compiling package cnocASN1 for target x86_64-linux.
           Compiling cnocasn1.pas
           Compiling cnocasn1decoder.pas
           Compiling cnocasn1tagfactory.pas
           Compiling cnocasn1encoder.pas
           Compiling cnocasn1element.pas
    [100%] Compiled package cnocASN1
    Installing package cnocASN1
    Installation package cnocASN1 for target x86_64-linux succeeded
    Re-install packages which are dependent on just installed packages
    Start compiling package cnocRSA for target x86_64-linux.
           Compiling cnocrsa.pas
    [100%] Compiled package cnocRSA
    Installing package cnocRSA
    Installation package cnocRSA for target x86_64-linux succeeded
    Start compiling package cnocOIDC for target x86_64-linux.
           Compiling cnocopenidconnect.pas
           Compiling cnocoidcidtoken.pas
    [100%] Compiled package cnocOIDC
    Installing package cnocOIDC
    Installation package cnocOIDC for target x86_64-linux succeeded
    

This will re-install the cnocasn1 package with the -gl option passed to the compiler. (-gl means build with debug-information) But this is not the only thing Fppkg does. It will also re-build cnocrsa and cnocoidc, because cnocrsa depends on cnocasn1 and cnocoidc depends on cnocrsa. Note that in this case recompiling both packages is not necessary, because the -gl option does not change the interface of the package. But Fppkg can not detect this and will rebuild and reinstall the packages. 

### Step 5, install a package from source

In the previous steps a package was installed from a central repository. But it is also possible to install a package from a local harddisk. To do this, change the current directory to the source-directory of the package. This is the directory that contains the fpmake.pp file. Then just call fppkg but omit the package-name from the command line. All dependencies will be resolved automatically in this case also. 

Hint: If you want to test this but do not have the sources of a fpmake-package available, most Free Pascal users do have the sources of the packages that come with the compiler installed onto their machine. Below is the cropped output of the installation of the fcl-db package: 
    
    
    $ fppkg install
    Start compiling package fcl-db for target x86_64-linux.
           Compiling src/base/dbconst.pas
           Compiling src/base/db.pas
           Compiling src/dbase/dbf_prssupp.pas
    
    
    
    Warning: Circular dependency detected when compiling target dbf_fields with target dbf_dbffile
           Compiling src/dbase/dbf_fields.pas
    Warning: Circular dependency detected when compiling target dbf_memo with target dbf_dbffile
           Compiling src/dbase/dbf_memo.pas
    Warning: Circular dependency detected when compiling target dbf_parser with target dbf
    Warning: Circular dependency detected when compiling target dbf_parser with target dbf_dbffile
           Compiling src/dbase/dbf_parser.pas
           Compiling src/dbase/dbf_parser.pas
           Compiling src/dbase/dbf_lang.pas
           Compiling src/dbase/dbf_collate.pas
    Warning: Circular dependency detected when compiling target dbf_idxfile with target dbf_dbffile
           Compiling src/dbase/dbf_idxfile.pas
    [100%] Compiled package fcl-db
    Installing package fcl-db
    Installation package fcl-db for target x86_64-linux succeeded
    Re-install packages which are dependent on just installed packages
    Start compiling package fpindexer for target x86_64-linux.
           Compiling src/fpmasks.pp
           Compiling src/fpindexer.pp
           Compiling src/memindexdb.pp
           Compiling src/ireaderhtml.pp
    
    
    
    Installing package cnocRSA
    Installation package cnocRSA for target x86_64-linux succeeded
    Start compiling package cnocOIDC for target x86_64-linux.
           Compiling cnocopenidconnect.pas
           Compiling cnocoidcidtoken.pas
    [100%] Compiled package cnocOIDC
    Installing package cnocOIDC
    Installation package cnocOIDC for target x86_64-linux succeeded
    

### Step 6, uninstall a package

Uninstalling a package is as simple as 'fppkg uninstall <packagename>'. 
    
    
    $ fppkg uninstall cnocasn1
    Uninstalling package cnocASN1
    

Fppkg does not do any dependency checks, so use with care. It is possible to leave your configuration in a broken state, when some other installed packages depend on the uninstalled package. (This will probably change in the future, not checking for dependencies on uninstall seems like a bug to me) 

### Step 7, fix broken dependencies

It might be that the dependencies between the packages get broken. This might happen when a package has been deleted, while some other packages still depend on it. (See step 6) Another way dependencies might get broken is when Free Pascal has been re-installed. 

Broken packages are marked (B) in the list of available packages. Running 'fppkg list' with the -v option gives some hints why a packages is broken: 
    
    
    $ fppkg list -v
    Start loading configuration file "/home/joost/.config/fppkg.cfg"
    Loaded global configuration from "/home/joost/.config/fppkg.cfg"
    
    
    
    Name                 Installed      Available     
    a52                  3.3.1          -             
    aspell               3.3.1          -             
    bfd                  3.3.1          -             
    bzip2                3.3.1          -             
    cairo                3.3.1          -             
    cdrom                3.3.1          -             
    chm                  3.3.1          -             
    cnocASN1             -              0.9.0         
    Package cnocRSA depends on package cnocASN1 which is not installed anymore
    Package cnocOIDC (user) needs to be rebuild, dependency cnocRSA (user) is broken
    cnocOIDC             0.9.0 (B)      0.9.0         
    Package cnocRSA depends on package cnocASN1 which is not installed anymore
    cnocRSA              0.9.0 (B)      0.9.0         
    dblib                3.3.1          -
    

To fix all broken dependencies, use 'fppkg fixbroken': 
    
    
    $ fppkg fixbroken
    Re-install packages which are dependent on just installed packages
    Install dependencies
    Start compiling package cnocASN1 for target x86_64-linux.
           Compiling cnocasn1.pas
           Compiling cnocasn1decoder.pas
           Compiling cnocasn1tagfactory.pas
           Compiling cnocasn1tagfactory.pas
           Compiling cnocasn1encoder.pas
           Compiling cnocasn1element.pas
           Compiling cnocasn1decoder.pas
    [100%] Compiled package cnocASN1
    Installing package cnocASN1
    Installation package cnocASN1 for target x86_64-linux succeeded
    Dependencies installed
    Start compiling package cnocRSA for target x86_64-linux.
           Compiling cnocrsa.pas
    [100%] Compiled package cnocRSA
    Installing package cnocRSA
    Installation package cnocRSA for target x86_64-linux succeeded
    Start compiling package cnocOIDC for target x86_64-linux.
           Compiling cnocopenidconnect.pas
           Compiling cnocoidcidtoken.pas
    [100%] Compiled package cnocOIDC
    Installing package cnocOIDC
    Installation package cnocOIDC for target x86_64-linux succeeded
    

### Step 8, fix broken dependencies manually

When some packages are broken, it is not possible to run any fppkg commands. Doing so will give messages like these: 
    
    
    $ fppkg install cnocasn1
    The FPC Package tool encountered the following error:
    Found broken packages, run "fppkg fixbroken" first
    

To install the missing package, use the -b parameter: 
    
    
    $ fppkg install -b cnocasn1
    Start compiling package cnocASN1 for target x86_64-linux.
           Compiling cnocasn1.pas
           Compiling cnocasn1decoder.pas
           Compiling cnocasn1tagfactory.pas
           Compiling cnocasn1tagfactory.pas
           Compiling cnocasn1encoder.pas
           Compiling cnocasn1element.pas
           Compiling cnocasn1decoder.pas
    [100%] Compiled package cnocASN1
    Installing package cnocASN1
    Installation package cnocASN1 for target x86_64-linux succeeded
    Re-install packages which are dependent on just installed packages
    

## Quick-start guide

Note: this section is only valid for fppkg from fpc version 2.5.1 or higher. 

### Setting up

When you run fppkg for the first time, it will create new configuration files with the default values. To check if these values are ok, run 'fppkg list' in a console window. You should see something like this: 
    
    
    Name                 Installed    Available   
    a52                  2.5.1-1      -           
    aspell               2.5.1-1      -           
    bfd                  2.5.1-1      -           
    lnet                 -            0.6.3-0  
    

A list with all packages which are installed and the packages which are available for download are showed. You can see which version of each package is available and/or installed. 

If fppkg only shows available packages but not any installed packages, you have to adapt your fppkg configuration. Run 'fppkg listsettings' to see the current settings and the location of the configuration files. Locate the compiler configuration file and open it in a text-editor. Now change the 'GlobalPrefix' setting to the location where fpc is installed. For example: 'GlobalPrefix=/usr/lib/fpc' or 'c:\pp'. Save the file and run 'fppkg list' again. 

If you have still problems please read the section about configuring fppkg. 

#### Configuring the compiler

The last step is configuring the Free Pascal compiler in such a way that it finds the installed packages. By default the packages are installed in a user-specific location and this location is by default not searched by the compiler. Run 'fppkg listsettings' to see where fppkg installs the user-specific packages. This location is listed as the 'LocalInstallDir'. 

Now add this line to your Free Pascal compiler configuration file fpc.cfg: 
    
    
    -Fu<LocalInstallDir>/units/$fpctarget/*
    

Replace <LocalInstallDir> with the value of the 'LocalInstallDir' setting as showed by 'fppkg list'. For example: 

Linux: (/etc/fpc.cfg or ~/.fpc.cfg) 
    
    
    -Fu~/.fppkg/lib/fpc/$fpcversion/units/$fpctarget/*
    

Windows: (fpc.cfg) 
    
    
    -Fuc:\users\username\AppData\Local\FreePascal\fppkg\units\$fpctarget\*
    

Now you can use the units from the installed packages in all your programs. If you place the line at the bottom of the fpc.cfg, you are sure that the compiler will prefer the (locally) installed packages above the globally installed ones. This way you can install newer versions of packages locally (specific for one user), without having to change the global setup. 

### Installing a package

Installing a package is as simple as 'fppkg install [packagename]'. This will download, compile and install the package. When no error appears, use 'fppkg list' to check if the package is correctly installed. By default the package is installed for the current user only, if you want to force a system-wide installation, use the '-g' parameter. Note however that in that case you need to have the proper rights. 

## Commandline arguments

Output of fppkg -h 
    
    
    Usage: fppkg [options] <action> <package>
    Options:
      -C --config-file   Specify the configuration file to use
      -c --config        Set compiler configuration to use
      -g --global        Prefer global configuration file over local configuration file.
      -h --help          This help
      -V --version       Show version and exit
      -v --verbose       Show more information
      -d --debug         Show debugging information
      -f --force         Force installation also if the package is already installed
      -r --recovery      Recovery mode, use always internal fpmkunit
      -b --broken        Do not stop on broken packages
      -l --showlocation  Show in which repository the the packages are installed
      -o --options=value Pass extra options to the compiler
      -n                 Do not read the default configuration files
      -p --prefix=value  Specify the prefix
      -s --skipbroken    Skip the rebuild of depending packages after installation
      -i --installlocation Specify the repository to install packages intoallation
      --compiler=value   Specify the compiler-executable
      --cpu=value        Specify the target cpu to compile for
      --os=value         Specify the target operating system to compile for
    Actions:
      update            Update packages list
      list              List available and installed packages
      build             Build package
      compile           Compile package
      install           Install package
      uninstall         Uninstall package
      clean             Clean package
      archive           Create archive of package
      download          Download package
      convertmk         Convert Makefile.fpc to fpmake.pp
      info              Show more information about a package
      fixbroken         Recompile all (broken) packages with changed dependencies
      listsettings      Show the values for all fppkg settings
      config            Get/Set configuration file values
    Config commands:
     config get a.b            Get setting from config file, section a, key b
     config get a b            Get setting from config file, section a, key b
     config set a.b c          Set setting from config file, section a, key b to value c
     config set a b c          Set setting from config file, section a, key b to value c
    
    

All the actions are described below. 

The package parameter can mean the following: 

  * _Packagename:_ The package name as it appears on the used repository. All available package names are listed by 'fppkg list'.
  * _Zipfile:_ The full name of a zip-file (including the .zip extension) which contains a package.
  * _Current directory:_ When there is no package parameter, fppkg looks for a file called 'fpmake.pp' in the current directory, and if it is found, use the package defined by that file. In this case the compiled files are not placed elsewhere (See the 'BuildDir' setting) but within the 'units' directory in the current directory.



## Actions

### Update

Update packages list 

### List

List available and installed packages 

### Build

Build package 

### Install

Install package 

### Download

Download package 

### Convertmk

Convert Makefile.fpc to fpmake.pp 

### Config

Use this to quickly get or set configuration settings. 

## Configuration

### General configuration

On startup fppkg reads it's settings from fppkg.cfg. If it can not find it, it will try to create one with some default values. In this configuration file the location of the local repository is set. Normally this is in the user's home-directory. It is also possible to add custom options which are always passed when fpmake is called. This can be used by fpmake-addins. For all other options see the global configuration file. 

Besides the global configuration file, there is also a file which is specific for a compiler version. You can add as much of these files as you want, using different compiler versions or compiler options. A common use is for cross-compiling. You can specify which configuration file to use on the fppkg command line with the '-c' option. The location of these configuration files is set in fppkg.cfg. when no configuration file is specified, the file called 'default' is used. (This can be set in fppkg.cfg) 

Most important settings in these configuration files are the locations of where the packages have to be installed. Fppkg supports two different locations for installed packages. One 'global' location, which normally can be used by all users of the computer. And a 'local' location which in most cases is in a user's home-directory. These locations can be set with the 'localprefix' and 'globalprefix' settings. If you want that fppkg detects which packages are installed by fpc itself, the 'globalprefix' has to be set to match the prefix of your fpc-installation. It is common to use the location of the local repository (set in fppkg.cfg) as setting for the localprefix. For a better explanation of all options see the compiler configuration files. 

The 'fppkg listsettings' command will show the values for all settings and the names of the used configuration files. From settings that allow macros the value as it is set by the user and the value when all macros are evaluated are both shown. 

### The global configuration file

The global configuration file contains general information for fppkg, like the repository to download from and where to place the temporary files. A typical global configuration file looks like this: (linux) 
    
    
    [Defaults]
    ConfigVersion=4
    LocalRepository={UserDir}.fppkg/
    BuildDir={LocalRepository}build/
    ArchivesDir={LocalRepository}archives/
    CompilerConfigDir={LocalRepository}config/
    RemoteMirrors=<http://www.freepascal.org/repository/mirrors.xml>
    RemoteRepository=auto
    CompilerConfig=default
    FPMakeCompilerConfig=default
    Downloader=lnet
    

When fppkg is started it first tries to load fppkg.cfg from a user-specific location. If a user-specific version can not be found fppkg searches for a system-wide fppkg.cfg. See the table for the exact location of these files. If both files do not exist a configuration file is created. When the current user is a superuser, a system-wide configuration file is made, else a user-specific one. When creating the configuration file fppkg tries some reasonable defaults for all settings. When you set the location of the fpc-installation in the enviroment-option FPCDIR, fppkg will use this location in the generated configuration files. 

When you have the installed FPC packages used from FPC instalation or from Lazarus instalation that you need also write repository sections into the global configuration file. A typical global configuration file looks like this: (Windows) 
    
    
    [Global]
    ConfigVersion=5
    RemoteMirrors=<https://www.freepascal.org/repository/mirrors.xml>
    LocalRepository=C:\Users\UserName\AppData\Local\FreePascal\fppkg\
    BuildDir={LocalRepository}build\
    ArchivesDir={LocalRepository}archives\
    CompilerConfigDir={LocalRepository}config\
    CompilerConfig=default
    FPMakeCompilerConfig=default
    RemoteRepository=auto
    FPMakeOptions=--lazarusdir=C:\lazarus
    Downloader=FPC
    InstallRepository=user
    
    [Repository]
    Name=lazarus_fpc
    Description=Packages which are installed along with the Lazarus Free Pascal Compiler 
    Path=C:\lazarus\fpc\3.2.0
    Prefix=C:\lazarus\fpc\3.2.0
    
    [Repository]
    Name=user
    Description=User-installed packages
    Path={LocalRepository}units/{CompilerVersion}
    Prefix={LocalRepository}
    

When you want to know for sure which configuration files fppkg uses, use the 'listsettings' action. It will list which configuration files are used plus the values for all settings. 

Location of configuration files on different OS'es  OS | User-specific configuration file | System-wide configuration file   
---|---|---  
Unix/Linux  | ~/.config/fppkg.cfg  | /etc/fppkg.cfg   
Windows  | %localsettings%\FreePascal\fppkg\fppkg.cfg  |   
  
The following settings are available: 

LocalRepository
    The location where local repository information is stored. Supports macros.
BuildDir
    The location where packages are build when a package is supplied. When the package in the current directory is used, this setting is ignored. Supports macros.
ArchivesDir
    Place where packages downloaded from a remote repository are stored. Supports macros.
CompilerConfigDir
    Place where the compiler-configuration files can be found. Supports macros.
RemoteMirrors
    The URL to a file which contains all mirrors for a remote repository.
RemoteRepository
    Contains the repository which has to be used. Selecting 'auto' will result in a random repository selected from the mirror list.
Compilerconfig
    The filename of the compiler-configuration file which has to be used by default.
FPMakeCompilerconfig
    The filename of the compiler-configuration file which has to be used by default for compiling the fpmake.pp files.
Downloader
    Select the download-mechanism. lnet is the default.
FPMakeOptions
    Specifies options which have to be added to each call to fpmake. Use this if you have fpmake-addons installed that need some specific settings. An example is the lazmkunit-addin, which needs the lazarusdir setting. (FPMakeOptions=--lazarusdir=/path/to/lazarus)
ConfigVersion
    The version number of the used configuration-file format. Is used internally to check if the configuration file is in the format fppkg expects. Do not change.
InstallRepository
    Repository name for update and install the packages.

#### Configuration for use with Lazarus

Some packages use the lazmkunit to integrate with Lazarus. When these packages are installed they need to know where Lazarus is installed. Add the following line with the path to your lazarus installation to fppkg.cfg: 
    
    
    FPMakeOptions=--lazarusdir=/path/to/lazarus
    

### Compiler configuration files

A compiler configuration file contains all information necessary to select which fpc-compiler to use. A typical compiler-configuration file looks like this: 
    
    
    [Defaults]
    ConfigVersion=4
    GlobalPrefix=/usr/local/lib/fpc
    LocalPrefix={LocalRepository}
    Compiler=/usr/local/bin/fpc
    OS=linux
    CPU=i386
    Version=2.5.1
    CompilerOptions=-XR/Library/Developer/CommandLineTools/SDKs/MacOSX.sdk -WM10.14
    

The following settings are available: 

GlobalPrefix
    The prefix which has to be used when installing global packages. Normally this is the prefix of where fpc is installed, so that the packages which are installed by fpc are recognised. Supports macros.
LocalPrefix
    Supports macros.
GlobalInstallDir
    This is the location where the global packages are installed. The default value is {GlobalPrefix}lib/fpc/{compilerversion}/ on unices, and {globalprefix} on other operating systems. Supports macros.
LocalInstallDir
    This is the location where the user-specifig (local) packages are installed. Add this path to the search path of the compiler if you want to use one of the packages installed into this location. The default value is {LocalPrefix}lib/fpc/{compilerversion}/ on unices, and {LocalPrefix} on other operating systems. Supports macros.
Compiler
    The executable name of the compiler which has to be used.
OS
    The target OS.
CPU
    The target CPU-family.
Version
    The version of the used compiler.
ConfigVersion
    The version number of the used configuration-file format. Is used internally to check if the configuration file is in the format fppkg expects.
CompilerOptions
    Additional compiler options to be used when compiling fpmake and/or other packages. All compiler invocations are done with the -n command-line, causing the compiler not to read your fpc config file. This means that if your system needs additional options to work correctly (such as the MacOS development kit location) then you must set them here.

### Using macros in the configuration files

To make life easier you can use macros within the configuration-files. Macro's are between accolades '{}' and they are substituted on by some dynamic values. The following macros are available: 

AppConfigDir
    Returns the location where fppkg stores it's user-specific configuration files. In fact it calls [GetAppConfigDir](<http://www.freepascal.org/docs-html/rtl/sysutils/getappconfigdir.html>) with Global=false.
UserDir
    Returns the location of the user's home-directory. ([GetUserDir](<http://www.freepascal.org/docs-html/rtl/sysutils/getuserdir.html>))
LocalRepository
    The value of LocalRepository in the global configuration file
CompilerVersion
    The version of the compiler as set in the compile configuration file, or the version detected from the compiler executable. This macro is not available in the global configuration file.

## Upgrade

After upgrading the compiler your old fppkg directory is outdated. 

  * If you use only one compiler version remove your **~/.fppkg/config/default** directory and do **fppkg update**.
  * If you use multiple compiler versions copy the **~/.fppkg/config/default** to **~/.fppkg/config/3.1.1** and then use **fppkg -c 3.1.1**.

---

_Source: [https://wiki.freepascal.org/fppkg](https://web.archive.org/web/20210624085521/https://wiki.freepascal.org/fppkg)_
