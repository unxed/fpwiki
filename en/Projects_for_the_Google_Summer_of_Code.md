# Projects for the Google Summer of Code

Main page: [Contests](<Contests.md> "Contests")

This page should list ideas of projects to be implemented in the Google Summer of Code, provided that we get funding. Tasks can be related to Free Pascal, Lazarus and the libraries deployed with them. 

## Contents

  * 1 Implement support for the extended RTTI
  * 2 Implement anonymous methods
  * 3 Tasks for Lazarus
    * 3.1 FPVectorial - Implement a CorelDraw importer
    * 3.2 LCL-CustomDrawn - Implement a custom drawn TCDListView, TCDGrid and other complex controls
    * 3.3 LazFreeType - Implement large new parts of the Pascal text rendering library
    * 3.4 LHelp and TurboPower iPro HTML component improvements
  * 4 Tasks for the Free Pascal Application Suite
    * 4.1 Implement the dynamic mode for X11, Windows 7 or Mac of the Virtual Magnifying Glass
    * 4.2 Finishing the basic parts of the Free Pascal Window Manager
  * 5 See Also



### Implement support for the extended RTTI

Delphi 2010 introduced an enhanced RTTI system that adds Attributes (similar to Java Annotations), an type-value framework (TValue) and ability to easily call methods with parameters (through TRttiMethod.Invoke). 

The task would consist of implementing at least the Attributes and TValue support 

More info on the RTTI features: 

<http://www.malcolmgroves.com/blog/?p=476>

<http://www.malcolmgroves.com/blog/?p=530>

<http://robstechcorner.blogspot.com/2009/09/so-what-is-rtti-rtti-is-acronym-for-run.html>

Mentor: ? 

### Implement anonymous methods

Anonymous methods (aka closures) feature was introduced in Delphi 2009 

<http://docs.embarcadero.com/products/rad_studio/delphiAndcpp2009/HelpUpdate2/EN/html/devcommon/anonymousmethods_xml.html>

Mentor: ? 

## Tasks for Lazarus

### FPVectorial - Implement a CorelDraw importer

A importer capable of reading the CorelDraw format would be highly desirable for many users of the FPVectorial Vectorial Graphics reading/writing and manipulating library. A initial implementation is already available in our subversion, but it hasn't even got past decompressing the file contents. A large database of CDR files can be found in the the web, which can be used as a test suite to test the reading of the files. There is no documentation about the CDR file format, but there are some free software projects written in other languages which can read it. They are, however, unsuitable for being used in Free Pascal application because external dependencies are highly undesirable, so we wish for a 100% Object Pascal solution. The applicant should read and understand how another project reads the CorelDraw files and then implement our own reader module in FPVectorial. The target version would initially be CorelDraw X13, but if the task proves easier then expected it might be expanded for other versions as well. 

More information here: [fpvectorial#CorelDraw](<fpvectorial.md> "fpvectorial")

Mentor: Felipe Monteiro de Carvalho 

### LCL-CustomDrawn - Implement a custom drawn TCDListView, TCDGrid and other complex controls

The Lazarus Custom Drawn Controls contains all basic controls already, but the more complex ones are still missing and they are very important. 

More information here: [Custom Drawn Interface](<Custom_Drawn_Interface.md> "Custom Drawn Interface")

Mentor: Felipe Monteiro de Carvalho 

### LazFreeType - Implement large new parts of the Pascal text rendering library

The original FreeType was written in Pascal and we have resuscitated it. In this bounty the student should implement large new parts not only for glyph drawing of truetype fonts but also true text layouting including arabic support, chinese support, etc. It should be able to draw texts, calculate the partial widths of a text, calculate the metrics of texts (width, height, ascent, etc) and in general be able to execute all tasks which are required by the Lazarus Component Library for text drawing. This task should also include integrating it into the LCL-CustomDrawn widgetset to enable text support on it in all platforms. 

More information here: [Custom Drawn Interface](<Custom_Drawn_Interface.md> "Custom Drawn Interface") and [LazFreeType](<LazFreeType.md> "LazFreeType")

Mentor: Circular 

### LHelp and TurboPower iPro HTML component improvements

We need people working in our LHelp CHM help viewer application and also in HTML rendering engine. The main bug of LHelp is slow startup when used with the LCL help CHM. iPro in general needs a lot of improvements for both CHM help viewing and general usage and those can be very easily tested with FPBrowser our web browser and LHelp. 

Mentor: Andrew Haines? 

## Tasks for the Free Pascal Application Suite

### Implement the dynamic mode for X11, Windows 7 or Mac of the Virtual Magnifying Glass

The Virtual Magnifying Glass is a The magnifier already has a dynamic mode for Windows 2000 throught Vista, but a dynamic mode would be very benefitial in all missing systems. 

See: <http://sourceforge.net/projects/magnifier/>

Mentor: Felipe Monteiro de Carvalho 

### Finishing the basic parts of the Free Pascal Window Manager

The Free Pascal Window Manager already has its basic parts working, but it needs to have implemented a taskbar with system tray icon and a main button with a configurable menu and desktop icons. Also the Window decoration should be changed from the wm2 style to a more normal style close to what KDE has. 

See: <http://sourceforge.net/projects/fpwm/>

Mentor: Felipe Monteiro de Carvalho 

## See Also

[Google Summer of Code Application Template](<Google_Summer_of_Code_Application_Template.md> "Google Summer of Code Application Template")

---

_Source: [https://wiki.freepascal.org/Projects_for_the_Google_Summer_of_Code](https://web.archive.org/web/20250114062527/https://wiki.freepascal.org/Projects_for_the_Google_Summer_of_Code)_
