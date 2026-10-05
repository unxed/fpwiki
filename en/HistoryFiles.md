# HistoryFiles

│ **English (en)** │  **[français (fr)](</HistoryFiles/fr> "HistoryFiles/fr")** │    
****

## Contents

  * 1 About
  * 2 Author
  * 3 License
  * 4 Download
  * 5 Change Log
  * 6 Dependencies / System Requirements
  * 7 Notes
  * 8 Installation
  * 9 Usage
  * 10 The Demo Example Application
  * 11 See also



### About

This component stores the recent files list into an .ini file and shows the result into a menu. It's possible to insert the list into any point of your menu. A method helps you to have access to all the properties and methods of the Menu Items, so for example you can specify the image of each item. 

The download contains the component and two demo projects: one for Lazarus and one for Delphi. 

### Author

[Andrea Russo](</index.php?title=User:Aru&action=edit&redlink=1> "User:Aru \(page does not exist\)")

### License

[LGPL](<http://www.opensource.org/licenses/lgpl-license.php>)

### Download

The latest stable release can be found on the [Lazarus CCR Files page](<http://sourceforge.net/project/showfiles.php?group_id=92177&package_id=208312>). 

### Change Log

  * Version 1.3.1 2019/12/16 Fix for Freepascal > 3.0; tested on Lazarus 2.0.6
  * Version 1.3.0 2014/07/09 Added LastItemIndex, GetItemValue, DeleteItem, ItemBitmap, ItemSelectedBitmap, CheckLastItem, ClearLastItem. New demos for Delphi and Lazarus. Added unicode support for Lazarus. Tested on Lazarus 1.2.4
  * Version 1.2.1 2009/05/22 new demo compatible with latest Lazarus version (tested on Lazarus 0.9.26.2), new demo for Kylix 3 Open Edition;
  * Version 1.2 2006/10/12



### Dependencies / System Requirements

  * None



### Notes

Status: Stable 

Issues: Tested on: Lazarus 2.0.6 (on Windows 10 and Debian); Delphi 2,3,4,6,7,D2005,D2009, D10.2 all personal or standard editions; Kylix 3 Open Edition 

### Installation

Open the package HistoryLazarus.lpk, compile and install it. 

### Usage

Properties 

* * *

  * FileMustExist  
If true the files that don't exists will be deleted from the history list  
(default False)


  * IniFile  
This is the name of the IniFile with full path  
(default History.ini)


  * IniKey  
This is the name of the key into the ini file


  * LocalPath  
If specified all the files that have the path equal to LocalPath are shows into the list without the path.  
If ShowFullPath is false this value it's indifferent.


  * MaxItems  
This is the max lenght of the recent list showed and stored into the ini file (it's a lifo list).  
(default 5)


  * ParentMenu  
This is the TMenuItem parent of the history list.


  * Position  
This is the insert position of the list into the parent menu.  
The possible position is between 0 and the value of ParentMenu.Count


  * Separator  
To divide the list by the other menu items if do you want.   
Possible values: sepNone, sepTop, sepBottom, sepBoth  
(default sepNone)


  * ShowFullPath  
If true the file names are showed with full path otherwise not.  
(see also the LocalPath property)  
(default True)


  * ShowNumber  
If true an item number is showed before the file name.   
(default True)


  * Sorted  
If true the list is sorted.  
(default False)


  * Count  
Indicates the number of items of the history list.  
(read only)


  * CheckLastItem  
If true the last item used it's automatically checked  
(with a normal check or with an image if the ItemSelectedBitmap it's assigned)


  * ItemBitmap  
If it is assigned is the image used for the menù items


  * ItemSelectedBitmap  
If it is assigned is the image used for the menù item selected (if CheckLastItem=true)


  * LastItemIndex  
It's the relative index (0..Count-1) of the last item used (selected or added)



Methods 

* * *

  * UpdateList(thefile: string)  
Insert the file into the list and rebuild the history menu.


  * UpdateParentMenu  
Rebuild the menu  
(called also by UpdateList).


  * GetItemValue(const Index : integer): string  
The index is the relative index into the history list (0..Count-1).  
Return the value (the full filename) assigned at the item.


  * DeleteItem(const Index : integer)  
The index is the relative index into the history list (0..Count-1).  
Delete the item from the menù.


  * ClearLastItem  
Clear the value of LastItemIndex



Events 

* * *

  * OnClickHistoryItem  
procedure (Sender: TObject; Item: TMenuItem; const Filename: string)  
When one history item is clicked this event returns the related file name.  
By this event do you personalize the item menu clicked (properties like image or checked; or assign events like OnDrawItem,etc.).


  * OnCreateItem  
procedure (Sender: TObject; Item: TMenuItem; const Filename: string)  
When a history item is inserted into the parent menu this event returns this item and the filename.  
By this event you can personalize each item menu (properties like image or checked; or assign events like OnDrawItem,etc.).



[![Warning-icon.png](https://wiki.freepascal.org/images/b/b2/Warning-icon.png)](</File:Warning-icon.png>)

**Warning:** Don't modify the tag property of the TMenuItem created by this component, this property is used in the source code.

[![Warning-icon.png](https://wiki.freepascal.org/images/b/b2/Warning-icon.png)](</File:Warning-icon.png>)

**Warning:** Don't re-assign the OnClick events that's used by the component to return the filename (OnClickHistoryItem);

### The Demo Example Application

The zip file contains a demo project for Lazarus into the DemoLazarus folder. 

### See also

  * [Using INI Files](<Using_INI_Files.md> "Using INI Files")

---

_Source: [https://wiki.freepascal.org/HistoryFiles](https://web.archive.org/web/20241202180627/https://wiki.freepascal.org/HistoryFiles)_
