# Deploying Your Application

│ **English (en)** │  **[español (es)](</Deploying_Your_Application/es> "Deploying Your Application/es")** │  **[français (fr)](</Deploying_Your_Application/fr> "Deploying Your Application/fr")** │  **[português (pt)](</Deploying_Your_Application/pt> "Deploying Your Application/pt")** │    
****

You've created your application in Lazarus, tested within the IDE. Now it's time to deploy it to test it on a specific platform. 

## Contents

  * 1 Windows Deployment
    * 1.1 InnoSetup
    * 1.2 HJ-Install
    * 1.3 LizaJet Installer
    * 1.4 NSIS (Nullsoft Scriptable Install System)
    * 1.5 Windows Installer XML (WiX) toolset
    * 1.6 Code signing
  * 2 Linux Deployment
    * 2.1 Creating a RPM Package on Linux
      * 2.1.1 Setting up your build environment
      * 2.1.2 Creating a binary only package
        * 2.1.2.1 Tip:
      * 2.1.3 Creating a source and binary package
    * 2.2 Creating a Debian Package on Linux
    * 2.3 Creating a Pacman (Arch) Package
    * 2.4 Automating the creation of RPM and DEB packages
    * 2.5 Snap
    * 2.6 Flatpak
    * 2.7 GUInseng
    * 2.8 The Zero Install system
    * 2.9 Simple deployment of Lazarus application directly to Desktop Environment (e.g., KDE Plasma 5/Manjaro)
    * 2.10 See also
  * 3 Creating a Makefile for your Lazarus software
  * 4 macOS Deployment
    * 4.1 An Application Bundle
    * 4.2 Using Packages on macOS
    * 4.3 Installing X11 and GTK on macOS
    * 4.4 Code signing and Notarization
    * 4.5 See also
  * 5 Android Deployment
  * 6 Apple iOS Deployment
  * 7 Other, General Cross-Platform Deployment
    * 7.1 BitRock
    * 7.2 fpGUI Installer
    * 7.3 InstallJammer
    * 7.4 CMake/CPack



## Windows Deployment

### InnoSetup

