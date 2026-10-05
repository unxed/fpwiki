# Lazarus Docs

│ [**Deutsch (de)**](</Lazarus_Docs/de> "Lazarus Docs/de") │  **English (en)** │    
****

## Contents

  * 1 Overview of the Lazarus Documentation
    * 1.1 Free Pascal Project
    * 1.2 Lazarus Project
  * 2 Installation of the help in Lazarus
    * 2.1 Online Help
    * 2.2 Offline help
  * 3 Using the help in Lazarus
    * 3.1 Online help
    * 3.2 Offline help
  * 4 Build the offline help
    * 4.1 Free Pascal Programming Guide
    * 4.2 Free Pascal Language Reference Guide
    * 4.3 Free Pascal User Guide
    * 4.4 Development Tutorial
    * 4.5 Runtime Library
    * 4.6 Free Components Library
    * 4.7 Free Pascal Code Documenter FPDoc
    * 4.8 Lazarus Components Library
  * 5 Used programms and toolchains
    * 5.1 FPDoc
    * 5.2 Make in SVN-fpcdocs



## Overview of the Lazarus Documentation

Basically you should make a difference between the projects ((Free Pascal Project, Lazarus Project) and the used formats of the help (Online, html, chm, inf) 

### Free Pascal Project

**Common**
    Free Pascal programming guide (prog.pdf, prog.chm, ... [[online](<http://www.freepascal.org/docs-html/prog/prog.html>)])
    Free Pascal language referenz guide (ref.pdf, ref.chm, ... [[online](<http://www.freepascal.org/docs-html/ref/ref.html>)])
    Free Pascal user guide (user.pdf, user.chm, ... [[online](<http://www.freepascal.org/docs-html/user/user.html>)])
    Development Tutorial (buildfaq.pdf, [[online](<http://www.stack.nl/~marcov/buildfaq/>)])
**Runtime Library**
    Runtime library (rtl.pdf, rtl.chm, ... [[online](<http://www.freepascal.org/docs-html/rtl/>)])
**Free components reference**
    Reference for package fcl (fcl.pdf, fcl.chm [[online](<http://www.freepascal.org/docs-html/fcl/>)])
**Programs**
    Free Pascal code documenter FPDoc (fpdoc.pdf, fpdoc.chm, ... [[online](<http://lazarus-ccr.sourceforge.net/fpcdoc/fpdoc/fpdoc.html>)])

### Lazarus Project

**Lazarus Komponenten Bibliothek**
    Reference for package lcl (lcl.chm, ...[[online](<http://lazarus-ccr.sourceforge.net/docs/lcl/>)], [Wiki LCL](<Lazarus_Documentation.md> "Lazarus Documentation"))

## Installation of the help in Lazarus

### Online Help

Is not to install. If a connection to the internet is available, you can use it with F1 (or CtrlF1 \- depends on the configuration). If offline help is installed and the topic is available, so the offline will be used first, then the online help. 

### Offline help

Is un der the path ($Lazarus/doc/) 

See also 

  * [Installing_Help_in_the_IDE](<Installing_Help_in_the_IDE.md> "Installing Help in the IDE")



  


## Using the help in Lazarus

### Online help

If you have connection to the internet, you have to press F1 (or CtrlF1 \- depends on the configuration) to show the help. If offline help is installed and the information is available, so is firdt the offline help used and secondary the online. 

### Offline help

If offline help is installed and the information is available, so is firdt the offline help used and secondary the online. 

## Build the offline help

The building of the offline help is complex. Threr are more building path used. 

### Free Pascal Programming Guide
    
    
    (prog.pdf, prog.chm, ... )
    

Available languages
    english
Sources
    in SVN under <http://svn.freepascal.org/svn/fpcdocs>
Sourceformat
    Latex (fpcdocs/prog.tex)
Toolchain
    make based
    Unix based, Windows could work
Formats
    html, pdf, chm
Buildtime
    not defined, if needed ?
In Lazarus installed
    yes ($(lazarusdir)/docs/chm/prog.chm)

### Free Pascal Language Reference Guide
    
    
    (ref.pdf, ref.chm, ... )
    

Available languages
    english
Sources
    in SVN under <http://svn.freepascal.org/svn/fpcdocs>
Sourceformat
    Latex (fpcdocs/ref.tex)
Toolchain
    make based
    Unix based, Windows could work
Formats
    html, pdf, chm
Buildtime
    not defined, if needed ?
In Lazarus installed
    yes ($(lazarusdir)/docs/chm/ref.chm)

### Free Pascal User Guide

(user.pdf, user.chm, ...) 

Available languages
    english
Sources
    in SVN under <http://svn.freepascal.org/svn/fpcdocs>
Sourceformat
    Latex (fpcdocs/user.tex)
Toolchain
    make based
    Unix based, Windows could work
Formate
    html, pdf, chm
Buildtime
    not defined, if needed ?
In Lazarus installed
    yes ($(lazarusdir)/docs/chm/user.chm)

### Development Tutorial
    
    
    (buildfaq.pdf)
    

Available languages
    English
Sources
    in SVN under <http://svn.freepascal.org/svn/fpcdocs>
Sourceformat
    Lyx (Latex) (fpcdocs/buildfaq/buildfaq.lyx)
Toolchain
    make based
    Unix based, Windows could work
Formate
    html, pdf
Buildtime
    not defined, if needed ?
In Lazarus installed
    no

### Runtime Library

(rtl.pdf, rtl.chm, ... ) 

Available languages
    English
Sources
    in SVN under <http://svn.freepascal.org/svn/fpcdocs>
Sourceformat
    Latex (fpcdocs/rtl.tex)
    Diverse Verzeichnis in fpcdocs im FPDoc Format (xml)
Toolchain
    make based
    Unix based, Windows could work
Formate
    html, pdf, chm
Buildtime
    not defined, if needed ?
In Lazarus installed
    yes ($(lazarusdir)/docs/chm/rtl.chm)

### Free Components Library

Reference for package fcl (fcl.pdf, fcl.chm) 

Available languages
    English
Sources
    in SVN under <http://svn.freepascal.org/svn/fpcdocs>
Sourceformat
    Latex (fpcdocs/fcl.tex)
    Diverse Verzeichnis in fpcdocs im FPDoc Format (xml)
Toolchain
    make based
    Unix based, Windows could work
Formate
    html, pdf, chm
Buildtime
    not defined, if needed ?
In Lazarus installed
    yes ($(lazarusdir)/docs/chm/fcl.chm)

### Free Pascal Code Documenter FPDoc
    
    
    (fpdoc.pdf, fpdoc.chm, ...)
    

Available languages
    English
Sources
    in SVN under <http://svn.freepascal.org/svn/fpcdocs>
Sourceformat
    Latex (fpcdocs/fpdoc.tex)
Toolchain
    make based
    Unix based, Windows could work
Formate
    html, pdf, chm
Buildtime
    not defined, if needed ?
In Lazarus installed
    yes ($(lazarusdir)/docs/chm/fpdoc.chm)

### Lazarus Components Library
    
    
     (lcl.chm, ..)
    

Available languages
    English
Sources
    in directory ($(lazarusdir)/docs/xml/lcl)
Sourceformat
    FPDoc format XML
Toolchain
    FPDoc based (xml)
    Programm $(lazarusdir)/docs/html/build_lcl_docs.lpr builds chm or html
Formats
    html, chm
Buildtime
    If you start it by yourself (Attention, built in $(lazarusdir)/docs/html/lcl/lcl.chm -> move ?!)
In Lazarus installed
    yes ($(lazarusdir)/docs/chm/lcl.chm)

## Used programms and toolchains

### FPDoc

    Tutorial : [FPCDocs_Tutorial (englisch)](<FPCDocs_Tutorial.md> "FPCDocs Tutorial")
    CHM Bckend : [chm_backend_for_fpdoc (englisch)](<chm_backend_for_fpdoc.md> "chm backend for fpdoc")
    How to make Lazarus Docs [ How_To_Make_Lazarus_Docs (english)](<How_To_Make_Lazarus_Docs.md> "How To Make Lazarus Docs")

### Make in SVN-fpcdocs

Please read README.DOCS and readmechm.txt.

---

_Source: [https://wiki.freepascal.org/Lazarus_Docs](https://web.archive.org/web/20190824010931/https://wiki.freepascal.org/Lazarus_Docs)_
