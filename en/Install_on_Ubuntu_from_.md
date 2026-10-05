# Install on Ubuntu from .deb files

[![Logo-ubuntu cof-orange-hex.svg](https://upload.wikimedia.org/wikipedia/commons/thumb/a/ab/Logo-ubuntu_cof-orange-hex.svg/50px-Logo-ubuntu_cof-orange-hex.svg.png)](</File:Logo-ubuntu_cof-orange-hex.svg>)

This article applies to [Ubuntu](</Category:Ubuntu> "Category:Ubuntu") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

**Please See[Installing Lazarus on Linux](<Installing_Lazarus_on_Linux.md> "Installing Lazarus on Linux") \- a page that covers most of what you need for most Linux Distributions.**

This page is substantially out of date and may be removed. 

│ **[Deutsch (de)](</Install_on_Ubuntu_from_.deb_files/de> "Install on Ubuntu from .deb files/de")** │  **English (en)** │    
****

## Contents

  * 1 In General
    * 1.1 Why install from .debs?
    * 1.2 .deb files
  * 2 In Particular: Lazarus 0.9.30 on Ubuntu 10.04
    * 2.1 Get The files
      * 2.1.1 Repository
      * 2.1.2 SourceForge
      * 2.1.3 Find SourceForge .deb files
      * 2.1.4 Download and Extract
      * 2.1.5 Inside the folders
    * 2.2 Install Free Pascal
      * 2.2.1 Errors
      * 2.2.2 Missing Dependencies
      * 2.2.3 Install missing dependencies
        * 2.2.3.1 a52cat
      * 2.2.4 Reinstall Free Pascal
        * 2.2.4.1 fp-units-multimedia
      * 2.2.5 Check the installation
    * 2.3 Install Lazarus
      * 2.3.1 Errors
        * 2.3.1.1 lcl-qt4-0.9.30
      * 2.3.2 Test
        * 2.3.2.1 Check with Synaptic
        * 2.3.2.2 Fix Broken packages
    * 2.4 Final test
  * 3 See also



## In General

### Why install from .debs?

The easiest way to install Lazarus on Ubuntu is via the software Centre or Synaptic. However if that is unsuccessful or the version of Lazarus in the repository is too old and you want a newer version then you can 'install from debs'. 

### .deb files

.deb files are the ideal files for installing on Ubuntu. You need to download the Lazarus .deb files you want. They will usually be on SourceForge. There will be a Lazarus .deb and a Free Pascal .deb. They will _match_ each other. 

## In Particular: Lazarus 0.9.30 on Ubuntu 10.04

### Get The files

<https://sourceforge.net/projects/lazarus/files/>

#### Repository

The versions in the Ubuntu repository are 

  * Lazarus 0.9.28
  * Free Pascal 2.4.0



These are too old. 

#### SourceForge

The versions on SourceForge are 

  * Lazarus 0.9.30
  * Free Pascal 2.4.2



These are what I want. 

#### Find SourceForge .deb files

The names of the .deb files I need to download are 

  * lazarus-0.9.30-i386.deb.tar (71.1MB)
  * fpc-2.4.2-0.i386.deb.tar (38.5 MB)



You can search for these directly in a Search engine with something like 
    
    
    Lazarus 0.9.30 .deb SourceForge
    

#### Download and Extract

Download these two files. Double click on them and accept the offer to extract. 

  * Extract Free Pascal into folder 'a'
  * Extract Lazarus in folder 'b'.
  * Move the folders onto the Desktop



#### Inside the folders

Once done, there are 21 .deb files in folder a and 12 .deb files in folder b. Two common mistakes when installing Lazarus are 

  * not installing Free Pascal first
  * not installing Free Pascal source code.



However, notice that Free Pascal source code is one of the files in folder a. Therefore, this will NOT need to be downloaded and installed separately. 

### Install Free Pascal

This should be done first. 

Open a terminal. 
    
    
    cd Desktop
    cd a
    dpkg -i *.deb
    

This last command will install Free Pascal and Free Pascal source code. 

#### Errors

Ideally, everything would run smoothly and the installation would complete without error. However when I did it, there were a number of errors. These errors are sometimes 'show-stoppers' so they should be sorted out if possible. The most common error is a missing dependency, as happened to me. 

#### Missing Dependencies

This means that a package that Lazarus expects to be installed on your system already is not there. The missing packages were: 

  * libgtk2-dev
  * libogg-dev
  * libvorbis-dev
  * libmodplug-dev
  * a52cat
  * \+ a few others. If you look through the output in the terminal you will see which packages failed to install and why it was.



#### Install missing dependencies

Open Synaptic. Search for each of the missing packages and install them. 

##### a52cat

This package is not in the Ubuntu 10.04 repository. We have to leave it and hope for the best. 

#### Reinstall Free Pascal

Repeat the previous installation procedure with: Open a terminal. 
    
    
    cd Desktop
    cd a
    dpkg -i *.deb
    

Free Pascal will now install fairly smoothly. 

##### fp-units-multimedia

Only the multimedia unit will not install. This is due to the missing a52cat package. However it may not be critical. 

#### Check the installation

You can check the Free Pascal installation in three ways: 

  * Run it



Open a terminal 
    
    
    fpc
    

it should open in the terminal. 

  * Look in Synaptic and check that fpc and fpc-src have been installed.
  * Find the files



You can see where the files are stored with 
    
    
    sudo dpkg -L fpc
    

### Install Lazarus

This is similar to Free Pascal 

Open a terminal. 
    
    
    cd Desktop
    cd b
    dpkg -i *.deb
    

and the installation will proceed. 

#### Errors

An error occurs installing package lcl-qt4-0.9.30. 

##### lcl-qt4-0.9.30

From the terminal output you can see that the unit can not be installed due to a dependency on ... However, if you read the information that appears in Synaptic for this package then it says 

"Actually this is an empty package but ..." 

I think we can safely ignore it. 

#### Test

Open Lazarus and try to create a simple 'Hello World' application with a button click. It worked with mine and the installation is complete. 

  


##### Check with Synaptic

You can check that Lazarus has been installed with Synaptic 

##### Fix Broken packages

Unfortunately, I now have 2 broken packages on my Ubuntu system, fp-multimedia and lcl-qt4. I choose 'Edit/Fix broken Packages and the fp-multimedia unit goes green but the lcl-qt4 package goes bright red - not normally a good sign. However if you select 
    
    
    Apply
    

then Ubuntu will fix the multimedia package and remove the qt4 package. Everything is then in order. 

### Final test

Create a Hello World application with a button and a label. If this works then things are looking good. Lazarus is installed. 

## See also

  * [Install on Ubuntu](<Install_on_Ubuntu.md> "Install on Ubuntu")

---

_Source: [https://wiki.freepascal.org/Install_on_Ubuntu_from_.deb_files](https://web.archive.org/web/20241104201146/https://wiki.freepascal.org/Install_on_Ubuntu_from_.deb_files)_
