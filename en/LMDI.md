# LMDI

│ **English (en)** │

## Contents

  * 1 About
  * 2 Screen Shot
  * 3 Author
  * 4 License
  * 5 Download
  * 6 Change Log
  * 7 Dependencies / System Requirements
  * 8 WidgetSets
  * 9 Installation
  * 10 Usage
  * 11 Examples



### About

The LMDI Suite ("Lazarus MDI" Interface Simulation) is composed of components to make a simulation of MDI application. It is written entirely based on components already in the VCL/LCL (TPanel, TImage, etc). The LMDI Suite contains the following components: 

**TButtonsBar** = A bar of buttons to minimize, restore and close the child windows (also can be used for other purposes). 

**TFormPanel** = A kind of "child-windows", which will be used as a skeleton for the component TChildDoc (see [MultiDoc](<MultiDoc.md> "MultiDoc")). 

**TTitleBar** = A bar of title, descendant of TButtonsBar, which will be used in child-windows and will drag these windows in the container (TMultiDoc). 

### Screen Shot

I'm writing a program to edit html/cpp/pascal/txt files (Source Page Editor) A screenshot of it is [![The source is not available yet, but it will be GPL](https://wiki.freepascal.org/images/1/18/Screenshot_SPE.jpg)](</File:Screenshot_SPE.jpg> "The source is not available yet, but it will be GPL")

### Author

LMDI was created by [Júnior Gonçalves](</User:Junior> "User:Junior")

[MultiDoc](<MultiDoc.md> "MultiDoc") was created by [Patrick Chevalley](</User:Pchev> "User:Pchev")

### License

Modified LGPL (same [MultiDoc](<MultiDoc.md> "MultiDoc") and [MDButtonsBar](<MDButtonsBar.md> "MDButtonsBar")), see docs\readme.txt 

### Download

The component and a demonstration program can be found in my [Website](<https://web.archive.org/web/20091019024107/http://br.geocities.com/hipernetjr/lmdi/index_en.html>) (Internet Archive). 

MyDBF Studio Sourcecode contains LMDI [Studio Download Page](<http://mydbfstudio.altervista.org/down.html%7CMyDBF>)

Another download link for LMDI (+ MultiDoc) (working in June 2017): <https://github.com/mehmetulukaya/laz-components>

### Change Log

  * Version 0.1 2007/12/31 First Beta Release.



### Dependencies / System Requirements

This component is exclusively derived from high level standard component (TPanel, TImage, etc). 

It must work on all the Lazarus platform without change. 

It was tested on Windows (2k and XP), but not tested in any Linux distro. 

### WidgetSets

  * Win32: OK. It's work nice;
  * GTK2 (Win32): OK. It's work nice! (With some tests, I did found a small problem in title bar height of first child);
  * QT (Win32): OK. It's work nice!



### Installation

  * Compile and install LMDI.lpk file.
  * Open the example demo/mdbb-runtime/mdbb.lpi



This example show some properties of TitleBar/ButtonsBar component. 

### Usage

ToDo 

### Examples

Soon (see demos directory too)

---

_Source: [https://wiki.freepascal.org/LMDI](https://web.archive.org/web/20250418092422/https://wiki.freepascal.org/LMDI)_
