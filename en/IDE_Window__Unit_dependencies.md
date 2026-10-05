# IDE Window: Unit dependencies

│ **[Deutsch (de)](</IDE_Window:_Unit_dependencies/de> "IDE Window: Unit dependencies/de")** │  **English (en)** │    
****

This window shows the unit dependencies implied by the uses sections. You can access it via _View / Unit Dependencies_. 

You can keep it open and if you installed docking, you can dock this window. The screen shot shows it on Gtk2 with anchordocking installed. 

[![UnitDependencies1.png](https://wiki.freepascal.org/images/0/04/UnitDependencies1.png)](</File:UnitDependencies1.png>)

Note: Before 1.1 this was a dialog that had only a tree with units. 

## Contents

  * 1 Units
    * 1.1 Additional files
    * 1.2 All package units
    * 1.3 All source editor
    * 1.4 All units
    * 1.5 Selected units
  * 2 Project and Packages



# Units

This page shows all scanned units and their interface and implementation uses sections. The uses sections of program and libraries are also called 'interface' on this dialog, which technically not correct, but convenient for this dialog. 

When the window is shown the first time it scans the units. On recent machines this usually takes a few seconds, on slow machines and units on slow network shares it might take a minute. The IDE caches units, so subsequent scans are faster. 

You can use the **Refresh** button at the lower right edge of the window to rescan. 

## Additional files

Enable this option and give a semicolon separated list of directories which .pp and .pas files are scanned too. 

## All package units

Enable this to also scan all packages currently open in the IDE. 

## All source editor

Enable this to also scan all .pp and .pas files in the source editor. 

## All units

This shows all scanned units. Units with _implementation_ uses section have a marker. When you want to refactor a package or you have some fpc problems with cycles lookout for these units. 

  * You can use the **filter** to show only those units containing the filter text.
  * Next to the filter are two toggle buttons. One groups the units in projects/packages, the other groups them via directories. By default both are enabled.
  * You can use the **search** to find a node containing the text. Use the nearby buttons to jump to the next or previous node containing the text.
  * Click on a unit to select it. You can select multiple units by using Ctrl and Shift modified.
  * Double click on a unit to open it in the source editor. On a project will open the project inspector. On a package opens the package editor.
  * Right click for more options.



## Selected units

This shows the units selected in the **All units**. 

Each unit has child nodes **interface _,_**_implementation'_ for the corresponding the _uses_ sections of the unit itself. And **used by interface** and **used by implementation** for _uses_ sections of other units using this unit. That means it shows direct connections between units, not indirect. For example project1.lpr uses UnitA uses UnitB, will list project1 uses UnitA, UnitA is used by project1 and uses UnitB, and finally UnitB is used by UnitA. 

  * You can use the **search** to find a node containing the text. Use the nearby buttons to jump to the next or previous node containing the text.
  * Double click on a unit to open it in the source editor.



# Project and Packages

The upper graph shows the project, packages and FPC directories. Select one or more to show their units in the lower graph. 

The lower graph shows the units of the selected packages. Double click on a unit to open it in the source editor.

---

_Source: [https://wiki.freepascal.org/IDE_Window%3A_Unit_dependencies](https://web.archive.org/web/20230604083239/https://wiki.freepascal.org/IDE_Window%3A_Unit_dependencies)_
