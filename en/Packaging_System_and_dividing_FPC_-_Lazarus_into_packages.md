# Packaging System and dividing FPC - Lazarus into packages

## Contents

  * 1 Introduction
  * 2 Order of building
  * 3 Requirements
  * 4 Tips
    * 4.1 External Library Dependencies
    * 4.2 Breaking the dependency cycle
    * 4.3 Outer RTL
    * 4.4 Try to offer as much sourcecode as possible per compiler invocation
  * 5 Documentation of the new package system
  * 6 Performance
  * 7 Planning



## Introduction

FPC is quite a large body of source, and the organisation of the source is a constant problem, with immense effects on release engineering, deployment and packaging. On this page I try to describe some of the problems involved with how FPC is packaged. This is not a constant situation, it evolves as new packages, dependencies and insights are acquired. 

This page is updated for the changes in early 2008 when fcl was split up into packages, and all packages (fv + fcl subpkgs + extra + base) reside in packages/ and have an own dependance system for build order. 

## Order of building

Besides merely splitting up the source into packages, there is also a dependency and order-of-building problem. Currently, the order of compilation is split up in certain generations: 

  1. rtl-compiler-rtl-compiler-rtl-compiler (the cycle)
  2. RTL
  3. packages/( a collection of packages)
  4. IDE / utils



Note, when packages in packages/ have dependancies on other packages, then the makefile(.fpc) in packages must be also fixed, to allow smooth paralel compilation. 

## Requirements

  * build order 
    * the order of building should be fairly predictable and static as not to hinder development too much, and the same on all OSes.
    * Dependencies on external libraries shouldn't be introduced to soon in the dependency hierarchy. (see also separate paragraph)
    * The part of the RTL that is compiled three times as part of the bootstrap should be as small as possible (see Outer RTL paragraph)
  * package format 
    * In the future, some packages (like Indy) might be externally maintained. These must be clearly marked as such, so that only emergency fixes and FPC specific release engineering are done on these packages. (other enhancement should go through the package owners)
    * It should be possible to map FPC package structure on lazarus packages and Delphi style dynamically loadable packages. Directly or indirectly.
    * At the present stage, the opinions about if there should be a lot of dynamically loadable packages (e.g. one per compilation pkg), or one big one are still divided. The package format should ideally support both. (IOW Multiple packages should be able to be fused into one .dll/.bpl/.so)
    * The same with respect to: source and binary packaging. Both should be possible AND AT THE SAME TIME.
    * The package format should provide lots of metadata for visual use. one line descriptions for units, multiple lines descriptions for the package itself.



## Tips

  * avoid package/unit/class names that are too general like "Image", "xml" or "sockets", to avoid annoying name clashes with Delphi and other 3rd party pkgs, unless the package is really as compat as possible substitute for the Delphi package. Prefix "fp" if necessary.



### External Library Dependencies

One should be careful where packages are put that have external libraries as dependencies, this because other packages that don't require the dependency on the lib itself, acquire it because they use some parts of a package that requires it. 

Examples: 

  * if one makes the RTL depend on some graphical library which is used for Unit Graph, all programs with that RTL might inherit that dependency
  * Another example (and true problem): the general db support in the FCL which depends on the client libraries of all supported databases.



This gets even more important if we start bundling packages into dynamic libs. If we would stuff all units in one big lib (which is easiest from a deployment and maintenance view), this big shared lib would have three dozen library dependencies, from Oracle to little things like X widget sets. 

### Breaking the dependency cycle

If you have a package that _must_ be relatively early in the hierachy, but there is a dependency on a library that is only sideways relevant, then you have a problem. There are several general solutions: 

  * If OOP, try to use plugin classes that can be specialized later.
  * If procedural Use the "driver" model. Try to build a record of procedurevariables that encapsulate the functionality, and can be registered later. Examples : threading (cthreads), memory drivers (cmem), widestrings (cwstring), local (clocale)
  * Go for a fully dynamical approach with a library plugin architecture. (**SF** , needs advanced dynamic linking support)



### Outer RTL

From time to time one sees the term "Outer RTL" on the FPC lists. The "Outer RTL" is a phrase coined in discussions about decreasing the size of the current RTL that is compiled 4 times in every build process. This mainly to decrease build time and thus make compiler development less annoying. Essentially it means that all units not required by the compiler should move out of the current RTL package. 

Some of these units can move to independant packages. Outer RTL is a name for a package that is more or less what remains. E.g.: 

  * Graph should move to a separate package, since implementation on certain OSes might need dependencies, and the chance that other parts of the RTL or FCL depend on it are slim.
  * A bunch of small units should move to a package "outerrtl". The exact name is still to be found. The main reason to keep these together is that we can then recreate the RTL dir structure for this package (with per OS and target) dirs, and manage it like it was the RTL. It would be a bit overkill to create a package per unit with a full dir structure.
  * compability units that shouldn't be used for new development (like DOS) could go into a tpcompat package. This will also clear up the (legacy) status of these units.



To my knowledge, the only reason against this approach is the inflation of the word "RTL". People might expect units in package RTL to be the same as specified in the manual of TP/BP or Delphi for "RTL". (as far as that exists, and the packaging isn't merely due to layout of the respective source trees). I find this reasoning highly doubtful, since we already break compability with Delphi/TP in much, much more complex ways, for such utter minor detail to be a serious enough source of confusion to complicate the buildprocess for it. 

### Try to offer as much sourcecode as possible per compiler invocation

Offering the compiler binary as much source as possible in one go is very important for performance because: This also goes for multiple "main" files. If a project has multiple .exe's, the compiler should be able to compile them in one run. 

  * it reduces the total amount the compiler binary has to start
  * it possibly reduces the amount of unit reloading and searching
  * it opens a door to paralel compiling with two compiler instances running on different cores (in the distant future)



## Documentation of the new package system
    
    
     20:13 < Synopsis> oliebol: there is no docs about fppkg
     20:13 < Synopsis> oliebol: the docs are the source :)
     20:14 < Synopsis> see compiler/utils/fppkg.pp and rtl/common/fpmkunit.pp
    

## Performance

Besides ease of use, a good package system and design could speed up building significantly. Specially if we somehow can get a tool that "lives" during the entire build process (preferably the compiler), due to caching .ppu's etc, speed improvements could be made. 

This is also a possibility for dual core to shine. 

## Planning

To get the packaging working fully, the following steps are envisioned: 

  1. get building working
  2. get installing working
  3. get packaging working
  4. decide on repository layout
  5. local repository setup
  6. design upload (=filecopy) to local repository
  7. design download (=filecopy) from local repository
  8. integrate network stuff
  9. remote repository
  10. signing of packages

---

_Source: [https://wiki.freepascal.org/Packaging_System_and_dividing_FPC_-_Lazarus_into_packages](https://web.archive.org/web/20240226045239/https://wiki.freepascal.org/Packaging_System_and_dividing_FPC_-_Lazarus_into_packages)_
