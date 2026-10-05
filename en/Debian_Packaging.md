# Debian Packaging

This page explains how to create Debian packages for the [FPC](<FPC.md> "FPC") compiler itself. 

  * for more general information about [FPC](<FPC.md> "FPC") release, see [Release engineering](<Release_engineering.md> "Release engineering") page.
  * for information on creating Debian packages for any application, see [Debian package structure](<Debian_package_structure.md> "Debian package structure").



  


An editor has declared this article to be a stub, meaning that it needs more information. Can you help out and [add some](<Debian_Packaging.md>)? If you have some useful information, you can [help](</Help:Editing> "Help:Editing") the Free Pascal Wiki by clicking on the edit box on the left and expanding this page. 

  


### Getting sources

FPC sources are managed using SVN. The most convenient way to get sources in order to build a release is using the svn export command : 
    
    
    svn export <http://svn.freepascal.org/svn/fpcbuild/tags/release_x_y_z> fpcbuild-x.y.z
    

### Building packages

The top make file of the fpcbuild-x.y.z branch supports the **deb** target which is intended to build Debian packages from source. 
    
    
    make deb
    

However, users may want to use their own packeging rules file 
    
    
    make deb DEBDIR=<some path to debian dir>
    

Or use a particular libgdb version 
    
    
    make deb GDBLIBDIR=<path/${OS_TARGET}/${CPU_TARGET}>
    

### How it works

**To be continued...**

---

_Source: [https://wiki.freepascal.org/Debian_Packaging](https://web.archive.org/web/20250516141921/https://wiki.freepascal.org/Debian_Packaging)_
