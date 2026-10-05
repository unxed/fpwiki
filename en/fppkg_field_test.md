# fppkg field test

## Contents

  * 1 Fppkg field test with fpc release 2.2.4
    * 1.1 How to use fppkg
    * 1.2 Install a package
    * 1.3 Update package list



# Fppkg field test with fpc release 2.2.4

The FPC releases after the 2.2.x releases will be partly based on the new [fppkg](<fppkg.md> "fppkg") package system. The basics of this package system are already available in FPC 2.2.4 so we can field test the system. 

At first, only one package is available for installation, but more will follow. We also want to test whether it is possible to release updates of packages that are part of fpc itself. So when there is a useful update for one of the packages, then we can use this system to release an update for only this package, without having to release a new full version of the compiler. 

The new package system is not yet fully documented and any help is appreciated. This document is just a first introduction to get started and collect knowledge about any problems we may find during this test. 

## How to use fppkg

When FPC 2.2.4 is installed the 'fppkg' program will also be installed. You can get a list of all installed packages and those packages which are available using the command 
    
    
    fppkg list
    

This will list all FPC packages that are already installed with their version, as well as all available packages that are not yet installed. The version number of the FPC packages will be equal to the FPC release they belong to, with a suffix that indicates the release. A suffix of '-0' indicates that it is a pre-release, for example a release candidate. A suffix of '-1' indicates that it is the version which was released together with the official compiler release. A version of '2.2.4-1', for example, indicates that the installed package is the package that came with the initial FPC 2.2.4 release. 

Other packages that are not part of an FPC release can follow their own version numbering. At the moment of writing, there is only one such package available, namely 'lNet'. 

## Install a package

The following command will install the lNet package 
    
    
    fppkg install lnet
    

fppkg will first try to install the package system-wide in the 'GlobalInstallDir'. When the current user has no rights to do so, the 'LocalInstallDir' will be used. You can find these locations with a 'fppkg --debug list' command. In fact the package-zip containing the sources is downloaded and extracted. Then the package is built and installed. For more information about the exact process check the article about [FPMake](<FPMake.md> "FPMake"). 

After installation the units in the packages are available for use. (Note that when you haven't installed the package in the system-wide location, you may have to add the 'LocalInstallDir' to your compiler configuration) 

## Update package list

When you want to check if there are any new packages available, you have to perform a 
    
    
     fppkg update
    

command to update the local copy of the 'database' with available packages. After a update 'fppkg list' will show the latest packages.

---

_Source: [https://wiki.freepascal.org/fppkg_field_test](https://web.archive.org/web/20220818203457/https://wiki.freepascal.org/fppkg_field_test)_
