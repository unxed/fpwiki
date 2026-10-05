# MDButtonsBar

│ **English (en)** │

## Contents

  * 1 About
  * 2 Screen Shot
  * 3 Author
  * 4 License
  * 5 Download
  * 6 Change Log
  * 7 Dependencies / System Requirements
  * 8 Installation
  * 9 Usage
  * 10 ToDo List



### About

MDButtonsBar (TMultiDocButtonsBar) is a very small component, derived from TPanel, for help you in MDI Applications, using [MultiDoc](<MultiDoc.md> "MultiDoc") component. 

### Screen Shot

[![Mdbuttonsbar.gif](https://wiki.freepascal.org/images/d/d1/Mdbuttonsbar.gif)](</File:Mdbuttonsbar.gif>)

### Author

[Júnior Gonçalves](</User:Junior> "User:Junior")

### License

[LGPL](<http://www.opensource.org/licenses/lgpl-license.php>)

### Download

The component and a demonstration program can be found on the [Lazarus CCR SourceForge site](<http://sourceforge.net/project/showfiles.php?group_id=92177&package_id=176905>) or on [My Geocities Web-Site](<http://geocities.yahoo.com.br/hipernetjr/mdbuttonsbar.zip>). 

### Change Log

  * Version 0.1 2006/03/16 First beta release.



### Dependencies / System Requirements

This component requires the [MultiDoc](<MultiDoc.md> "MultiDoc") Component. 

It is supposed to work on all the Lazarus platforms without change. 

Tested only in Win2k. 

### Installation

  * Compile and install tmdbuttonsbar.lpk
  * Open the example demo/demomultidoc.lpi



This example can be used as a skeleton for a new application (this is an "advanced" example of demo of MultiDoc). 

### Usage

At design time: 

  * On the application main form place a TMultiDoc.
  * Create a child form with a main TPanel.
  * Put all the object you want for the child to the panel, write the event, etc...
  * Do not rely on some TForm event as this form is never show.
  * Add an TMdButtonsBar.
  * Set the HintMinimize, HintRestore, HintMaximize properties.
  * Set the VisibleButtons property.
  * Use events OnCloseClick, OnRestoreClick and OnMinimizeClick to handle the actions in MultiDoc (See Demo)


  * If possible, change MultiDoc package to Register in Palete Page MultiDoc too :-)!



### ToDo List

  * Inactive Buttons;
  * Property to change Style MDIButtons (KDE, WinXP...).

---

_Source: [https://wiki.freepascal.org/MDButtonsBar](https://web.archive.org/web/20250324170704/https://wiki.freepascal.org/MDButtonsBar)_
