# FPC Dotted RTL

## Contents

  * 1 The project
  * 2 Compiling
  * 3 Makefiles
    * 3.1 RTL
    * 3.2 Packages
  * 4 fpmake
    * 4.1 working principle
    * 4.2 Transformations file
  * 5 Other utilities



## The project

The FPC distribution has support for creating a "dotted RTL". This is a set of units from RTL/Packages where all unit names are namespaced (dotted). 

The units that are found in Delphi 2009 (and upwards) and FPC have the same name. For units that exist in FPC but not in Delphi, the occasion has been used to make the names more consistent. New namespaces have been chosen for all packages, and the last part of the unitname has been cleaned up. 

The uses clause of the units in the RTL and Packages have been modified so they refer to the correct unit 

This is managed entirely through makefiles and fpmake. 

Note that fpcmake (the older tool) has no knowledge of the dotted RTL, it does not need this knowledge. 

The files that are dotted reside in a subdirectory _namespaced_. They set a define and include the non-dotted unit file. 

## Compiling

Only the RTL and the packages can be compiled with dotted units. The creation of dotted units is managed through the FPC_DOTTEDUNITS define in the makefiles. 

To make the dotted rtl you must do, in the FPC dir 
    
    
    make -C rtl FPC_DOTTEDUNITS=1 clean all
    make -C packages FPC_DOTTEDUNITS=1 clean all
    

and a 
    
    
    make -C rtl FPC_DOTTEDUNITS=1 install
    make -C packages FPC_DOTTEDUNITS=1 install
    

afterwards. 

## Makefiles

Here we explain how the makefiles manage the creation of dotted RTL/Packages. 

### RTL

The Makefile.fpc files in the RTL directory have been cleaned up. The observation was made that almost all targets are common to all platforms (logical, it is the RTL). What differs per target for each os are the dependencies and possibly some compiler options. 

So a file was made **inc/Makefile.rtl** , which is used as an include file for the **Makefile.fpc** files in the OS-Specific directories. It is entirely parametrized. 

