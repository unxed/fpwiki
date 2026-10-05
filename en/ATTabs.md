# ATTabs

│ **English (en)** │  **[русский (ru)](<../ru/ATTabs.md>)** │

## Contents

  * 1 About
  * 2 History
  * 3 Requirements
  * 4 How to use it like TPageControl
  * 5 License
  * 6 Download



## About

_ATTabs_ is a tab-control component for Delphi and Lazarus. It's not replacement for standard tab control, but is alternative one, it has different API. It paints tabs OS-indenendant, using custom drawing on canvas. 

Screenshot: 

[![ATTabs demo.png](https://wiki.freepascal.org/images/b/b3/ATTabs_demo.png)](</File:ATTabs_demo.png>)

Author: Alexey Torgashin 

## History

GitHub repo has the [history.txt file](<https://github.com/Alexey-T/ATFlatControls/blob/master/history.txt>) which lists all major changes. 

## Requirements

  * Lazarus: 1.8+.
  * Tested on: Windows, Linux GTK2/Qt, macOS.



## How to use it like TPageControl

Make some new panel, which will be like TPageControl. Place ATTabs on it, make Align=alTop. Now you create several Forms or Frames onto this panel (they have Parent:=Panel1; Align:=alClient;). How to switch these frames on changing tabs? 

  * Add tabs OnChange event handler.
  * In this event handler: a) hide all frames, b) show frame, which is assigned to current TabIndex.



When you create a new tab, you can set its TabObject property. Set TabObject to a frame. When OnChange is called, get TabIndex, call GetTabData() for this index, and read TabObject for this tab. Show the frame, which is stored in this TabObject, hide all others. 
    
    
      var
        d: TATTabData;
      begin
        d:= t.GetTabData(t.TabIndex);
        if Assigned(d) then
          (d.TabObject as TMyFrame).Show;
      end;
    

## License

License: MPL 2.0. Also you can use it under LGPL. 

## Download

ATTabs is merged into ATFlatControls repo: <https://github.com/Alexey-T/ATFlatControls/> . Some wiki documentation is in the "wiki" subfolder of that repo.

---

_Source: [https://wiki.freepascal.org/ATTabs](https://web.archive.org/web/20250126125546/https://wiki.freepascal.org/ATTabs)_
