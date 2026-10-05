# MultiDoc

│ **English (en)** │

## Contents

  * 1 About
  * 2 Screen Shot
  * 3 Author
  * 4 License
  * 5 Download
  * 6 Change Log
  * 7 Dependencies / System Requirements
  * 8 Notes
  * 9 Installation
  * 10 Usage
  * 11 Convert a Delphi MDI application



### About

MultiDoc is a Lazarus component to replace the standard MDI interface. 

This component permit to write pseudo-MDI application with Lazarus. It is not a real implementation of the MDI interface but it give to your application the capability to use many resizable sub-form. 

There are two components: 

  * TMultiDoc : the main form docking area.
  * TChildDoc : the child container with resizeable border and button.



I do them with a [graphical software](<images/b/b4/Multidoc_skychart.md> "Multidoc skychart.jpg") in mind but they are probably useful for a range of applications. 

### Screen Shot

[![Multidoc demo.png](https://wiki.freepascal.org/images/b/b6/Multidoc_demo.png)](</File:Multidoc_demo.png>)

### Author

[Patrick Chevalley](</User:Pchev> "User:Pchev")

### License

Modified LGPL, see readme.txt 

### Download

The component in version 0.1 and 0.2 and a demonstration program can be found on the [Lazarus-CCR sourceforge site](<http://sourceforge.net/project/showfiles.php?group_id=92177&package_id=176905>). 

The component in version 0.3 and 0.3.1 can be found [here](<http://br.geocities.com/hipernetjr/lmdi/index.html>). _(link is broken, is someone having the file can upload somewhere?)_ but could be find [here](<http://br.oocities.com/hipernetjr/lmdi/index_ptbr.html>) :-) 

Another download link of MultiDoc & LMDI (working in June 2017): <https://github.com/mehmetulukaya/laz-components>

**Last available version from SVN:**

A number of small fix where added to version 0.2 over the last years. If you use this version that not depend on LMDI you can get the most current version from the Skychart project.  
Type the following command:  

    
    
    svn co http://svn.code.sf.net/p/skychart/code/tags/V36/skychart/component/multidoc multidoc

**New Multiframe component:**

Since 2012 I replace the use of this component in Skychart project by Multiframe.   
It work almost the same but use frames for the child windows, so it no more require the main TPanel trick. 

To get this component:  

    
    
    svn co http://svn.code.sf.net/p/skychart/code/trunk/skychart/component/multiframe multiframe 

### Change Log

  * The Multiframe SVN [changelog](<http://sourceforge.net/p/skychart/code/3050/log/?path=/trunk/skychart/component/multiframe>) can be read on Sourceforge.
  * Since 2012 this component is replaced by [Multiframe](<http://sourceforge.net/p/skychart/code/HEAD/tree/trunk/skychart/component/multiframe/>), using docked frames for the same functionality.
  * The SVN [changelog](<http://sourceforge.net/p/skychart/code/2149/log/?path=/trunk/skychart/component/multidoc>) can be read on Sourceforge.
  * Version 0.3.1 2008/01/01 Fixed the following: 
    1. Moved to public section some properties, to avoid exceptions in design-time
  * Version 0.3 2007/12/31 Fixed the following: 
    1. This version was developed by [Júnior Gonçalves](</User:Junior> "User:Junior") and depends on the [LMDI](<LMDI.md> "LMDI") package
    2. Through of {$IFDEF}'s the component compiles into Lazarus/Delphi unchanged
    3. Great move. Now the mouse not "escape" while the cursor is dragged (Tested in GTK2 (Windows) and Windows XP)
    4. ChildDoc is now derived from TFormPanel (see suite [LMDI](<LMDI.md> "LMDI"))
    5. No more code necessary on each component to focus child form
    6. It is possible to change the "skin" of the title bar, according to the OS (see component TTitleBar)
  * Version 0.2 2007/01/06 Fix the following: 
    1. BorderWidth property not working
    2. Wrong button order
    3. Center the cursor to the title bar when moving the panel
    4. Gtk2 and FPC 2.1.1 compatibility
    5. Crash when closing a child using the close button
    6. Change licensing to modified LGPL
  * Version 0.1 2006/01/20 First beta release.



### Dependencies / System Requirements

This component is exclusively derived from high level standard component (TPanel, TCustomSplitter, TSpeedButton). It must work on all the Lazarus platform without change. 

Tested on Linux and Windows. 

### Notes

You are not limited to show the docking area in the full main form. You can place the TMultiDoc anywhere you want or use multiple one! A combination with a TNotebook may give something similar to a multi-desktop. 

### Installation

  * Compile and install multidocpackage.lpk
  * Open the example demo/demomultidoc.lpi



This example can be used as a skeleton for a new application. 

### Usage

At design time: 

  * On the application main form place a TMultiDoc.
  * Create a child form with a main TPanel.
  * Put all the object you want for the child to the panel, write the event, etc...
  * Do not rely on some TForm event as this form is never show.



At run time: 

  * Create a new child from TMultiDoc.NewChild
  * Create a child form with the new childdoc as owner.
  * Assign the main panel to the Dockedpanel property.



  


### Convert a Delphi MDI application

Converting a MDI application is simplified by the availability of the same component for Delphi and Kylix. 

Convert first your application to MultiDoc in Delphi,then convert to Lazarus. This require some work but among the advantage you avoid the ugly XP border around your child forms. 

Replace the standard MDI function as below: 
    
    
      MDIChildCount   -> MultiDoc1.ChildCount
      ActiveMdiChild  -> MultiDoc1.ActiveObject
                      or MultiDoc1.ActiveChild
      MDIChildren[i]  -> MultiDoc1.Childs[i].DockedObject
                      or MultiDoc1.Childs[i]
    
    

The Delphi component is include in the package. Unzip multidoc-delphi.zip and install MultiDocD6.dpk or MultiDocK3.dpk.

---

_Source: [https://wiki.freepascal.org/MultiDoc](https://web.archive.org/web/20250124213926/https://wiki.freepascal.org/MultiDoc)_
