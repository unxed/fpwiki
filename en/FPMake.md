# FPMake

│ **English (en)** │  **[русский (ru)](<../ru/FPMake.md> "FPMake/ru")** │    
****

**FPMake** is a build-system for Pascal code specifically. It can be compared with other build systems like Make, cons, scons, etc. 

With FPMake the code is split up into packages. A FPMake-package can contain units, binaries, examples and documentation. Each package can depend on other packages, in which case a package can use the units of it's dependences. FPMake uses two different locations to search for packages, a system-wide (global) and a local location. By default the global location points to the location of the fpc-installation, while the local location is user-specific. 

Some parts of fpc itself are compiled using FPMake. (The packages fastcgi and fcl-web in fpc 2.6 and all packages, utilities and the ide in fpc 2.7 and up) 

The packages-manager [fppkg](<fppkg.md> "fppkg") can be used to manage the FPMake packages and to install them from a remote repository. 

  


## Contents

  * 1 How does it work
  * 2 Building FPMake packages
  * 3 Installing FPMake packages
  * 4 How to create FPMake-packages
    * 4.1 Simple example fpmake.pp
    * 4.2 More complex example fpmake.pp
    * 4.3 Changing Working Directory
    * 4.4 Adding directories
    * 4.5 Appending Compiler Options
  * 5 Common error messages
    * 5.1 Unknown target for unit "[unitname]" in dependencies for [targetname] in package [packagename]
    * 5.2 Could not find unit directory for dependency package "rtl"



## How does it work

FPMake works with Pascal-applications called fpmake, of which each can compile and install one or more FPMake-packages. All settings, location of the sources and instructions are defined inside the source of this fpmake-executable. This source-file is normally saved with the name fpmake.pp. The fpmake.pp is compiled into the fpmake-executable, and this executable can build and install the final packages. 

## Building FPMake packages

The easiest is to use [fppkg](<fppkg.md> "fppkg") to build a FPMake package. But it can also be done manually. Locate the sources of the package you want to install and the fpmake.pp file. Now compile the fpmake.pp file: 
    
    
    fpc fpmake.pp
    

Typical output looks like: 
    
    
    Free Pascal Compiler version 2.7.1 [2014/11/26] for i386
    Copyright (c) 1993-2014 by Florian Klaempfl and others
    Target OS: Darwin for i386
    Compiling fpmake.pp
    Assembling (pipe) fpmake.s
    Linking fpmake
    203 lines compiled, 3.6 sec
    

Now run the new fpmake(.exe)-executable to build the package(s). This example only contains one package. 

On unices or alike: 
    
    
    ./fpmake build --globalunitdir=/usr/local/lib/fpc/2.7.1
    

On Windows or alike: 
    
    
    fpmake.exe build --globalunitdir=c:\pp
    

Typical output looks like: 
    
    
    Start compiling package fcl-base for target i386-darwin.
          Compiling src/ascii85.pp
          Compiling src/avl_tree.pp
          Compiling src/base64.pp
          Compiling src/fpmimetypes.pp
    [100%] Compiled package fcl-base
    

The --globalunitdir parameter gives the location of the already installed global packages. This way FPMake can find the packages that this package depends on. When there is a second location where packages are installed, in addition the --localunitdir parameter can be used. 

If you want to know which path you have to use for the --global(local)unitdir parameter, it's the directory which contains a directory called 'fpmkinst'. 

To remove all files generated during the build and to be able to do a 'clean' build, perform a clean. 

On unices or alike: 
    
    
    ./fpmake clean
    

On Windows or alike: 
    
    
    fpmake.exe clean
    

Use the -h parameter to get an overview of all other available commands and options. 

## Installing FPMake packages

Just like building packages, using [fppkg](<fppkg.md> "fppkg") to install FPMake packages is the easiest. But it canbe done manually. First compile the package and then call the fpmake executable to install the package. 

On unices or alike: 
    
    
    ./fpmake install --globalunitdir=/usr/local/lib/fpc/2.7.1 --prefix=/usr/local --baseinstalldir=/usr/local/lib/fpc/2.7.1
    

On Windows or alike: 
    
    
    fpmake.exe install --globalunitdir=c:\pp --prefix=c:\pp --baseinstalldir=c:\pp
    

Typical output looks like: 
    
    
    Installing package fcl-base
    Installation package fcl-base for target i386-darwin succeeded
    

The globalunitdir contains the location where to look for packages that the FPMake package can depend on, just like when building the package. The baseinstalldir is the location where the package should be installed to. In general this is the same path as is used for the global- or local-unitdir. Some packages also installs files outside of the baseinstalldir. like configuration files. These packages most often also need a prefix, so providing one is advisable. 

## How to create FPMake-packages

