# Windows CE Development Notes

[![WinCE Logo.png](https://wiki.freepascal.org/images/8/86/WinCE_Logo.png)](</File:WinCE_Logo.png>)

This article applies to [WinCE](</Category:WinCE> "Category:WinCE") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

│ **English (en)** │  **[中文（臺灣） (zh_TW)](</Windows_CE_Development_Notes/zh_TW> "Windows CE Development Notes/zh TW")** │    
****

## Contents

  * 1 Other Interfaces
    * 1.1 Platform specific Tips
    * 1.2 Interface Development Articles
  * 2 Debugging your applications
  * 3 Alignment problems
    * 3.1 What is misaligned data access?
    * 3.2 How to solve this?
  * 4 Implementation details
    * 4.1 ApplicationType
    * 4.2 Positioning and size of Dialogs and Forms
    * 4.3 The Title "OK" and "X" buttons
    * 4.4 TWinControl.PopupMenu showing
    * 4.5 TRadioButton and TGroupBox
    * 4.6 TCheckListBox
    * 4.7 Tab Controls (TPageControl)
    * 4.8 DataAware controls
    * 4.9 Brushes
    * 4.10 SetProp - GetProp - RemoveProp APIs
    * 4.11 Modal Dialogs and the OK and X buttons
    * 4.12 Overlapping menu bottom
    * 4.13 Standard dialogs
    * 4.14 Running in desktop Windows
    * 4.15 Smartphone menus
    * 4.16 Regressions
    * 4.17 Networking crash bug
    * 4.18 ShowMessage regression
  * 5 List of devices with information about them
  * 6 Links



## Other Interfaces

  * [Lazarus known issues (things that will never be fixed)](<Lazarus_known_issues_\(things_that_will_never_be_fixed\).md> "Lazarus known issues \(things that will never be fixed\)") \- A list of interface compatibility issues
  * [Win32/64 Interface](<Win32/64_Interface.md> "Win32/64 Interface") \- The Windows API (formerly Win32 API) interface for Windows 95/98/Me/2000/XP/Vista/10, but not CE
  * [Windows CE Interface](<Windows_CE_Interface.md> "Windows CE Interface") \- For Pocket PC and Smartphones
  * [Carbon Interface](<Carbon_Interface.md> "Carbon Interface") \- The Carbon 32 bit interface for macOS (deprecated; removed from macOS 10.15)
  * [Cocoa Interface](<Cocoa_Interface.md> "Cocoa Interface") \- The Cocoa 64 bit interface for macOS
  * [Qt Interface](<Qt_Interface.md> "Qt Interface") \- The Qt4 interface for Unixes, macOS, Windows, and Linux-based PDAs
  * [Qt5 Interface](<Qt5_Interface.md> "Qt5 Interface") \- The Qt5 interface for Unixes, macOS, Windows, and Linux-based PDAs
  * [GTK1 Interface](<GTK1_Interface.md> "GTK1 Interface") \- The gtk1 interface for Unixes, macOS (X11), Windows
  * [GTK2 Interface](<GTK2_Interface.md> "GTK2 Interface") \- The gtk2 interface for Unixes, macOS (X11), Windows
  * [GTK3 Interface](<GTK3_Interface.md> "GTK3 Interface") \- The gtk3 interface for Unixes, macOS (X11), Windows
  * [fpGUI Interface](<fpGUI_Interface.md> "fpGUI Interface") \- Based on the fpGUI library, which is a cross-platform toolkit completely written in Object Pascal
  * [Custom Drawn Interface](<Custom_Drawn_Interface.md> "Custom Drawn Interface") \- A cross-platform LCL backend written completely in Object Pascal inside Lazarus. The Lazarus interface to Android.



### Platform specific Tips

  * [Android Programming](<Android_Programming.md> "Android Programming") \- For Android smartphones and tablets
  * [iPhone/iPod development](<iPhone/iPod_development.md> "iPhone/iPod development") \- About using Objective Pascal to develop iOS applications
  * [FreeBSD Programming Tips](<FreeBSD_Programming_Tips.md> "FreeBSD Programming Tips") \- FreeBSD programming tips
  * [Linux Programming Tips](<Linux_Programming_Tips.md> "Linux Programming Tips") \- How to execute particular programming tasks in Linux
  * [macOS Programming Tips](<macOS_Programming_Tips.md> "macOS Programming Tips") \- Lazarus tips, useful tools, Unix commands, and more...
  * [WinCE Programming Tips](<WinCE_Programming_Tips.md> "WinCE Programming Tips") \- Using the telephone API, sending SMSes, and more...
  * [Windows Programming Tips](<Windows_Programming_Tips.md> "Windows Programming Tips") \- Desktop Windows programming tips



### Interface Development Articles

  * [Carbon interface internals](<Carbon_interface_internals.md> "Carbon interface internals") \- If you want to help improving the Carbon interface
  * Windows CE Development Notes \- For Pocket PC and Smartphones
  * [Adding a new interface](<Adding_a_new_interface.md> "Adding a new interface") \- How to add a new widget set interface
  * [LCL Defines](<LCL_Defines.md> "LCL Defines") \- Choosing the right options to recompile LCL
  * [LCL Internals](<LCL_Internals.md> "LCL Internals") \- Some info about the inner workings of the LCL
  * [Cocoa Internals](<Cocoa_Internals.md> "Cocoa Internals") \- Some info about the inner workings of the Cocoa widgetset



## Debugging your applications

General info is here [Windows_CE_Interface#Debbuging Windows CE software on the Lazarus IDE](<Windows_CE_Interface.md> "Windows CE Interface")

  * Sometimes GDB will crash, just reset debugger and start again.
  * If you have an open call stack window it will take lots of time each time you trace or debug your program and it sometimes crashes the debugger, so use it only when needed.
  * Always put breakpoints where needed; you can not-or hardly can- pause your program or even stop/reset it.
  * You can strip your program with --only-keep-debug which save you around 1 mg.There are some issues with strip utility, sometimes it makes your program not executable, So I generally don't recommend using it.
  * Sometimes stepping takes lots of time, just be patient ,I've experienced once a simple assignment took 1 min for debugger to execute that code.



## Alignment problems

Using ARM processors, some times you may get a EBusError exception with a message about misaligned data access. The following section explains what this is and how to fix it. 

  


### What is misaligned data access?

Suppose a CPU has a 8bit data bus to memory and you want to read a byte. Since you have a 8bit bus, you are able to address all individual bytes, so reading is not a problem. Now imagine a CPU with a 32bit data bus and reading a byte. Data is read in chunks of 32bits (=4 bytes) and it is addressed by multiples of 4. For getting the exact byte, the CPU "shifts" the right byte. 

  
Now we do the same for reading a 32bits integer. On an 8 bit CPU, you have to read 4 times one byte and then you have the integer. On a 32bits CPU it can be read in one go if the address is aligned to 4 bytes. If not, it has to be read in 2 halves and then combined to one integer. The x86 supports reading like this, although it is inefficient. The SPARC and some implementations of the ARM (*) don't support this and raise a bus error if you read data like this. 

  
(*) there are extensions to the ARM core which can handle it, but this Depends on what functionality the manufacturer of a specific core has Implemented. 

  


### How to solve this?

Use typecasts of pointers very carefully. 16-bit and 32-bit values need to be 4-byte aligned in memory. Otherwise bus error will occur. Or use newly added "unaligned" keyword before using them. You can use unaligned keyword both in left or right side of your assignments. 

Examples: 
    
    
    var
      p1: ^Longint;
      l: longint;
    begin
      p1^ := 20;  //might cause error if p points to an unaligned memory, i.e. not 4-byte aligned.
      unaligned(p1^) := 20;  //it is ok 
      l := pl^;  //this can also generate error
      l := unaligned(pl^);  //it is ok 
    end.
    

When using Pack records, compiler automatically generates all access to the record members as unaligned access. Currently there are some small issues which sometimes with nested pack records compiler does not generate the required code. You just have to use unaligned keyword. But note that using packed records will influence speed a lot as all of its members are treated as unaligned. So use them only if needed. 

## Implementation details

### ApplicationType

The Windows CE interface will try to automatically detect the kind of device which is being run if nothing is specifyed by the user. The possible devices are: 

  * atDefault - The LCL will detect automatically the type
  * atPDA - A PDA or Smartphone with a touch screen and a small screen. Forms are usually maximized to ocupy the whole work area, and custom positioning and sizing is ignored. Windows CE in default resolution will always have a Width of 240 pixels, and the Height can change from device to device, but 320 pixels is a good guess. In some devices it is possible to specify a bigger size if the screen supports it.
  * atKeyPadDevice - Simple phones and some GPS devices and Bar-code scanners. Devices of this type had are very limited and have a small screen, lacking a touch screen or other pointing device. The very limited navigation capabilities mean that the application must be simplified at a maximum. Under WindowsCE for example only 2 top level menus are possible and all forms need to be maximized.
  * atDesktop - A full desktop application, for which the operating system just happens to be WinCE. Should work just like any desktop application.



Note that before Lazarus 0.9.29 atKeyPadDevice was named atSmartphone and there was an unsupported atHandheld. 

How to specify a fixed device: 
    
    
    program MyProgram;
    
    ...
    
    begin
      Application.ApplicationType := atKeyPadDevice;
      // On Application.Initialize a default device would be detected if 
      // ApplicationType = atDefault, but now the detection is suppressed.
      Application.Initialize;
      ...
    end.
    

### Positioning and size of Dialogs and Forms

At run-time the position and size of forms on the WinCE interface is different then designed. Specifically, it's dependent on the kind of device being run and on the border style. 

  


ApplicationType | BorderStyle | Behavior | WindowState | Title bar | Status   
---|---|---|---|---|---  
atDesktop | Any | Not yet defined. Currently it should work just like on win32, althougth maybe the Left/Top position should always be ignored, because a Handheld may have a small screen. It's also necessary to check if all border styles are supported (how could one resize a form on a handhelp?) | As on desktops | As on desktops | Implemented   
atPDA, atKeyPadDevice | bsDialog | Accepts custom sizing. The X, Y position is ignored and always set to be the top-left corner, but taking into account the existence of an eventual Taskbar on the top. | Not applicable | Separate title bar with a "OK" close button | Implemented   
atPDA, atKeyPadDevice | bsNone | Allows full control of the size and position to the user. | Not applicable | Doesn't exist | Implemented   
atPDA, atKeyPadDevice | bsSizable, bsSingle, bsToolWin, bsSizeToolWin | Rejects custom size and position and is always set to fill the entire work area excluding the menu. The definition of Work Area already excludes the Taskbar | Not applicable | Merged into the taskbar with a "OK" close button | Implemented   
  
Obs: 

  * Initially atDefault was defined as atPDA, but since 0.9.27 (maybe earlier) in Application.Initialize the type will be checked automatically and the ApplicationType will be changed to the correct value.


  * See also: [The LCL in various platforms#TForm.BorderStyle](<The_LCL_in_various_platforms.md> "The LCL in various platforms")



**Links with information**

  * Code to position a form on the work area: [[1]](<http://groups.google.com/group/microsoft.public.pocketpc.developer/browse_thread/thread/f4bba1733d3f6cfa/342497d1797af97a?lnk=st&q=group%3Amicrosoft.public.pocketpc.developer#342497d1797af97a>)



### The Title "OK" and "X" buttons

A very common problem found in previous LCL for WinCE versions was that under Windows CE, by default, there exists no close button available in the title bar. There is, however, a "X" **minimize** button, which minimizes the form. Because there is a "X" written on it, and because a very similar button in desktops closes the form, the majority of Lazarus for WinCE users expected that button to close the form. To attend this expectation we have changed the default behavior for our forms and now they will all have a "OK" close button instead of a "X" minimize button. 

Predicting that some people will desire to have the X button, it was provided a switch to control the title buttons policy. There are some values possible: 

Value | Description   
---|---  
tpAlwaysUseOKButton  | The default one and all forms will have an "OK" close button.   
tpOKButtonOnlyOnDialogs  | Can also be choosen and all forms will have "X" minimize buttons instead of forms with bsDialog borderstyle, which will still have an "OK" close button.   
tpControlWithBorderIcons  | If biMinimize is included in the form's BorderIcons, then a "X" minimize button will be shown. A "OK" button is shown otherwise.   
  
Be very careful using any option which allows the "X" button, because: 

  * Clicking in the X in the main form of the application won't quit it. You are responsible for providing code so that when the user starts a new instance of your application it will detect the running instance, show the running instance and then quit, or else you will fill the users memory with instances of your application.



Example code to change the default policy below: 
    
    
    {$ifdef LCLWinCE}
    uses
      WinCEInt;
    {$endif}
    
    ...
    
    begin
    {$ifdef LCLWinCE}
      WinCEWidgetset.WinCETitlePolicy := tpOKButtonOnlyOnDialogs;
    {$endif}
    end;
    

**Screenshots**

This is a title with "X" minimize button: 

[![Title with x button.PNG](https://wiki.freepascal.org/images/1/18/Title_with_x_button.PNG)](</File:Title_with_x_button.PNG>)

This is a title with "OK" close button: 

[![Title with ok button.PNG](https://wiki.freepascal.org/images/7/71/Title_with_ok_button.PNG)](</File:Title_with_ok_button.PNG>)

**Implementation details**

The "OK" button will send a WM_COMMAND message, which is handled on wincecallback.inc, with wID=IDOK. There we respond by throwing a WM_CLOSE to the form. 

**Bug Reports**

  * <http://bugs.freepascal.org/view.php?id=10284>



### TWinControl.PopupMenu showing

Showing the popup menu of a windowed control is implemented using the native behavior of the Windows CE platform for touch screens. The user holds the pointer in a given control for a certain amount of time and the popup shows. This is implemented on the WM_LBUTTONDOWN message handler by calling SHRecognizeGesture. 

For more details see: 

  * [http://svn.freepascal.org/cgi-bin/viewvc.cgi?view=rev&root=lazarus&revision=13728](<http://svn.freepascal.org/cgi-bin/viewvc.cgi?view=rev&root=lazarus&revision=13728>)



### TRadioButton and TGroupBox

See Also: [Win32/64 Interface#Background color of Standard Controls](<Win32/64_Interface.md> "Win32/64 Interface")

Several different problems with TCustomCheckBox descendents (like TRadioButton) and TGroupBox were found on Lazarus 0.9.25 and are being fixed. 

1 - The autosize of TCustomCheckBox descendents under Windows CE produces a too small area, both inside a TRadioGroup and a alone in a form. That indicates that there was a problem with TWSWinCECustomCheckBox.GetPreferedSize. Even with the code exactly equal to the win32 interface the width was still too small, so an extra 10 pixels factor was added arbitrarely to fix that until a better solution is found - Fixed 

2 - Aditionally TCustomCheckBox descendents inside a TGroupBox (but not on a form) paint a white background instead of painting the correct background color. The way to handle background color changing on a series of controls is by handling the WM_CTLCOLORSTATIC message. Even with the code exactly the same as the Win32 one that still happens, and for unknown misterious reasons if the control is on a form instead of a GroupBox it works normally. 

3 - A third problem found while investigating the other ones is that the difference between the windows client are and the client area that LCL expects was being ignored under Windows CE. This would be seen when one places a control on a TGroupBox or other controls with a border that the position of the places control was wrong - Fixed 

**TRadioGroup before fixes**

[![Wince radiogroup before.png](https://wiki.freepascal.org/images/5/55/Wince_radiogroup_before.png)](</File:Wince_radiogroup_before.png>)

(Obs: The captions should be "Item1", "Yet another Item", "Item2", "Item3") 

**TRadioGroup after fixes**

Not yet fixed 

**Bug Tracker items**

  * <http://bugs.freepascal.org/view.php?id=10288>



### TCheckListBox

See also: [Win32/64 Interface#TCheckListBox](<Win32/64_Interface.md> "Win32/64 Interface")

TCheckListBox presented some color problems under Windows CE which needed to be fixed. Specifically it was found that on several places where system colors were used, the constant SYS_COLOR_INDEX_FLAG wasn't added. LCLIntf.GetSysColor makes sure the constant is added, so it should always be used instead of Windows.GetSysColor. On desktop Windows this constant equals to zero, so it can be ignored. 

The drawing code itself is located at TCustomListBox.LMDrawListItem and TCustomListBox.DrawItem, but what needed fixing was LCLIntf.GetSysColor. 

The colors of the TCheckListBox items under Windows CE are: 

Description | Code | Value before fix | Correct value | Corrected?   
---|---|---|---|---  
background of the entire item | GetSysColor(COLOR_WINDOW) | black | white | Yes   
background of unselected text | default background color | white | white | Yes   
background of selected text | SetBkColor(GetSysColor(COLOR_HIGHLIGHT)) | black | blue | Yes   
background of selected item | GetSysColor(COLOR_HIGHLIGHT) | black | blue | Yes   
color of selected text | SetTextColor(GetSysColor(COLOR_HIGHLIGHTTEXT)) | black | white | Yes   
color of disabled text | SetTextColor(GetSysColor(COLOR_GRAYTEXT)) | black | Gray | Yes   
color of normal text | SetTextColor(GetSysColor(COLOR_WINDOWTEXT)) | black | black | Yes   
  
Acording to the MSDN docs a result of 0 for GetSysColor under Windows CE should identify that it isn't available. Now, 0 is also an RGB value (black), so maybe the WinCE docs are wrong and the Win32 docs should be used. 

**TChecklistbox before fixes**

[![Wince checklistbox.png](https://wiki.freepascal.org/images/1/14/Wince_checklistbox.png)](</File:Wince_checklistbox.png>)

**TChecklistbox after fixes**

[![Wince checklistbox after workaround.png](https://wiki.freepascal.org/images/a/ac/Wince_checklistbox_after_workaround.png)](</File:Wince_checklistbox_after_workaround.png>)

**MSDN Docs**

  * GetSysColor for WinCE: <http://msdn2.microsoft.com/en-us/library/ms929466.aspx>
  * GetSysColor for Win32: <http://msdn2.microsoft.com/en-us/library/ms724371(VS.85).aspx>
  * SetTextColor for WinCE: <http://msdn2.microsoft.com/en-us/library/ms961529.aspx>
  * SetBkColor for WinCE: <http://msdn2.microsoft.com/en-us/library/ms961456.aspx>



**Bug tracker items**

  * <http://www.freepascal.org/mantis/view.php?id=10180> \- Fixed
  * <http://bugs.freepascal.org/view.php?id=13387> \- New bug, regression



### Tab Controls (TPageControl)

While most controls work just "as-is" using normal Win32 code, Tab Controls present a big amount of "gotchas" which makes development of a tab control harder than in Win32. 

A small list: 

  * Vertical text is not supported, so tab buttons located on the left and right won't show text. They are converted into bottom-aligned button style instead.
  * Microsoft recommends using tabs with bottom style under Windows CE, so that the user won't cover the touch screen with his hand when changing tabs.
  * The default style is a bit old. However, the new style seems too minimalistic. You can barely see it's a tab control at all. The call to set new style (flat stype) is on the code (TWinCEWSCustomNotebook.CreateHandle), just commented, so it will be easy to change in the future



Some docs are located here: 

<http://msdn2.microsoft.com/en-us/library/aa921319.aspx>

**Background and sheet position problems**

The background of each page seams not to be painted by default, and because the border around the control is very thin and incomplete (not all edges have borders), you can barely see what the client area of the sheet is. 

I used a "magic" change on the sheet position given by WinCE as described here: 

<http://www.pocketpcdn.com/forum/viewtopic.php?t=499>

Here is a TPageControl before the sheet position adjustment: 

[![Before tab adjust.png](https://wiki.freepascal.org/images/8/81/Before_tab_adjust.png)](</File:Before_tab_adjust.png>)

And after: 

[![After tab adjust.png](https://wiki.freepascal.org/images/7/7e/After_tab_adjust.png)](</File:After_tab_adjust.png>)

**Bug tracker items**

  * ~~<http://www.freepascal.org/mantis/view.php?id=8938>~~ \- Fixed
  * <http://bugs.freepascal.org/view.php?id=16781>
  * <http://bugs.freepascal.org/view.php?id=13457>



### DataAware controls

For some unknown reason, data-aware controls crash the application. 

**Bug tracker items**

  * <http://bugs.freepascal.org/view.php?id=10442>



  


### Brushes

CreateBrushIndirect doesn't exist under Windows CE, so the interface maps it to CreateSolidBrush, CreateDIBPatternBrushPt and GetStockObject. 

If special flags are used they are ignored and CreateSolidBrush is used instead. 

From CreateBrushIndirect on wincewinapi.inc: 
    
    
      if lb.lbStyle= BS_NULL then
        Result := Windows.GetStockObject(NULL_BRUSH)
      else if lb.lbStyle = BS_DIBPATTERNPT then
        Result := CreateDIBPatternBrushPt(pointer(lb.lbHatch), lb.lbColor)
      else { lb.lbStyle = BS_SOLID }
        Result := Windows.CreateSolidBrush(LB.lbColor);
    

### SetProp - GetProp - RemoveProp APIs

Windows CE does not have SetProp and GetProp API's which is used a lot. So I ([User:Roozbeh](</User:Roozbeh> "User:Roozbeh")) have created/emulated them. The current implementation is not exactly as win32 API. In win32 you can set multiple properties with different names to each window but here I ignore names so we can only have one kind of property assigned to each control. (It is easy to implement that, but I find it currently of no use). Also removeprop is not called yet! So a memory leak happens each time you exist the program. (I don't know if wince also frees memory itself or not when window handles are destroyed).Should be implemented soon. 

### Modal Dialogs and the OK and X buttons

Under Windows CE some dialogs will present both the OK and the X button. It is possible to distinguish between them according to the ModalResult that they send. The OK close button will send mrOK, while the X minimize button will close the dialog and send a mrCancel result. 

Here is the dialog produced by the following code under Windows CE: 
    
    
    MessageDlg('Shall I test a very, really, long string?', mtConfirmation, [mbYes, mbNo, mbOk, mbCancel],0);
    

  
[![Messagedlg wince.PNG](https://wiki.freepascal.org/images/8/82/Messagedlg_wince.PNG)](</File:Messagedlg_wince.PNG>)

**Bug reports** : 

  * <http://bugs.freepascal.org/view.php?id=11439>
  * <http://bugs.freepascal.org/view.php?id=13834>



### Overlapping menu bottom

There is a flaw in Windows CE. The routine Windows.SystemParametersInfo(SPI_GETWORKAREA, 0, @WR, 0); doesn't return what we expect in some systems, and returns it on other ones. The problem is that on some systems the menu bar is considered part of the work area, and in others it isn't. So, when a menu is present we need to check if our program is overlapping the menubar, and if it is, we should make our window smaller. 

Some software solve this by applying a factor of 26 to the window height, but this approach isn't that good because there is no way to determina if the device is of the kind that needs the factor. 

The solution was implemented in CESetMenu, so the window will only have it's final size after the menu is set. This makes it unclear at the moment if the size of the window would be wrong in some events of the form. 

Here is a screenshot of an application which is overlapping the menu: 

[![Wince overlapping menu.png](https://wiki.freepascal.org/images/f/f6/Wince_overlapping_menu.png)](</File:Wince_overlapping_menu.png>)

**Microsoft Newsgroup Links** : 

Routine to maximize a window in Windows CE. Not very good, because it just uses a lot of ifdefs: 

[http://groups.google.com/group/microsoft.public.pocketpc.developer/browse_thread/thread/f4bba1733d3f6cfa/342497d1797af97a?lnk=gst&q=unreliable#342497d1797af97a](<http://groups.google.com/group/microsoft.public.pocketpc.developer/browse_thread/thread/f4bba1733d3f6cfa/342497d1797af97a?lnk=gst&q=unreliable#342497d1797af97a>)

Workarea problem description: 

<http://groups.google.com/group/microsoft.public.windowsce.embedded.vc/browse_thread/thread/2f4ecdade7fe48aa?q=menu+SystemParametersInfo+group:microsoft.public.windowsce.*#44941f01c43edbd7>

**Fixed in version:** 0.9.27 

### Standard dialogs

The native dialogs in Windows CE are too simplistic and limit the user to a small number of directories. Better dialogs will be built instead. There are some missing controls in the LCL, so those will need to be created first. 

Steps to implement the standard dialogs: 

  1. ~~Implement TShellTreeView~~
  2. ~~Implement TShellListView~~
  3. ~~Implement TFilterComboBox~~
  4. ~~Implement TOpenDialog and TSaveDialog~~
  5. Implement other dialogs
  6. Fix any bugs that appear



The implementation should be very similar to this: 

<http://www.codeproject.com/KB/mobile/cefileopendlg.aspx>

In the save dialog I would remove the TFileListBox to make room for a TEdit. 

A special care will need to be made to make sure that the dialog fits any resolution. 

**Images**

State of the open and save dialogs in Lazarus 0.9.28 

[![Wince opendialog 0 9 28.PNG](https://wiki.freepascal.org/images/2/2f/Wince_opendialog_0_9_28.PNG)](</File:Wince_opendialog_0_9_28.PNG>)[![Wince savedialog 0 9 28.PNG](https://wiki.freepascal.org/images/5/54/Wince_savedialog_0_9_28.PNG)](</File:Wince_savedialog_0_9_28.PNG>)

**Bug tracker items**

  * <http://bugs.freepascal.org/view.php?id=11572>



### Running in desktop Windows

Yes, the Windows CE interface also works for desktop Windows. I (Felipe Carvalho) implemented this feature to speed up debugging, as the desktop windows debugger is much quicker, and also the desktop compiler is much quicker. There are still bugs to be solved for this interface, but it has served well its purpose until now. 

### Smartphone menus

A particularly hard to implement feature are smartphone menus, which are very limited and often hard to work with. Their most visible limitation is the number of top-level menu items being at most 2, so that they can be easily accessed in devices without touchscreen. 

Another important thing to notice is that PocketPC and Windows Mobile top-level menus are implemented with a TOOLBAR windows class, not with MENU class. The MENU class is destined for popup menus, those which are activated by clicking in the top-level menu, or in a popup menu. 

Also, the top-level menu can only be set to a resource menu, it cannot be completely programatically created, but it can be altered later, which offers an equivalent solution. The menu is set to a resource menu with SHCreateMenuBar. 

**Differences between smartphone and pocketpc menus** : 

Some usual things don't work with smartphone menus, for example if a menu item has been set as not having a submenu, then it cannot be changed to have one, that's why we need many menu solutions in wincemenures.rc 

**Flow of information of menu items** : 

First the handle of the menus are created with TWinCEWSMenuItem.CreateHandle and TWinCEWSMenu.CreateHandle, which just creates an empty menu item/menu. The real building of the item is processed in TWinCEWSMenuItem.AttachMenuEx, which connects a menu item with it's corresponding parent menu, or parent menu item. This routine will save all menu items attached (which is the effective creation) in WinCEWSMenus.MenuItemsList: TStringList, so that they can be later found when clicked. 

With the menus built and attached, the routine LCLIntf.SetMenu is called to attach a menu to a window. In Windows CE only one menu is visible at a time (except for atHandheld and atDesktop application modes), so this will be the only menu visible in the device. This routine will then procede to set this single device menu to the best fit for the application menu setting all the top-level menus and their properties, such as supporting a submenu. With this in place, the already created handle of the menus is attached to the single device menu by calling TWinCEWSMenuItem.AttachMenuEx for each subitem of the top-level menu. Further items are already attached to those handles, so only a double loop is necessary, to cover all subitems of the all top-level menu items. 

Upon clicking a menu, the list in WinCEWSMenus.MenuItemsList: TStringList is read and the menu is searched for, this works for all not-top-level menus. 

**Menu item click** : 

A click in a menu item is received as WM_COMMAND message, then we have a list of menu items called MenuItemsList and we search in this list for the menu "command". Note that the top-level items actually have the fixed values 1001 and 1002, so secondary forms cannot receive top-level menu clicks in atKeyPadDevice mode, because there is a clash between the menu IDs of both forms. 

**Revisions related to this implementation** : 

[http://svn.freepascal.org/cgi-bin/viewvc.cgi?view=rev&root=lazarus&revision=22236](<http://svn.freepascal.org/cgi-bin/viewvc.cgi?view=rev&root=lazarus&revision=22236>), 22238, 22239 

**Bug tracker items** : 

  * ~~<http://bugs.freepascal.org/view.php?id=10440>~~
  * ~~<http://bugs.freepascal.org/view.php?id=10383>~~
  * ~~<http://bugs.freepascal.org/view.php?id=14691>~~
  * <http://bugs.freepascal.org/view.php?id=16435>



Reference: 

  * <http://channel9.msdn.com/wiki/MobileDeveloper/Menu/>



### Regressions

From time to time there can be regressions and specially in 0.9.29 a good amount of regressions were introduced in the Windows CE Widgetset. This documents the effort to find and fix those regressions. 

Table about the behavior of a single issue in time: 

Revision | Version | Compilation Obs | Group Box | Label | PageControl | ComboBox | TStringGrid | Window   
---|---|---|---|---|---|---|---|---  
23000 | - | - | correct position | The label inside a group box doesn't even show. Other labels show correct position. | Border broken | ? | ? | ?   
23136 | - | - | correct position | The label inside a group box doesn't even show. Other labels show correct position. | Border broken | Works normally | ? | ?   
23137 | - | - | correct position | The label inside a group box / pagecontrol doesn't even show. Other labels show in (0,0). | Full border drawn | Works normally | ? | ?   
23289 | - | - | correct position | The label inside a group box / pagecontrol doesn't even show. Other labels show in (0,0). | Full border drawn | Works normally | ? | ?   
23290 a 23426 | - | Doesn't build in Windows CE   
23427 | - | Freeze under FPC 2.4.x caused by rev 23290   
23500 | - | Freeze under FPC 2.4.x caused by rev 23290 | correct position | The label inside a group box / pagecontrol doesn't even show. Other labels show in (0,0). | Full border drawn | Works normally | ? | ?   
23618 | - | Fixes the Freeze caused by rev 23290 | correct position | The label inside a group box / pagecontrol doesn't even show. Other labels show in (0,0). | Full border drawn | Works normally | ? | ?   
23750 | - | - | correct position | The label inside a group box / pagecontrol doesn't even show. Other labels show in (0,0). | Full border drawn | ? | ? | ?   
23900 | - | - | correct position | The label inside a pagecontrol doesn't even show. Other labels show in (0,0). | Full border drawn | Works normally | ? | ?   
24084 | - | - | correct position | The label inside a pagecontrol doesn't even show. Other labels show in (0,0). | Full border drawn | Works normally | ? | ?   
24085 | - | - | ? | ? | ? | Freezes the form | ? | ?   
24176 | - | - | correct position | The label inside a pagecontrol doesn't show. All labels have zero X,Y position | Full border drawn | Freezes the form | ? | ?   
24177 | - | - | painted wrongly | The label inside a pagecontrol doesn't show. the label inside a group box has too small x-position | Border broken | ? | ? | ?   
24500 | - | - | painted wrongly | The label inside a pagecontrol doesn't show. the label inside a group box has too small x-position | Border broken | ? | Works | Maximized doesn't work   
25000 | 0.9.29 | - | painted wrongly | The label inside a pagecontrol doesn't show. the label inside a group box doesn't even show | Border broken | ? | Works | ?   
25170 a 25178 | - | Doesn't build in Windows CE and rev25170 also breaks the TStringGrid scrollbar   
25180 | 0.9.29 5 may 2010 | - | painted wrongly | The label inside a pagecontrol doesn't show. the label inside a group box has too small x-position | Border broken | Freezes the form | Works | ?   
25213 | 0.9.29 | - | painted wrongly | The label inside a pagecontrol doesn't show. the label inside a group box has too small x-position | Border broken | ? | Works | ?   
25214 | 0.9.29 | - | fixed | The label inside a pagecontrol doesn't show | fixed | ? | Broke in this rev | ?   
25300 | 0.9.29 | - | = | = | = | = | Broken | ?   
25483 | 0.9.29 | - | = | = | = | = | = | Maximized doesn't work   
25484 | 0.9.29 | - | = | = | = | = | = | Maximized fixed   
  
Table of what was broken and fixed in each revision: 

Revision | Version | Description   
---|---|---  
[23137](<http://svn.freepascal.org/cgi-bin/viewvc.cgi?view=rev&root=lazarus&revision=23137>) | - | Broke label in form positioning to (0, 0), fixed pagecontrol frame The problems in rev23137 were caused by the introduction of LCLIntf.SetViewPortOrgEx which made this routine no longer be implemented, as now it was calling LCLIntf version of SetViewPortOrgEx (not implemented under WinCE) instead of the WinAPI: 
    
    
    function TWinCEWidgetSet.SetWindowOrgEx(DC: HDC; NewX, NewY: Integer;
      OldPoint: PPoint): Boolean;
    begin
     { roozbeh:does this always work?it is heavily used in labels and some other drawing controls}
      Result := Boolean(SetViewPortOrgEx(DC, -NewX, -NewY, LPPoint(OldPoint)));
    //  Result:=inherited SetWindowOrgEx(dc, NewX, NewY, OldPoint);
    end;
    

rev24177 fixed that, but it didn't fix the problem completely.   
[23794](<http://svn.freepascal.org/cgi-bin/viewvc.cgi?view=rev&root=lazarus&revision=23794>) | - | Fixes label showing in Group Box   
[24085](<http://svn.freepascal.org/cgi-bin/viewvc.cgi?view=rev&root=lazarus&revision=24085>) | - | New Autosize causes freeze in TComboBox   
[24177](<http://svn.freepascal.org/cgi-bin/viewvc.cgi?view=rev&root=lazarus&revision=24177>) | - | Broke Group Box title, Group Box painting, label inside control X position, pagecontrol frame   
[25170](<http://svn.freepascal.org/cgi-bin/viewvc.cgi?view=rev&root=lazarus&revision=25170>) | - | Breaks TStringGrid Scrollbar. See <http://bugs.freepascal.org/view.php?id=16576>  
25185, 25194, 25201, 25202, 25203, [25214](<http://svn.freepascal.org/cgi-bin/viewvc.cgi?view=rev&root=lazarus&revision=25214>) | - | Fixes Group Box title, Group Box painting, label painting and pagecontrol frame. Removes support to clipping to achieve that.   
[25213](<http://svn.freepascal.org/cgi-bin/viewvc.cgi?view=rev&root=lazarus&revision=25213>) | - | Breaks TStringGrid Painting   
25314, 25350, 25355, [25357](<http://svn.freepascal.org/cgi-bin/viewvc.cgi?view=rev&root=lazarus&revision=25357>) | - | Fixes Combo Box autosize freeze.   
[25359](<http://svn.freepascal.org/cgi-bin/viewvc.cgi?view=rev&root=lazarus&revision=25359>) | - | Fixes SpinEdit autosize freeze.   
[25484](<http://svn.freepascal.org/cgi-bin/viewvc.cgi?view=rev&root=lazarus&revision=25484>) | - | Fixes Window wsMaximized   
[25661](<http://svn.freepascal.org/cgi-bin/viewvc.cgi?view=rev&root=lazarus&revision=25661>) | - | Fixes TStringGrid drawing, breaks changing a label inside a TGroupBox   
[25710](<http://svn.freepascal.org/cgi-bin/viewvc.cgi?view=rev&root=lazarus&revision=25710>) | - | Fixes TStringGrid scroll bars   
  
**Bug tracker items** : 

  * <http://bugs.freepascal.org/view.php?id=13457>
  * ~~<http://bugs.freepascal.org/view.php?id=14760>~~
  * ~~<http://bugs.freepascal.org/view.php?id=15945>~~
  * ~~<http://bugs.freepascal.org/view.php?id=16433>~~
  * ~~<http://bugs.freepascal.org/view.php?id=16576>~~



### Networking crash bug

Something in FPC introduced between 2.2.0 and 2.2.2 is causing a crash in WinCE applications. 

FPC 2.2.0 was tagged in revision 8339 FPC 2.2.2 was tagged in revision 11490 

The involved FPC Branch is: <http://svn.freepascal.org/cgi-bin/viewvc.cgi/branches/fixes_2_2/>

**List of merged revisions into FPC 2.2.2** : 

8404, 8410 - Only linux 8411 - Compiler 8430 - RTL, Compiler 8438 - Utils 

  
**Bug tracker items** : 

  * 


### ShowMessage regression

About this bug report: <http://bugs.freepascal.org/view.php?id=22718>

Revision | Observations   
---|---  
34300 | Works correctly   
34400 | Doesnt compile on WinCE   
34600 | Doesnt compile on WinCE   
34700 | Works correctly   
35000 | Works correctly   
35500 | Works correctly   
35700 | Works correctly   
35800 | Works correctly   
35950 | Works correctly   
35960 | Works correctly   
35961 | Bug present   
35962 | Bug present   
35963 | Bug present   
35965 | Bug present   
35975 | Bug present   
35980 | Bug present   
36000 | Bug present   
  
## List of devices with information about them

There are differences between devices, so it would be interresting to have a list with information about each of them. Please contribute. 

  


Device Name | WinCE Version | ApplicationType (Auto-detection works?) | Touchscreen? | Width* | Height* | Menu Size** | Observations   
---|---|---|---|---|---|---|---  
WM 5.0 Emulator - PocketPC | 5.0 | atPDA (yes) | Yes | 240 | 268 | 26 |   
WM 5.0 Emulator - Smartphone | 5.0 | atKeyPadDevice (yes) | No | 176 | 180 | 0 |   
WM 5.0 Emulator - Smartphone QVGA | 5.0 | atKeyPadDevice (yes) | No | 240 | 266 | 0 |   
WM 2003 SE Emulator - Pocket PC | 4.20 | atPDA (?) | Yes | 240 | 294 | 0 |   
WM 2003 SE Emulator - Pocket PC Square | 4.20 | atPDA (?) | Yes | 240 | 214 | 0 |   
  
  * * - Values available for a maximized program will all extras activated (start menu, menu correction, PID, etc).
  * ** - The size of the correction which needs to be applyed when a menu is present. The LCL should calculate this automatically.



Abreviations: 

  * WM = Windows Mobile
  * SE = Second Edition



## Links

Here are some links that might be useful for creating Windows CE interfaces. 

  * 3rd Party GUI controls for .Net Compact Framework [BeeMobile controls for WinCE](<http://beemobile4.net>)
  * [How to Port Your Win32 Code to Windows CE](<http://www.developer.com/ws/pc/article.php/10947_1483401_1>)
  * [Old but useful article show flags required for creating wince controls](<http://www.sundialsoft.freeserve.co.uk/sddoc001.htm>)
  * [Page with nice descriptions of lots of flags](<http://www.orbworks.com/wince/resource/data/header.html>)
  * [Adapting Application Menus to the CE User Interface](<http://www.developer.com/ws/pc/article.php/1490871>)
  * [Differences between normal Win32-API and Windows CE (old wince)](<http://www.rainer-keuchel.de/wince/wince-api-diffs.txt>)
  * [Resource-Definition Statements](<http://msdn.microsoft.com/library/default.asp?url=/library/en-us/wcesdkr/html/_Tools_CONTROL_Control.asp>)
  * [GUI Control Styles](<http://www.autoitscript.com/autoit3/docs/appendix/GUIStyles.htm>)
  * [Developing Orientation and dpi Aware Applications](<http://msdn.microsoft.com/library/default.asp?url=/library/en-us/dnppcgen/html/developing_orientation_and_resolution_aware_apps.asp>)
  * [Developing Screen Orientation-Aware Applications](<http://msdn.microsoft.com/library/default.asp?url=/library/en-us/dnppcgen/html/screen_orientation_awareness.asp>)
  * [Introduction to User Interface Controls in a Windows Mobile-based Smartphone ](<http://msdn.microsoft.com/library/default.asp?url=/library/en-us/dnppcgen/html/installation_with_dpi-specific_resources.asp>)
  * [WinCE port of KOL GUI library](<KOL-CE.md> "KOL-CE")
  * [Step by Step: Writing Device-Independent Windows Mobile Applications with the .NET Compact Framework](<http://msdn.microsoft.com/en-us/library/bb278110.aspx>)

---

_Source: [https://wiki.freepascal.org/Windows_CE_Development_Notes](https://web.archive.org/web/20240920204130/https://wiki.freepascal.org/Windows_CE_Development_Notes)_
