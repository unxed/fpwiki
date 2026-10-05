# Installing Lazarus on Linux

## Contents

  * 1 Introduction
  * 2 Required Linux packages
    * 2.1 Debugger
  * 3 Make a Choice
    * 3.1 The Package Manager Model
    * 3.2 Debian-based 64-bit with Lazarus 32-bit
    * 3.3 FPC and Lazarus from SourceForge
    * 3.4 Build Lazarus from Source
      * 3.4.1 Get the Lazarus Source
      * 3.4.2 Menu items and Icons
      * 3.4.3 Help Files
      * 3.4.4 What does the BigIDE argument to make do?
    * 3.5 FPCUPdeluxe
  * 4 See Also
    * 4.1 Other Relevant Pages
    * 4.2 Testing FPC installation
  * 5 Further Reading



## Introduction

This page is about installing FPC and Lazarus on a Linux system. Its intended for new and 'regular' users, it does not cover some very advanced methods and concentrates on current releases. Linux users should think of FPC and Lazarus as being two distinct but related processes, FPC does not change much, on the other hand, you will probably want to update your Lazarus install or perhaps install multiple Lazarus versions. Mixing the install methods, especially after V2.0 may cause problems so please read carefully. 

**Notes about terms used on this page -**

  * _We refer to various downloadable files such as**fpc_something** \- you should replace the 'something' with whats appropriate for your system, for example, early 2019 using a DEB based 64bit distro it might be **fpc-laz_3.0.4-1_amd64.deb**. Mid 2021 lazarus-something.deb is **lazarus-project_2.0.12-0_amd64.deb**._
  * _**Package Manager** means a range of things on different different Linux systems. While we give command line examples, thats just because its easier to write. You will get the same result if you use your favourite GUI Tool such as **Synaptic** , **Ubuntu Software Centre** or **YaST2**. Maybe try double clicking on a downloaded package in your file manager._
  * _What ever Package manager tool you use, make sure you are using one that resolves**dependencies** (that is, it also installs thing that are required by the main package you are installing), most GUI apps do. You should avoid using tools like dpkg or rpm and instead use apt or yum depending on your flavour of Linux._



## Required Linux packages

