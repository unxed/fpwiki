# IDE Window: Restriction Browser

## Navigation

The Restriction Browser can be reached from the Lazarus IDE [Main Menu](<Main_menu.md> "Main menu") > [View](<Main_menu.md> "Main menu") > Restriction Browser menu item. 

Alternatively, the Restriction Browser is also located in the fourth tab of the [Object Inspector](<IDE_Window__Object_Inspector.md> "IDE Window: Object Inspector"). 

## Overview

The Restriction Browser lists the Lazarus [LCL](<LCL.md> "LCL") widgetset and component limitations for various compiler targets - operating systems and graphics. 

  
[![Restriction Browser.png](https://wiki.freepascal.org/images/2/23/Restriction_Browser.png)](</File:Restriction_Browser.png>) [![Object Inspector](https://wiki.freepascal.org/images/4/45/ObjectInspector_4th_Tab.png)](</File:ObjectInspector_4th_Tab.png> "Object Inspector")

## Adding a restriction

For the lcl: see the lcl/interfaces/<widgetset>/issues.xml 

For other packages: Add an xml file, e.g. _issues.xml_ like this: 
    
    
    <?xml version="1.0" encoding="UTF-8"?>
    <package name="lcl">
      <widgetset name="gtk2">
        <issue name="TCheckBox.Alignment">
          <short>CheckBox Alignment property is not supported</short>
          <descr>Use BiDiMode = bdRightToLeft as a workaround</descr>
        </issue>
      </widgetset>
    </package>
    

Then open the package editor of your package, add the file to the package, and change its [file type](<Lazarus_Packages.md> "Lazarus Packages") to "issues xml file", by right clicking the file to open the popup menu, then File type.

---

_Source: [https://wiki.freepascal.org/IDE_Window%3A_Restriction_Browser](https://web.archive.org/web/20250219025244/https://wiki.freepascal.org/IDE_Window%3A_Restriction_Browser)_
