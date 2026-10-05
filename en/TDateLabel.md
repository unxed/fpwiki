# TDateLabel

│ **English (en)** │

## Contents

  * 1 About
  * 2 Screenshot
  * 3 Author
  * 4 License
  * 5 Download
  * 6 Change Log
  * 7 Dependencies / System Requirements



### About

_TDateLabel_ is a descendant of TLabel. It overrides the Caption property and makes it read only. It also exposes two new properties: DateTime and Format. 

Its main caracteristics are : 

  * DateTime allows you to assign a TDateTime value to the control.


  * Format allows you to define the format string to be used by the caption.



The purpose/advantage of this control is to allow the developer to display a formated date/time elegantly on a form while the control still retains the value in a true TDateTime data type. This allows the developer to use all of the standard date functions (date math functions) found in the DateUtil unit. 

This control is useful for implementing custom calendar interfaces for day planner projects. 

The download contains the component, an installation package and a demo application, that illustrates the features of the component along with some instrumentation for evaluating the chart on a given system. 

### Screenshot

Here is an exemple of _TDateLabel_. This example shows four TDateLabels, all containing the same DateTime value, presenting the value with different formatting. The border has been turned on to give perspective to the text alignment. 

[![TDateLabel.png](https://wiki.freepascal.org/images/1/1b/TDateLabel.png)](</File:TDateLabel.png>)

### Author

Extreme Programmers, LLC 

### License

[modified](<http://svn.freepascal.org/svn/lazarus/trunk/COPYING.modifiedLGPL>) [LGPL](<http://svn.freepascal.org/svn/lazarus/trunk/COPYING.LGPL>) (same as the FPC RTL and the Lazarus LCL). You can contact the author if the modified LGPL doesn't work with your project licensing. 

### Download

The latest stable release can be found on [SourceForge Lazarus-CCR](<http://sourceforge.net/project/showfiles.php?group_id=92177&package_id=328117>). 

### Change Log

  * Version 1.0 _22 June 2009_



### Dependencies / System Requirements

  * None



Status: _Stable_

Issues: None

---

_Source: [https://wiki.freepascal.org/TDateLabel](https://web.archive.org/web/20240701000000/https://wiki.freepascal.org/TDateLabel)_