### Simple example fpmake.pp
    
    
      program fpmake;
    
      uses fpmkunit;
    
      Var
        P: TPackage;
        T: TTarget;
    
      begin
        With Installer do
        begin
          P := AddPackage('my-nice-program');
          P.OSes := [win32,openbsd,netbsd,freebsd,darwin,linux];
          T := P.Targets.AddUnit('myunit');
          T.ResourceStrings := True;
          T := P.Targets.AddUnit('myprogram');
          T.Dependencies.Add('myunit');
          Run;
        end;
      end.
    

### More complex example fpmake.pp
    
    
      program fpmake;
    
      uses fpmkunit;
    
      type TWidgetSet = (wsGDI, wsX, wsCarbon);
    
      var WidgetSet : TWidgetSet;
          P : TPackage;
    
      procedure DetermineWidgetSet;
      Var
        I : Integer;
      begin
        if Defaults.OS in AllWindowsOSes then
          WidgetSet := wsGDI else
        if Defaults.OS = MacOS then
          WidgetSet := wsCarbon else
        if Defaults.OS in AllUnixOSes then
          Widgetset := wsX;
    
        // Check paramstr() to see if the widgetset was overriden on the commandline;
        For I := 1 to ParamCount do
          If ParamStr(i) = '--widgetset=X' then
            WidgetSet := wsX;
      end;
    
      begin
        DetermineWidgetSet;
        With Installer do
        begin
          // ...
          Case WidgetSet of
            wsGDI : P.UnitPath.Add('corelib/gdi');
            wsX   : P.UnitPath.Add('corelib/x11');
            // etc.
          end;
          //  ...
          Run;
        end;
      end.
    

### Changing Working Directory

If working with units in a subfolder relative to ./fpmake, then "Directory" can be changed. 
    
    
        With Installer do
        begin
          ...
          Directory:='some/subdir';
          T:= Targets.AddUnit('unitinfolder');
          ...
        end;
    

### Adding directories

You can add directories with the unit path: 
    
    
      Case Defaults.OS of
        Win32, Win64: 
          begin
            T.UnitPath.Add('corelib/gdi');
            T.Dependencies.Add('gfx_gdi');
          end;
        Linux: 
          begin
            T.UnitPath.Add('corelib/x11');
            T.Dependencies.Add('gfx_x11');
          end;
      end;
    

Often, it's more comfortable to use set constants like AllWindowsOSes, AllUnixOSes instead of specific OS names. 

### Appending Compiler Options

Additional custom compiler options (i.e. compiler command line parameters) can be appended by using TTarget.Options, TPackage.Options, or Defaults.Options. 
    
    
      var T : TTarget;
      ...
      T.Options.Append('-dSOMEDEFINE');
      T.Options.Append('-xyzAnythingYouNeed');
    

Or 
    
    
      var P : TPackage;
      ...
      P.Options.Append('-dSOMEDEFINE');
    

Or 
    
    
      Defaults.Options.Append('-dSOMEDEFINE');
    

**Note** : Before FPC 2.4.0, Options property was a simple string, and it seems it supported passing only one parameter to the compiler. Since FPC 2.4.0 (more precisely, since svn revision 13223) Options is a TStrings instance, so it's much more flexible. This also means that you should use $ifdefs if you want to use Options and want your fpmake.pp be compatible with both FPC < 2.4.0 and >= 2.4.0. 

## Common error messages

A few common error messages are explained here 

### Unknown target for unit "[unitname]" in dependencies for [targetname] in package [packagename]

This indicates a problem in the fpmake.pp file. There is a dependency on a unit, added using Target.Dependencies.AddUnit('unitname'). But there is no corresponding target added to the package with the same unitname. 

In other words: for every dependency on a unit, there must be a corresponding target within the same package. When there is a dependency on a unit in another package, add a dependency on that package. A dependency on a single file within a different package will not work. 

Also note that the unit name has to be without the extension of the corresponding file. (Target.Dependencies.AddUnit('unitnams.pas') will not work, while Targets.Addunit requires the extension to be present) 

### Could not find unit directory for dependency package "rtl"

In general the fpmake system scans the places typically used for fpc installations. In the case that fpmake doesn't find the FPC directory, you'll get the above mentioned error. In such case, set the FPCDIR environment variable to the base dir of the FPC installation, e.g.: 
    
    
     export FPCDIR=~/src/lib/fpc/2.6.4
    

or 
    
    
     set FPCDIR=C:\lazarus\fpc\2.6.4\
    

This can also be useful if you have to disambiguate over multiple FPC installations.

---

_Source: [https://wiki.freepascal.org/FPMake](https://web.archive.org/web/20230701000000/https://wiki.freepascal.org/FPMake)_
