# packages

See also [Shared Libraries](<shared_library.md> "shared library") , [List of packages in FPC](<Package_List.md> "Package List") and [packages(disambiguation)](<packages\(disambiguation\).md> "packages\(disambiguation\)")

The FPC/Lazarus project unfortunately is a bit ambiguous with respect to the term packages. In general it is a collection of related units, but the precise way these units are related varies. The most common definitions: 

  1. A set of units in FPC that are treated together for installation purposes. Both in the [fpcmake](</index.php?title=fpcmake&action=edit&redlink=1> "fpcmake \(page does not exist\)") as the newer [fppkg](<fppkg.md> "fppkg") packagemanager contexts.
  2. A set of units in Lazarus (with designtime parts that require registration into the IDE). See [Lazarus Packages](<Lazarus_Packages.md> "Lazarus Packages")
  3. A set of units compiled into a DLL with some extra features that make the package a logical part of the main program, instead of an "external" part. Also called **Delphi package**. I call this **Library package** from now on.



A Delphi designtime package is both 2+3, a Lazarus designtime package is only 2 (at the moment). A Delphi runtime package is only 3. Strictly speaking a runtime package is not necessarily also a DLL/BPL. See DCP below. 

This article is meant as a brainstorm session, and requirements analysis relating to the latter definition, one of the missing pieces of the Delphi compatibility. Most created from browsing an hour on the web, looking at pages about fixes and modding the Delphi packages system, and examining the linker backend of FPC. 

Note that in time Lazarus packages might be implemented as Library (Delphi) packages. 

## Contents

  * 1 Delphi packages
    * 1.1 What do we need them for? What's "special" about packages?
    * 1.2 What problems do packages not solve?
    * 1.3 Lazarus and binary packages
    * 1.4 Implementation
  * 2 What needs to be done
    * 2.1 Details
  * 3 Initialization order and the system unit init problem (plugin units like memmanager)
  * 4 Lazarus and Library packages
  * 5 Versioning
  * 6 Compiling a package
  * 7 Compiling with packages
  * 8 Extensions
  * 9 Debugging
  * 10 PPUMove
  * 11 See also



## Delphi packages

Since Delphi packages is a ambiguous name (it refers both to the IDE angle and the principle, moreover "Delphi" is trademarked), I think **Library package** would be a better designation. I'll at least use it for this article till something better comes up. 

A Library-package can be seen from an implementation point, then it is essentially a DLL with some extras, or from a language point, then it is a library that consists out of a couple of units, and the library itself (the package) is also an own entity with respect to dependencies. (have a dependency on the library, then you have a dependency on all units in it). 

### What do we need them for? What's "special" about packages?

A library package allows to **transparently** split a program in multiple binaries (exe + dll/so) . Dynamically loaded, it also allows for transparent plugin systems. The transparent issue is the key that sets library packages apart from normal libraries. Packages allow splitting the program up by "merely" grouping units into library-packages without much additional effort. The transparency is not 100%, unit dependencies may shift, and not any division of a codebase into packages might be possible due to unit initialization order requirements. However these cases will be rare, when there has been some thought invested in how the modular structure of the program has been set up. In short, the compiler knows everything from both sides (dll/so +exe), while DLL linking is more a blind link based on a header (which may or may not match) 

This transparency is fundamentally obtained by having only one instance of each unit's state. So only one set of RTL/FCL state, resourcestrings, unit initialization/finalization, exception handling, and VMT tables, in combination with the usual build metadata (timestamps and interface CRCs that are found in PPUs) that allow the compiler to make sure the package and the project that uses them match in version, details etc. This means within the running program, there is only one set of locale settings, registered classes, memory manager etc. Since this applies to every unit, this also means that only one instance of the state of YOUR units is exist. This because the RTL/FCL is not something magical and one could make a replacement. This is important because several higher level language concepts rely on this. (like that _all_ threadvars are initialized for every thread). 

A side Windows specific use might be implementing COM components that fully integrate with the main-program. This because a COM component is usually done by registering a DLL (not a .EXE). A package is also a DLL, but they are still part of the main program. I mention this because the Delphi OO bridge package (Open Office connectivity pack by Clootie) might use this. 

