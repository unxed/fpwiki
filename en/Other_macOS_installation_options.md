# Other macOS installation options

│ **English (en)** │ 

[![macOSlogo.png](https://wiki.freepascal.org/images/1/15/macOSlogo.png)](</File:macOSlogo.png>)

This article applies to [macOS](</Category:macOS> "Category:macOS") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

  
****

## Contents

  * 1 Official FPC and Lazarus Installation Options
  * 2 Other FPC Installation Options
    * 2.1 Fink
    * 2.2 fpcupdeluxe
    * 2.3 MacPorts
    * 2.4 Daily development version
  * 3 Other Lazarus IDE installation options
    * 3.1 Fink
    * 3.2 fpcupdeluxe
    * 3.3 MacPorts
    * 3.4 Daily development version



## Official FPC and Lazarus Installation Options

For the officially supported FPC and Lazarus installation options, please see [Installing Lazarus on macOS](<Installing_Lazarus_on_macOS.md> "Installing Lazarus on macOS"). It is recommended that new users use the officially supported options. 

## Other FPC Installation Options

### Fink

You could also consider using [Fink](<http://finkproject.org/>), a package manager for macOS to install FPC. Fink also offers numerous [Free Pascal crosscompilers](<Cross_compiling.md> "Cross compiling") for many processors and operating systems. An extra bonus is easy installation as well as clean removal of FPC and a huge number of other open source software packages. 

### fpcupdeluxe

You could also consider using [fpcupdeluxe](<fpcupdeluxe.md> "fpcupdeluxe") to install FPC. 

### MacPorts

You could also consider using [MacPorts](<https://www.macports.org>), to install FPC. An extra bonus is easy installation as well as clean removal of FPC and a huge number of other open source software packages. 

### Daily development version

An automated daily build of the development version of FPC for aarch64 (Apple M1 processor) can be downloaded from [SourceForge](<https://sourceforge.net/projects/macos-lazarus-snapshots/files/arm64/>). 

1\. Move fpc-src to your home directory 

2\. Install with a script like: 
    
    
    #!/bin/sh
    cd ~/fpc-src
    sudo make FPC=$PWD/ppca64 OPT="-ap -FD/Library/Developer/CommandLineTools/usr/bin 
    -XR/Library/Developer/CommandLineTools/SDKs/MacOSX.sdk" install
    

The highlighted lines should be all on one line. 

## Other Lazarus IDE installation options

### Fink

You could also consider using [Fink](<http://finkproject.org/>), a package manager for macOS to install Lazarus. Note at the time of writing (Early 2020) Fink offers Lazarus 2.0.6. The extra bonus of Fink is easy installation as well as clean removal of a huge number of other open source software packages, including [ Free Pascal cross-compilers](<Cross_compiling.md> "Cross compiling") for many processors and systems. The choice for Lazarus is between a Carbon or Cocoa look, a gtk2 look, a Qt4-based and a Qt5-based version: 
    
    
    $ fink install lazarus-aqua
    // or
    $ fink install lazarus-cocoa
    // or
    $ fink install lazarus-gtk2
    // or
    $ fink install lazarus-qt4
    // or
    $ fink install lazarus-qt5
    

You will be asked, whether to install a number of dependencies, including the Free Pascal Compiler, the Lazarus sources. Simply hit RETURN at the prompt and go for a coffee. It may take quite some time to build all packages, in particular on older Macs. 

After installation, Lazarus can be started from the folder /Applications/Fink/. The actual details of FPC and Lazarus are in subdirectories of /sw 

With any Lazarus package from Fink, these widget sets are supported for your program on macOS: 

carbon (Aqua), cocoa, gtk2, qt4 (Aqua), qt5 (Aqua), nogui, win32, win64 and wince. 

If you install FPC from Fink, you will be fine downloading Lazarus source and compiling as detailed below. However, it is possibly not a good idea to mix Fink FPC with the Sourceforge binary install of Lazarus. 

### fpcupdeluxe

You could also consider using [fpcupdeluxe](<fpcupdeluxe.md> "fpcupdeluxe") to install Lazarus. 

### MacPorts

MacPorts also has a package description of the cocoa-64 bit Intel and ARM version of Lazarus. Install it with: 
    
    
    $ sudo port install lazarus
    

### Daily development version

An automated daily build of the development version of the Lazarus IDE for aarch64 (Apple M1 processor) can be downloaded from [SourceForge](<https://sourceforge.net/projects/macos-lazarus-snapshots/files/arm64/>) and for Intel x86_64 also from [SourceForge](<https://sourceforge.net/projects/macos-lazarus-snapshots/files/intel/>). 

1\. Move lazarus-src to your home directory 

2\. Use a script to start Lazarus like: 
    
    
    #!/bin/sh
    open ~/lazarus-src/lazarus.app --args "--pcp=~/.laz_dev"

---

_Source: [https://wiki.freepascal.org/Other_macOS_installation_options](https://web.archive.org/web/20230529022138/https://wiki.freepascal.org/Other_macOS_installation_options)_
