# FPC Subtarget Support

As of version 3.3.1, medio 2023, FPC has support for subtargets. 

## Contents

  * 1 What are subtargets
  * 2 Using subtargets
    * 2.1 Using subtargets in Makefiles
    * 2.2 Using subtargets in fpcmake
    * 2.3 Using subtargets with fpmake



## What are subtargets

A subtarget is a named set of configuration options for the compiler which is applied during compilation and installation of the RTL, Packages and utils. The compiler itself can also be compiled with a subtarget if you so desire. 

This named set of configuration options is supplied as a configuration file much like the default configuration file (fpc.cfg). This file is read in addition to the regular configuration file, and must have a fixed name. 

For example, to create a subtarget 'myfeature', you must create a configuration file called **fpc-myfeature.cfg**. The configuration name must be lowercase in the filename. 

When compiling, you can select a subtarget on the command-line using the **-t** compiler option: 
    
    
    fpc -tmyfeature
    

Only 1 subtarget can be specified, if you specify a second, you will get an error. 

If so desired, you can combine subtargets into a new subtarget with the help of the #INCLUDE directive. 

In the subtarget definition file, you can put any options that you would specify on the command-line or in a configuration file. 

For example: 
    
    
    -dUNICODERTL
    -Municodestrings
    

This configuration will always be loaded, also in the case when you specify **-n**. So in case of the following command-line 
    
    
    fpc -n -tmyfeature
    

will not load _fpc.cfg_ , but will load _fpc-myfeature.cfg_. 

When you specify a subtarget, besides loading the configuration file, the compiler does 3 other things: 

  * The **$fpctarget** macro will become **CPU-OS-SUBTARGETNAME**. For example


    
    
    x86_64-linux-myfeature
    

  * It will define a macro **FPC_SUBTARGET_SUBTARGETNAME** , for example:


    
    
    FPC_SUBTARGET_MYFEATURE
    

  * It will define a macro **FPC_SUBTARGET** with value **SUBTARGETNAME** , for example:


    
    
    FPC_SUBTARGET=myfeature
    

Because the **$fpctarget** macro changes value when you specify a subtarget, this has an effect on the search path: 

The default configuration file uses this macro to specify the unit path. As a result, the compiler will search in a different directory for units. 

For example for linux 64-bit, if you specify **-tmyfeature** Instead of looking for units in the default 
    
    
    /usr/local/lib/fpc/3.3.1/x86_64-linux
    

instead, it will search in directory 
    
    
    /usr/local/lib/fpc/3.3.1/x86_64-linux-myfeature
    

  


## Using subtargets

### Using subtargets in Makefiles

To compile the FPC RTL and packages for a subtarget, you can specify the SUB_TARGET define 
    
    
    make SUB_TARGET=myfeature clean all
    

This has 2 effects: 

  1. The compiler is called with **-tmyfeature**
  2. When doing an install, the subtarget name will be appended to the installation directory.


    
    
    make SUB_TARGET=myfeature install
    

will (on Linux) install into 
    
    
    /usr/local/lib/fpc/3.3.1/x86_64-linux-myfeature
    

### Using subtargets in fpcmake

The default fpcmake makefile generator creates now by default makefiles that understand the SUB_TARGET option. 

  


### Using subtargets with fpmake

The **fpmake** tool also understands the subtarget concept, you can set the subtarget with the **-s** option. 

The Free Pascal rtl/packages use the subtarget concept to create the unicode RTL (where _string_ =_unicodestring_) in parallel to the regular single-byte RTL (where string=ansistring or shortstring)

---

_Source: [https://wiki.freepascal.org/FPC_Subtarget_Support](https://web.archive.org/web/20241104201610/https://wiki.freepascal.org/FPC_Subtarget_Support)_
