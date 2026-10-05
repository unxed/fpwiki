# Win32/64 Interface

[![Windows logo - 2012.svg](https://upload.wikimedia.org/wikipedia/commons/thumb/5/5f/Windows_logo_-_2012.svg/50px-Windows_logo_-_2012.svg.png)](</File:Windows_logo_-_2012.svg>)

This article applies to [Windows](</Category:Windows> "Category:Windows") only.

See also: [Multiplatform Programming Guide](<../Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

│ **English (en)** │  **[русский (ru)](<../../ru/Win32/64_Interface.md>)** │

## Contents

  * 1 Introduction
  * 2 Current issues
    * 2.1 Scrolling
    * 2.2 Focus indication on themed controls
    * 2.3 Navigating controls with the arrow keys
  * 3 Implementation details
    * 3.1 Background color of Standard Controls
    * 3.2 TCheckListBox
  * 4 FAQ
    * 4.1 Processing user messages in your window
    * 4.2 Processing non-user messages in your window
      * 4.2.1 Example
  * 5 Other Interfaces
    * 5.1 Platform specific Tips
    * 5.2 Interface Development Articles



## Introduction

The win32/64 interface is arguably the most polished and well developed interface of Lazarus, being also the most used one considering the number of downloads. Despite being the most complete, there are still some problems with it that need fixing. 

Another important point about the win32/64 interface is that it is currently undergoing [migration to unicode](<../LCL_Unicode_Support.md> "LCL Unicode Support"). 

Win64: please see warning [here](<../Windows_Programming_Tips.md> "Windows Programming Tips") on not using certain FPC/Lazarus Win64 versions. 

  


## Current issues

### Scrolling

The scrolling is currently done by moving child controls instead of the client area as the LCL expects. For example in some cases it looks as if the children are being scrolled in reverse. The truth is that the scrolling is pretty much broken. Scrolling the child controls is incompatible with the other widgetsets and also has this drawback: Moving one child after the other generates several move messages. The LCL receives the messages and has to react each time (for instance it has to realign all anchored children). For a single widgetset you can attempt to overcome this problem, but the solution will never work well for all the other widgetsets. This approach works for Delphi's VCL, but not for the LCL. Therefore another approach must be implemented: 

**Solution**

A solution would be to insert a 'client area' window between each child's window and its parent window. The child windows are placed on the 'client area' window, and when the children are scrolled the 'client area' window gets moved instead. This is already done by the other widgetsets. 

Mattias: I will implement this eventually, but my winapi interface knowledge is limited and I have a lot of other Lazarus tasks already, so I can't say when I will implement it. 

**Related bug reports**

  * [10400](<http://bugs.freepascal.org/view.php?id=10400>)
  * [10188](<http://bugs.freepascal.org/view.php?id=10188>)
  * [10471](<http://bugs.freepascal.org/view.php?id=10471>)
  * [10091](<http://bugs.freepascal.org/view.php?id=10091>)



### Focus indication on themed controls

Controls (TCheckBox, TButton, TRadioButton, etc) loose their focus indication when the application is using themes. 

**Hints to solve**

  * According to Paul it isn't related to WM_PAINT



**Related bug reports**

  * <http://bugs.freepascal.org/view.php?id=10685>



### Navigating controls with the arrow keys

**Related bug reports**

  * <http://bugs.freepascal.org/view.php?id=10742>



## Implementation details

This list explains how specific parts of the LCL are implemented on the Win32 interface to help people find the appropriate code in it to do bug fixes. 

### Background color of Standard Controls

See also: [Windows_CE_Development_Notes#TRadioButton_and_TGroupBox](<../Windows_CE_Development_Notes.md> "Windows CE Development Notes")

One might notice that on Windows there is no implementation for TWSWin32WinControl.SetColor and neither does it have for most standard controls (GroupBox, RadioButton, CheckBox, etc), even thougth those controls can have their background color changed. 

The reason is that this is implemented by handling a WM_CTLCOLOR* message. Most standard controls (GroupBox, RadioButton, CheckBox, etc) will send a WM_CTLCOLORSTATIC message. On this message one can set the background color and a handle to the brush used to paint the control must be returned. 

Another important detail about this is that child controls have their messages sent to the WindowProc of their parent. So, if one has a Form with a GroupBox and a couple of CheckBoxes inside the GroupBox the messages of the GroupBox will go to the Form and the messages of the CheckBoxes will go to the GroupBox (including the WM_CTLCOLORSTATIC message). To overcome this the win32 widgetset uses SetWindowLong to reset the WindowProc of controls with child controls to our centralized WindowControl on win32callback.inc. 

### TCheckListBox

See also: [Windows_CE_Development_Notes#TCheckListBox](<../Windows_CE_Development_Notes.md> "Windows CE Development Notes")

  * TWin32WSCustomCheckListBox implements some minimal methods
  * TWin32WSCustomListBox implements handle creating and most methods
  * TWin32CheckListBoxStrings is the main TStrings descendent for this class



On Windows, a TCheckListBox is a normal window of the class 'LISTBOX' which has the LBS_OWNERDRAWFIXED style set. When the listbox is created a WM_MEASUREITEM message will be sent, and after a WM_DRAWITEM message will be sent whenever an item needs to be painted. In the handler for this message a message LM_DRAWITEM message will be created. 

The LM_DRAWITEM message is then intercepted by TWin32WidgetSet.CallDefaultWndHandler and handled in it's internal function: 

procedure DrawCheckListBoxItem(CheckListBox: TCheckListBox; Data: PDrawItemStruct); 

And here is the real code to paint the TCheckListBox items. 

Note: This is kind of ugly, maybe this code should be moved to the LCL so we have a generic code to paint items in case the widgetset doesn't do it itself. 

MSDN Docs about the LISTBOX: 

  * <http://msdn2.microsoft.com/en-us/library/bb775146(VS.85).aspx>
  * <http://msdn2.microsoft.com/en-us/library/bb775149(VS.85).aspx>



## FAQ

### Processing user messages in your window

write me 

### Processing non-user messages in your window

To have a custom processing of messages <= WM_USER you should use SetWindowLong from the Windows unit. It will return the address of the current WndProc, so you can just have your WndProc like this: 
    
    
    begin
     if Msg = WM_COPYDATA then
     ...
     else CallOldWindowProc;
    end;
    

And you don't lose anything. With a clever code you can even use the same wndproc for any control, just take care to call the correct old wndproc in each case. 

#### Example

By intercepting the WM_NCHITTEST message you can avoid dragging the window. 

Note: The part were the function interacts with "WM_NCHITTEST" has a very strange result in Windows XP. You will be unable to move the window. In Vista on the other hand, you still can. Commenting this section out seems to do no harm, and you are able to move the program's window in Windows XP. 
    
    
    uses
       ...Windows...
    ...
    var
      PrevWndProc: WNDPROC;
    ...
    function WndCallback(Ahwnd: HWND; uMsg: UINT; wParam: WParam; lParam: LParam):LRESULT; stdcall;
    begin
      if uMsg=WM_NCHITTEST then
      begin
        result:=Windows.DefWindowProc(Ahwnd, uMsg, WParam, LParam);  //not sure about this one
        if result=windows.HTCAPTION then result:=windows.HTCLIENT;
        exit;
      end;
      result:=CallWindowProc(PrevWndProc,Ahwnd, uMsg, WParam, LParam);
    end;
    
    //install our message handler
    procedure TForm1.FormCreate(Sender: TObject);
    begin
      PrevWndProc:=Windows.WNDPROC(SetWindowLongPtr(Self.Handle,GWL_WNDPROC,PtrUInt(@WndCallback)));
    end;
    

  


## Other Interfaces

  * [Lazarus known issues (things that will never be fixed)](<../Lazarus_known_issues_\(things_that_will_never_be_fixed\).md> "Lazarus known issues \(things that will never be fixed\)") \- A list of interface compatibility issues
  * Win32/64 Interface \- The Windows API (formerly Win32 API) interface for Windows 95/98/Me/2000/XP/Vista/10, but not CE
  * [Windows CE Interface](<../Windows_CE_Interface.md> "Windows CE Interface") \- For Pocket PC and Smartphones
  * [Carbon Interface](<../Carbon_Interface.md> "Carbon Interface") \- The Carbon 32 bit interface for macOS (deprecated; removed from macOS 10.15)
  * [Cocoa Interface](<../Cocoa_Interface.md> "Cocoa Interface") \- The Cocoa 64 bit interface for macOS
  * [Qt Interface](<../Qt_Interface.md> "Qt Interface") \- The Qt4 interface for Unixes, macOS, Windows, and Linux-based PDAs
  * [Qt5 Interface](<../Qt5_Interface.md> "Qt5 Interface") \- The Qt5 interface for Unixes, macOS, Windows, and Linux-based PDAs
  * [GTK1 Interface](<../GTK1_Interface.md> "GTK1 Interface") \- The gtk1 interface for Unixes, macOS (X11), Windows
  * [GTK2 Interface](<../GTK2_Interface.md> "GTK2 Interface") \- The gtk2 interface for Unixes, macOS (X11), Windows
  * [GTK3 Interface](<../GTK3_Interface.md> "GTK3 Interface") \- The gtk3 interface for Unixes, macOS (X11), Windows
  * [fpGUI Interface](<../fpGUI_Interface.md> "fpGUI Interface") \- Based on the fpGUI library, which is a cross-platform toolkit completely written in Object Pascal
  * [Custom Drawn Interface](<../Custom_Drawn_Interface.md> "Custom Drawn Interface") \- A cross-platform LCL backend written completely in Object Pascal inside Lazarus. The Lazarus interface to Android.



### Platform specific Tips

  * [Android Programming](<../Android_Programming.md> "Android Programming") \- For Android smartphones and tablets
  * [iPhone/iPod development](<../iPhone/iPod_development.md> "iPhone/iPod development") \- About using Objective Pascal to develop iOS applications
  * [FreeBSD Programming Tips](<../FreeBSD_Programming_Tips.md> "FreeBSD Programming Tips") \- FreeBSD programming tips
  * [Linux Programming Tips](<../Linux_Programming_Tips.md> "Linux Programming Tips") \- How to execute particular programming tasks in Linux
  * [macOS Programming Tips](<../macOS_Programming_Tips.md> "macOS Programming Tips") \- Lazarus tips, useful tools, Unix commands, and more...
  * [WinCE Programming Tips](<../WinCE_Programming_Tips.md> "WinCE Programming Tips") \- Using the telephone API, sending SMSes, and more...
  * [Windows Programming Tips](<../Windows_Programming_Tips.md> "Windows Programming Tips") \- Desktop Windows programming tips



### Interface Development Articles

  * [Carbon interface internals](<../Carbon_interface_internals.md> "Carbon interface internals") \- If you want to help improving the Carbon interface
  * [Windows CE Development Notes](<../Windows_CE_Development_Notes.md> "Windows CE Development Notes") \- For Pocket PC and Smartphones
  * [Adding a new interface](<../Adding_a_new_interface.md> "Adding a new interface") \- How to add a new widget set interface
  * [LCL Defines](<../LCL_Defines.md> "LCL Defines") \- Choosing the right options to recompile LCL
  * [LCL Internals](<../LCL_Internals.md> "LCL Internals") \- Some info about the inner workings of the LCL
  * [Cocoa Internals](<../Cocoa_Internals.md> "Cocoa Internals") \- Some info about the inner workings of the Cocoa widgetset

---

_Source: [https://wiki.freepascal.org/Win32/64_Interface](https://web.archive.org/web/20240920204132/https://wiki.freepascal.org/Win32/64_Interface)_
