# Installation hints for old versions

## Contents

  * 1 Installing Lazarus
    * 1.1 Installing Lazarus under Linux
      * 1.1.1 Ubuntu / Debian
        * 1.1.1.1 Problems compiling GTK2 apps with FPC 2.2.2
      * 1.1.2 Installing 0.9.28 using rpms
      * 1.1.3 Installing on Mandriva
        * 1.1.3.1 Lazarus 0.9.30 on Mandriva 2010
        * 1.1.3.2 Lazarus 0.9.28 on Mandriva 2010
        * 1.1.3.3 Lazarus 0.9.28 on Mandriva 2009 (2009.0) (i386) (GTK-2)
        * 1.1.3.4 Lazarus 0.9.26 on Mandriva 2007 (2007.0) (i386) (GTK-1)
        * 1.1.3.5 Lazarus 0.9.28 on Mandriva 2007/or -2006 32 bit
        * 1.1.3.6 Lazarus 0.9.28 on Mandriva 2009 32 bit
        * 1.1.3.7 Lazarus untested snapshot on Mandriva 2007 32 bit
        * 1.1.3.8 Installing Lazarus + fpc For Mandriva X86_64
      * 1.1.4 Installing on Slackware
        * 1.1.4.1 Installing Lazarus on Slackware 13.0
        * 1.1.4.2 Installing Lazarus 0.9.28, for Slackware 12.0 users
      * 1.1.5 Installing on openSUSE
        * 1.1.5.1 Installing Lazarus 0.9.30. For openSUSE 11.1
        * 1.1.5.2 Installing Lazarus 0.9.28. For openSUSE 11.1 users
      * 1.1.6 Install Lazarus 0.9.4 on Mandrake 10.1
        * 1.1.6.1 Step A - Install the stable compiler (FPC Version 1.0.10)
        * 1.1.6.2 Step B - Install the current compiler (FPC Version 1.9.6)
        * 1.1.6.3 Step C - Install the Integrated Development Environment (Lazarus)
      * 1.1.7 Installing from source
        * 1.1.7.1 Downloading Lazarus Source Code
        * 1.1.7.2 Compiling and running
      * 1.1.8 Installing Lazarus under Debian GNU/Linux
      * 1.1.9 Installing Lazarus 64 bit version on Red Hat EL 5
      * 1.1.10 Fedora Core 11
    * 1.2 Installing Lazarus under Windows
      * 1.2.1 Installing from source
    * 1.3 Installing Lazarus under FreeBSD
      * 1.3.1 FreeBSD 6+
    * 1.4 Installing Lazarus under PC-BSD 1.0rc1+
    * 1.5 Installing Lazarus under macOS
    * 1.6 Installing Lazarus under Haiku
    * 1.7 Additional experiences and advice on installing FPC and Lazarus



## Installing Lazarus

### Installing Lazarus under Linux

#### Ubuntu / Debian

##### Problems compiling GTK2 apps with FPC 2.2.2