[Inno Setup](<Inno_Setup.md> "Inno Setup") [[1]](<http://www.jrsoftware.org/>) is a free installer for Windows programs. It may be used completely free of charge, even when deploying commercial applications, full Delphi source code is available. See [Inno Setup](<Inno_Setup.md> "Inno Setup") for more details and links to the Lazarus installer that is written using Inno. 

Features: 

  * Inno supports all versions of Windows in use today -- Windows 95, 98, ME, NT4, 2000, 2003, XP, Vista, 7, 8. The latest version that can create installers for Windows versions before Windows 2000 is version 5.4. See [[2]](<http://www.jrsoftware.org/files/is5-whatsnew.htm>)
  * It can compare file version info, replace in-use files, use shared file counting.
  * It can register DLL/OCXs and type libraries
  * It can install fonts.
  * It can create shortcuts anywhere, including in the Start Menu and on the desktop.
  * It can create registry and .INI entries
  * It has integrated Pascal scripting engine that allows e.g. manipulation of COM/ActiveX objects (e.g. MS Word), setting firewall rules etc.
  * It supports multilingual installs
  * It supports passworded and encrypted installs
  * It supports silent install and uninstall.



While it is free for use, Inno Setup is copyrighted software, not public domain software. There are some restrictions on distribution and use; see the <http://www.jrsoftware.org/files/is/license.txt> file for details. 

Inno Setup is used to create the Lazarus installer for Windows. 

### HJ-Install

HJ-Install is a freeware installer for Windows 95, 98, ME, NT 4.0, 2000 and XP. It is created with Internet distributed, CD-Rom, single-floppy and multi-floppy installations in mind, but it can also be used in network situations and as a stand-alone scripting engine. The installer is small and adds only 138 Kb. It doesn't support changing the registry and ini files, and un-install. See <http://www.freebyte.com/hjinstall/>

### LizaJet Installer

LizaJet Installer is a commercial installer, but a free edition is available. Uses Object Pascal for scripting. See <http://www.lizajet.com>

### NSIS (Nullsoft Scriptable Install System)

A professional open source system to create Windows installers. A full-featured NSIS installer has an overhead of only 34 KB. See <http://nsis.sourceforge.net/Main_Page> and <http://hmne.sourceforge.net/>

### Windows Installer XML (WiX) toolset

A toolset that builds Windows installation packages from XML source code. The toolset supports a command line environment that developers may integrate into their build processes to build MSI and MSM setup packages. See <http://wix.sourceforge.net/>

### Code signing

For details of code signing Windows executables to avoid your end users receiving warnings that the publisher of the software is "unknown" when attempting to run your executable, see [Code Signing for Windows](<Code_Signing_for_Windows.md> "Code Signing for Windows"). 

## Linux Deployment

### Creating a RPM Package on Linux

On Linux, the best way to deploy applications is by using the native package system. Of the many package systems available, RPM (RedHat Package Manager) is the most commonly utilized format, and is defined as the standard one in the Linux Standard Base. 

To create a RPM file, you need to create a spec text file with the information necessary to build the software, and also an environment to build that spec. The best editor to create RPM packages is Emacs, because it recognizes the spec extension, highlights the text appropriately and even has a menu option to build rpm packages. 

To build the rpm file use either the emacs menu or this command line (man rpmbuild for more information): 
    
    
    rpmbuild -ba --clean $HOME/RPM/SPECS/myprogram.spec
    

#### Setting up your build environment

RPM Packages are installed as root to the base system (/ directory), but an accident as root can destroy your machine. To avoid this, packages can be build as a normal user. The build process includes a real install, to make sure the package works, but this install is made to a fake root directory represented by the $RPM_BUILD_ROOT variable. 

First, go to your home directory (or another directory inside it) and create the following directory structure: 

  * RPM
  * RPM/BUILD - This directory is utilized by RPM to build the package.
  * RPM/RPMS - Here you can find binary RPMs after you build them.
  * RPM/SOURCES - Place your compressed tar files and patches here.
  * RPM/SPECS - Place all your spec files here.
  * RPM/SRPMS - Here you can find source RPMs after you build them.



Next you will need to create a configuration file to tell rpm builder software where he can find your build directories. Go to your home directory and create a file named .rpmmacros and place the text below on it. Make sure to change the directories to the correct ones on your system. 
    
    
    %_topdir                /home/felipe/RPM/
    %_tmppath               /home/felipe/tmp
    
    %_signature             gpg
    %_gpg_name              Mandrakelinux
    %_gpg_path              ~/.gnupg
    

#### Creating a binary only package

The easiest way to create a RPM package is to make it install already compiled software. There are some reason why we might want to avoid compiling the software in the spec file: 

  * It requires creating a Makefile and makefiles are complex


  * Some packages don't have any binary software in them, so they don't need to be built.



Each RPM Package contains a single compressed tar archive. Place the archive under the RPM/SOURCES directory. Zip, gz and bz2 compressions should work ok. The file can either contain the full source of the project if you with to create a source and a binary package or a directory with the files already in place, like if they were installed in the user machine, if you with to create a binary only package. 

To build a binary package open a spec file with Emacs text editor. On Emacs open the menu "RPM spec" --> "RPM Build" --> "Build Binary Package". This will create a .rpm file under the directory RPM/RPMS 

Below is a spec file that doesn't build the software. In this case the software is a Braille Course composed of html and Macromedia Flash files. Flash files cannot be build on Linux, so it's not possible to create a source package in this case. 
    
    
    %define name    braillevirtual 
    %define version 1.0 
    %define release mdk 
    %define dir     braillevirtual
    %define root    /home/felipe/tmp/ROOT/
    
    Summary:   Curso On-Line de Braille
    Name:      %{name}
    Version:   %{version}
    Release:   %{release}
    Vendor:    Laboratório de Brinquedos da Faculdade de Educação da USP
    URL:       www.braillevirtual.fe.usp.br
    License:   Distribuível livremente
    Group:     Books/Other
    Packager:  Felipe Monteiro de Carvalho
    Source0:   home/felipe/Programacao/SOURCES/braillevirtual.tar.bz2
    BuildRoot: %{root}
    
    %description
    O Braille Virtual é um curso on-line público e gratuito destinado à difusão e ensino do sistema Braille de leitura e escrita para cegos a pessoas que vêem. É orientado especialmente a pais, crianças, professores e funcionários de escolas inclusivas e pretende facilitar a comunicação entre estas pessoas e as pessoas com cegueira.
    
    %prep
    cd $RPM_SOURCE_DIR
    cp braillevirtual.tar.bz2 $RPM_BUILD_DIR
    cd $RPM_BUILD_DIR
    rm -rf %{dir}
    rm -f braillevirtual.tar
    bunzip2 -d braillevirtual.tar.bz2
    tar -xvf braillevirtual.tar
    
    %install
    mkdir -p $RPM_BUILD_ROOT/usr/share/
    rm -rf $RPM_BUILD_ROOT/usr/share/%{dir}
    cp -r $RPM_BUILD_DIR/%{dir} $RPM_BUILD_ROOT/usr/share/
    
    %clean
    rm -rf $RPM_BUILD_DIR/*.*
    
    %files
    /usr/share/%{dir}/
    
    %changelog
    
    * Mon Oct 24 2005 1.0-mdk.noarch.rpm
    
    - Nova atualização do pacote de instalação. Inclusão dos exercícios avançados.
    
    * Wed May 12 2005 1.0-mdk.noarch.rpm
    
    - Atualizei o pacote para refletir as mudanças no site. Várias pequenas mudanças.
    
    * Sun May 05 2005 1.0-1mdk.i586.rpm
    
    - O pacote de instalação do braille Virtual para linux é criado.
    

##### Tip:

The following packages are required for an very basic GUI Application in GTK+ (binary only): 
    
    
     libc6                libx11-6         libgdk-pixbuf2.0-0         libgtk2.0-0         libglib2.0-0
     libpango-1.0-0       libcairo2	libatk1.0-0                libxcb1             libglib2.0-0
     libpangocairo-1.0-0  libxfixes3       libpangoft2-1.0-0          libfontconfig1      libxrender1
     libxinerama1         libxi6           libxrandr2                 libxcursor1         libxcomposite1
     libxdamage1          libxext6         libffi6                    libpcre3            libthai0
     libpixman-1-0        libfreetype6     libpng12-0                 libxcb-shm0         libxcb-render0
     zlib1g               libxau6          libxdmcp6                  libselinux1         libharfbuzz0b
     libexpat1            libdatrie1       libgraphite2-3
    

You can get a list of used libraries with: `ldd [executable of your project]`. 

And find the packages in which it belongs: 

RPM based: `rpm -qf <lib name>`

DEB based: `dpkg -S <lib name>`

Where <lib name> is the name of library, before => or ")" in list 

Example: `rpm -qf libgtk-x11-2.0.so.0`

A script (DEB based) using pipes, in order to join all these commands in a single command line, could be (replace 'amd64' by 'i386' if you install 32 bits packages): 
    
    
     $ ldd yourAppli | awk '/=>/{print $(NF-1)}' | while read n; do dpkg -S $n; done | awk '{print $1}' | sed 's/:amd64://' | sort | uniq | while read n; do echo "-- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --> Usefullness of the package named $n:"; apt-cache search ^$n; echo "...which installs the following libraries on the system:"; dpkg -L $n |grep .so; echo "...and which depends itself on the following packages on the system:"; apt show $n | grep Depends; done;
    

#### Creating a source and binary package

Linux distributions only accept RPM packages that can build the software. This will probably require creating build scripts and/or a Makefile. For more information see the section Creating a Makefile for your Lazarus software. 

You need to use the %build section to compile the software. Below is an example of a spec file capable of building the software. Be very careful when studying this as every little detail is important and follows a precise pattern. 
    
    
      Name:           magnifier
      Version:        3.2.1
      Release:        1 
      Summary:        Virtual Magnifying Glass
      Group:          Accessibility
      License:        GPL Version 2
      URL:            http://magnifier.sourceforge.net
      Source0:        http://internap.dl.sourceforge.net/sourceforge/magnifier/magnifier-3.2.1.zip
      BuildRoot:      %{_tmppath}/%{name}-%{version}-%{release}-root-%(%{__id_u} -n)
      BuildRequires:  fpc >= 2.0.2, lazarus >= 0.9.12
      Requires:       ImageMagick
    
      %description
      Virtual Magnifying Glass is a free, open source, multiplatform, screen magnification tool. It is simple, customizable, and easy-to-use.
    
      %prep
      %setup -q magnifier
      %build
      make TARGET=%{_target_cpu}
    
      %install
      ./install.sh DESTDIR=$RPM_BUILD_ROOT
    
      %clean
      rm -rf $RPM_BUILD_ROOT/
    
      %files
      %defattr(-,root,root,-)
      /usr/share/magnifier/
      /usr/bin/magnifier
    
      %changelog
      * Wed Feb 15 2006 Felipe Monteiro de Carvalho <felipemonteiro.carvalho at gmail.com> - 3.2-mdk.i386.rpm
      - The Linux RPM package is created.
    

### Creating a Debian Package on Linux

Debian packages are similar to installers. They can be downloaded by users from a website or ftp. You can craft them manually from the binary file by using their [specific structure](<Debian_package_structure.md> "Debian package structure") and a .deb extension. However, they will not be automatically included in Debian distributions. 

If you want to make a package available into Debian distributions, you need to provide a [source upstream](<Debian_upstream.md> "Debian upstream") that can generate the Debian package as well as other files like the .orig.tar.gz and .dsc file. Those will be necessary for anybody to build your package for their target platform. 

There is an IDE plugin here: <https://github.com/prof7bit/LazPackager>

### Creating a Pacman (Arch) Package

Please see [https://wiki.freepascal.org/pacman](<pacman.md>)

### Automating the creation of RPM and DEB packages

To simplify the work of creating RPM and DEB packages one can create a script which generates them, just like is done here: 

<http://sourceforge.net/p/magnifier/code/HEAD/tree/trunk/build/build_package.sh>

### Snap

See [Create snap package](<Create_snap_package.md> "Create snap package"). 

### Flatpak

See [Flatpak](<Flatpak.md> "Flatpak"). 

### GUInseng

[GUInseng](<http://www.guinseng.de/>) is a a GTK+ based installer for Linux released under GPL. 

### The Zero Install system

[The Zero Install system](<http://0install.net/>) is a Python based install system that aims to provide a better way of installing software than traditional systems (tarballs, apt-get, bundles, setup.exe, etc). 

### Simple deployment of Lazarus application directly to Desktop Environment (e.g., KDE Plasma 5/Manjaro)

One of the first things anyone coming from Delphi/Windows environment wants to know is how to QUICKLY deploy a newly-created Linux GUI program outside of the IDE, without spending a lot of time having to first study various package management systems. Unlike Windows, Linux is a disparate collection of Distributions ("Distros" - e.g., Debian, Arch, Ubuntu, etc.) and Desktop Environments ("DE" - e.g., KDE, Gnome, XFCE, Mate, etc.) Deployment entails some knowledge of one's Disto and DE, however the learning curve and effort is a tiny fraction of the effort required to set up and use an installation package system (e.g., DEB or RPM installation systems). The following focuses on Manjaro (ArchLinux) Distribution running the KDE Plasma 5 Desktop Environment. Before getting started, the following are required. 

_Prerequisites_

**You must know the Path to your application's Binary.** It will be located in the Target directory that you specified under _Project Options_. If you just left default settings, the Binary will be located for example for Linux x86-64 target at: _[...]/MyProgram/lib/x86_64-linux_. The file will generally not have any extension (no _.exe_) and will have the same name as your Project. Linux executable binaries typically either have .bin extension or no extension. 

**You must have sufficient rights to the file.** By default, Lazarus should have created the file with your username and group as owner. If not for some reason, change ownership using _chown_ terminal command or file manager, if available. (KDE Dolphin no longer allows _sudo_ changes within the file manager.) 

**The file must be executable.** Lazarus should have already set the file to be executable, but if not, change this using _chmod_ or file manager, if available. 

**As inital test, make sure your application can be launched directly from the Terminal.** For example, within the same directory as you application binary (e.g., MyProgram) execute: 
    
    
      $ ./MyProgram
    

Make sure you can launch your application from Terminal before proceeding. 

_Desktop entries_

To get your application to correctly launch in your Desktop Environment, you have to tell it some information by way of creating a _Desktop Entry_ file ending in _.desktop_ extension. This is simply a text file located in a particular directory and having specific keys. For Manjaro/ArchLinux, the following is helpful reference material: 

ArchLinux Desktop Entries [[3]](<https://wiki.archlinux.org/index.php/Desktop_Entries>)

The foregoing is simply a guide, not to be taken too literally. For example, for KDE Plasma 5 on Manjaro, use of separate Path and Exec keys does NOT work. Instead, a single Exec with the application binary's full path is required. You can uncover such details and quirks by examining the existing .desktop files of other applications already installed for your particular Distro and DE. 

Using the example of KDE Plasma 5 on Manjaro, proceed as follows, using hypothetical _MyApp_ application program just created with Lazarus. Determine the location for _.desktop_ files. For Manjaro, these were found to be at _/usr/share/applications/_. Now, create your _.desktop_ file: 
    
    
      $ sudo nano /usr/share/applications/myapp.desktop
    

By convention, use lowercase for the foregoing file name. Now, enter your application details (using hypothetical "MyApp" application binary). 
    
    
      [Desktop Entry]
      Type=Application
      # Change Name to your application
      Name=My Application
      # Change icon to your icon - leave OFF any file extension
      Icon=myappicon
      Comment=My First App
      # The FULL path to the folder in which the executable is run
      # Change to your actual application binary's full path
      Exec=/mysamplepath/Lazarus/MyApp/lib/x86_64-linux/MyApp
      Terminal=false
      # Sample Category - will determine where it is listed in Menu tree
      Categories=Office;
    

You will be storing your icon separately, as required by your DE, so that it may be displayed in the DE's menu along with your application name. In Manjaro, icons are required to be placed at _/usr/share/pixmaps_. If needed, a good source for icons is available at: Open Icon Library [[4]](<https://sourceforge.net/projects/openiconlibrary/>) For KDE Plasma 5 on Manjaro, 48x48 PNGs work well. Note that you must leave .png extension off above icon reference to avoid validation errors. Now validate your Desktop Entry file: 
    
    
      $ sudo desktop-file-validate /usr/share/applications/myapp.desktop
    

Correct any errors. Finally, complete installation by updating the DE's desktop database: 
    
    
      $ sudo update-desktop-database
    

Your application should now be listed in KDE's menu tree and easily launched with mouse click. If you need to clean up KDE's menu (e.g., delete incorrect menu item), launch _kmenuedit_ from Terminal or right click (if available) directly on menu item. 

### See also

  * [How to create an Ubuntu snap package](<Create_snap_package.md> "Create snap package")
  * [Publish project on Launchpad](<Publish_project_on_Launchpad.md> "Publish project on Launchpad")



## Creating a Makefile for your Lazarus software

The following is a command line build system for a Lazarus software. This is particularly useful if you wish to create an RPM package suitable for including on GNU/Linux distributions. The comments on the files should explain what each part does. 

make.sh 
    
    
    # Detects and parses the architecture
    ARCH=$(uname -m)
    
    case "$ARCH" in
     "i686") ARCH="i386";;
     "i586") ARCH="i386";;
     "i486") ARCH="i386";;
    esac
    
    echo "Target architecture: $ARCH"
    
    # Detects and parses the OS
    OS="linux"
    
    echo "Target operating system: $OS"
    
    # Command line to build the sofware
    fpc -S2cgi -OG1 -gl -WG -vewnhi -l -Fu/usr/lib/lazarus/components/opengl/gtk2x11/ -Fi/usr/lib/lazarus/components/opengl/gtk2x11/include/ -Fu/usr/lib/lazarus/components/jpeg/ -Fu/usr/lib/lazarus/lcl/units/$ARCH-$OS/ -Fu/usr/lib/lazarus/lcl/units/$ARCH-$OS/ -Fu/usr/lib/lazarus/lcl/units/$ARCH-$OS/gtk2/ -Fu/usr/lib/lazarus/packager/units/$ARCH-$OS/ -Fu. -o./magnifier -dLCL -dLCLgtk2 magnifier.dpr
    

install.sh 
    
    
    # Parses command line options. Currently supported options are:
    # DESTDIR		Destination root directory
    
    DESTDIR=""
    
    for arg; do
      case $arg in
        DESTDIR=*) DESTDIR=${arg#DESTDIR=};;
      esac;
    done
    
    # Does the install
    # "mkdir -p" is equivalent to ForceDirectories Pascal function
    
    mkdir -p $DESTDIR/usr/share/magnifier
    
    cp ./topleft.bmp $DESTDIR/usr/share/magnifier/
    cp ./topright.bmp $DESTDIR/usr/share/magnifier/
    cp ./bottomleft.bmp $DESTDIR/usr/share/magnifier/
    cp ./bottomright.bmp $DESTDIR/usr/share/magnifier/
    cp ./top.bmp $DESTDIR/usr/share/magnifier/
    cp ./left.bmp $DESTDIR/usr/share/magnifier/
    cp ./bottom.bmp $DESTDIR/usr/share/magnifier/
    cp ./right.bmp $DESTDIR/usr/share/magnifier/
    cp ./icon3.ico $DESTDIR/usr/share/magnifier/
    
    mkdir -p $DESTDIR/usr/bin
    
    cp ./magnifier $DESTDIR/usr/bin/
    

clean.sh 
    
    
    rm -rf *.o
    rm -rf *.ppu
    rm -rf *.pas~
    rm -rf *.sh~
    rm -rf *.bak
    rm -rf *~
    rm -rf magnifier
    

uninstall.sh 
    
    
    #
    # Don´t use "rm -rf" in here, because you should only remove the files you created
    rm -f /usr/share/magnifier/topleft.bmp
    rm -f /usr/share/magnifier/topright.bmp
    rm -f /usr/share/magnifier/bottomleft.bmp
    rm -f /usr/share/magnifier/bottomright.bmp
    rm -f /usr/share/magnifier/top.bmp
    rm -f /usr/share/magnifier/left.bmp
    rm -f /usr/share/magnifier/bottom.bmp
    rm -f /usr/share/magnifier/right.bmp
    rm -f /usr/share/magnifier/icon3.ico
    rm -f /usr/share/magnifier/magnifier
    rm -f /usr/bin/magnifier
    rmdir /usr/share/magnifier
    

Makefile 
    
    
    # Targets
    # The First target is the one build when there is nothing on make command line
    all:
    	./make.sh
    clean:
    	./clean.sh
    install:
    	./install.sh
    uninstall:
    	./uninstall.sh
    

## macOS Deployment

### An Application Bundle

See [Application Bundle](<Application_Bundle.md> "Application Bundle") for information on how to create an application bundle (a directory with application files that is treated by macOS as an executable). 

Most macOS applications are distributed as a disk image file (.dmg extension). Normally you download or copy a program's disk image to your Mac and double-click it. This mounts the disk image and opens a window on the desktop, where you'll typically see an application bundle as described in the previous point. To install, a user simply drags the bundle to the hard drive, typically into his or the general Applications folder. Therefore, many disk images present a link to the general Applications folder for easier dragging. "Uninstalling" is by deleting the bundle. 

Once the installation is complete, you eject (unmount) the mounted disk image, for example by dragging it to the trash. You can also delete the .dmg file by dragging it to the trash as well. 

Your bonus point is, if the application can also run directly from the (read-only) disk image. 

Here is the `app2dmg.sh` script which I use to create a .dmg file for an application bundle: 
    
    
    #!/bin/sh
    
    if [ "${1}" = "" ]
      then
          echo "Usage: app2dmg.sh app_name.app disk_size_in_mb"
          exit
    fi
    
    if [ "${2}" = "" ]
      then
          echo "Usage: app2dmg.sh app_name.app disk_size_in_mb"
          exit
    fi
    
    if [ ! -r "${1}" ]
      then
        echo "${1} is not in the current directory!"
        exit
    fi
    
    APP=`echo "${1}" | sed "s/\.app//"`
    DATE=`date "+%d%m%Y"`
    VOLUME="${APP}_${DATE}"
    
    echo "Application name: ${APP}"
    echo "Volume name: ${VOLUME}"
    
    if [ -r ${APP}*.dmg.sparseimage ]
      then
        rm ${APP}*.dmg.sparseimage
    fi
    
    hdiutil create -size ${2}M -type SPARSE -volname "${VOLUME}" -fs HFS+ ${VOLUME}.dmg
    
    hdiutil attach ${VOLUME}.dmg.sparseimage
    cp -R ${APP}.app "/Volumes/${VOLUME}/"
    cp README_FIRST.TXT "/Volumes/${VOLUME}/"
    hdiutil detach -force "/Volumes/${VOLUME}"
    
    hdiutil convert "${VOLUME}.dmg.sparseimage" -format UDBZ -o "${VOLUME}.dmg" -ov -imagekey zlib-level=9
    rm ${VOLUME}*.dmg.sparseimage
    

You can also read [this guide](<http://el-tramo.be/guides/fancy-dmg>) for how to create a fancy disk image. 

### Using Packages on macOS

The Apple `pkgbuild` and `productbuild` command line tools can produce package .pkg files or the aptly named, third party GUI [`Packages`](<http://s.sudre.free.fr/Software/Packages/about.html>) application which builds flat packages (cf bundle-style packages built by the same author's GUI [`Iceberg`](<http://s.sudre.free.fr/Software/Iceberg.html>) application. 

If you absolutely require that several files are installed in different locations, then (and only then!) should you create a package file (.pkg extension) for the Apple Installer. This package file can then be moved, downloaded or emailed. Once the end user has it, they can double-click the package and the `Installer.app`, which will perform the installation, is launched. 

Package files have certain advantages: 

  * You have control over permissions;
  * Multiple files can be placed in different locations;
  * You can run scripts as part of the installation process;
  * A logged-in user is not needed to install a package;
  * Most, if not all, Mac management systems can install Apple installer packages.



The installer .pkg file is actually a directory, as is the resulting application bundle (.app extension) that it copies to the Applications folder. To the user, .pkg and .app files look and act like ordinary files, but they're really directories whose details have been hidden from the user. You can see what's inside a .pkg or .app file by entering normal `cd` commands in a Terminal window (for example, `cd progname.app`) or by Ctrl-Clicking the file and choosing _Show Package Contents_ from the popup menu. 

To create a .dmg file, run the macOS Disk Utility (located in Applications > Utilities). Select Images > New > Image from Folder and choose the folder containing your .pkg file and any other files you want to include in the disk image. In the Convert Image dialog, enter the name of the .dmg file to create, select where it should be saved, and select "compressed" as the image format. The .dmg file that Disk Utility creates is then ready for distribution. 

### Installing X11 and GTK on macOS

GUI apps created with Lazarus that use the GTK widgetset require X11 and the GTK libraries to run. The [XQuartz open source project](<https://www.xquartz.org>) `X11.app` was shipped with Leopard 10.5 through Lion 10.7 as an optional installation. After Lion 10.7, Apple stopped providing X11 but it can still be downloaded from the XQuartz project website. 

To install the GTK libraries on a user's system, you should include instructions for using [Fink](<http://www.finkproject.org/>) or [MacPorts](<http://www.macports.org/>). Do _not_ manually package the libraries and install those in the default Fink/MacPorts locations, because then they may be overwritten in case the user decides to install one of these distributions. A good practice is to put those libraries in the application bundle itself, which requires [using the install_name_tool](<http://qin.laya.com/tech_coding_help/dylib_linking.html>) program. Putting them somewhere else is possible, but in most cases not conform to the Apple Guidelines as well as not what users expect. Before simply using directories common to other systems, you should carefully read the guidelines about the file system layout [File System Overview](<http://developer.apple.com/library/mac/#documentation/MacOSX/Conceptual/BPFileSystem/BPFileSystem.html>). 

### Code signing and Notarization

See [Code Signing for macOS](<Code_Signing_for_macOS.md> "Code Signing for macOS") (required from macOS 10.7+) and [Notarization for macOS](<Notarization_for_macOS_10.14.md> "Notarization for macOS 10.14.5+") (required from macOS 10.14.5+). 

Note: It is still possible not to code sign and notarise your applications if you do not mind your endusers facing scary warning dialogs from Apple - Gatekeeper can (still) be by-passed by Conctrol-Clicking on the application or right clicking on the application for the context menu, and then choosing _Open_ and not _Move to trash_ or _Cancel_. Notarization requires you to enrol in the [Apple Developer Program](<https://developer.apple.com/programs/>) ($US 99 annually). 

### See also

  * [Apple's Software Delivery Legacy Guide](<https://developer.apple.com/library/archive/documentation/DeveloperTools/Conceptual/SoftwareDistribution4/Introduction/Introduction.html>).


  * [The Luggage](<https://github.com/unixorn/luggage/wiki>) Uses a Makefile describing where you want each file in the package to end up on the target machines with rules that will generate a package root with appropriate permissions, copy the files into place, create a package version-stamped with the date, then wrap the package in a disk image file. This allows packages to be built in a consistent and repeatable way using source files and scripts.


  * [munkipkg](<https://github.com/munki/munki-pkg>) is a simple tool for building packages in a consistent, repeatable manner from source files and scripts in a project directory. Files, scripts, and metadata are stored in a way that is easy to track and manage using a version control system like git.


  * [macOS PackageMaker](<macOS_PackageMaker.md> "macOS PackageMaker") \- legacy - discontinued after Xcode 4.4.



## Android Deployment

(This information needs to be consolidated here and the below links adjusted) 

  * [Android](<Android.md> "Android")
  * [FPC JVM Android Development](<FPC_JVM_Android_Development.md> "FPC JVM Android Development")
  * [Android Programming](<Android_Programming.md> "Android Programming")



## Apple iOS Deployment

(This information needs to be consolidated here and the below links adjusted) 

  * [iOS Designer](<iOS_Designer.md> "iOS Designer")
  * [iOS Portal](<Portal_iOS.md> "Portal:iOS")
  * [iPhone/iPod development](<iPhone/iPod_development.md> "iPhone/iPod development")



## Other, General Cross-Platform Deployment

### BitRock

[![gui-newproject.png](https://wiki.freepascal.org/images/9/97/gui-newproject.png)](</File:gui-newproject.png>)

[Bitrock](<BitRockInstallBuilder.md> "BitRockInstallBuilder") (website www.bitrock.com) make an installer that will deploy to Windows, Mac OS and many flavors of Linux. If your application is open-source, they will give you a full license to download and use their 'InstallBuilder' application.[[Application form link](<http://installbuilder.bitrock.com/open-source-licenses.html>)]. Commercial application licenses are available. 

### fpGUI Installer

This is a cross-platform CLI and GUI installer. It is loosely based on the concept of the Loki Installer (implemented in the C programming language) created by [Loki Games](<http://www.lokigames.com/>). The difference being that fpGUI Installer is implemented from scratch using Object Pascal and the fpGUI Toolkit for the GUI parts. This installer is still under active development, but to date no public release has been made. A public release is planned though. No official name has been given to this project either - thus the not ideal "fpGUI Installer" name. 

Here is a screenshot of fpGUI Installer replacing the original Kylix 3 installer: 

[![fpGUI Installer](https://wiki.freepascal.org/images/1/16/fpgui_installer.png)](</File:fpgui_installer.png> "fpGUI Installer")

Features: 

  * Installer setup is done via an easily managed XML file. A GUI front-end is used to edit the XML, but it can be done by hand too.
  * CD-ROM or Downloadable installers. The difference being that the CD-ROM installer is normally unpacked and multiple languages, tools etc are included. A Downloadable installer is a single file download and normally packaged for a specific language.
  * Automatic icon shortcut generation
  * No external libraries or runtime environments required. The idea is that a installer must not have system requirements (eg: a JVM or the Qt library etc), hence fpGUI toolkit was used for the GUI installer, because it talks directly to GDI or X11.
  * Command Line Installer and GUI Installer included as standard.
  * Support uninstall as well
  * fpGUI Installer has been tested on Windows (Win98 thru Win7), Linux (various distros), FreeBSD and OpenSolaris.
  * Supports 32-bit and 64-bit setup files.
  * Group installer mode: The installer can present a user friendly interface to install multiple other projects.
  * Customisable graphics banner and UI theme.



### InstallJammer

InstallJammer is a multiplatform GUI installer designed to be completely cross-platform and function on Windows and most all versions of UNIX. 

InstallJammer features a very powerful install builder with support for multiple themes and a high level of configurability for installers. Installers are built as single executable files for easy distribution over the web and handle installing everything you need for your application in a simple, cross-platform way. 

  * Active development of InstallJammer has been discontinued. The source is available for all versions and will always remain so, but your efforts might be better spent on an installer that is active in its development.
  * Download: <https://sourceforge.net/projects/installjammer/files/InstallJammer/>
  * Documentation: <http://installjammer.com/docs/>



### CMake/CPack

CMake is a multiplatform build system that manages the compilation, linking, testing, and setup creation process. It is mainly intended for C/C++ software development, but can also be used for creating setups for Lazarus/FPC projects. The part of CMake that creates the setups is called CPack and is actually a front-end for other software packages that do the real work. In an FPC context, you would write a CMakeLists.txt file that contains lists of files to be installed and then have CPack call NSIS/WiX/... to perform the actual work. 

Features: 

  * Windows (32+64 Bit), Linux, macOS support
  * Works with all important back-ends (NSIS, WiX, DEB...)
  * Just one text file CMakeLists.txt needs to be mainained for all target platforms
  * Multi-platform setups can be created with minimal work
  * Download: <http://www.cmake.org/>



Using CMake/CPack makes most sense if you have a mixed-language project with Pascal and C/C++ parts that all go into one installer, and you need a platform-independent solution for both parts. If you have a pure FPC project, the overhead might be too much for you.

---

_Source: [https://wiki.freepascal.org/Deploying_Your_Application](https://web.archive.org/web/20250124212637/https://wiki.freepascal.org/Deploying_Your_Application)_
