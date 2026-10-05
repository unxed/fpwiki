# RackCtls

│ **English (en)** │

## Contents

  * 1 About
  * 2 Author
  * 3 License
  * 4 Download
  * 5 Change Log
  * 6 Dependencies / System Requirements
  * 7 Installation
  * 8 The _RackCtls_ Example Application



## About

_RackCtls_ is a a collection of components with an "Hi-fi system" appearance: 

[![RackCtls.png](https://wiki.freepascal.org/images/d/de/RackCtls.png)](</File:RackCtls.png>)

  * **TLEDButton** a button with a LED.
  * **TLEDButtonPanel** its matching panel.
  * **TScrewPanel** a panel with screws in its corners.
  * **TLEDDisplay** 7-segment LED display for numerical values.
  * **TLEDMeter** LED bar graph, Vu-meter style.



  
The download contains the component, an installation package and a demo application, that illustrates the features of the component. 

## Author

Original author: [Simon Reinhardt](<http://www.picsoft.de>) Lazarus adaptation by: Luca Olivetti 

## License

From the source file: 

Diese Komponenten sind Public Domain, das Urheberrecht liegt aber beim Autor. 

(translation: 

These components are Public Domain, the copyright however remains with the author. ) 

## Download

The latest stable release can be found [here](<http://ventoso.org/luca/rackctls/>) or on [Lazarus CCR](<http://sourceforge.net/project/showfiles.php?group_id=92177&package_id=278218>). 

The source repository is [here](<https://github.com/olivluca/rackctls>). 

## Change Log

  * Version 1.20.4 TLEDDisplay can show numbers in hex, use FPC resources for the icons
  * Version 1.20.3 Fix memory leak in TLedDisplay, remove ctl3d/parentctl3d properties (removed from lazarus)
  * Version 1.20.2 Fixed default color for TLedButtn/TLedButtonPanel
  * Version 1.20 Initial release



## Dependencies / System Requirements

  * None that I know of



Status: _Stable_ (I hope) 

Issues: 

  * The original TButtonPanel has been renamed to TLedButtonPanel since Lazarus has already a TButtonPanel in its standard components palette
  * The license is unclear to me



## Installation

  * Unzip the file
  * Open the package RackCtlsPkg.lpk in the lazarus ide
  * Click install



## The _RackCtls_ Example Application

The sample application is in the LazDemo subdirectory 

  * Open RackDemo.lpi
  * compile
  * run

---

_Source: [https://wiki.freepascal.org/RackCtls](https://web.archive.org/web/20240308084923/https://wiki.freepascal.org/RackCtls)_
