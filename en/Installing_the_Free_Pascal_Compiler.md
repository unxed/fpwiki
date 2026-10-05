# Installing the Free Pascal Compiler

│ **English (en)** │  **[中文（中国大陆） (zh_CN)](</Installing_the_Free_Pascal_Compiler/zh_CN> "Installing the Free Pascal Compiler/zh CN")** │ 

While the Free Pascal Compiler comes pre-built for several processor architectures and operating systems, there may be occasions where a pre-built compiler won't do, and you need to build it yourself. There are a number of reasons for doing this, including: 

  * Intellectual curiosity - you want to discover the compiler's speed, using a medium sized application like the compiler itself.
  * Cross Compiling - you want to build a compiler that creates binaries for a different CPU architecture, operating environment, or both, than the one you are using right now.
  * New environment - you want to build the compiler for a processor or operating system that either a version of the compiler does not currently exist, or you are porting it to a new operating system or processor.
  * Do it yourself - you prefer to build applications from source where possible.
  * Security - for organizational policy, legal requirements, or operational security, programs from untrusted sources cannot be used, and must be recompiled locally. Or you want to confirm the compiler you have is the same as the one it produces, possibly running a checksum on the new one to confirm it matches the published one.



And the most obvious reason of all: 

  * New Version - you are creating a new release of the compiler.



This document will try to help you do these things. 

## Contents

  * 1 Overview
  * 2 Linux
    * 2.1 Downloading FPC binary packages
    * 2.2 Downloading FPC tar balls
    * 2.3 Downloading FPC sources from Git or zip
      * 2.3.1 Documentation sources
    * 2.4 Compiling the FPC source, using preinstalled FPC
    * 2.5 Compiling the FPC source, using just downloaded FPC 3.2.2
  * 3 FreeBSD
    * 3.1 Installing from the ports collection
    * 3.2 Installing from tar
  * 4 OpenBSD
    * 4.1 Installing from tar
  * 5 Installing from source on BSD/Linux
  * 6 Windows
    * 6.1 Installing an FPC release with the Windows installer
    * 6.2 Installing from source
      * 6.2.1 Obtaining the source with a command line git client
        * 6.2.1.1 Source for a release version of FPC
        * 6.2.1.2 Source for a non-release version of FPC
      * 6.2.2 Obtaining a bootstrap compiler
      * 6.2.3 Compiling the FPC source
  * 7 macOS
    * 7.1 Installing an FPC release from the official macOS packages
      * 7.1.1 Step 1: Download Xcode (optional)
      * 7.1.2 Step 2: Xcode Command Line Tools
      * 7.1.3 Step 3: Download FPC
    * 7.2 Installing from source
      * 7.2.1 FPC development version
      * 7.2.2 FPC Fixes 3.2
    * 7.3 Other installation options
  * 8 Testing the FPC Install
  * 9 Useful command-line switches for compiling from source
  * 10 Installing cross compilers
  * 11 See also



## Overview

