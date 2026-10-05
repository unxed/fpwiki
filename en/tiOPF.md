# tiOPF

│ **[Deutsch (de)](</tiOPF/de> "tiOPF/de")** │  **English (en)** │  **[italiano (it)](</tiOPF/it> "tiOPF/it")** │  **[日本語 (ja)](</tiOPF/ja> "tiOPF/ja")** │    
****

## Contents

  * 1 About
  * 2 Authors
  * 3 License
  * 4 Support
  * 5 Download
  * 6 Dependencies / System Requirements
  * 7 Installation
    * 7.1 The Packages
    * 7.2 The Setup
  * 8 Usage
  * 9 Links
    * 9.1 Basics about tiOPF and Design Patterns



### About

The [TechInsite Object Persistence Framework](<http://tiopf.sourceforge.net>) (tiOPF) is an Open Source framework of Delphi/Object Pascal code that simplifies the mapping of an object oriented business model into a relational database. The framework is mature and robust. It has been in use on production sites since 1999. It is free, open source, and available for immediate download with full source code. 

Some of the key features of the tiOPF include: 

  * Lets you build an object oriented application that can swap databases with the flick of a switch like a command line parameter or a change of a compiler directive. Currently there are persistence layers for: 
    * Interbase via IBX
    * Oracle via DOA
    * MySQL via [Zeos](<Zeos_tutorial.md> "Zeos tutorial")
    * MySQL via [SqlDB](<SQLdb_Package.md> "SQLdb Package")
    * XML via MSDOM
    * XML via XMLLite
    * Paradox via BDE
    * MS Access via ADO
    * MS SQL Server via ADO
    * MS SQL Server via SqlDB
    * Firebird via FBLib
    * Firebird via SqlDB
    * Firebird via Zeos
    * PostgreSQL via SqlDB
    * HTTP Remote Persistence (for n-tier applications with built-in generic application server)
    * Text files (CSV and TAB files)
  * Family of abstract base classes for building a complex object model
  * 27 Persistent Object-aware components for building complex GUIs (Delphi only).
  * Model-GUI-Mediators implementation for enabling any standard GUI component to become Object-aware. MGM currently has mediators defined for: VCL, LCL and [fpGUI Toolkit](<fpGUI.md> "fpGUI").
  * 1600+ DUnit2/[FPTest](<FPTest.md> "FPTest") tests to guarantee stability
  * 160+ pages of documentation to get you started
  * News groups for support
  * Automated, daily builds and unit testing. This is done under Linux and Windows and uses FPC & Delphi compilers.
  * Lots of demos focusing on specific parts of the framework for easy learning.
  * Cross platform. Currently tested on Windows, Linux and FreeBSD (32 & 64-bit).



### Authors

Peter Hinrichsen - Original Developer.  
[Graeme Geldenhuys](</User:Ggeldenhuys> "User:Ggeldenhuys") \- Ported to Free Pascal and current maintainer. 

### License

tiOPF uses a dual license. Developers can use the [Mozilla Public License 1.1](<http://opensource.org/licenses/mozilla1.1.php>) or the Modified LGPL license (as used by libraries of FPC and Lazarus). 

### Support

The best way to get support is by signing up to tiopf.support news group — see here: <http://tiopf.sourceforge.net/Support.shtml>

### Download

For some years now, the tiOPF project does not make official release downloads. The tiOPF projects works on a similar principal to a "rolling release". Thus if you want the latest version with the latest features and fixes, you must get the source code from the Git code repository. 

You can use the following commands to check out the source: 
    
    
    git clone git://tiopf.git.sourceforge.net/gitroot/tiopf/tiopf
    

You will now have a 'tiopf' directory containing the tiOPF repository. By default Git will also have checked out the 'master' branch for you. The tiOPF project doesn't use the 'master' branch for development, so switch to the 'tiopf2' branch as follows: 
    
    
     git branch tiopf2 origin/tiopf2                (1)
     git checkout tiopf2                            (2)
    

  1. Creates a local branch named 'tiopf2', which points to the remote tiopf2 branch.
  2. Switch to your local 'tiopf2' branch.



Another simpler way of doing this is: 
    
    
    git clone --branch=tiopf2 git://tiopf.git.sourceforge.net/gitroot/tiopf/tiopf
    

You will now have a 'tiopf' directory containing the tiOPF repository. 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** When working with Free Pascal and tiOPF, the only supported compiler is the latest released FPC (and the related fixes branch), and the 'tiopf2' branch of tiOPF.

  
For a short introduction to using Git, you can refer to a message posted in the tiopf.development newsgroup. [tiopf.development;article=3272](<http://geldenhuys.co.uk/webnews/webnews.cgi?user=anonymous;group=tiopf.development;article=3272>). For very good and detailed documentation on Git, we highly recommend you browse through the official Git documentation as well, located here: <http://git-scm.com/documentation>

### Dependencies / System Requirements

  * Compiler: FPC 2.6.4. The latest released FPC version.
  * Components for your required persistence layer, if it is not included with the compiler.



**Status** : Stable (tested on Windows, Linux and FreeBSD) 

**Issues** : None 

### Installation

#### The Packages

Inside the <tiopf>\Compilers\FPC directory there are four packages. 

tiOPF.lpk
    Core units (run-time only package)
tiOPFGUI.lpk
    GUI related units and tiOPF+LCL custom components [**deprecated**] (run-time only package).
tiOPFGUIDsgn.lpk
    Registers/Installs the tiOPF+LCL custom components into the Lazarus component palette (design-time only package). The tiOPF+LCL custom GUI components used under Lazarus are unmaintained and **deprecated**. It is preferred to use the Model-GUI-Mediator components instead. See tiOPFLCL.lpk package instead.
tiOPFLCL.lpk
    GUI related units which replaces tiOPFGUI.lpk and does not contain any of the tiOPF custom GUI components. This packages uses the Mediators which makes standard LCL components "object-aware" and is the preferred way of hooking up your UI to you business objects. (run-time only package)
tiOPFHelpIntegration.lpk
    Integrates the fpdoc generated help files into Lazarus's help system (design-time only package)

#### The Setup

  * Open Lazarus IDE
  * Open the package _tiOPF.lpk_ with 'Package -> Open Package File (.lpk)' located in the <tiopf>\Compilers\FPC\ directory.
  * Click on Compile
  * Open the _tiOPFLCL.lpk_ package and click Compile



Optional 

  * Open the _tiOPFHelpIntegration.lpk_ package and click Compile and the Install (Lazarus should rebuild and restart).



_NOTE #1_  
The SqlDB+Firebird database components are set as the default persistence layer for Free Pascal in the tiOPF.lpk package. This was simply done because SqlDB in included with the Free Pascal Compiler, and Firebird is a popular database option. If you don't need this persistence layer you can simply disable it as described below. 

Persistence layers are controlled by a Compiler Directive under _Compiler Options_ -> _Other_ -> _Custom Options_. eg: The LINK_FBL directive relates to the FBLib components. The LINK_SQLDB_IB directive relates to the SqlDB (Interbase/Firebird) components. For all the available options see the end of the _tiOPFManager.pas_ unit. 

_NOTE #2_  
For the Integrated Help to work, Lazarus needs to know how to find the html help files. Please read the _tiOPFHelpIntegration.txt_ file located in <tiopf>\Compilers\FPC\ for further instructions. 

### Usage

In Lazarus, open your project and add tiOPF as a Required Package _(Project - > Project Inspector -> Add)_. Include _tiObject_ in your uses clause. You are now ready to create objects descending from TtiObject or TtiObjectList. 

See the example projects in the Demos directory for additional examples. 

### Links

tiOPF home page: <http://tiopf.sourceforge.net/>

#### Basics about tiOPF and Design Patterns

For people who are searching basics about tiOPF or Design Patterns, there are a couple of published articles at (<http://geldenhuys.co.uk/articles/>) 

Written by Graeme Geldenhuys 

2008-08
    Simple Factory Pattern (download pdf) [150KB]
2008-09
    Model-GUI-Mediator (download pdf - 251KB) & (source code - 9KB)
2008-11
    Iterator Pattern (download pdf - 147KB) & (source code - 4KB)
2009-01
    The Adapter Pattern (download pdf) [237KB]
2009-02
    Intro to Git - source code management (download pdf) [257KB]
2009-03
    The State Pattern (download pdf) [217KB]
2009-07
    Relationship Manager (download pdf) [375KB]
2009-09
    Hierarchies in SQL - Nested Sets (download pdf) [163KB]
2011-12
    The Facade Design Pattern (download pdf) [297KB]

---

_Source: [https://wiki.freepascal.org/tiOPF](https://web.archive.org/web/20250529010244/https://wiki.freepascal.org/tiOPF)_