Not withstanding the rather intimidating list below here, its most likely you will need to install only a few packages, they will bring along what is needed. 

  * A **gtk2** debian based install - libx11-dev and libgtk2.0-dev.
  * A gtk2 rpm based install - libX11-devel and gtk2-devel.
  * A gtk2 pacman based install - gtk2 libx11.
  * A **Qt5** debian based system - libqt5pas-dev (but with a current Lazarus, your repo one may not be suitable [https://wiki.freepascal.org/Qt5_Interface#libqt5pas](<Qt5_Interface.md>)).
  * A Qt5 rpm based install - qt5pas-devel (but with a current Lazarus, your repo one may not be suitable [https://wiki.freepascal.org/Qt5_Interface#libqt5pas](<Qt5_Interface.md>)).
  * A Qt5 pacman based system - qt5pas, your repo one will probably be OK.
  * A **Qt6** system similar to Qt5, just change 5 to 6.
  * A **gtk3** system will need libgtk-3-dev or similar.



Find installation scripts at the <https://github.com/LongDirtyAnimAlf/fpcupdeluxe/tree/master/installscripts> for these OSes: 

  * Alpine Linux
  * ArchLinux
  * Debian (and Ubuntu)
  * Fedora
  * Oracle Linux
  * SUSE Linux



The full list (for Gtk2, Qt5, Qt6) for Debian-based OS is listed in the script "debian.sh" here: <https://github.com/LongDirtyAnimAlf/fpcupdeluxe/blob/master/installscripts/debian.sh> . Here is the current content of that script: 
    
    
    sudo apt-get install -y xterm 
    sudo apt-get install -y git 
    sudo apt-get install -y subversion 
    sudo apt-get install -y unzip 
    sudo apt-get install -y wget 
    sudo apt-get install -y make 
    sudo apt-get install -y gcc 
    sudo apt-get install -y build-essential
    sudo apt-get install -y binutils
    sudo apt-get install -y gdb
    sudo apt-get install -y libc6-dev
    sudo apt-get install -y libc-bin
    sudo apt-get install -y texinfo
    
    sudo apt-get install -y libglib2.0-dev
    sudo apt-get install -y libgtk2.0-0-dev
    sudo apt-get install -y libgtk2.0-dev
    sudo apt-get install -y gtk2-engines-pixbuf
    sudo apt-get install -y libcairo2-dev
    sudo apt-get install -y libXxf86vm-dev
    sudo apt-get install -y libxtst-dev
    sudo apt-get install -y libx11-dev
    sudo apt-get install -y libglvnd-dev
    
    sudo apt-get install -y appmenu-gtk2-module
    sudo apt-get install -y libcanberra-gtk-module
      
    sudo apt-get install -y libxft2
    sudo apt-get install -y libfontconfig1
    
    sudo apt-get install -y libsdl2-dev
    sudo apt-get install -y libsdl2-image-dev
    sudo apt-get install -y libsdl2-mixer-dev
    sudo apt-get install -y libsdl2-net-dev
    sudo apt-get install -y libsdl2-ttf-dev
    
    sudo apt-get install -y qt5-qmake
    sudo apt-get install -y qtbase5-dev
    sudo apt-get install -y qtbase5-dev-tools
    sudo apt-get install -y qtdeclarative5-dev
    sudo apt-get install -y libqt5x11extras5-dev
    
    sudo apt-get install -y qt6-base-dev
    sudo apt-get install -y qt6-base-dev-tools
    

Some distribution may require appmenu-gtk2-module. If you find starting Lazarus has a 20 odd second delay, do try this additional package. Seems to be a particular problem on Ubuntu. 

### Debugger

Linux (and Windows) users on Intel or AMD processors can use FPDebug backend (or, more correctly, FPDebug internal Dwarf-Debugger) and for most users that's a good choice. It's reported to be faster and in some respects, more feature rich than gdb. And it's bundled in with Lazarus. 

If it's not auto selected (perhaps because you also have gdb installed), in Lazarus, Tool->Options->Debugger->Debugger-Backend. Click the "Change Type" button and select "FPDebug internal Dwarf-Debugger". 

Other platforms, or perhaps users with specific needs ( see <https://forum.lazarus.freepascal.org/index.php/topic,55131.msg417949.html#msg417949> ) will need to stick with gdb. 

## Make a Choice

You do need to decide, early on, where you will be getting your Lazarus install from. You can, at any stage change your mind but at the cost of some backtracking. We don't recomend you get your FPC and Lazarus install kits from different places, its sometimes possible but sometimes unreliable. Broadly, your choices are - 

  * Use your **Package Manager** to install both FPC and Lazarus. This means you are locked into the version of Lazarus that your distribution maintainers offer. Due to distro and Lazarus release cycles being out of sync, at any one time, its unlikely that your distribution will have the latest release of Lazarus. And many users do find those new releases attractive. You can install only one version of Lazarus under this model. But on the other hand, using the package manager approach is very easy and probably suits casual users.


  * Use the pre compiled packages (both FPC and Lazarus) made available by the FPC/Lazarus Team at **SourceForge**. Here you will find a wide choice of matched versions but you can only install one version at a time. The install process is still very easy ! This is possibly the best bet for new users.


  * Install FPC from either your Package Manager or SourceForge and then download the Lazarus **source code and compile** it yourself. May take a touch longer but is almost as easy and you do end up with the ability to add extra Lazarus installs, perhaps a 'production' version and a version based on the next release candidate. Or an old version to work on legacy code. In addition, you can easily make your own changes to the Lazarus Component Library, maybe just debug statements, maybe additional features you can feed back to help improve Lazarus further.


  * Use **FPCUPdeluxe** \- it can install a range of versions of both FPC and Lazarus for you without requiring you to understand the underlying structure.



### The Package Manager Model

If you are happy with the versions offered by your distribution this might be the way to go. Almost all Linux distributions come with some sort of Package Manager, you are probably familiar with your's. Between then, the DEB and RPM based package systems represent much of the Linux community. And if you soon run into the limitations of this model, your package manager will help you back out again. But first, check the version available. Look up 'lazarus' in your GUI Package Manager or do - 
    
    
    // DEB Users -
    apt-cache policy lazarus
    
    // RPM User -
    yum search lazarus
    

Note the names as well as the versions, we need three packages, fpc-something, fpc-source-something and, finally lazarus_something. Assuming you distribution's package has its dependencies set up correctly, all you need do is choose to install Lazarus in either your Package Manage GUI or do: 
    
    
    // DEB Users -
    sudo apt-get install lazarus
    
    // RPM Users -
    sudo yum install lazarus
    
    // or
    sudo dnf install lazarus
    

**Notes :**

  * Some distro prepared packages do not include the tools necessary to add cross compiling to its capabilities. If you may want to cross compile in the future, please consider one of the next listed models.
  * Some distro package managers break Lazarus up in small 'chunks', by default you get the GTK2 chunks. If you plan to use Qt5 also install libqt5pas-dev and lcl-qt5. You can make Qt5 apps using the GTK2 IDE.
  * If you plan to work with GTK3, you will need to install libgtk-3-dev, its not declared as a dependency of Lazarus yet because the GTK3 interface is not, yet, complete.
  * If you use Debian Bullseye, you should consider getting Lazarus (2.2.0) from Bullseye-Backports, there is an annoying bug in the main repo version that affects IDE rebuilding. Other Debian based distros will probably pick up the Backport or Testing version and they will be fine.



### Debian-based 64-bit with Lazarus 32-bit

On Debian system, like Ubuntu or Mint, run in terminal: 
    
    
    sudo dpkg --add-architecture i386
    sudo apt-get update
    

And then install the FPC-Lazarus i386 deb-packages in right order: 

  * fpc-laz first
  * fpc-src next
  * finally lazarus-project



If you have error message like "cannot find GCC", then run in terminal: 
    
    
    sudo apt-get install -y gcc:i386
    

### FPC and Lazarus from SourceForge

If your Package Manager cannot offer you the correct version of FPC then probably SourceForge can (The Raspberry Pi being a notable exception) (! But be prepared for your package manager to complain about being bypassed, just insist you know what you are doing ! 

All packages can be found at <https://sourceforge.net/projects/lazarus/files/> , if you want just FPC, you will find it down in the corresponding Lazarus release directory. 

Navigate down, select the appropriate packages, again, fpc_something, fpc-src_something and lazarus_something, save them locally and install. WARNING, very important, this catches many users out ! Like most Linux packages, these packages have dependencies, you must install using a tool that resolves dependencies. Commands like "dpkg -i fpc_something.deb" will, most definitely leave an incomplete installation ! 

Further, you might need to manually install gdb, the GNU debugger if you are not going to use fpdebug. A debugger is not absolutely essential but it makes life a lot easier. If you plan to work with GTK3, or Qt5 you will need to manually install libgtk-3-dev or libqt5pas-dev. 

**On a DEB based System.** On a older DEB based system, you almost certainly should use gdebi to install any downloaded DEB. It might already be installed, if not, install it first. You can use gdebi at the command line or double click a downloaded package in your file manager. But remember, most important, install first fpc, then fpc-src, then and only then, Lazarus. On more recent deb based systems, the apt command can resolve dependencies, you could then use this command (but note the **very important ./** in front of each deb file, if you don't specify the exact location of your downloaded deb file, it will go and get the repository based one) - 
    
    
    sudo apt install ./fpc_something.deb ./fpc-src_something.deb ./lazarus-something.deb
    

in mid 2022 for example, you might download from <https://sourceforge.net/projects/lazarus/files/Lazarus%20Linux%20amd64%20DEB/Lazarus%202.2.2/> and move the files to a suitable place and then type - 
    
    
    sudo apt install ./fpc-laz_3.2.2-210709_amd64.deb  ./fpc-src_3.2.2-210709_amd64.deb  ./lazarus-project_2.2.0-0_amd64.deb
    

**On an RPM Based System** Installing downloaded packages at the command line as shown below. (Author is unaware of the GUI Package tool that does, in fact, resolve dependencies) - 
    
    
    // RPM Users -
    yum localinstall fpc_something.rpm
    yum localinstall fpc-src_something.rpm
    yum localinstall lazarus-something.rpm
    

Note : later RPM based systems use the **dnf** command instead of yum, same syntax. 

### Build Lazarus from Source

Surprisingly easy, but that is because Lazarus routinely rebuilds itself, eg when a Lazarus package is added. So, its also a useful test that all required dependencies are really present. It allows you to have multiple versions of Lazarus installed ( [ see --pcp=xxx](<Multiple_Lazarus.md> "Multiple Lazarus") ) and both Lazarus and LCL are in your own disk space so no write issues. However, you will need to manually add an entry in your OS Menu system and, perhaps distinctive Lazarus icons. 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** Remember, FPC and Lazarus are two, separate products. Before thinking about building Lazarus, you must have a working and tested FPC. Mid 2023 FPC 3.2.2 was what you probably should be using, many distributions will have it available, otherwise, you need to install it from one of the ways documented on [ Installing the Free Pascal Compiler](<Installing_the_Free_Pascal_Compiler.md> "Installing the Free Pascal Compiler") .

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** Dependencies - First and foremost, a suitable version of FPC, see above. You probably want to build the GTK2 version of Lazarus, most distros will have gtk2 preinstalled but you may need to install its dev libraries. So, install libx11-dev and libgtk2.0-dev, (rpm based systems, libX11-devel, gtk2-devel) that will bring gtk2 itself along if necessary. Possibly (see above) gdb. If building a QT5 version, you will need libqt5pas-dev. If you are going to work with GTK3, you will need to install libgtk-3-dev

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** Systems with limited memory, such as the **Raspberry Pi** may need you to increase swap space to at least a Gig before building Lazarus, RasPi page. See [ Setting Raspberry Pi Swap Size](<Lazarus_on_Raspberry_Pi.md> "Lazarus on Raspberry Pi")

The example here pulls down what some consider 'brave', the trunk or main development version. The same process can get you releases, fixes or release candidates. Note this is not a script, it is a series of commands, to be copied one at a time, so you can see whats happening ! 

#### Get the Lazarus Source

There are two different ways to download the Lazarus Source (since August 2021) from gitlab, in fact there are several variations of each possible too). Note, in all cases, here, we get a directory with the Lazarus source, build it and run it from that directory. 

**As a Zip/Tar ball.**

If you want to build a release Lazarus from source or just want a quick snapshot of trunk or fixes, or perhaps want to test the release candidate (and that a great idea) then you may not need access to the developer capabilities that a Git install provides. So, you don't need any git tools on your local machine and its quick and easy. While you can do it through the Gitlab web interface from <https://gitlab.com/freepascal.org/lazarus/lazarus> its easier to describe with command line tools, start by cd-ing to a suitable place, I use $HOME/bin/Lazarus, then - 
    
    
    $> wget <https://gitlab.com/freepascal.org/lazarus/lazarus/-/archive/lazarus_3_6/lazarus-lazarus_3_8.zip>
    $> unzip lazarus-lazarus_3_8.zip
    $> mv lazarus-lazarus_3_8 lazarus_3_8
    $> cd lazarus_3_8
    $> make bigide
    

Now, see below for the section about Starting Lazarus to make it all work and look nice. 

**Some possible downloads**

  * Current stable - lazarus_3_6 - <https://gitlab.com/freepascal.org/lazarus/lazarus/-/archive/lazarus_3_0/lazarus-lazarus_3_6.zip>


  * trunk, or as its now known, main. This is the default from gitlab - <https://gitlab.com/freepascal.org/lazarus/lazarus/-/archive/main/lazarus-main.zip>



  


You can see all these options, and lot more on the gitlab site by pulling down the dropbox, left of center, near the top of the page. 

**As a Git repository.**

On the other hand, if you will be contributing patches, fixes, to Lazarus, or tracking down bugs by bisecting recent releases, then a proper git install is undoubtedly the way forward. You will need git installed locally, again, go to a suitable directory and: 
    
    
    git clone <https://gitlab.com/freepascal.org/lazarus/lazarus.git> lazarus-main
    cd lazarus-main
    

This is a bit slower process the first time, later you will use git to update it (run "git pull" from this folder "lazarus-main") and that's a lot faster. As bug reports must be tested against main, it probably does not make sense downloading git versions of anything other than main and maybe fixes. 

Remembering that if you have less than 4G ram, you must set 1G or preferably 2G of swap. 
    
    
    cd lazarus-main    # or, where ever you have put your Lazarus source code
    make clean
    make bigide
    

**Starting Lazarus**

You can now start Lazarus with the command ./lazarus or, if you are in another directory, something like - 
    
    
    /home/[your-user-name]/bin/Lazarus/lazarus_3_6/lazarus
    

You can add that directory to your path, put a script in ~/bin to start it or whatever, its Linux, you choose ! Here is an example script to put in your personal bin directory, note it includes FPC, if thats already on your path, leave that line out - 
    
    
    #!/bin/bash  
    RELEASE="lazarus_3_6"                 # You will probably need to adjust this
    cd "$HOME/bin/Lazarus/""$RELEASE"
    PATH="$HOME/bin/FPC/fpc-3.2.2/bin:""$PATH"  # don't need this if a package install of FPC
    ./lazarus --pcp="$HOME/.""$RELEASE"
    

Out of the box, lazarus stores its config files in $HOME/.lazarus - that might be all you need. But multiple installs require multiple config directories, thats why the --pcp switch on the command line. Alternatively, you can put a lazarus.cfg file in the top level Lazarus directory, where the lazarus binary is containing your chosen command line switch, this is a great way to ensure you don't mix up config data, lazarus does not like that ! 

#### Menu items and Icons

Building from source leaves you without the nice main menu entries and pretty icons that people who installed using packages get. But even that is easy to fix. You need two things, a suitable icon and an appropriate desktop file. Here is how - 

First, download a Lazarus icon to use (its just an image, does not need to be from right version of Lazarus), 
    
    
    cd ~/.icons
    wget https://gitlab.com/freepascal.org/lazarus/lazarus/-/raw/main/images/icons/lazarus256x256.png
    cd -
    

Then create a desktop file, eg ~/.local/share/applications/lazarus.desktop that looks a bit like this 
    
    
    [Desktop Entry]
    Name=Lazarus
    GenericName=ide
    Comment=ide
    Exec=/home/myusername/bin/Lazarus/fixes_2_0/lazarus
    Icon=/home/myusername/.icons/lazarus256x256.png
    Terminal=false
    Type=Application
    Categories=Utility;Development;
    

You will almost certainly have to change some things and might want to change more - 

  * **Name=** \- This is what will appear in your main menu.
  * **Exec=** \- This is the path to your executable lazarus, in my case I keep my various installs in ~/bin/Lazarus, yours may be different. Its unlikely your user name is 'myusername' so please change it. Note you could add a --pcp command line switch as mentioned above. If you have multiple installs of Lazarus, make multiple desktop files and make sure each uses a separate set of config files. Most important ! You might like to point this item to the little bash script mentioned above, its an easy way of ensuring Lazarus gets the FPC path setting.
  * **Icon=** The full path to the icon we downloaded earlier. The icon does not need be exactly there, but its a good place.
  * **Categories=** This line determines which sub-menu Lazarus will show up under. As shown, its under "Programing" on my system, "GNOME;Utility" will pop it up under "Accessories".



#### Help Files

FPC and Lazarus have quite a wide range of help files and related resources. When building Lazarus from source, you don't get the pre installed help chm that respond, for example, when you press F1. But they are easy to add, download a zip file into, typically, a directory called 'help'. Then, in Lazarus, Tools->Options->Help->Help_Options->CHM_Help_Viewer. Set Help Files Path to the directory you just put all the Chm files into. Get these files from here - 

  * If using Lazarus Main, <https://www.stack.nl/~marcov/doc-chmbeta.zip>
  * If using a (recent) release, you might be better with <https://www.stack.nl/~marcov/doc-chm.zip>



These files are very kindly updated from time to time by Marcov. If you prefer, you can build your own in the source tree. 

There are ways to access, possibly a newer version of the same data - 

  * An on line version - <https://lazarus-ccr.sourceforge.io/docs/>
  * A preview of the next release <https://dsiders.gitlab.io/lazdocsnext/> maintained by Don, thanks Don!



#### What does the `BigIDE` argument to `make` do?

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

### FPCUPdeluxe

Another approach is [fpcupdeluxe](<fpcupdeluxe.md> "fpcupdeluxe"), its an application that you run on your computer to manage your FPC / Lazarus install. Its very suited to people who just want to get a working install quickly and are happy to allow fpcupdeluxe to do the thinking for them. 

## See Also

### Other Relevant Pages

  * [Installing the Free Pascal Compiler](<Installing_the_Free_Pascal_Compiler.md> "Installing the Free Pascal Compiler")
  * [Cross_compiling](<Cross_compiling.md> "Cross compiling")
  * [Cross_Compile_to_RasPi_from_Linux](<Cross_Compile_to_RasPi_from_Linux.md> "Cross Compile to RasPi from Linux")
  * Generic [Installing Lazarus](<Installing_Lazarus.md> "Installing Lazarus")
  * [Lazarus on Raspberry Pi](<Lazarus_on_Raspberry_Pi.md> "Lazarus on Raspberry Pi")
  * Install the help system, [Installing Help in the IDE](<Installing_Help_in_the_IDE.md> "Installing Help in the IDE")
  * A number of useful additions but in particular Unit/Identifier Dictionary, [Cody](<Cody.md> "Cody").
  * Some useful tricks associated with the IDE [IDE tricks](<IDE_tricks.md> "IDE tricks")



### Testing FPC installation

See [Testing the FPC Install](<Installing_the_Free_Pascal_Compiler.md> "Installing the Free Pascal Compiler") for a very simple test that FPC is working. 

If that worked, well done ! Now proceed to installing Lazarus. 

## Further Reading

Much discussion in a forum thread created by Handoko, <https://forum.lazarus.freepascal.org/index.php/topic,41524.0.html> This forum thread may be of particular interest to people who have upgraded an existing Linux install to a more recent version (ie Ubuntu 10.04 to 18.04) as that does seem to trigger some unexpected dependency problems. Please, ask for help, document such problems here when you fix them.

---

_Source: [https://wiki.freepascal.org/Installing_Lazarus_on_Linux](https://web.archive.org/web/20250420135537/https://wiki.freepascal.org/Installing_Lazarus_on_Linux)_