There are a number of ways to install the Free Pascal Compiler and its libraries on your system. The current version and trunk, and sometimes beta and release candidates, are available directly from the [FPC website](<https://sourceforge.net/projects/freepascal/files/>) (tar, exe, rpm, dmg, pkg files). In addition, versions of FPC are provided (including rpm and deb files) from the [Lazarus download site](<http://sourceforge.net/projects/lazarus/files/>). Linux users can almost certainly find FPC in their distribution's repositories but may find that it is outdated. 

It is possible to build FPC from source, typically each release needs to be built with the previous release. It is possible to install FPC in the application space (often requiring root or administrator access) or in a your own user space. These things depend on your particular operating system. 

There is an extensive discussion of how to install and build Free Pascal compilers available here <http://www.stack.nl/~marcov/buildfaq.pdf> \- it may be a little too detailed for some users, but it is very comprehensive. 

## Linux

### Downloading FPC binary packages

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** You have a number of way to install FPC presented here, which one is best for you depends very much on what you are planning to do and if you need a very current version. If you are also planning on installing Lazarus, perhaps a quick look at [Installing_Lazarus](<Installing_Lazarus.md> "Installing Lazarus") and [Installing_Lazarus_on_Linux](<Installing_Lazarus_on_Linux.md> "Installing Lazarus on Linux") is a very good idea. 

With the recent release of FPC 3.2.2, only a few distributions will have the new FPC compiler in its repositories. You can do one of - 

  * If you use **rpm packages** , you can find FPC322 in <https://sourceforge.net/projects/freepascal/files/Linux/3.2.2/> or more generally in <https://www.freepascal.org/download.html> You might install using a command like -
        
        yum localinstall fpc-3.2.2-1.x86_64.rpm
        // or perhaps
        rpm -Uvh fpc-3.2.2-1.x86_64.rpm
        

  * If you use **deb packages** try the ones made available by, but not bundled with Lazarus at <https://sourceforge.net/projects/lazarus/files/>. Install using gdebi or, perhaps this command (note the ./ in front of filename, if you don't use an explicit path to filename, apt will search its own official repo, not what you want)-
        
        apt install ./fpc-laz_3.2.2-1_amd64.deb
        

Note the actual name changes from time to time, use the name you downloaded !



If you already have a FPC installed from your distro package manager, you should use that same package manager to remove them before installing under another model. Some quite strange things can happen if your compile uses a mixture of FPC versions ! 

### Downloading FPC tar balls

Another option is installing from a tar, an easy and possibly even more useful model. These tars are available for a wide range of Unix like systems. You need to download the appropriate binary tar for your OS and CPU and you may also need the source files, common for all OS/CPU. You install FPC this way in your own space, not as root. 

Here is a series of commands, not a script, that will install FPC on a 64 bit Linux system. It could be scripted but would would need sanity and error checking. Note that I like to install things like this in my $HOME/bin directory, if you prefer having it in $HOME, it is even easier, I am sure you can see the differences. 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** A tar install **does not resolve dependencies** , if they are not already present, first install binutils, make, gcc. 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** A tar ball install will, if you manage paths properly, allow you to have several versions of FPC installed.

(if you want something a bit newer, please see <https://www.freepascal.org/develop.html> ) 
    
    
    cd
    mkdir -p bin/FPC/Tars
    mkdir bin/FPC/SRC
    mkdir bin/FPC/fpc-3.2.2
    cd bin/FPC/Tars
    # download src and compiler tars, change for different CPU (or Mac ?) 
    
    wget https://sourceforge.net/projects/freepascal/files/Source/3.2.2/fpc-3.2.2.source.tar.gz
    wget https://sourceforge.net/projects/freepascal/files/Linux/3.2.2/fpc-3.2.2.x86_64-linux.tar
    
    tar xf fpc-3.2.2.x86_64-linux.tar
    cd fpc-3.2.2.x86_64-linux
    ./install.sh
    # when asked where to install, enter $HOME/bin/FPC/fpc-3.2.2, accept all defaults after that.
    
    cd ../../SRC
    tar xzf ../Tars/fpc-3.2.2.source.tar.gz
    
    # Set a path to where the compiler is, add line at end of .bashrc, if you don't use bash, adjust !
    # (! e.g. Use ~/.profile instead of ~/.bashrc so that the path is available also in Lazarus IDE.)
    echo "PATH=\"\$HOME/bin/FPC/fpc-3.2.2/bin\":\"\$PATH\"" >> ~/.bashrc
    source ~/.bashrc
    
    # trivial test
    cd 
    fpc -iV
    

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** The second wget above assumes you are working with an amd64 (AMD or Intel) type machine, other platforms require a different tar ball. For example, for a **Raspberry Pi** , you would use - 
    
    
    wget https://sourceforge.net/projects/freepascal/files/Linux/3.2.2/fpc-3.2.2.arm-linux.tar
    

which has the armhf 32bit compiler. Incidentally, the Raspberry Pi may need you to increase swap space to at least a Gig if you plan to later build any large applications with FPC.

### Downloading FPC sources from Git or zip

If you plan to build your own compiler from source, this is the approach for you. 

The FPC source files are stored in a git repository (<https://gitlab.com/freepascal.org/fpc/source>). The repository keeps track of all the changes by developers in the source tree and has multiple branches. Once you have the sources, please see Installing from source under BSD/Linux for instructions on how to install them. 

Anyone can download (but not upload) the source using either git or, easier if you want a one off snapshot, zip. And possibly even SVN. The default is to get the main (aka trunk) version of the source but you also select any one of the very many branches or tags. 

**Git** \- is most appropriate if you are going to add or sorrect something in the source and submit your change to the developers. Its a bigger and slower download but once done, easy to keep up to date. Note that its very unlikely contributions to anything other than main will be accepted. Here is an example initial download of main, assuming you already have git installed - 
    
    
    git clone <https://gitlab.com/freepascal.org/fpc/source>
    

Once the git download is finished, cd into the new directory and build as documented below. You can now keep your git repository up to date using the normal git commands. Git is well documented online but ([https://wiki.freepascal.org/FPC_git](<FPC_git.md>)) has some fpc specific help. 

**Zip** \- a possibly easier option is to download a "zip ball' archive of the source, from the web interface you can select which branch or tag you are interested in (combo box that defaults to 'main'). There are quite a lot of branches and tags to choose from, so please choose carefully. The web interface will let you download the zip of the selected branch or tag directly or command line people may do one of (with the last part of command line indicating which branch): 
    
    
    wget <https://gitlab.com/freepascal.org/fpc/source/-/archive/fixes_3_2/source-fixes_3_2.zip>
    wget <https://gitlab.com/freepascal.org/fpc/source/-/archive/release_3_2_2/source-release_3_2_2.zip>
    

You would unzip the zipball and build the source as described below. 

#### Documentation sources

The documentation sources are in a separate repository called fpcdocs, so the command to get them is: 
    
    
    cd ~
    git clone  https://gitlab.com/freepascal.org/fpc/documentation.git fpcdocs
    

If you want to learn more about subversion, read this excellent [Subversion book](<http://svnbook.red-bean.com/>) which is also available online in different formats for free. 

For more information, see the [Free Pascal website](<https://www.freepascal.org/develop.html>). 

### Compiling the FPC source, using preinstalled FPC

  * First install the release FPC from RPM/DEB or tarballs as described above.
  * Get FPC sources (trunk) as described above.
  * Make FPC sources with (execute in the source (trunk) directory):


    
    
    make clean
    make all
    make install INSTALL_PREFIX=install/to/directory
    

Decide about install/to/directory (default one is /usr/local) and also add options that you need. 

  * Check your ~/.fpc.cfg or /etc/fpc.cfg. It either has to be valid for the trunk compiler as well or you have to move it to the ../etc directory above your compiler executable. E.g. if your compiler is in ~/software/fpc/3.2.0/lib/fpc/3.2.0 (~/software/fpc/3.2.0/bin), you have to move the fpc.cfg to ~/software/fpc/3.2.0/lib/fpc/etc.
  * If you have no valid global fpc.cfg file, you have to create one in install/to/directory/lib/fpc/etc. For this run the following commands


    
    
    cd install/to/directory/lib/fpc
    mkdir etc
    fpcmkcfg -d basepath="install/to/directory/lib/fpc/\$fpcversion" -o etc/fpc.cfg
    

Uninstall (assuming default INSTALL_PREFIX is used): 
    
    
    sudo rm -rf /usr/local/bin/fp*
    sudo rm -rf /usr/local/bin/pas2fpm
    sudo rm -rf /usr/local/bin/instantfpc
    sudo rm -rf /usr/local/lib/fpc
    sudo rm -rf /usr/local/share/doc/fpc*
    sudo rm -rf /usr/local/lib/*/fpc
    

### Compiling the FPC source, using just downloaded FPC 3.2.2

  * Download FPC 3.2.2 (latest official release), ie tarball fpc-3.2.2.x86_64-linux.tar.
  * Unpack this tarball to e.g. "~/Downloads/fpc", do CD to the folder containing the file "install.sh", run from Terminal:


    
    
    $ ./install.sh
    

It will ask several questions in the Terminal, answer them. You will have installed FPC 3.2.2 to the folder e.g. "/home/user/fpc-3.2.2". 

  * Download the Git snapshot of FPC:


    
    
    $ git clone <https://gitlab.com/freepascal.org/fpc/source>
    

  * Do CD to new folder "source" and call:


    
    
    $ make clean PP=/home/user/fpc-3.2.2/bin/fpc
    $ make all PP=/home/user/fpc-3.2.2/bin/fpc
    

Here you must specify the "PP=" key and the full path to the "fpc" file installed from tarball. Command "make clean" is required if you **compile again**. 

Final compiled binary will be the file "source/compiler/ppcx64". 

## FreeBSD

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** If you wish to also install Lazarus, you can omit installing FPC with the steps below as the Lazarus port will install it for you. See [Installing Lazarus on FreeBSD](<Installing_Lazarus_on_FreeBSD.md> "Installing Lazarus on FreeBSD").

### Installing from the ports collection

The FreeBSD ports collection has FPC v3.2.3 version in `/usr/ports/lang/fpc` and FPC Development v3.3.1 version in `/usr/ports/lang/fpc-devel`. Nowadays all units were merged into only one package of FPC/FPC-devel. The FPC/FPC-devel source is now installed by default; it previously needed to be copied and uncompressed from `/usr/ports/distfiles/freepascal`. 

This must be done as root. 
    
    
    # cd /usr/ports/lang/fpc && make install && make clean
    

If you want use development (aka trunk) version of fpc with all units included do the following: 
    
    
    # cd /usr/ports/lang/fpc-devel && make install clean
    

Also, you can install FPC and units from pkg binaries: 
    
    
    # pkg install lang/fpc
    

Installing development FPC and units from pkg binaries: 
    
    
    # pkg install lang/fpc-devel
    

Once FPC is installed you can check if it's working by simply running as a normal user: 
    
    
    $ fpc test
    

which should produce output similar to this: 
    
    
     Free Pascal Compiler version 3.2.3 [2024/06/17] for x86_64
     Copyright (c) 1993-2024 by Florian Klaempfl and others
     Target OS: FreeBSD for x86-64
     Compiling test
     Fatal: Cannot open file "test"
     Fatal: Compilation aborted
     Error: /usr/local/bin/ppcx64 returned an error exitcode
    

### Installing from tar

Select the appropriate 32 bit/64 bit tar file based on your system. For x86_64 64-bit FreeBSD, download from [x86_64](<https://www.freepascal.org/down/x86_64/freebsd.html>). For i386 32-bit FreeBSD, download from [i386](<https://www.freepascal.org/down/i386/freebsd.html>). 

To install Free Pascal from a terminal: 
    
    
    $ fetch ftp://ftp.freepascal.org/pub/fpc/dist/3.2.2/x86_64-freebsd/fpc-3.2.2.x86_64-freebsd11.tar
    $ tar -xf fpc-3.2.2.x86_64-freebsd11.tar
    $ cd fpc-3.2.2.x86_64-freebsd
    $ bash install.sh
    

Replace the desired Free Pascal version (3.0.4, 3.2.0 or 3.2.2) and architecture (x86_64 or i386) as required. 

If you want to install the Free Pascal Compiler globally, for example in `/usr/local`, run the install.sh script as root. `install.sh` script uses bash shell syntax. If you get `Bad substitution` error, make sure you run script with bash. 

If you are on FreeBSD 12 or newer, the default linker is the `lld`. This may cause problems if you have code that uses, for example, the _cthreads_ unit as in the following program: 
    
    
    uses cthreads;
    begin
        writeln('hello'); 
    end.
    

The executable generated for this program will cause a segmentation fault when run. To fix this issue, you need to install the GNU linker: 
    
    
    root# pkg install binutils
    

and add a symbolic-link from `/usr/local/bin/ld.bfd` to `/bin/ld.bfd`. 

## OpenBSD

### Installing from tar

Select the appropriate 32 bit/64 bit tar file based on your system. For x86_64 64-bit OpenBSD, download from [x86_64](<https://sourceforge.net/projects/freepascal/files/OpenBSD/3.2.2/fpc-3.2.2.x86_64-openbsd.tar>). For i386 32-bit OpenBSD, download from [i386](<https://sourceforge.net/projects/freepascal/files/OpenBSD/3.2.2/fpc-3.2.2.i386-openbsd.tar>). 

To install Free Pascal from a terminal: 
    
    
    $ wget https://sourceforge.net/projects/freepascal/files/OpenBSD/3.2.2/fpc-3.2.2.x86_64-openbsd.tar
    $ tar -xf fpc-3.2.2.x86_64-openbsd.tar
    $ cd fpc-3.2.2.x86_64-openbsd
    $ bash install.sh
    

Replace the desired Free Pascal version (3.0.4, 3.2.0, or 3.2.2) and architecture (x86_64 or i386) as required. 

If you want to install the Free Pascal Compiler globally, for example in `/usr/local`, run the install.sh script as root. `install.sh` script uses bash shell syntax. If you get `Bad subtitution` error, make sure you run script with bash. OpenBSD uses ksh as default shell. You need to install bash manually. 

## Installing from source on BSD/Linux

Effectively, you need: 

1\. A file with all FPC sources downloaded from git (<https://gitlab.com/freepascal.org/fpc/source>) or perhaps (<https://sourceforge.net/projects/freepascal/files/Source/3.2.2/fpc-3.2.2.source.tar.gz>). 

2\. A starting (bootstrap) FPC compiler. An FPC release can always be built by the previously released FPC version, and FPC trunk can always be built by the current FPC release. You can download a bootstrap Free Pascal Compiler or use your distribution's package management/software system to install one. 

FPC build process: 

  * Fetch necessary files (starting compiler), FPC source file or source svn directory
  * If using FPC source files: extract/de-tgz in work directory,
  * Build: enter work/fpc/ and run:


    
    
    # Linux use:   
    export MAKE=`which make` ; echo $MAKE 
    # FreeBSD use (default csh, or tcsh):
    set MAKE=`which gmake` ; echo $MAKE
    # FreeBSD use (bash):
    export MAKE=`which gmake` ; echo $MAKE
    $MAKE all OPT='-gl' FPC=/path/to/startingcompiler-name-ppcx64
    # $MAKE is make on Linux and gmake on BSD 
    # /path/to/ can be omitted when ppc386 (32 bit) or ppcx64 (64 bit) is in the path
    

  * Install FPC. Again in work/fpc, run


    
    
    $MAKE install FPC=compiler/ppcx64 PREFIX=$THEPREFIX
    #replace the FPC=compiler/ppcx64 (or ppc386 for 32 bit) with the relevant compiler if not on Intel x86
    #THEPREFIX= usually is /usr/local or just /usr, but eg on NetBSD it is /usr/pkg for ports)
    

  * Create a symlink:


    
    
    ln -s $THEPREFIX/lib/fpc/3.2.2/ppcx64 $THEPREFIX/bin/ppcx64
    

  * Install sources:


    
    
    $MAKE install sourceinstall PREFIX=$THEPREFIX
    

  * Create a symlink for default FPC source path:


    
    
    ln -sf $THEPREFIX/share/src/3.2.2/fpc /usr/share/fpcsrc
    

  * Set up fpc.cfg configuration file:


    
    
    $THEPREFIX/lib/fpc/3.2.2/samplecfg $THEPREFIX/lib/fpc/3.2.0 $ETCDIR
    

  * Optionally test to see if `ppcx64 -i` (or whatever compiler your architecture uses) gives output, else give a warning that user needs to add $PREFIX/bin to the current path. Try to compile a program with `ppcx64 -viwn`, and see if that gives errors.



Notes: 

  * If you need fpcmake package lists, you need to generate or supply them yourself, (in the port, or in an extra archive) either way, do a dummy install to /tmp/pack and determine the files installed with 
        
        find . >ll
        



  * $THEPREFIX and $ETCDIR should be user configurable. Otherwise local installs aren't possible.


  * BSDHIER=1 on all make commands forces BSD hierarchy conventions.



## Windows

### Installing an FPC release with the Windows installer

[![Warning-icon.png](https://wiki.freepascal.org/images/b/b2/Warning-icon.png)](</File:Warning-icon.png>)

**Warning:** If you are intending to also install Lazarus using the official Windows installer which installs both FPC + Lazarus, please ignore these instructions and instead refer to [Installing Lazarus on Windows](<Installing_Lazarus_on_Windows.md> "Installing Lazarus on Windows") to avoid having to resolve any resulting conflicts.

By far the easiest way to get a working installation of the Free Pascal Compiler is to download and run the appropriate self-extracting Windows installer from the official [SourceForge repository for FPC 3.2.2](<https://sourceforge.net/projects/freepascal/files/Win32/3.2.2/>) \- this release contains the current release versions of the Free Pascal Compiler and the Free Pascal libraries. The Windows installer packages install very easily, and should work 'out-of-the-box'. You will be taken through a typical Windows installation wizard to install the FPC binaries and Free Pascal libraries: 

  * Choose an installation location for FPC (suggested `C:\FPC\3.2.2`)
  * Choose installation type (recommended Full Installation)
  * Choose file associations, configuration file creation etc
  * Install
  * View README



### Installing from source

Installing from source with a command line git client is not for novices, since you also need a working bootstrap compiler to be able to compile the Free Pascal Compiler from source. Instructions for obtaining a bootstrap compiler are included below. 

#### Obtaining the source with a command line git client

The easiest way to get the Free Pascal Compiler source is by using a git client. The exact commands may vary between git clients; the commands given below are for the command line git client downloadable from [GitForWindows.org](<https://gitforwindows.org/>). 

##### Source for a release version of FPC

First create a directory in which you'd like to keep the source. Any normal user can do this. Create a directory for FPC (eg `C:\Source`) by typing the following at a command prompt: 
    
    
    C:\Users\you> cd \
    C:\> mkdir Source
    C:\> cd Source
    

Now type issue the following command at a command prompt: 
    
    
    C:\Source> git clone -b release_3_2_2 https://gitlab.com/freepascal.org/fpc/source.git fpc-3.2.2
    

Wait while the git repository is downloaded to the fpc-3.2.2 directory in your newly created Source directory. This can take a while... have a look at the helpful [Free Pascal Forums](<https://forum.lazarus.freepascal.org/>) while you wait :-) You may also like to read the excellent [How to use the Forum](<Forum.md> "Forum") Wiki article which explains how to get answers to pesky questions quickly. New Forum users are always welcome. 

##### Source for a non-release version of FPC

There are two current non-release branches of the Free Pascal Compiler: the development (main) branch and the Fixes 3.2 branch which includes additional fixes to the released 3.2.2 version. Developers, and those who like living on the bleeding edge and testing new features and fixes, will choose the development version; more normal users, who wish to use a stable branch with some additional fixes since the latest release version, will choose the Fixes branch. The instructions below cover both these branches. 

If it does not already exist, create a directory in which you'd like to keep the FPC source (eg `C:\Source`). Any normal user can do this. Now type the following command at a command prompt: 

_For the Fixes branch of FPC 3.2_
    
    
    C:\Source> git clone -b fixes_3_2 https://gitlab.com/freepascal.org/fpc/source.git fpc-fixes-3.2.0
    

_For the Development (main) branch of FPC_
    
    
    C:\Source> git clone -b main https://gitlab.com/freepascal.org/fpc/source.git fpc-main
    

Hint: To update your local copy of the repository with subsequent changes, you need only do: 

_For the Fixes branch of FPC 3.2_
    
    
    C:\> cd C:\Source\fpc-fixes-3.2
    C:\Source\fpc-fixes-3.2> git clean -f -d
    C:\Source\fpc-fixes-3.2> git pull
    

_For the Development (main) branch of FPC_
    
    
    C:\> cd C:\Source\fpc-main
    C:\Source\fpc-trunk> git clean -f -d
    C:\Source\fpc-trunk> git pull
    

#### Obtaining a bootstrap compiler

To obtain a bootstrap Free Pascal Compiler, download the distribution package [fpc-3.2.2.i386-win32.exe](<https://www.freepascal.org/down/i386/win32.html>) and run it - it is a self-extracting installer, so just follow the instructions to install it. The installer should set the PATH environment variable as appropriate. 

Then restart windows. 

#### Compiling the FPC source

**Before you start**

  * The installation PREFIX in the instructions that follow is totally dependent on the directory in which you installed FPC. The Windows FPC installer uses a default location of `C:\FPC` and the FPC 3.2.2 release files are placed in `C:\FPC\3.2.2`. Since versions change relatively frequently, it is recommended that you just select and maintain a single PREFIX default location without any regard to FPC version numbers. A reasonable PREFIX to adopt is the one used by the Windows FPC installer (`C:\FPC`) but you must also make sure that the `C:\FPC\bin\i386-win32\` directory is added to your path environment variable.


  * Check that the `make.exe` which is found first in your path environment variable is the GNU one included with your bootstrap FPC compiler. If you have a recent version of Delphi installed, watch out for its version of `make` being earlier in your path. It does not understand the FPC Makefile and you will receive many errors if you try to use it. You can check this by opening a command prompt and typing `make -v`. The result should be (or very similar):


    
    
     [![fpc windows make.png](https://wiki.freepascal.org/images/8/8a/fpc_windows_make.png)](</File:fpc_windows_make.png>)
    

  * If you need to change your path so that the correct `make.exe` is found, you can either do it temporarily or permanently. 
    * To change it temporarily (it is in effect only until you close the current command prompt) to find `make.exe` in `C:\FPC\3.2.2\bin\i386-win32`, at a command prompt type: 
          
          C:\Source> set PATH=C:\FPC\3.2.2\bin\i386-win32;%PATH%
          

    * To change it permanently, open Control Panel > System and choose "Advanced system settings" under "Related settings" towards the bottom of the page. Now choose "Environment Variables..." in the System Properties dialog. Here you will probably need to change the Path that appears under "System variables" so that `C:\FPC\3.2.2\bin\i386-win32` occurs first.



**Instructions**

  * At a command prompt, navigate to your local FPC source directory (eg `C:\Source\fpc-3.2.2`) by typing:


    
    
    C:\> cd Source\fpc-3.2.2
    

  * Compile the FPC source with:


    
    
    C:/Source/fpc-3.2.2> make distclean all
    

  * To overwrite an existing FPC installation in, for example, `C:\FPC`) type:


    
    
    C:/Source/fpc-3.2.2> make install PREFIX=C:\FPC
    

  * If you also need the cross-compiler to x86_64, type the following commands which will compile the cross-compiler and then install it:


    
    
    C:/Source/fpc-3.2.2> make all OS_TARGET=win64 CPU_TARGET=x86_64 INSTALL_PREFIX=C:\FPC\3.2.2 PP=C:\FPC\3.2.2\bin\i386-win32\ppc386.exe DATA2INC=C:\FPC\3.2.2\bin\i386-win32\data2inc.exe
    
    C:/Source/fpc-3.2.2> make crossinstall OS_TARGET=win64 CPU_TARGET=x86_64 INSTALL_PREFIX=C:\FPC\3.2.2 PP=C:\FPC\3.2.2\bin\i386-win32\ppc386.exe DATA2INC=C:\FPC\3.2.2\bin\i386\win32\data2inc.exe
    

The cross-compiler to compile programs for Windows 64 bit may now be found here: `C:\FPC\3.2.2\bin\i386-win32\ppcrossx64`. 

Done! 

## macOS

[![Warning-icon.png](https://wiki.freepascal.org/images/b/b2/Warning-icon.png)](</File:Warning-icon.png>)

**Warning:** If you are intending to also install Lazarus using the official macOS installation packages which install both FPC + Lazarus, please ignore these instructions and instead refer to [Installing Lazarus on macOS](<Installing_Lazarus_on_macOS.md> "Installing Lazarus on macOS") to avoid having to resolve any resulting FPC conflicts.

### Installing an FPC release from the official macOS packages

#### Step 1: Download Xcode (optional)

Xcode is a 12GB download which will take up 16GB of disk space. You **only** need to download and install the full Xcode development environment **if** you need: 

  * the SDKs for **iOS** , **iPadOS** , watchOS and tvOS; or
  * to validate and upload apps to the Mac App Store; or
  * to [notarise](<Notarization_for_macOS_10.14.md> "Notarization for macOS 10.14.5+") apps for distribution outside of the Mac App Store.



Xcode 11.3.1 for use on macOS 10.14 Mojave must now be installed by downloading it from [Apple Developer Connection](<http://developer.apple.com/>) (ADC), which requires free registration. Xcode 11.4.x for use on macOS 10.15 Catalina can be installed from the [Mac App store](<https://apps.apple.com/us/genre/mac-developer-tools/id12002?mt=12>). Note that you must first move any old Xcode versions from the Applications folder into the trash or rename the Xcode app (eg Xcode.app to Xcode_1014.app). You can select which version of Xcode to use with the command line utility `xcode-select`. 

Older systems: 

The developer tools can be installed from the original macOS installation disks or a newer copy downloaded from the [Apple Developer Connection](<http://developer.apple.com/>) (ADC), which requires free registration. Download the Xcode file, it will end up in your Downloads directory as a zip file. Click it. It is unarchived into your Downloads directory. You may be happy with it there but maybe not. Other users will see the path to it but be unable to use it. And it is untidy there. So I moved mine and then told xcode-select where it was moved to (in a terminal) - 
    
    
    mv Downloads/Xcode.app /Developer/.
    sudo xcode-select -s /Developer/Xcode.app/Contents/Developer
    

#### Step 2: Xcode Command Line Tools

This is shown here as a separate step because it really is a separate step in addition to Step 1. Don't confuse this with the internal Xcode command line tools that the Xcode GUI will tell you are already installed. FPC does not use those Xcode internal command line tools, so do the following (it is quick and easy)- 
    
    
    sudo xcode-select --install
    sudo xcodebuild -license accept
    

You only need to enter the first of the two commands above unless you have also installed the full Xcode package. If you have only installed the command line tools, you should omit entering the _xcodebuild_ command. 

#### Step 3: Download FPC

Download the Free Pascal Compiler (FPC) and FPC source from the [Free Pascal website](<https://www.freepascal.org/download.html>). When you arrive at the download page, select a mirror and then choose the correct version for your operating system. 

These installation packages are built by the FPC developers and track formal releases. As these installation packages are not code signed nor notarized by Apple, you need to either control-click on the installation package and choose "Open" or right-click on the installation package and choose "Open" in the contextual menu and confirm you want to install from an Unknown Developer. 

### Installing from source

There are two current non-release branches of the Free Pascal Compiler: the development (trunk) branch and the Fixes 3.2 branch which includes additional fixes to the released 3.2.2 version. Developers, and those who like living on the bleeding edge and testing new features and fixes, will choose the development version; more normal users, who wish to use a stable branch with some additional fixes since the latest release version, will choose the Fixes branch. The instructions below cover both these branches. 

The source code is kept in a version control system called _git_ : 

  * macOS 10.5 and higher already contain a command line git client if you have installed the Xcode command line utilities.


  * You also need the latest released Free Pascal Compiler version (3.2.2 as of March 2022) installed in order to be able to successfully compile the development (trunk) version.



![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** **When building a native aarch64 Free Pascal Compiler (ppca64) for an Apple Silicon M1 processor Mac, change the CPU_TARGET in the instructions below from x86_64 (Intel 64 bit) to aarch64 (ARM 64 bit - Apple Silicon M1) and change any reference to ppcx64 to ppca64**

#### FPC development version

  * [FPC New Features in development](<FPC_New_Features_Trunk.md> "FPC New Features Trunk")
  * [FPC User Changes in development](<User_Changes_Trunk.md> "User Changes Trunk") \- may break existing code.



Note that since the development version of FPC (was known as "trunk" in SVN; now known as "main" in GIT) is by definition still under development, some of the features may still change before they end up in a release version. 

Create a directory where you would like to put the source (eg fpc_main in your home directory). You don't need to be root to do this. Any normal user can do this. Open an Applications > Utilities > Terminal and do the following: 
    
    
     []$ git clone -b main https://gitlab.com/freepascal.org/fpc/source.git fpc_main
    

This will create a directory called `fpc_main` and download the FPC main source to it. 

To subsequently update your local source repository with the latest source changes you can simply do: 
    
    
     []$ cd
     []$ cd fpc_main
     []$ git clean -f -d
     []$ git pull
    

To build and install FPC (the highlighted text should be all on one line): 
    
    
     []$ cd
     []$ cd fpc_main
     []$ make distclean all FPC=/usr/local/lib/fpc/3.2.2/ppcx64 OS_TARGET=darwin CPU_TARGET=x86_64 OPT="-XR/Library/Developer/CommandLineTools/SDKs/MacOSX.sdk/"
     []$ sudo make install FPC=$PWD/compiler/ppcx64 OS_TARGET=darwin CPU_TARGET=x86_64
    

You will also need to update the links for the compiler in `/usr/local/bin` which will be pointing to the previous FPC version. For example: 
    
    
     []$ cd /usr/local/bin
     []$ sudo rm ppc386
     []$ sudo rm ppcx64
     []$ sudo ln -s /usr/local/lib/fpc/3.3.1/ppc386
     []$ sudo ln -s /usr/local/lib/fpc/3.3.1/ppcx64
    

Note that you will need to build a new `ppc386` compiler if you want to continue to compile 32 bit applications by replacing these lines (this is not be possible after Xcode 11.3.1 and macOS 10.14.6 Mojave because of Apple's removal of the 32 bit frameworks): 
    
    
     []$ make clean all FPC=/usr/local/lib/fpc/3.2.2/ppcx64 OS_TARGET=darwin CPU_TARGET=x86_64 OPT="-XR/Library/Developer/CommandLineTools/SDKs/MacOSX.sdk/"
     []$ sudo make install FPC=$PWD/compiler/ppcx64 OS_TARGET=darwin CPU_TARGET=x86_64
    

with these two lines: 
    
    
     []$ make clean all FPC=/usr/local/lib/fpc/3.2.2/ppc386 OS_TARGET=darwin CPU_TARGET=i386 OPT="-XR/Library/Developer/CommandLineTools/SDKs/MacOSX.sdk/"
     []$ sudo make install FPC=$PWD/compiler/ppc386 OS_TARGET=darwin CPU_TARGET=i386
    

#### FPC Fixes 3.2

Create a directory where you would like to put the source (eg `fpc_fixes32` in your home directory). You don't need to be root to do this. Any normal user can do this. Open an Applications > Utilities > Terminal and do the following: 
    
    
     []$ git clone -b fixes_3_2 https://gitlab.com/freepascal.org/fpc/source.git fpc_fixes32
    

This will create a directory called `fpc_fixes32` and download the FPC source to it. 

To subsequently update your local copy of the repository source with the latest source changes you can simply do: 
    
    
     []$ cd fpc_fixes32
     []$ git clean -f -d
     []$ git pull
    

To build and install FPC (the highlighted text should be all on one line): 
    
    
     []$ cd
     []$ cd fpc_fixes32
     []$ make distclean all FPC=/usr/local/lib/fpc/3.2.2/ppcx64 OS_TARGET=darwin CPU_TARGET=x86_64 OPT="-XR/Library/Developer/CommandLineTools/SDKs/MacOSX.sdk/"
     []$ sudo make install FPC=$PWD/compiler/ppcx64 OS_TARGET=darwin CPU_TARGET=x86_64
    

You will also need to update the links for the compiler in `/usr/local/bin` which will be pointing to the previous FPC version. For example: 
    
    
     []$ cd /usr/local/bin
     []$ sudo rm ppc386
     []$ sudo rm ppcx64
     []$ sudo ln -s /usr/local/lib/fpc/fixes-3.2/ppc386
     []$ sudo ln -s /usr/local/lib/fpc/fixes-3.2/ppcx64
    

Note that you will need to build a new `ppc386` compiler if you want to continue to compile 32 bit applications by replacing these lines (this may not be possible after Xcode 11.3.1 and macOS 10.14.6 Mojave because of Apple's removal of the 32 bit frameworks): 
    
    
     []$ make clean all FPC=/usr/local/lib/fpc/3.2.2/ppc386 OS_TARGET=darwin CPU_TARGET=i386 OPT="-XR/Library/Developer/CommandLineTools/SDKs/MacOSX.sdk/"
     []$ sudo make install FPC=$PWD/compiler/ppc386 OS_TARGET=darwin CPU_TARGET=i386
    

### Other installation options

See [Other FPC Installation Options](<Other_macOS_installation_options.md> "Other macOS installation options"). 

## Testing the FPC Install

You might like to try a simple and quick test of FPC at this stage. From the command line (Mac - Open an Application > Utilities > Terminal) start an editor (e.g. pico or nano), paste this very short program and save it as the file `helloworld.pas` (using the WriteOut command in GNU nano): 
    
    
    program helloworld;
    begin
    	writeln('hello world !');
    end.
    

Now exit the editor and compile this simple code by typing this command, before pressing `↵ Enter`: 
    
    
    fpc helloworld.pas
    

It should very quickly make an executable called, you guessed it, "helloworld". Run this executable by typing this command and then pressing `↵ Enter`: 
    
    
    ./helloworld
    

If that worked, well done! 

## Useful command-line switches for compiling from source

  * Significantly speed up the FPC source tree compilation 
    * `FPMAKEOPT="-T x"` (x = the number of CPU cores you have)


  * Reduce the number of times the compiler itself gets recompiled 
    * `NOWPOCYCLE=1`


  * Add debug information and disable optimizations during compilation of the compiler/RTL 
    * `OPT="-gl -O-"`



## Installing cross compilers

A cross compiler allows you to create binaries (executables) for a platform different from the platform being used for compilation. For example, working under macOS and creating executables for Win64 or Win32 (only up to and including macOS 10.14.6 due to Apple's removal of 32 bit frameworks). For details on how to do this, see [Cross Compiling](<Cross_compiling.md> "Cross compiling"). 

## See also

  * [FPC Wiki Documentation](<FPC.md> "FPC").
  * [FPC Official Documentation](<https://www.freepascal.org/docs.html>).
  * [FPC 3.2.2 New Features](<FPC_New_Features_3.2.md> "FPC New Features 3.2.2").
  * [FPC 3.2.2 User Changes](<User_Changes_3.2.md> "User Changes 3.2.2") \- may break existing code.
  * [FPC New Features in development version](<FPC_New_Features_Trunk.md> "FPC New Features Trunk").
  * [FPC User Changes in development version](<User_Changes_Trunk.md> "User Changes Trunk") \- may break existing code.

---

_Source: [https://wiki.freepascal.org/Installing_the_Free_Pascal_Compiler](https://web.archive.org/web/20250403110941/https://wiki.freepascal.org/Installing_the_Free_Pascal_Compiler)_
