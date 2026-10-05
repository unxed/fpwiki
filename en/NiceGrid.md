# NiceGrid

The following text is based on the _readme.html_ file which is included in the NiceGrid installation directory. It is written by the original author of the component, [Priyatna](<priyatna.md> "priyatna.org"). The text was also published on the [author's web site](<http://www.priyatna.org/>) which, however, is not available any more. 

## Contents

  * 1 Introduction
  * 2 License
  * 3 Version History
    * 3.1 Version 3.0
    * 3.2 Version 2.2
    * 3.3 Version 2.1
    * 3.4 Version 2.0
  * 4 Download and installation
  * 5 Working with Headers
  * 6 Contacts



## Introduction

[![nicegrid.png](https://wiki.freepascal.org/images/6/6b/nicegrid.png)](</File:nicegrid.png>)

**NiceGrid** is a Delphi and Lazarus component that is aimed to be a standard string grid replacement. It is written from scratch, not descending from `TStringGrid`. The main reason why I wrote this component is to have a grid component that is nice and smooth. Here's some features of NiceGrid: 

  * Headers can be merged and or multilined.
  * Smooth scrolling, not aligned to top left cell coordinate.
  * All aspect of grid colors can be customized: header light color, header dark color, header color, grid color, text color, etc.; resulting a real nice looking grid.
  * Alternate row color.
  * Can be customized at design time.
  * Each column can have its own horizontal and vertical alignment, color, and font.
  * Each column can be hidden.
  * Can be auto fit to width.
  * Can be auto calculate column width.
  * `BeginUpdate` and `EndUpdate` methods for bulk cells access.



Since it is a new component, there are several main differences between `TNiceGrid` and `TStringGrid`: 

  * Headers are excluded from cells, unlike `TStringGrid` that treats fixed rows as regular cells (Row 0, for example), `Cells[0,0]` in NiceGrid will access the top left editable cells, not fixed cell.
  * The only way to access the data is using `Cells` property or using direct array referencing style: `NiceGrid1[0,0]`. There are no `Cols` or `Rows` properties.
  * Replacements: `FixedRows` -> `Header`, `FixedCols` -> `Gutter`.



## License

This library is released under **Mozilla Public License**. You can use it in your freeware, shareware or commercial softwares. You can send your modification to me, and if I decide to include it in the main distribution, I will add your name as a contributor. You can read full licensing information in the file "License.txt" in the NiceGrid installation folder. 

## Version History

### Version 3.0

  * First version on CCR after the original web site is no longer available. The component now can be used also in Lazarus.



### Version 2.2

  * Bug fixed: `OnCellChange` event and friends only fired by user input, not by code (It seems that this behavior conforms with VCL design. I just get the point. ;-)
  * Bug fixed: messing up with accelerator keys. NiceGrid can't capture chars that already defined as accelerators of other control. This is fixed now.
  * C++ Builder port by C. S. Phua <csphua@teledynamics.com.my>.



### Version 2.1

  * Problems in clipboard operations with some hidden columns, fixed.
  * `OnAddRow` event added, makes possible to set default values to new added row.
  * `DeleteRow` method added.
  * `CanResize` property in Column, this will control column width when `FitToWidth` property is set.
  * Some drawing enhachements.



### Version 2.0

  * Bitmap buffering is removed. Believe me, this is not as easy as I thought.
  * Lots of enhanchement in editing capabilities and clipboard operations, most of them are adapted from well-known Microsoft Excel™.
  * `Header`'s `Title` is more flexible now. It doesn't have to be complete. Just remember two basic rules: ';' for multiline and '|' for each header cell. Header and gutter have their own font.
  * Fill Down and Fill Right feature. This feature is actually created for my own purpose, but I think it's not a bad idea. To fill down a column, try to type something and then end with '*' and press ENTER. To fill right a row, use '>'.
  * Each column can be hidden. But use this feature wisely, because paste operation (from clipboard) will be applied also to hidden columns. It may make confuse end-user. I'll try to patch this later.
  * Cell editing can be done at design time via `Column.Strings` property.
  * Filter, Validation and Notification features: `OnFilterChar`, `OnValidate`, `OnCellChanging`, `OnCellChange` events; also `EnableValidation`/`DisableValidation` methods.



## Download and installation

The easiest way for Lazarus users is to get the package from the [Online Package Manager](<Online_Package_Manager.md> "Online Package Manager") (OPM) which is installed with newer Lazarus versions. Simply open its menu item, check "NiceGrid" in the tree list, and click "Install". Then OPM will take care of the download and the installation. 

Since the OPM version may not always be up-to-date, you can also download the development version from the "Lazarus Component and Code Repository" (CCR) where a zipped snapshot is provided (<https://sourceforge.net/p/lazarus-ccr/svn/HEAD/tree/components/nicegrid/>). Alternatively, if you use SVN you can use the following SVN command in a console window: 

` svn checkout <svn://svn.code.sf.net/p/lazarus-ccr/svn/components/nicegrid> nicegrid `

This applies also for Delphi users. 

In **Lazarus** , open the _NiceGridLaz.lpk_ file in the package editor ("Package" > "Open Package File (.lpk)"), click "Use" > "Install", and let the IDE recompile itself. 

In **Delphi** , open _NiceGridD7.dpk_ in the IDE and press the _Install_ button. Another package is provided in the download for newer Delphi versions, named _NiceGridXE11_ ; although tested under Delphi XE11 (Alexandria) it should work in other Delphi versions as well. 

The component will appear on the **priyatna.org** palette tab. 

## Working with Headers

All features explained below are available at design time, but I will cover it using codes, for easy following. 

NiceGrid will automatically scan the column title and make appropriate merging and multi-lining. The only thing you must do is setting each column's `Title` property. This is a string type property. To make a multi-lined caption, use ';' (semicolon) character. 

For example: 
    
    
    NiceGrid1.Columns[0].Title := 'First Line;Second Line';
    

will make 

First Line   
Second Line   
---  
  
The `HeaderLine` property determines how many lines will be allocated for headers. Each line in headers can be set also via column's `Title` property, separated by '|' character. 

For example: 
    
    
    NiceGrid1.HeaderLine := 2;
    NiceGrid1.Columns[0].Title := 'First Line|Second Line';
    

will make 

First Line   
---  
Second Line   
  
To merge two header cells, set each cell to exactly the same value, including ';' characters if they are multi-lined. 

For example: 
    
    
    NiceGrid1.HeaderLine := 2;
    NiceGrid1.ColCount := 2;
    NiceGrid1.Columns[0].Title := 'One|Two';
    NiceGrid1.Columns[1].Title := 'One|Three';
    

will make 

One   
---  
Two | Three   
  
Using a combination of '|' and ';' characters, we can make a complex header. 

For example: 
    
    
    NiceGrid1.HeaderLine := 2;
    NiceGrid1.ColCount := 5;
    NiceGrid1.Columns[0].Title := 'Merged;Multilined|Merged;Multilined';
    NiceGrid1.Columns[1].Title := 'First Group|One';
    NiceGrid1.Columns[2].Title := 'First Group|Two';
    NiceGrid1.Columns[3].Title := 'Second Group|Three';
    NiceGrid1.Columns[4].Title := 'Second Group|Four';
    

will make 

Merged  
Multilined | First Group | Second Group   
---|---|---  
One | Two | Three | Four   
  
## Contacts

Don't hesitate to report any bug or whish to me: 

Priyatna Bandung - Indonesia 

<http://www.priyatna.org/>

[mailto:me@priyatna.org](<mailto:me@priyatna.org>)

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** The http address is no longer available. You may report bugs also in the Lazarus bug tracker for the CCR project

---

_Source: [https://wiki.freepascal.org/NiceGrid](https://web.archive.org/web/20240101000000/https://wiki.freepascal.org/NiceGrid)_