In the common **Makefile.rtl** the following definitions are made: 

  * For each unit, a variable **NAMEUNIT** is defined. For example **SYSTEMUNIT** or **WINDOWSUNIT**.



    

    Depending on the **FPC_DOTTEDUNITS** , this will be the non-dotted or the dotted name.

  * For each unit, a **NAME_DEPS** variable is defined that specifies the dependencies for this unit.
  * In the **prerules** section of the os-specific **Makefile.fpc** , you can define **NAME_DEPS_OS** and **NAME_DEPS_CPU** for OS and CPU specific dependencies.



    

    These defines will be taken into account in the target rules (see below)
    You should only use _NAMEUNIT$(PPUEXT)_ when defining unit dependencies, to make sure the correct unit is taken.

  * For each unit, 2 targets are defined:


    
    
    name$(PPUEXT)''' : $(NAME_DEPS) $(NAME_DEPS_OS) $(NAME_DEPS_CPU)
                     $(COMPILER) $(NAME_OPTS) $<
    
    dottedname$(PPUEXT) : $(NAME_DEPS) $(NAME_DEPS_OS) $(NAME_DEPS_CPU)
                     $(COMPILER) $(NAME_OPTS) $<
    

Depending on **FPC_DOTTEDUNITS** , one of these 2 units will be compiled. 

The common targets have been removed from the OS-specific **Makefile.fpc** files, but OS-Specific make rules for os-specific targets remain in these OS-specific **Makefile.fpc** files: 

  * The build units. For build units (**buildrtl**), the uses clause of the build unit is modified so it uses either dotted or non-dotted units.
  * Loaders. (assembler files)
  * Other os-specific files.



To regenerate the OS-Specific Makefiles, you must include the **inc/Makefile.rtl** using the -x option. For example: 
    
    
    cd rtl/linux
    fpcmake -x ../inc/Makefile.rtl -Tlinux
    

The **rtl/regenmakefiles.sh** script does this for all directories below the RTL. The **packages/regenmakefiles.sh** script does this for all directories in the packages. 

  


### Packages

Noting that, since the use of fpmake, all package makefiles were in essence the same except for the package name a single makefile exists now in **build/Makefile.pkg**. 

This file is generated from **build/Makefile.fpc** and contains the rules to regenerate _fpmake_ correctly. 

The package-specific Makefile.fpc has been removed. All package-level Makefiles have now been replaced with a 2-line Makefile: 

  * a single line to set the package name.
  * a single line to include the **build/Makefile.pkg**



All the rest is handled by **fpmake** , see below. 

Every package directory now has a 'namespaces.lst' file which contains the mapping of non-dotted to dotted unit names (see below). it also contains some rule lines that specify how to map unit paths to include paths. 

## fpmake

### working principle

fpmake could already perfectly handle dotted names. What was needed was a way to keep the current files with as little modifications as possible. For this reason, a system was devised where the non-dotted names of units were mapped to dotted unit names. 

2 changes have been made in fpmake to make this possible: 

  * It now understands a _transformations_ file. The transformations listed in this file are applied to all target and dependency definitions.


    
    
     This file can be specified with a property **NameSpaceMap** on the **TPackage** object. The name is relative to the package dir. 
    

  * a **\--ns** or **\--namespace** option was added. When added, the tranformations listed in the **NamespaceMap** are applied to the targets.



### Transformations file

The transformations file is a file containing rules. There are 2 kinds of lines 

  * A filename transformation line


    
    
    OldFile=NewFile
    

    

    for example
    
    
    src/mysql56dyn.pp=namespaced/Api.Mysql56dyn.pp
    

  * a path transformation line. This is of the form


    
    
    {<X><OP>:<dir>}[=DestDir]
    

    

    Here _X_ is the path to transform, it is one of 

  * **s** the source path
  * **i** the include path
  * **u** the unit path


    _OP_ is the transformation operation to perform. It is one of 

  * '_*_ It replaces the complete specified directory _dir_ with _DestDir_
  * '_-_ It removes the specified _dir_ from the search path.
  * '_+_ It adds the specified _dir_ to the search path if it was not there yet.



For example 
    
    
    {s*:src/}=namespaced/
    

will replace all **src** directories in source paths with **namespaced**
    
    
    {i+:src/}
    

Will a dd _src_ as a include file path. 

## Other utilities

The utils/dotutils directory contain some utilities to help converting a project with non-dotted names to dotted names (or both) 

  * **genunitnames** takes as input a list of tranformations (nondotted=dotted) and generates UNITNAME variables for use in a Makefile.


  * '_addnamespacetofpake_ will add a line NameSpaceMap:='namespaces.lst' to a fpmake program source. It attempts to add this in the correct location.


  * **encloseunit** enclodes standard input in a IFDEF block


    
    
    {$IFDEF FPC_DOTTEDUNITS}
    {$ENDIF FPC_DOTTEDUNITS}
    

    

    if your editor supports filtering a selection in a block, this can be used to quickly enclose a a block of lines in an ifdef

  * **fixuses** takes as a parameter a list of transformations (nondotted=dotted) to apply, and as standard input a uses line.



it will then transform the uses clause to a form like this: 
    
    
    {$IFDEF FPC_DOTTEDUNITS}
    // dotted uses clause
    {$ELSE}
    // non-dotted uses clause
    {$ENDIF FPC_DOTTEDUNITS}
    

    

    if your editor supports filtering a selection in a block, this can be used to quickly enclose a a block of lines in the above-mentioned form

  * **makedottedfiles** will read a list of units and transform them to use a uses clause in the following form:



{$IFDEF FPC_DOTTEDUNITS} // dotted uses clause {$ELSE} // non-dotted uses clause {$ENDIF FPC_DOTTEDUNITS} The unit will then be written to a new file. It has various options which are listed with the **-h** command-line options. 

  * **prefixunits** will read a single unit and transform it to use a uses clause in the following form:



{$IFDEF FPC_DOTTEDUNITS} // dotted uses clause {$ELSE} // non-dotted uses clause {$ENDIF FPC_DOTTEDUNITS} The unit will then be written to a new file. It has various options which are listed with the **-h** command-line options. 

  * **proxyunit** is a small program that takes 2 names (non-dotted and dotted) of units and writes a dotted unit file which



sets the correct define and includes the original filen. 

  * **reworkmakefile** reworks makefile rules: it replaces hardcoded unit names xyz in a rule with variable XYZUNIT.



    

    (see genunitnames for how to create the variables)

  * **replaceunitnames** replaces unit names in a makefile with the **$(NAMEUNIT)** macro.


  * **replaceword** replaces a word with another word in a list of files.



  
The following units do the actual work: 

  * **namespacetool** is a unit used to generate the **namespaces.lst** files in the packages directory, based on a list of known aliases.
  * **prefixer** is the unit used in **makedottedfiles** which actually rewrites the uses clause of a unit.
  * **rewritemakefile** is the unit used in **reworkmakfile** which does the actual work.



There is a **readme.txt** file with more explanations, and the **known.txt** file contains all transformations used in RTL and packages

---

_Source: [https://wiki.freepascal.org/FPC_Dotted_RTL](https://web.archive.org/web/20250323152004/https://wiki.freepascal.org/FPC_Dotted_RTL)_
