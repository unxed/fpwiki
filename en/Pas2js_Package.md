# Pas2js Package

## Contents

  * 1 Overview
    * 1.1 Source
    * 1.2 Compiling a package
    * 1.3 PPMD
    * 1.4 Specializations
    * 1.5 Contains
    * 1.6 Requires
    * 1.7 Using a package
    * 1.8 The -Jc option
    * 1.9 Optimizations
    * 1.10 Dynamic loading
  * 2 Implementation
    * 2.1 ToDos



# Overview

This is a proposal for pas2js packages, working similar to FPC/Delphi packages. 

Advantages of packages: 

  * load code dynamically, e.g. the website starts with a small program and then loads the other code when needed. Or as addon in an IDE.
  * big applications that uses most of its packages, might compile faster as less js needs to be generated.
  * eventually unloading a package, e.g. to load a new version



Disadvantages of packages: 

  * the package js contains all elements, even those never used by the program.
  * duplicate specializations
  * version hell. A package must be shipped with compatible packages. If you change something in the RTL, all packages must be recompiled. This is mitigated when you use Lazarus lpk, so that Lazarus will automatically recompile packages if something has changed.



## Source

A pas2js package has a pascal source starting with keyword **package** instead of _program_ , and it has an optional **requires** and **contains** clause. 

## Compiling a package

When pas2js compiles a package, it generates a pkgname.ppmd, a pkgname.js and optionally a pkgname.js.map file. A **ppmd** stands for "pas2js package meta data". 

  * Options _-FU_ , _-FE_ and _-o_ apply canonically.
  * Option _-Jc_ has no effect.
  * New option _-FP <dir;dir;...>_ to specify the package search path (additional to _-Fu_)



## PPMD

The ppmd is a json file and contains the package units including their sources, plus data about specializations (specialized generics) and whatever else is needed. 

## Specializations

Each pas2js package can have its own specializations - same as in Delphi: 

  * Two dependent packages use the same TList<TExample>.
  * Two independent packages B and C might have two different TList<TExample>. For example package A defines a TExample, and there is no TList<TExample> in A, and packages B and C both use A and define TList<TExample>, then there are two different TList<TExample>, although they have the same name.



## Contains

All units of a package must be listed in the **contains** clause Contrary to Delphi if a package uses a unit outside a package pas2js creates an error. This means a package can only use packaged units. 

## Requires

Lists all packages needed by the package. 

## Using a package

Compiling a program/library with packages: 

pas2js gets a new command line option _-Fp <pkgname;pkgnam;...>_ to specify the packages of a program/library and _-FP <dir;dir;...>_ to add directories to the package search path. pas2js searches the ppmd files first in the -FP search paths and then in the unit search paths given by -Fu. It loads all the ppmd recursively. Then it starts the normal compilation, reading all units, searching units first in the packages, then in the unit path. 

Then it generates js only for the program sources. 

The html needs to load all js files of the packages and the program. 

## The -Jc option

If -Jc is given when compiling a program with packages pas2js concatenates all the package js and the program into one js (generating a big js.map). 

Without -Jc pas2js concatenates all program js, leaving out the packages. 

## Optimizations

With precompiled package js files pas2js cannot remove unneeded code from the packages, as it does not yet have any post-js optimization. Theoretically since the packages contain all the sources, it could generate a program the current way (not using the package js files). 

## Dynamic loading

Packages can be loaded after the program was loaded. 

Unloading is not supported (yet) as this requires more bookkeeping what was loaded. 

# Implementation

## ToDos

  * Scanner: list of used files
  * Parser: **package** , **requires** , **contains**
  * Compiler: Read _-Fp_ and _-FP_ options
  * Compiler: Read recursively **ppmd** files and check checksums
  * Compiler: when searching an unit consider **contains**
  * Compiler: when loading a unit load from **ppmd**
  * Resolver: mark specializations in packages, generate lists for each package,
  * Pas2js Resolver: compute the place where the specialization is created (near first occurrence)
  * Analyzer: when compiling a package mark all public/protected declarations of package units
  * Converter: 
    * create package js
    * create package specializations (the generic is in another package) 
      * many changes to access of local variables, e.g. the declarations of the generic unit
  * Compiler: write **ppmd**
    * checksum of all sources
    * **requires** list with checksums
    * **contains** with sources with linebreaks set to #10
  * With -Jc: generate all js into one file - similar to static linking. Omitting not needed elements (if enabled). Dynamic loading of packages is not supported.
  * Without -Jc: generate all program/library modules into one js. 
    * source map
  * Options -FU, -FE and -o apply canonically
  * libpas2js hooks: 
    * after loading an unit source from ppmd, for decryption
    * before saving an unit source to ppmd, for encryption
    * after loading ppmd, for reading custom data
    * before saving ppmd, for writing custom data
  * Create a release with packages, adding another lpk for each package. The rtl.js will be in the RTL package.

---

_Source: [https://wiki.freepascal.org/Pas2js_Package](https://web.archive.org/web/20250319114617/https://wiki.freepascal.org/Pas2js_Package)_
