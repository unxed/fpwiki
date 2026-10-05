# Install IDE packages without the IDE

## Contents

  * 1 Install with command line tool lazbuild
  * 2 Install manually without IDE nor lazbuild
    * 2.1 Quick
    * 2.2 Compile lazbuild
    * 2.3 Compile and register the required packages
    * 2.4 Register the .lpk file
    * 2.5 Setup the $HOME/.lazarus/staticpackages.inc file
    * 2.6 Setup $HOME/.lazarus/idemake.cfg
    * 2.7 Compile the package with lazbuild
    * 2.8 Execute _make idepkg_
  * 3 Distributing an IDE with preinstalled packages
    * 3.1 Third party packages
    * 3.2 Compile the IDE
    * 3.3 User Options
  * 4 See Also



## Install with command line tool lazbuild

With current Lazarus, it is possible to compile and install IDE packages using the lazbuild tool. See [lazbuild](<lazbuild.md> "lazbuild"). 

## Install manually without IDE nor lazbuild

For completeness/reference, the old version of this article is presented below. 

[![Warning-icon.png](https://wiki.freepascal.org/images/b/b2/Warning-icon.png)](</File:Warning-icon.png>)

**Warning:** Old information below. There is a much easier way to install packages in current Lazarus.

To manually install a package in the IDE you need the following 

  * compiled lazbuild (LCL, components, lazbuild)
  * all required packages must be setup and compiled
  * the .lpk file must be registered
  * setup the $HOME/.lazarus/staticpackages.inc file (or corresponding file in your primary config path)
  * setup $HOME/.lazarus/idemake.cfg (or corresponding file in your primary config path)
  * compile the package with lazbuild
  * execute _make idepkg_



### Quick

The easiest way is to install the package in the IDE, copy the needed files to the target directory/filesystem and adapt the filenames. 

### Compile lazbuild

Install FPC and lazarus - or do a make lazbuild. See . 

### Compile and register the required packages

Do the steps on this page recursively until only the base Lazarus packages are needed. 

### Register the .lpk file

The IDE must find the .lpk file for a packagename. It searches the .lpk file in the following places: 

  * In the $HOME/.lazarus/packagefiles.xml file
  * In the <lazarus_path>/packager/globallinks/



### Setup the $HOME/.lazarus/staticpackages.inc file

Create this file if it does not exist. Add the packagename plus a comma to this file. For example this file can look like this: 
    
    
     Printer4Lazarus,
     RunTimeTypeInfoControls,
     H2PasWizard,
    

### Setup $HOME/.lazarus/idemake.cfg

The idemake.cfg contains the compile parameters. The easiest way to create this file is to install the package in a IDE and then copy the file to the target machine and adapt the file paths. 

Please note that under Windows you need to place idemake.cfg into Lazarus directory (e.g. C:\Lazarus\\) instead of your user profile directory. Note: May 2012 BigChimp: is this still true? I found it in my primary config path!! 

### Compile the package with lazbuild
    
    
    lazbuild path_to_package.lpk
    

### Execute _make idepkg_
    
    
    cd path_to_lazarus
    make idepkg
    

## Distributing an IDE with preinstalled packages

### Third party packages

Let's say you copied the third party package "Closy" into lazarus/components/closy. When the IDE searches for a package "closy", it searches in its recently used list and in <lazarus>/packager/globallinks. An installer should use the latter. 

Create an .lpl file: 
    
    
    lazarus/packager/globallinks/closy-1.lpl
    

The file name of the lpl file consists of <package name>-<short version>.lpl. In this example the package name is the **lowercase** "closy". Closy has the version _1.0.0.0_. Trailing _0_ can be omitted, creating the short version _1_. The file must contain the path to the lpk: 
    
    
    $(LazarusDir)/components/closy/closy.lpk
    

### Compile the IDE

  * Execute **make clean all** to update lazbuild, startlazarus and other tools.
  * Use the IDE or lazbuild to install all needed packages and compile the new IDE.



Hint: Since 1.2 lazbuild contains the option --add-package to install packages. 

  * You now have a lazarus directory with all needed .ppu files, the lazarus executable and common tools.



### User Options

Now you need to setup the start configuration for the user. 

When the IDE starts, it loads its configuration files from the PCP (_primary config path_). If it cannot find a configuration file it tries the SCP (_secondary config path_) and copies the file from the SCP directory to the PCP directory. 

  * You can pass the PCP and SCP as command line options --pcp=<path> and --scp=<path> to lazarus and lazbuild.
  * You can see the current values in the IDE via _Main menu > View > IDE Internals > About IDE > Global IDE Options_.
  * The default SCP under Unix is _/etc/lazarus_ , under Windows it is the Lazarus directory itself.
  * The default PCP under Unix is _~/.lazarus_.



The list of packages to be installed into the IDE are listed in _miscellaneousoptions.xml_. Your installer should create the file _< SCP>/miscellaneousoptions.xml_ with the following content: 
    
    
    <?xml version="1.0" encoding="UTF-8"?>
    <CONFIG>
      <MiscellaneousOptions>
        <Version Value="3"/>
        <BuildLazarusOptions>
          <StaticAutoInstallPackages Count="6">
            <Item1 Value="AnchorDocking"/>
            <Item2 Value="InstantFPCLaz"/>
            <Item3 Value="Cody"/>
            <Item4 Value="AnchorDockingDsgn"/>
            <Item5 Value="SynEditDsgn"/>
            <Item6 Value="TurboPowerIProDsgn"/>
          </StaticAutoInstallPackages>
        </BuildLazarusOptions>
      </MiscellaneousOptions>
    </CONFIG>
    

If the user does not already have a Lazarus configuration the IDE will use the above file as a start. 

## See Also

  * [Lazarus Packages](<Lazarus_Packages.md> "Lazarus Packages")

---

_Source: [https://wiki.freepascal.org/Install_IDE_packages_without_the_IDE](https://web.archive.org/web/20250219174012/https://wiki.freepascal.org/Install_IDE_packages_without_the_IDE)_