It is important to realize that the key words that set packages apart from other dynamic linking schemes are **automatic** and **safe**. I.O.W. the compiler takes care of nearly everything (except for some details of versioning), and no manual labour and knowledge except for creating which package contains what unit is needed. The same binary can be generated without packages by simply turning "compile with packages" off, without source changes. 

I recently was made aware of the fact that in Lazarus, with its package dependency systems, _maybe_ dynamically loading and unloading design time packages could be used to be able to have multiple versions of the same package in the IDE. Also if the code for unit initialization tables is completely put in the main program, the requirements of unit initialization and order also relax somewhat. I assume Delphi does so too. 

### What problems do packages not solve?

  * Library versioning. There are some rudimentary controls in Delphi, but they are not fine-grained and bulletproof enough to work for a language that has 10000 revisions every year (fpc+lazarus) on 20 platforms. Note though that Delphi doesn't fundamentally solve it either. It only does damage control by having fewer versions
  * FPC/Lazarus has the counter advantage that its core is already redistributable. In theory you could deliver a FPC/Lazarus to develop (library package based) plugins with.
  * it doesn't solve anything if not all parts are compiled with the **same (version** of) fpc
  * initialization of global resources other than memory (and VMTs) are not supported. (TLS? See cthreads init discussion July 2009 on Core)
  * initialization of a exe+packages system is less flexible then from a single binary. Unless loadlibraried, packages always initialize first. (maybe not if the completed unit initialization sequence is generated as part of the mainprogram, and the BPL doesn't contain its own initialization)



Note that there _is_ runtime checkable metadata in Library packages, so this could be expanded to tackle a few of the above. E.g. the FPC version could be checked by putting .ppu version in the meta data etc. 

### Lazarus and binary packages

If we keep the following paragraph in mind, lets make some reasonable assumptions 

  * To be conservative we limit ourselves to two releases annually (e.g. a release and an update to it by Lazarus every year).
  * Rule of thumb, the most recent versions and the versions right before that are the most used versions (because FPC users generally do update, but slowly due to e.g. Linux distributions that still come with older versions). Add another version for Debian/Ubuntu that seem to lag more heavily than the others, and which is too popular to be ignored.
  * Say that of the 20 possible platforms, we support the top tier platforms only, and lets be rigorous and strip FreeBSD and not primary architectures (like Linux on PowerPC), then we keep Windows, Linux and OS X.
  * However Windows and Linux come in two architectures: 32 and 64-bit. Since the slightly "odd" platforms are a major FPC attraction, you can't discount them all. Limit yourself to Linux/32 and Win32 and you might as well use Delphi and Kylix.
  * OS X even in five (i386,x86_64 in the near future,powerpc,powerpc64 and ARM for IPhone)
  * Windows also means WinCE. At least the ARM (pocketPC compatible) version. We marginalize wince versioning for now.



So then we come to 10 platforms in 4 versions: for every release of a binary component, 40 binary componentlibs need to be built. While you can try to clip this here and there a bit (e.g. stop OS X PPC support sometime soon), you occasionally also might have to support some more versions. FPC 2.6.0 is still occasionally seen in business use currently, and 2.6.2, 2.6.4 and 3.0.0 followed it. That's just major releases, not counting Lazarus respins and updates that might not be 100% compatible. 

And then there is of course a lot of "expert" users using 3.1.1 in 2500 revisions/year, but you can assume they know to compile their own. OTOH if it is a binary only trial version, you can't. 

Note that this means that we also already skip the Tier two platform range that might still be interesting. FreeBSD (an occasional server platform) and a lot of embedded Linux (e.g. on ARM). Also we don't include the gaming platforms (NDS, GBA, XBOX), assume only one OS X version per architecture etc. 

So, summarising, I think the number of 40 builds might stand. Think also what this (40 binary builds) means for the support burden. Contrary to the 4-5 the average Delphi component builder does. Some might be crosscompilable, but some (like the 5 OS X targets) will be harder. Moreover crosscompiling or VMs don't really absolve you from testing on the target. 

Yet another problem is the fragmented nature of Linux, which till now we considered whole. Do you really want to build a set of .rpms,.debs,.ipk's for each and every release? OS X and WinCE also have potential target platform versioning issues. 

I hope that I at least made you think twice about the practicalities of binary distribution, and why it isn't as simple as copying the Delphi "binary packages" model. "Dorothy, I think we are not in Kansas any more". 

### Implementation

Some implementation points, the core ones are marked with ***** : 

  * If your system uses packages, 
    * ***** the RTL and other all units the packages use, must also be in a package. In other words, a package can't USES a unit directly in the .EXE
    * ***** An unit can exist only once in one package (see Swart reference below), and probably only once in EXE+packages too .
    * On load of a package this condition (uniqueness of an unit) is checked, but possibly only when packages are dynamically loaded by the program. (the rest being handled by normal dll dependencies) This combined with the previous condition avoids problems like multiple definitions of the VMTs and RTL state in general. Note also that this means a list of units per package must be accessible to the main program on load (?) It is not entire clear what "module" means in library package context. Probably only the library package itself, but maybe a little additional metadata or also e.g. a list of units and their CRCs. 
      * So note that apps that load packages dynamically, must have all relevant modules loaded. The following scenario would be interesting. Package C depends on B and A, and B also depends on A. Now the exe is statically (but shared) linked to pkg A (the RTL dll), and then dynloads first B and then C. Does this work? (this thought experiment is mainly about how dynamic the dependencies between packages are. Some central unit registration might need active updating from compiletime tables for this)
      * Note also that they all use the same RTL, and thus the same memory managers and can use higher level functions (like ansistrings and classes) OVER packages borders. See separate paragraph on memorymanager initialization
    * Since they are DLLs, due to the Windows DLL symbol resolving, packages can probably survive some minor patching (allowing implementations to be fixed, probably the Delphi fixpacks (or however they call them) work this way), but in general it must be pretty much the same packages as the program was compiled with. We probably need tooling (ppudump?) to check this for fixes releases.
    * Aside from RTL state, also other system or process wide wide state (errorhandling, "modal" form handling) might be affected
    * You don't need to manually flag stuff as exported symbol. Everything in the interface of units (hidden or visible) is imported if you import the unit. This puts a strain on the OSes library loader system though.
  * One of the additional features over normal shared libraries are pascal level initializer's ("Initialization") and a module finalizer ("Finalization"). I'm not sure if they are library procedures that traverse compiler generated tables, or if they are wholly compiler generated. 
    * Note that probably means that all units in a package are initialized and finalized at the same time. This means that a program built with packages might force a little bit more order on unit initialization than exactly the same program built without. (because it has the additional requirement that all units in a package are initialised in a sequence). This might have consequences for the compiler.
  * Some form of identification and dependency management. I'm not sure how much these requirements are additional to normal library/DLL versioning, and what is more language dependant. It seems for runtime loading there should be a _runtime_ check on presence of all units the library depends on, maybe also an crc check to verify the right ones.
  * The package .dpk file is like a source file. It has a "requires" clause which is like a USES clause for package dependencies, and a CONTAINS clause for units to be included. (test: what happens with dependant units?)
  * The compiled file is called .bpl in Delphi. This is effectively a (special) DLL. In other words its linking is finalized. The needed metadata (.ppu, inline function and weak packaged units (see next point) go into a .dcp file.
  * Weak packaging means that an unit is logically part of a package, but its code is not in the BPL, but entirely (as .ppu+.o) in the .dcp. This is signalled in the unit source with a {$weakpackageunit} (or similar) directive. One of the main uses seems to be to get rid of optional DLL dependencies in packages. E.g. if a certain package can have a static dependency on a DLL, but it is not always needed, the dependency is extracted to a kind of plugin unit. Only when the main program or another package uses it, it will be linked in, and pull in the said dll. Example: unit cmem in package RTL would typically be weak packaged, as does windows and the other winapi units.
  * Similarly there's {$DENYPACKAGEUNIT} to exclude an unit from a package at all. (?!)
  * Like DLLs, packages can be statically linked (so that its presence is required for startup), and programs can load additional packages using loadpackage (unloadpackage?). Since a package can only depends on existing packages, and not on the exe (RTL also in package remember!), this means that dynloaded packages can be crafted AFTER .exe generation, allowing for plugin systems.
  * packages are loaded by a procedure called "safeloadlibrary" which is a loadlibrary wrapper that saves the FPU statusword and disables some Windows' errorwindows on failure to load. (?!?)
  * relocation, for this a little thought experiment using the following realistic Delphi scenario: Assume packages A and B both import packages RTL, C, D, but are compiled separately and used in one .EXE program. This means that the compiler can't guarantee all BPL's are already on a unique base address and address space (since A and B could be compiled without the other present). -> they are relocatable, though probably DLL loading will resolve this transparently mostly.
  * **Unix** ELF support symbol versioning. Not used much but afaik core language runtimes like libc do use it.



References 

  * [Swart on pkgs](<http://www.inquiry.com/techtips/delphi_pro/10min/10min0301-1.asp>)



## What needs to be done

This roughly splits into three or four parts: (note that afaik Delphi supports all this) 

  1. Be able to create and use packages on all platforms. (statically) 
     1. RTL into a DLL
     2. mainprogram uses RTL, and can access all symbols it could without. (properties, vars, typed constants, functions, RTTI)
     3. Add another package to the mix. (and multiple in general)
  2. Be able to use package dynamically 
     1. Make sure the RTL checks all required packages for a dynloaded package are loaded.
  3. Implement the language/semantic parts. 
     1. language and compiler support for package dependancies (requires etc)
     2. Some way to handle versioning (PPL versioning?)
     3. Delphi's "build with packages" and the package list to link is signaled from outside the source. IOW cmdline parameters or a small file with data in the FPC case.



The language support requires a new concept above mere ppu level, that of collection of ppus, that is crosslinked to ppu level. This because on package level, the dependancies are administrated on package level, and in the source on unit level. (the keywords of a "library" unit explicit hint on this "requires" is for package level dependancies, and the unit list is the unit list the package supports). However instead of "requires" this can also be done using cmdline parameters (toggling "build with packages" and specifying the package list in the Delphi IDE) 

  


### Details

  * Seems Delphi has an extra level of indirection in typeinfo relating to dynamic loading[Barry Kelly on PPtypeinfo vs ptypeinfo](<http://stackoverflow.com/questions/3443097/what-is-the-identity-pointer-before-a-ttypeinfo-there-for>)



## Initialization order and the system unit init problem (plugin units like memmanager)

In normal FPC programs, the system unit is inited first, and then the main program uses clauses is walked. This allows the user to specify units to initialize before any other unit. If the system unit is in the RTL package, the RTL package will init as a whole. If it is not in a package, it makes plugging units into the basic RTL difficult. So probably the RTL package is an exception in the rule that packages init as a whole. Of course the RTL package can't be loadlibraried, since it is always a requirement for program startup. 

So most likely it works as follows: 

  1. the system unit inits first, and must be (kept) hardened so that plugin units (like other memmanagers) can plug in. (it already is)
  2. Then units are inited in normal order as would it be a whole program, but a package initializes fully when the first unit would initialize.



**_OR_ I'm dead wrong about packages initializing as a whole**. Since the compiler has a list of all units (.ppu's in .dcp) it could create init tables as it sees fit. Then also the "package can't depend on unit not in packages in the dependancies (requires)" part might be wrong. It could be that I mixed up references about dynamic and static loading here. (because when dynamic loading, most units will already be initialized, but there is no need for that when statically loading?!?) 

## Lazarus and Library packages

Even though Delphi uses this system for its packages, it doesn't mean Lazarus should also. Borland was mostly fixed on one platform (this is pre-Kylix) when they designed this, and releases less than once per year. 

Since we need packages regardless of what Lazarus does, Lazarus can evaluate at a later time what they use or do. They might e.g. decide against using packages out of versioning reasons (they want to release more frequent than Delphi) or because it can't be implemented in a reasonable way on one of the core platforms Lazarus must support. 

Note that now I see two possible reasons why Borland used packages for the Delphi IDE 

  1. no restart needed to install Designtime packages.
  2. binary only commercial (most "trial") packages,



The first applies to Lazarus also, but could be less practical due to the resulting more complicated versioning (since Lazarus, contrary to Delphi, has daily versions). At best maybe only releases would use it, while the old system is retained for development? 

The second is not applicable to Lazarus anyway out of licensing concerns (distributing binary only components linked against GPL is a GPL violation, dynamically linking is no different from static in the sense of the license). 

## Versioning

A somewhat harder nut to crack is library versioning. Borland only released a dozen delphi's over more than a decade (updates are apparantly kept compatible), which keeps the versioning problem somewhat managable for them. 

We all know the notorious cygwin, but keep in mind that cygwin also only releases an incompatible DLL every six months, less in more recent times. Granted, currently we don't make a semiannual schedule, but we are also not THAT far from a comparable frequency, and thus similar versionitis. 

I don't have any ready made proposals here. It will probably need evaluation after the initial implementation period, how easy it is to maintain compatible versions. However this has the potential to become a stifling compatibility hold on the minor release schedule. 

During a quick brainstorm at a FPC meeting possible problems are: 

  * normal methods and procedures can be added, and the DLL lazy resolving (on name) will be robust against such changes. However inserting VMT methods is dangerous.
  * (FPC <->FPC-in-Lazarus-distro incompabilities (?))



The sheer size (in number of symbols that must remain the same) of the possible interface between a mainprogram on one hand and RTL/FCL/LCL on the other is overwhelming. 

## Compiling a package

That's easy a library unit is compiled, and "build with packages" is on, a library package will be created. If a unit required during compilation is in a package, that package will become a dependancy. 

## Compiling with packages

This is a bit harder. The problem is that in the source we find only unit names, and we must somehow translate this to packages. This means that a table must be constructed with unit to package mappings, and package dependancies. 

Such a table can be constructed in two ways: 

  1. Dynamically -> Scanning for library package header files (*.ppl) and examining them on startup if true.
  2. Statically -> packages must be registered.



I suspect delphi does this dynamically, since runtime packages don't need to be registered. This means that a scan for .ppls (*.dcp in Delphi) and some structures must be added, and the unit search mechanism must be changed to search in these structures first only IF "BUILD WITH PACKAGES" IS ON. 

NOTE: a reader noted that Delphi does register its packages. Next to the "build with packages" option, is a list of packages to be linked. It is not clear if the packages on this list are just searched, but only actively linked if units are actually used (smartlinking style), or the specified packages are ALWAYS a dependancy. 

## Extensions

Delphi | FPC | Description   
---|---|---  
*.dcp  | *.ppl (ppumove)  | Combined .ppu files for this library.   
*.bpl, .so  | ? , .so  | The linkable library file.   
  
Note that combining .ppu's into a ppl means that there won't be duplicate .ppu's (one for static, one for packages), so no need to complicate the dir structure. 

## Debugging

When a package is connected to a project as a dependency, set needed debug (+ other) flags in "Additions and Overrides" page of project options. Then they affect all the dependencies, too. 

The Lazarus IDE build process is different from normal projects, thus for packages installed in the IDE put macro $(IDEBuildOptions) to "Custom Options" of the package. It will then get all the IDE's build flags. 

## PPUMove

ppumove is a standalone binary in the FPC distribution that creates a shared libs from already compiled units. As far as I can see, this is already the beginning of a package system, except that it is manual (create packages manually yourself). This support and the makefile targets mostly dates back to the late 0.99.x/1.0.x series. (already before fpcmake) 

The RTL (on linux only?) makefile has a "shared" target, and Florian says it should be possible to create a shared lib of the rtl by running make clean all CREATESHARED=1 on Linux. Non x86_32 Linux needs the units to be compiled with PIC though, but the makefile seems to already add -Cg this for non x86_32. 

## See also

  * [PPUMove documentation](<http://www.freepascal.org/docs-html/user/userse42.html#x149-1490008.7>) (note link might break when doc is updated. Just search for "ppumove" in the FPC user manual)
  * [PPUMove manual page](<http://www.freepascal.org/tools/ppumove.html>)
  * <http://hallvards.blogspot.com/2006/09/hack13-access-globals-faster.html>

---

_Source: [https://wiki.freepascal.org/Package](https://web.archive.org/web/20250601000000/https://wiki.freepascal.org/Package)_