From this [post](<http://lists.freepascal.org/lists/fpc-pascal/2009-January/019838.html>) by Michalis Kamburelis: 

In case someone wonders how to workaround this [bug](<http://mantis.freepascal.org/view.php?id=11837>) (occurs with GTK >= 2.13.4, e.g. on Ubuntu 8.10 we have GTK 2.14): 

I found that the quick workaround that works is to pass --noinhibit-exec to ld, that is "-k--noinhibit-exec" to FPC command-line. The ld will then treat unresolved references as mere warnings, and will produce working executable (assuming your program doesn't actually call obsoleted GTK2 functions). 

Of course, you can also upgrade to the lastest FPC release 2.2.4. Above workaround is useful only if you have to stick to old released FPC 2.2.2. 

#### Installing 0.9.28 using rpms

The next easiest way is to the RPMs for FreePascal and Lazarus from the SourceForge repository. 

You need to download the selected version of 

  * the compiler (eg fpc-2.2.4-0.i386.rpm)
  * the pascal source library (eg fpc-src-2.2.4-081004.i386.rpm)
  * the Lazarus package (eg lazarus-0.9.28-0.i386.rpm).



Here is an example of a script to install or update to ver.0.9.28. Save it as something like "InstallLazarus.sh", give it execute permission, and run after getting root privileges using su. Don't mind if you get "not installed" message. 
    
    
     #!/bin/sh
     URL=<http://sourceforge.net/projects/lazarus/files/Lazarus%20Linux%20i386%20RPM/Lazarus%200.9.28>
     wget $URL/fpc-2.2.4-0.i386.rpm/download
     wget $URL/fpc-src-2.2.4-0.i386.rpm/download
     wget $URL/lazarus-0.9.28-0.i386.rpm/download
     rpm -ev lazarus
     rpm -ev fpc
     rpm -ev fpc-src
     rpm -ivh fpc-*
     rpm -ivh lazarus-*
    

#### Installing on Mandriva

##### Lazarus 0.9.30 on Mandriva 2010

Install as given lower down however on compiling a program you may get two error messages telling you that you are missing pixbuf 2.0 and lgtk-x11-2.0. to fix this install from the the software installer libdgk_pixbuf2.0_0-devel and libgtk+2.0_0-devel. 

##### Lazarus 0.9.28 on Mandriva 2010

Install as given lower down however on compiling a program you may get two error messages telling you that you are missing pixbuf 2.0 and lgtk-x11-2.0. to fix this install from the the software installer libdgk_pixbuf2.0_0-devel and libgtk+2.0_0-devel. 

##### Lazarus 0.9.28 on Mandriva 2009 (2009.0) (i386) (GTK-2)

Not much has changed compared to Mandriva 2007. The following instruction shall not differ for an x86_64 architecture. 

1/ Do not install FPC from Drake. If you have done so, desinstall it. Since Mandriva does not provide the complete set of tools (FPC+sources+documentation+Lazarus) it is better, in my opinion, to have complete control on all of them through rpms. 

2/ Collect fresh rpms on sourceforge (<http://sourceforge.net/project/showfiles.php?group_id=89339>): select the RPM version of the latest lazarus release compatible with your architecture and download fpc, fpc-src, fpc-docs and lazarus RPMs. 

3/ Browse the directory where you downloaded the RPMs (with Nautilus or whatever file browser your using) and double click on the RPMs in this ordrer: 
    
    
      (a) fpc-<version>-<arch>.rpm
      (b) fpc-docs-<version>-<arch>.rmp
      (c) fpc-src-<version>-<arch>.rpm
      (d) lazarus-<version>-<arch>.rpm
    

4/ It is possible that the Drake (the software that will manage your RPMs) will complain about missing libraries or tools. Use Drake to collect them. For instance, you shall have installed developpement tools such as "GNU Make". Search and if necessary install "GNU Make". You might be missing the ld (the linkage utility), you can install for instance the "gcc" package. 

5/ As before, it is possible to search for and install "galaxy-gnome" and "gtk-switch" with Drake. Nonetheless, they are not required for Lazarus to work properly. Gnome Galaxy are a set of cool themes you can use in the System menu->Preferences->Themes. 

6/ Lazarus might complain about the FPC sources that are not correctly set. Go to Configuration->Environement then set the FPC sources directory to your local installation (it should be /usr/local/fpcsrc/<version>/). 

7/ When launched, the libcanberra-module-gtk might not be found. I tried to build a symlink (ln -s) but if Lazarus manages to find this library... it crashes. It might be safer to ignore this error. 

8/ Have fun with the coolest RAD of the Free World. 

##### Lazarus 0.9.26 on Mandriva 2007 (2007.0) (i386) (GTK-1)

Installation in Mandriva is very easy. However, there are a few points to check as the installation proceeds. Here is a typical installation using the .tar modules (see the trick for the theme at the end of the procedure!): 

\- During the Mandriva installation, select the "Gnome desktop" group. Although you will use lazarus in KDE or other desktop, this will install some GTK modules needed by the lazarus environment. 

\- Install the FPC 2.2.2 compiler using either the .rpm or .tar, modules. Using the .tar modules you would type: "sh install.sh" then RETURN many times. I do this in superuser mode. 

\- Install the compiler source code. just untar the file "fpc-src-2.2.4.source.tgz" in the folder /usr/local/src/fpc/ then rename the created folder "fpc" to "2.2.2" . When complete, you have the folder /usr/local/src/fpc/2.2.2/ that contains all the source code. The source code is only needed by lazarus editor, but you can recompile FPC and have an optimized version of the libraries, and show that everything is working well: 

\- If you want to compile FPC (optional), then go to the FPC source directory (/usr/local/src/fpc/2.2.4/) and type "./configure", "make clean","make all","make install","make clean". FPC is now installed. Check the content of "/etc/fpc.cfg". This file has to be modified when the make fails. 

\- Lazarus can be installed in user mode: just unzip/untar the file "lazarus-0.9.26.0.tar.gz" in your user directory. Then just type "make clean all" in the user mode. The last phase of the process (link) usually fails due to missing library files: 

\- Check in the directory /usr/lib/ for the following symbolic links: 

libglib.so -> libglib-1.2.so.0 

libgtk.so -> libgtk-1.2.so.0.9.1 

libgdk.so -> libgdk-1.2.so.0.9.1 

libgdk_pixbuf.so -> libgdk_pixbuf.so.2.0.0 

The file "libgdk_pixbuf.so.2.0.0" is included in the module "libgdk_pixbuf-2.0.22.0-10mdv2007.0.rpm" This file name is critical. In particular, DO NOT use "libgdk_pixbuf-2.0.so.0" (misleading isn't!). 

After all the links have been fixed, the "Linking ../lazarus" stage will not crash any more and the "./lazarus" command should start the program! 

\- Go to the "environment/environment options" menu and change the "FPC source directory" to /user/local/src/fpc/2.2.4 

USEFUL TO KNOW: 

\- The last step is to fix the theme. The package "galaxy-gnome-1.0.4-3mdv2007.0" is available in the Mandriva control center. Install it. 

\- The package "gtk-theme-switch-1.0.1-4mdv2007.0.i586.rpm" is available on the web! (strangely it did not show in the control center) Install it manually by typing "rpm -iv --test gtk-theme-switch-1.0.1-4mdv2007.0.i586.rpm" then retype the line without the "--test" in my case there was a warning message related to the signature. I decided to ignore it! 

\- in the USER MODE, type "switch", then chose "galaxy" theme. 

The Lazarus / FPC tool is now installed! 

##### Lazarus 0.9.28 on Mandriva 2007/or -2006 32 bit

Write this urpmi-command as root: 
    
    
    urpmi http://surfnet.dl.sourceforge.net/sourceforge/lazarus/lazarus-0.9.28-0.i386.rpm http://surfnet.dl.sourceforge.net/sourceforge/lazarus/fpc-2.2.4-0.i386.rpm http://surfnet.dl.sourceforge.net/sourceforge/lazarus/fpc-docs-2.2.4-0.i386.rpm http://surfnet.dl.sourceforge.net/sourceforge/lazarus/fpc-src-2.2.4-0.i386.rpm
    

Lazarus 0.9.28 depends on fpc 2.2.4 and Mandriva 2007 is shipped with fpc-2.1.1-0.2mdk.i586. To prevent your system to upgrade to fpc-2.1.1 or newer in the future, when you run commands as urpmi --auto-select, you can write fpc in /etc/urpmi/skip.list. 

##### Lazarus 0.9.28 on Mandriva 2009 32 bit

Installation needs similar action as above. Because the distribution comes with fpc-2.2.4-1, it is necessary to uninstall this package. Write following commands as root: 
    
    
    rpm -e fpc
    urpmi http://surfnet.dl.sourceforge.net/sourceforge/lazarus/lazarus-0.9.28-0.i386.rpm http://surfnet.dl.sourceforge.net/sourceforge/lazarus/fpc-2.2.4-0.i386.rpm http://surfnet.dl.sourceforge.net/sourceforge/lazarus/fpc-docs-2.2.4-0.i386.rpm http://surfnet.dl.sourceforge.net/sourceforge/lazarus/fpc-src-2.2.4-081004.i386.rpm
    

And place fpc in /etc/urpmi/skip.list. 

##### Lazarus untested snapshot on Mandriva 2007 32 bit
    
    
    urpmi $(for RPM in $(lynx --source http://michael-ep3.physik.uni-halle.de/Lazarus/ | grep i386.rpm | cut -d '"' -f 4-4) ; do echo -n http://michael-ep3.physik.uni-halle.de/Lazarus/$RPM ' '; done )
    

##### Installing Lazarus + fpc For Mandriva X86_64

This was tested on Mandriva 2010.0 X86_64. 

  * Download the X86_64 snapshots from <http://michael-ep3.physik.uni-halle.de/Lazarus/> Lazarus Snapshots
  * I used these:


    
    
    http://michael-ep3.physik.uni-halle.de/Lazarus/snapshots/fpc-2.4.0-20100412.x86_64.rpm
    http://michael-ep3.physik.uni-halle.de/Lazarus/snapshots/fpc-src-2.4.0-20100412.x86_64.rpm
    http://michael-ep3.physik.uni-halle.de/Lazarus/snapshots/lazarus-0.9.29.24591-20100412.x86_64.rpm
    

You can install them with the command urpmi as root, and write all these 3 urls as paramters on the command line. 

  * To download and install the current snapshot:



` urpmi $(lynx --source <http://michael-ep3.physik.uni-halle.de/Lazarus/> |grep -v lazarus-0.9.28.3| grep 64.rpm | cut -d '"' -f 6-6 | awk '{print "<http://michael-ep3.physik.uni-halle.de/Lazarus/>"$0}') `   
You might have to install lynx and make first:` urpmi lynx make`. 

  * Once the RPMs have been installed. Run lazarus for the first time. It will pop up a dialog complaining that the "/usr/bin/ppc386" and "/usr/lib/lazarus" are not valid. Accept ("OK") the alternate choices it suggests.
  * When compiling your first project, you may find that lazarus complains of missing libs (e.g. gdk, glib, etc.). Install the necessary libs as needed, I installed these (that I recall, YMMV):


    
    
    glib-devel
    lib64gtk+1.2-devel
    lib64gtk+2.0_0-devel
    lib64gdk-pixbuf2
    lib64gdk-pixbuf2-devel
    

#### Installing on Slackware

##### Installing Lazarus on Slackware 13.0

There is no real difference from the slackware 12.2 or 12.0 version, therefore the procedure described below should work just as well. 

Please note that you will NOT be able to install lazarus on gtk, therefore you MUST use the "LCL_PLATFORM=gtk2" statement. Slackware only provides the KDE enviroment, but the gtk2 type panels are fully supported (no extra installation required). At most, there is one or two symbolic link to add! 

##### Installing Lazarus 0.9.28, for Slackware 12.0 users

This have worked in Slackware-12.0 on a Pentium-3 computer: 

  * The Free Pascal Compiler (FPC) is installed in SUPERUSER mode
  * The lazarus in installed in USER mode
  * The FPC compiler will be recompiled
  * The lazarus Integrated Development Environment (IDE) source code is loaded from the SVN server
  * The GTK-2 widget set is used ("LCL_PLATFORM=gtk2")



\-- Download "fpc-2.2.4.i386-linux.tar" in a user folder and install it. Go to this folder and type: 
    
    
     bash-3.1$ tar -xvf fpc-2.2.4.i386-linux.tar
     bash-3.1$ su 
     bash-3.1$ password:xxxxxx
     bash-3.1# sh install.sh
     ...
     (use the default answers for the next 6 questions: press "return" 6 times)
     ...
     bash-3.1# exit
     bash-3.2$ fpc
     free pascal compiler version 2.2.4 [] for i386 
     Copyright (c) 2008 ...
     ...
     ...
    

You have installed the compiler. You may want to recompile it or not, but in any case you will need to download the source code as it is required by the lazarus IDE. 

\-- Download "fpc-2.2.4.source.tar.gz" in a temporary folder. If you do not want to recompile the source then just skip this section. If you would like to run your own compiled version of FPC, then just type: 
    
    
     bash-3.1$ tar -xvzf fpc-2.2.4.source.tar.gz
     bash-3.1$ cd fpc-2.2.4
     bash-3.1$ make clean all
     bash-3.1$ su
     bash-3.1$ password: xxxxxxxx
     bash-3.1# make install
     bash-3.1# exit
     bash-3.1$ fpc
     free pascal compiler version 2.2.4 [...] for i386 
     Copyright (c) 2008 ...
     ...
     ...
    

Now you have your own compiled version working. 

\-- The lazarus IDE editor needs to look at the FPC source code. Even if you do not want to recompile FPC, you need its source code. However, in order to save some space, I only keep a clean (not compiled) copy of the source code. I start from the file "fpc-2.2.4.source.tar.gz" again and I copy it to the /usr/local/src/FPC folder: 
    
    
     bash-3.1$ su
     bash-3.1$ password: xxxxxxxx
     bash-3.1# cp fpc-2.2.4.source.tar.gz /usr/local/src/
     bash-3.1# cd /usr/local/src/
     bash-3.1# tar -xvzf fpc-2.2.4.source.tar.gz
     bash-3.1# rm fpc-2.2.4.source.tar.gz
     bash-3.1# exit
    

The version number is included in the folder name "fpc-2.2.4". This way I can keep many versions the the compiler source and eventually switch between them. 

\-- The lazarus IDE is kept in my USER "~/lazarus" folder and always compiled in USER mode. I usually download the "lazarus-0.9.28-0.tar.gz" file, but here we can also get the latest development version from the SVN server: 
    
    
      bash-3.1$ svn co <http://svn.freepascal.org/svn/lazarus/trunk> lazarus
    

This is for the first time you load it. Next time you will only need to type: 
    
    
      bash-3.1$ svn update
    

If you do not have SVN installed on your computer, here is how to get it quickly: 

\--Download the two files: "subversion-1.4.6.tar.gz" and "subversion-deps-1.4.5.tar.gz" (or later versions). Type: 
    
    
      bash-3.1$ tar -xvzf subversion-1.4.6.tar.gz
      bash-3.1$ tar -xvzf subversion-deps-1.4.6.tar.gz
      bash-3.1$ cd subversion-1.4.6
      bash-3.1$ ./configure
      bash-3.1$ make
      bash-3.1$ su
      bash-3.1$ password:xxxxxx
      bash-3.1# make install
      bash-3.1$ exit
    

\-- At this point you have the folder "~/lazarus" containing the source code. You should compile it very simply: 
    
    
      bash-3.1$ make clean all LCL_PLATFORM=gtk2
    

The "LCL_PLATFORM=gtk2" means of course that you will develop your applications in GTK-2. The default widget set for lazarus is GTK-1 but you will not be able to use it in Slackware 12.0. I strongly recommend that you use the "LCL_PLATFORM=gtk2" option now. After a few minutes, the compiler stops: 
    
    
      ...
      ...
      Linking ../Lazbuild
      987 linescompiled ...
      make [2] leaving ...
      make [1] leaving ...
      bash-3.1$
    

\-- Just type: 
    
    
      bash-3.1$ ./lazarus
    

WOW! You get a message: "Free Pascal sources not found". Just follow the instructions and indicate your Free Pascal Compiler source directory in the panel: "Environment->Environment_Options->Files". As explained earlier, on my computer this should point to "/usr/local/src/fpc-2.2.4". Note that when you change this folder, you should click on "Environment / Rescan_FPC_source_directory". 

Voila! 

#### Installing on openSUSE

##### Installing Lazarus 0.9.30. For openSUSE 11.1

Free Pascal Compiler requires:  
\- Gnu binutils (gnu as, gnu ld, gnu make) 

These utils can be installed by: 
    
    
    zypper in -t pattern devel_basis
    

Lazarus also requires these components: 1) glib2 devel 2) gtk2 devel 

**Important** : The lazarus rpm requires the gtk 2 version, not the version 1.2. 

These libraries can be installed by: 
    
    
    zypper -n install gtk2 glib2
    

Download these binary files (RPM) 

    

  * fpc-2.4.2-0.i686.rpm (yes i686 and not i386)
  * fpc-src-2.4.2-0.i686.rpm
  * lazarus-0.9.30-0.i686.rpm



install them opening a terminal session (mouse's right button -> Menu: Open in terminal) 
    
    
    rpm -Uvh fpc-2.4.2-0.i686.rpm
    rpm -Uvh fpc-src-2.4.2-0.i686.rpm
    rpm -Uvh lazarus-0.9.30-0.i686.rpm
    

##### Installing Lazarus 0.9.28. For openSUSE 11.1 users

Free Pascal Compiler requires:  
\- Gnu binutils (gnu as, gnu ld, gnu make) These utils can be installed by: 
    
    
     zypper in -t pattern devel_basis
    

Lazarus also requires these components: 1) glib2 devel 2) gtk2 devel 

**Important** : The lazarus rpm requires the gtk 2 version, not the version 1.2. 

These libraries can be installed by: 
    
    
     zypper -n install gtk2 glib2
    

Download these binary files (RPM) 

    

  * fpc-2.2.4-0.i386.rpm (yes i586 and not i386)
  * fpc-src-2.2.4-0.i386.rpm
  * lazarus-0.9.28.2-0.i586.rpm



install them opening a terminal session (mouse's right button -> Menu: Open in terminal) 
    
    
     rpm -Uvh fpc-2.2.4-0.i386.rpm
     rpm -Uvh fpc-src-2.2.4-0.i386.rpm
     rpm -Uvh lazarus-0.9.28.2-0.i586.rpm
    

#### Install Lazarus 0.9.4 on Mandrake 10.1

Do the following steps to install your Lazarus Integrated Development Tool, with the Free Pascal Compler. 

I am a novice linux user. I find it so difficult to install or upgrade software on linux. This time I finally succeeded upgrading lazarus to the 0.9.4 version (3 jan 2005 version) and I hope that this sumary could be usefull for other novice users. 

  * System: linux "Mandrake 10.1 Official" on a Pentium III (or II) type computer.


  * Lazarus: Version 0.9.4 (built: 3 january 2005).


  * FPC: Version 1.9.6 (built: 2 january 2005).


  * File names:


    
    
       File_1 = "fpc-1.0.10i386.tar" from:
       <http://www.freepascal.org/down-linux-1386.html>
    
    
    
       File_2 = "fpc-1.9.6-050102.i386-linux.tar" from:
       <http://www.lazarus.freepascal.org>
       then follow the links: "binaries" / "linux" / "Current Lazarus Binary
       RPM"
    
    
    
       File_3 = "lazarus-050103.tgz" (from the lazarus site)
    

  * System modules: Lazarus uses the GTK+ library (version 1 only, not version2). Go the "Mandrake Control Center" and install the modules:


    
    
          libgdk-pixbuf-xlib2-0.22.0-5mdk 
          libgdk-pixbuf2-devel-0.220.-5mdk 
     
    

There are 3 steps: 
    
    
       Step A - Install the stable compiler (V1.0.10)
       Step B - Install the current compiler (V1.9.6)
       Step C - Install the Integrated Development Environment (Lazarus)
       Step A - Install the stable compiler (FPC Version 1.0.10)
    

##### Step A - Install the stable compiler (FPC Version 1.0.10)

In order to compile the compiler, you need a compiler! We use the "stable compiler", (Free Pascal Compiler version 1.0.10) for that. It used to be included on the "Mandrake Install disk" but I did not find it in the Mandrake 10.1 distribution! However, it is available on the web (see the address above): 
    
    
       A.1- In user mode ("myself") type: mkdir /home/myself/freepascal/V1.0.10
       A.2- copy File_1 to /home/myself/freepascal/V1.0.10
       A.3- type: tar -xvf File_1 (of course you type the actual 
       file name given above)
       A.4- type: cd fpc
       A.5- go to the superuser mode (type: "su" then your password)
       A.6- type: sh install.sh
       A.7- you want to install it in the [/usr/local folder] then 
       just press: return
       A.8- anwser "Y" to all the 11 questions, and answer "return" 
       to the last question.
    

At this point you shoud have the first "stable" compiler installed! It will be used to compile the "current" compiler: The command: "fpc" shows a help message that shows the "1.0.10" version number. Also, check that now you have the folder: 
    
    
       /usr/local/lib/fpc/1.0.10/
    

##### Step B - Install the current compiler (FPC Version 1.9.6)

This is very similar to the previous section: 
    
    
       B.1- In user mode ("myself") type: mkdir /home/myself/freepascal/V1.9.6
       B.2- move File_2 to: /home/myself/freepascal/V1.9.6
       B.3- type: tar -xvf File_2 (actual name given above)
       B.4- type: cd fpc
       B.5- go to the superuser mode ("su")
       B.6- type: sh install.shl (takes about 10 minutes)
       B.7- anwser "Y" to all the 12 questions, and answer "return" to the last
       question.
    

At this point check that you have a directory: 
    
    
       /usr/local/lib/fpc/1.9.6/units/i386-linux/
    

filled with the libraries. There are all sort of folders in there. Now check that the file fpc.cfg has the lines: 
    
    
       -Fu/usr/local/lib/fpc/1.9.6/units/$fpctarget 
       -Fu/usr/local/lib/fpc/1.9.6/units/$fpctarget/* 
       -Fu/usr/local/lib/fpc/1.9.6/units/$fpctarget/rlt 
    

Note that you see the word: $fpctarget NOT $target. Note that you see: 1.9.6 NOT 1.0.10. If the lines do not match then edit and change the file. Use any editor you like of course. In VI (type: "vim") you type: 
    
    
       B.7.a - "I" to go to the "insert" mode
       B.7.b - type in your lines as shown above
       B.7.c - press "ESCAPE" to go back to the command mode
       B.7.d - type ":wq" to save and exit. (":q!" exit without save!)
    

The last "trick" is to check (and change, if necessary) that the symbolic link "ppc386" points to the good compiler. Type: 
    
    
       B.8.a- cd /usr/local/bin
       B.8.b- ls -al
    

The displayed line should show: 
    
    
       ppc386 --> /usr/local/lib/fpc/1.9.6/ppc386
    

If it does, then skip to step B9, otherwise type : 
    
    
       B.8.c- rm ppc386 (then "y")
       B.8.d- type: ln -s /usr/local/lib/fpc/1.9.6/ppc386
       B.9 - Go back to the user mode (type: "exit").
       B.10- type: fpc
    

Wonderfull! Now you see from the help message that you have the version 1.9.6 compiler. The last step is to load the IDE. 

##### Step C - Install the Integrated Development Environment (Lazarus)

The Lazarus can installed in your own user account ("myself"). That can be done in the user mode: 
    
    
       C.1- type: cd ~
       C.2- Copy File_3 to: /home/myself/
       C.3- cd ~
       C.4- tar -xvzf File_3 this will create the lazarus folder
       C.5- cd lazarus
       C.6- make clean all (takes about 10 minutes)
       C.7- type: ./lazarus
    

This should bring up the IDE. The first time you run lazarus, you get an error message about a bad "Free pascal Source Directory". Click: "ignore", then go to the menu: "Enviromnment/Environment Options" and set the "FPC source directory" to /usr/local/share/src/fpc-1.9.6/. 
    
    
       C.8- In order to check that the application is working properly,
       create the simplest application. Just drag and drop a button on 
       the form and press F9.
       C.9- Finally, create a shortcut icon on your desktop that executes lazarus
       in the /home/myself/lazarus/ directory. Enjoy.
    

Finally, I would like to make a few comments: 

  * I do not know how to install the .rpm files. For that reason I always use the .tar files. Most people commented to me that the RPM file is a prefered method.
  * There is a stable version 1.0.11 but there was no .tar file available for this one. The version 1.10 is available and works well, so I used it.
  * If you get an error message about some files "missing" this is because you have not installed some modules. Instaling modules is an art (and a hobby)! Try to install the followings modules. I do not know if they are nessary but they were already installed on my system:


    
    
          kernel-source-2.6-2.6.8.1-12mdk  
          libgdk-pixbuf2.0-0-devel-2.4.9-9mdk  
    

  * Beware of the files "ppc386" "ppc396.cfg" "fpc.cfg" "fpc" when your instalation fails. After a while, you see them everywhere. Some of them are hidden sometimes! Remove all those files before you start the instalation again. Only /etc/fpc.cfg and /user/lib/ppc386 are necessary.


  * FPC can be installed in the cross-compiler (multi-platform) mode. The file to use for that has the name structure "fpcsrc-...-.tar". I could install it, but more difficult to install. In this this procedure however, the "linux-only" file ("fpc-...-linux.tar") is very easy to install.



I hope that this was not too pedantic and could be useful to someone. I wish you a good luck. Alain Michaud 31 January 2005 

#### Installing from source

If you prefer to install from source and compile the files yourself, follow these instructions. Because the whole lazarus stuff is installed into one directory, uninstall is very easy and you don't need to be root to install lazarus. You can get tgz files for fpc, fpcsrc and lazarus from the downloads section or you can download it directly via svn. 

Here is an example of installing 0.9.28 to Ubuntu 6.06. If you understand Linux commands and bash script, you can get what steps are needed. Just copy the script (change the version number when new version has been released), paste it into a text editor, and save it as something like "install_lazarus.sh". Give it execute permission, and run it in a console. Note: In this example, fpc is installed in /opt. So when prompted 'Install prefix', enter '/opt/fpc'. 
    
    
     #!/bin/sh
     
     #installing required packages
     sudo apt-get install build-essential
     sudo apt-get install libgtk2.0-dev
     sudo apt-get install libgdk-pixbuf-dev
     
     #installing FreePascal source
     cd /opt
     sudo wget <http://nchc.dl.sourceforge.net/lazarus/fpc-src-2.2.4.source.tgz>
     sudo tar -xvf fpc-src-2.2.4.source.tgz
     sudo mv fpc fpcsrc
     
     #installing FreePascal
     sudo mkdir fpc
     cd fpc
     sudo wget <http://nchc.dl.sourceforge.net/lazarus/fpc-2.2.4.i386-linux.tar>
     sudo tar -xvf fpc-2.2.4.i386-linux.tar
     echo "Enter '/opt/fpc' when prompted 'Install prefix'"
     sudo sh install.sh
     
     #adding fpc path to the PATH
     echo "#FPC PATH" >> ~/.bash_profile
     echo "if [ -d /opt/fpc/bin ] ; then" >> ~/.bash_profile 
     echo PATH=/opt/fpc/bin:"${PATH}" >> ~/.bash_profile
     echo "fi" >> ~/.bash_profile
     
     #installing Lazarus
     cd ../
     sudo wget <http://nchc.dl.sourceforge.net/lazarus/lazarus-0.9.28-0.tar.gz>
     sudo tar -zxvf lazarus-0.9.28-0.tar.gz
     PATH=/opt/fpc/bin:"${PATH}"
     sudo chmod -R 777 lazarus
     cd lazarus
     make clean all
     ./lazarus
    

Note: You have to manually set fpc-src path in the Environmental Options. 

##### Downloading Lazarus Source Code

The lazarus repository has moved from cvs to svn (Subversion). SVN provides an easy way to update your sources by only downloading the changes. This is the recommended way and saves you a lot of time. A connection to the internet is needed for this: you don't need to be root. 

Please note these instructions are for SubVersion. 

Lazarus does not need any special permissions, neither during installation nor at runtime. 

With the move we also changed the directory layout of the lazarus sources. The main difference is that all ide sources are moved from the root to a separate ide subdir. As anybody might understand, this has an impact on the Makefiles, the searchpaths etc. We are in progress of fixing things, so if something doesn't work, please let us know. 

Now the most important thing.... getting the sources
    
    
    
     []$ svn checkout <http://svn.freepascal.org/svn/lazarus/trunk/> lazarus
    

(or any other dir where you want to place your sources) 

On subsequent occasions, to update simply type 
    
    
     []$ svn update lazarus
    

For more information on Subversion, see: <http://subversion.tigris.org/>

Note: the CVS is still accessable, but is frozen. This means that no new updates are comitted. 

##### Compiling and running

Whether you checkout from cvs or svn, the next step is: 

compile lazarus
    
    
    
    []$ cd lazarus
    []$ make  (gmake on BSD)
    

If fpc is installed correctly, the compilation should work without problems. If not, see FAQ. 

Start lazarus
    
    
    []$ ./lazarus
    

The IDE should start. If you started lazarus in a terminal, you can see some notes about missing settings. This is normal at first start. The IDE automatically tries to find out where the freepascal compiler and its sources are installed by searching in the most common directories. 

Check the paths
    
    Use the IDE menu to go to 

    Environment -> Environment Options -> Files

The 'FPC Source directory' should point to your fpc source directory. This directory normally ends with /fpc/ or /fpcsrc/ (e.g. /usr/src/fpcsrc or /home/username/freepascal/fpc) and contains directories like 'compiler', 'docs', 'fcl', 'rtl' and 'packages'. 

See here for the documentation about this dialog: [IDE_Window:_Environment_Options](<IDE_Window__Environment_Options.md> "IDE Window: Environment Options")

Hint
    

To update lazarus you can use 
    
    
    []$ svn update lazarus
    

then for either update pathway: 
    
    
    []$ make clean all   (gmake on BSD)
    

This will rebuild lazarus and create an IDE without lazarus packages. To link your installed packages do **after** the above: 
    
    
    []$ ./lazbuild --build-ide=
    

You may have to append other options if for example you use a custom config directory (ie. add --pcp="C:\Documents and Settings\<USER>\Local Settings\Application Data\lazarus-tests") 

#### Installing Lazarus under Debian GNU/Linux

There are preliminary Debian packages for lazarus available for download. They are not the latest versions, however. Make sure you read /usr/share/doc/lazarus/README.Debian carefully before you start using it. Feedback is needed and appreciated; please send your comments to Carlos Laviola <claviola@debian.org>. 

Note that for a fully working Lazarus install, no older or incompatible versions of, for example, the fpc source or fpc compiler must be installed. Remove them by typing 
    
    
     dpkg -r <package name>   
    

without .deb extension. And then install the newest versions as described. 

#### Installing Lazarus 64 bit version on Red Hat EL 5

Linking may fail when you do your first compile with an error "incompatible libglib" if the following symbolic links are not present in the /usr/lib64/ directory 

libglib.so -> libglib-1.2.so.0 libgtk.so -> libgtk-1.2.so.0.9.1 libgdk.so -> libgdk-1.2.so.0.9.1 

The steps for successful installation on Red Hat were 

yum install gimp gimp-devel gtk+* gtk+-devel 

Then install the following rpms 

fpc-2.2.4-0.x86_64.rpm fpc-docs-2.2.4-0.x86_64.rpm lazarus-0.9.28.2-0.src.rpm lazarus-debuginfo-0.9.28.2-0.x86_64.rpm fpc-2.2.4-3.fc10.x86_64.rpm fpc-src-2.2.4-091006.x86_64.rpm lazarus-0.9.28.2-0.x86_64.rpm 

add -Fl/usr/lib64 under #ifdef cpux86_64 in fpc.cfg 

Then add the symbolic links listed above. 

#### Fedora Core 11

Needed packages: 
    
    
    gtk2-devel glibc-devel binutils
    

Useful packages for development: 
    
    
    subversion wget gcc bison rpm-build
    

### Installing Lazarus under Windows

The current releases of the Windows Lazarus binary packages install very easily, and should work 'out-of-the-box'. 

It is even possible to install the whole Lazarus/FPC package on a portable USB drive (capacity at least 256 MB), for use in environments where you are not allowed to install software on your Windows workstation or where you haven't got administrator privileges. You do have to be a little careful about adjusting the paths in the compiler and environment options and the fpc.cfg file. It may also be necessary to keep the directory for test compilation on your portable drive. 

<< Q:DOES ANYONE KNOW HOW TO SET UP RELATIVE PATHS IN THESE TAGS AND FILES, SO THAT THE ADDRESSING WORKS WHEN YOU MOVE THE USB DEVICE TO ANOTHER MACHINE WHERE IT HAS A DIFFERENT DRIVE LETTER? 

A: This is what I do. It's relatively convoluted, but it's the best solution I've found. I have a "bin" directory on my USB drive, where I have several scripts and utilities installed. Inside that directory is a batch file called "setenv.bat" which sets an environment variable called THUMBDRIVE. It is set by using this command in the batch file: 
    
    
      set THUMBDRIVE=%CD:~0,2%
    

This is used in setenv.bat to set some paths to other things I have installed on the USB drive. I also have a link in the root directory of the thumb drive with this property: 
    
    
     %SystemRoot%\system32\cmd.exe /k bin\setenv
    

so that when I click on that link when the thumb drive folder is diplayed after inserting it, it will open a command prompt at the thumb drive with the environment variables set from setenv.bat. 

Also inside the bin directory is [sed](<http://sed.sf.net>) (the actual binary is one I obtained from the mingw distribution). So I created another batch file called fixlaz.bat which takes one argument, the drive letter which is currently in the Lazarus/fpc settings files that you want to change (note that this is the previous drive letter the last time you ran fixlaz.bat, not the current one of your USB drive which fixlaz.bat already knows). You will need to create this batch file to fit where you installed Lazarus in the root directory structure of the drive if you didn't install it directly in the root folder, and then repeat these lines also for the editoroptions.xml and fpc.cfg files (fpc.cfg is the the fpc bin directory, which might be buried deep in the lazarus folder): 
    
    
     copy %THUMBDRIVE%\lazarus\environmentoptions.xml %THUMBDRIVE%\lazarus\environmentoptions.bak
     sed -e 's/%1/%THUMBDRIVE%/g' %THUMBDRIVE%\lazarus\environmentoptions.bak > %THUMBDRIVE%\lazarus\environmentoptions.xml
    

So to use it, I would type at the command prompt of the USB drive: 
    
    
     fixlaz G:
    

if "G:" was the previous drive letter used the last time I ran it. This will then scan the file(s) and replace "G:" with the current drive letter of the USB drive, which is in the %THUMBDRIVE% environment variable (after running setenv.bat). Note that you could write it to save the current drive letter in a separate file, so that you wouldn't have to remember it yourself the next time. But this works well enough for me right now. >>

The binary package is available for Linux and Windows from 

<http://sourceforge.net/project/showfiles.php?group_id=89339>

Download the latest release (currently Lazarus-0.9.28-0-win32.exe) and launch the application. You will be taken through a typical Windows installation, in which the FPC compiler and source libraries are installed within the same directory structure as Lazarus, and the IDE should launch and operate without significant problems, provided you have uninstalled(!!!) any previous version of Lazarus and/or FPC (often found in the C:\pp directory). 

You can also use a Lazarus Snapshot. For download locations see [Lazarus Snapshots Downloads](<Lazarus_Snapshots_Downloads.md> "Lazarus Snapshots Downloads"). 

Tip: It's perhaps a good idea to reboot your Windows after you installed Lazarus and before you try to install additional lazarus components as zeoslib fore example. 

#### Installing from source

If you prefer to install from sources, then follow these instructions (Thanks to Joe for the initial Step-by-step description). Please note these instructions are for SubVersion. 

You have to download the lazarus source from one of the [snapshots servers](<Lazarus_Snapshots_Downloads.md> "Lazarus Snapshots Downloads"). Then unzip it to c:\lazarus for example [below called $(LazarusDir)]. 

Or you use SVN: 
    
    
     []$ svn checkout <http://svn.freepascal.org/svn/lazarus/trunk/>
    

You have to install at least fpc-2.2.4, but a fpc-2.5.1 snapshot is also possible. It compiles for me, and lazarus.exe works. The path to my fpc is: c:\pp\bin\win32. In this directory you can find a lot of exe (make, grep, ppc386, and so on). 

Open a command line. Start->Run...>command or choose MS-DOS icon. 

Type: 
    
    
    //Replace $(LazarusDir) with the path you have unzipped/checked out Lazarus
    cd $(LazarusDir)\ide
    set path=c:\pp\bin\win32;$(LazarusDir)  //of course change the first to
                                            //the path of yours
    windres -i lazarus.rc -o lazarus.res
    cd $(LazarusDir)
    make
    

If you are lucky then you can type: lazarus.exe. The source editor, dialogs and the property editor work, but not perfectly... Look and you will see. 

You can compile examples also: 
    
    
    cd $(LazarusDir)\examples
    make
    

~~If you extracted lazarus to another drive, eg.: d:\lazarus. It can happen that you need the gnu utility to make it. If you have it, you can take its path to set path=...; <gmakepath> but it is simpler not to choose drive d:\~~

That's all. 

### Installing Lazarus under FreeBSD

#### FreeBSD 6+

The latest version of Lazarus into FreeBSD port tree is 0.9.28.3 (Fixes branch). We can to install lazarus using FreeBSD ports. 
    
    
    []# cd /usr/ports/editors/lazarus && make install clean clean-depends
    

If you get the messages about missing source files, go to [menu bar >> Environment >> Environment Options >> Files Tab: FPC source library] and enter the directory of the FPC source files. It can be obtained from /usr/ports/distfiles/freepascal 

### Installing Lazarus under PC-BSD 1.0rc1+

You can install Lazarus on PB-BSD by simply downloading the Lazarus PBI from [PBI Dir](<http://www.pbidir.com/>)

Note that you must install glib* port from /usr/port/devel/glib* or glib packages by pkg_add -r glib12 glib20. I will fix this in new PBI releases. 

**[other OpenBSD/NetBSD/DragonFlyBSD goes here]**

### Installing Lazarus under macOS

First need to install the Apple developer tools. Then you can install the latest release from [[1]](<http://sourceforge.net/projects/lazarus/files/>) or the daily snapshots from the [snaphot servers](<Lazarus_Snapshots_Downloads.md> "Lazarus Snapshots Downloads"). 

More elaborate instructions can be found in [Installing Lazarus on macOS](<Installing_Lazarus_on_macOS.md> "Installing Lazarus on macOS"). 

If you install all 3 .dmg snapshot files, this also installs the FPC compiler and the FPC and Lazarus source too. If you only need to run Lazarus to develop and compile your own apps, you don't need to install the FPC and Lazarus sources as described above. If you need to rebuild Lazarus or want to help with its development, then you should to install the Lazarus source to a folder where you have read-write permission, as described above. 

Now you can start Lazarus from the folder /Applications. 

### Installing Lazarus under Haiku

Lazarus requires Qt under Haiku. Qt is not installed by default under Haiku. You need to install package available from this site : <http://qt-haiku.ru/>

Currently, there is no binary package to install Lazarus. 

You will have to compile Lazarus from sources. 

Detailed instructions to build Lazarus under Haiku are available here : [Installing Lazarus on Haiku](<Installing_Lazarus_on_Haiku.md> "Installing Lazarus on Haiku")

### Additional experiences and advice on installing FPC and Lazarus

On linux, getting Lazarus to work can be complicated if you have an older version installed. If you have already installed earlier versions of FPC and/or Lazarus, you have to take great care that the PATHS are adjusted so that only your new versions are called. 

Pitfalls that I have experienced in various attempts with SuSE linux include: 

The rpm versions often install FPC in /bin or /usr/bin, with the libraries in /lib or /usr/lib. 

If you perform a make install on a source version or use a .tar.gz, the compiler is usually installed in /usr/local/bin with the libraries in /usr/local/lib. 

So if earlier versions have been installed from rpm, make sure all links are changed to reflect the directory structure of your new compiler. The place to make the changes is in /etc/fpc.cfg ; make sure there are no remnant configuration files in your home directory or in your FPC download directory. Typically they are found in ~/.fpc.cfg 

Make sure the links in /etc/fpc.cfg point to the correct versions of the libraries (particularly if you have older versions still lurking around). 

In /usr/local/bin or /usr/bin there is a file called fpc ; this is the executable that is called when you invoke the compiler. One of the first things it does is to open ppc386 in the same directory. This has to be the same version as the fpc file, or can be a link pointing to the true ppc386 in some other directory (such as a library directory, or the fpc directory in your own development space); be sure to update your link if you install a newer version of the compiler. 

Try running the compiler from the command line using one of the simple example programs or one which you wrote yourself. 

Turning to Lazarus: 

If you use a downloaded snapshot (.tar.gz) or get the source from CVS, enter your lazarus directory and type 
    
    
    make
    ./lazarus 
    

and hopefully the IDE should load. There may be a message saying it can't find the sources; if so, go to the menu at the top and select Environment -> Environment options and insert the name of the directory where you expect to find your lazarus files (eg ~/FreePascal/lazarus), your compiler (e.g. /usr/local/bin/ppc386), the FreePascal source directory (e.g. /usr/local/lib/fpc/$version/fpc or a directory in your own space e.g. ~/FreePascal/fpc). Then close the dialog and start programming. 

You may find further problems when you start programming or compiling; most of these will be related to PATH selection. 

Many of the difficulties are dealt with in the Lazarus FAQ, found either on this WiKi site, or a more complete list on the main Lazarus site 

[http://www.lazarus.freepascal.org/modules.php?op=modload&name=FAQ&file=index](<http://www.lazarus.freepascal.org/modules.php?op=modload&name=FAQ&file=index>)

From the top menu select Run -> Compiler Options (or Project -> Compiler Options in more recent releases of Lazarus) and select the Paths tab on the resulting dialog. There are several boxes for entry of paths for Unit files, Include files, Other source files, Libraries etc. For each of these, it is worth pressing the ... (ellipsis) button on the right, and you will be given a blank window for inserting path information, and a lower window giving some suggestions. Select as many as you like (or ALL!) and press the ADD button to transfer the selections to the upper window. When your selection is complete, press OK and move on to the next box. If you know of other paths which need to be followed, press the Browse button associated with each box, and insert the additional paths. 

When you think you have got your paths right, try to compile your application. If there are further path-related errors ('can't find unit .......') then go back and check your paths, both in /etc/fpc.cfg and in your Run -> Compiler Options dialog (or Project -> Compiler Options depending on your version). 

Once you have got the environment and PATHS correct, it is worth saving the settings to a file, otherwise you will have to go through this process for each new project. In the Run -> Compiler options dialog there is a Load/Save button; select this and then save the setings to the filename of your choice: mine is ~/FreePascal/default.cfg. Then each time I start a new project, I can go to the Run -> Compiler Options dialog (or Project -> Compiler Options), select Load/Save, and load my default.cfg file to the new project. You can, of course, modify the options for each new project if you so desire.

---

_Source: [https://wiki.freepascal.org/Installation_hints_for_old_versions](https://web.archive.org/web/20240121085900/https://wiki.freepascal.org/Installation_hints_for_old_versions)_
