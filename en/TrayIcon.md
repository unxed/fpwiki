# How to use a TrayIcon

│ **English (en)** │    


## Contents

  * 1 About
  * 2 Documentation
    * 2.1 Methods
    * 2.2 Properties
    * 2.3 Events
  * 3 Authors
  * 4 License
  * 5 Download
  * 6 Example 1 - Using TIcon
  * 7 Example 2 - Creating the icon with TLazIntfImage
  * 8 Subversion
  * 9 Help, Bug Reporting and Feature Request
  * 10 Change Log
  * 11 Technical Details
  * 12 External Links



### About

**[TTrayIcon](<TTrayIcon.md> "TTrayIcon")** is a multiplatform System Tray component. You can find TrayIcon on the [Additional tab](<Additional_tab.md> "Additional tab") of the [Component Palette](<Component_Palette.md> "Component Palette") (0.9.23+). 

TrayIcon used to be an optional component, but is part of LCL since Lazarus 0.9.23 

To start quickly, please read [the demonstration program](<TrayIcon.md> "TrayIcon"). 

### Documentation

Below is a list of all methods, properties and events of the component. They have the same names and work the same way on the visual component and on the non-visual object. 

A function works on all target platforms unless written otherwise. 

#### Methods

**Show**

**procedure** Show; 

Shows the icon on the system tray. 

**Hide**

**procedure** Hide; 

Removes the icon from the system tray. 

**GetPosition**

**function** GetPosition: TPoint; 

Returns the position of the tray icon on the display. This function is utilized to show message boxes near the icon. Currently it´s only a stub, no implementations are available and TPoint(0, 0) is returned. 

#### Properties

**Hint**

**property** Hint: string; 

A Hint will be shown the string isn't empty 

**PopUpMenu**

**property** PopUpMenu: TPopUpMenu; 

A PopUp menu that appears when the user right-clicks the tray icon. 

#### Events

**OnPaint**

**property** OnPaint: TNotifyEvent; 

Use this to implement custom drawing to the icon. Draw using the canvas property of the icon. 

[![Note-icon.png](https://wiki.freepascal.org/images/b/be/Note-icon.png)](</File:Note-icon.png>)

**Note:** Does not work on win32.

**OnClick**

**property** OnClick: TNotifyEvent; 

**OnDblClick**

**property** OnDblClick: TNotifyEvent; 

**OnMouseDown**

**property** OnMouseDown: TMouseEvent; 

  
**OnMouseUp**

**property** OnMouseUp: TMouseEvent; 

**OnMouseMove**

**property** OnMouseMove: TMouseMoveEvent; 

### Authors

  * [Felipe Monteiro de Carvalho](</User:Sekelsenmat> "User:Sekelsenmat")
  * [Andrew Haines](</User:AndrewH> "User:AndrewH")



[![Note-icon.png](https://wiki.freepascal.org/images/b/be/Note-icon.png)](</File:Note-icon.png>)

**Note:** Windows: [Ozz Nixon](</User:Ozznixon> "User:Ozznixon")

### License

Modified LGPL. 

### Download

Status: Stable 

Can be located at Lazarus 0.9.22 or inferior at the directory: lazarus/components/trayicon 

And on Lazaurs 0.9.23 or superior it is automatically installed with LCL 

### Example 1 - Using TIcon

As of Lazarus 0.9.26 TIcon has been fully implemented and it is no longer necessary to load the icon from a resource file on Windows. The icon can be loaded in the IDE or with usual code. 

Go to the Additional tab of components, and add a TTrayIcon to your form. Then change it's **Name** property to SystrayIcon 

Next add a button to the form. Double click the button and add this code to it: 
    
    
    procedure MyForm.Button1Click(Sender: TObject);
    begin
      SystrayIcon.Icon.LoadFromFile('/path_to_icon/icon.ico');
      SystrayIcon.ShowHint := True;
      SystrayIcon.Hint := 'my tool tip';
     
      SystrayIcon.PopUpMenu := MyPopUpMenu;
     
      SystrayIcon.Show;
    end;

### Example 2 - Creating the icon with TLazIntfImage

You can use TLazIntfImage to draw quickly your icon, as in the example code below: 
    
    
    procedure TForm1.DrawIcon;
    var
      TempIntfImg: TLazIntfImage;
      ImgHandle, ImgMaskHandle: HBitmap;
      px, py: Integer;
      TempBitmap: TBitmap;
    begin
      try
        TempIntfImg := TLazIntfImage.Create(16, 16);
        TempBitmap := TBitmap.Create;
        TempBitmap.Width := 16;
        TempBitmap.Height := 16;
        TempIntfImg.LoadFromBitmap(TempBitmap.Handle, TempBitmap.MaskHandle);
     
        // Set the pixels red
        for py := 0 to TempIntfImg.Height - 1 do
          for px := 0 to TempIntfImg.Width - 1 do
            TempIntfImg.Colors[px, py] := colRed;
     
        // Copy it to a TBitmap
        TempIntfImg.CreateBitmaps(ImgHandle,ImgMaskHandle, False);
        TempBitmap.Handle := ImgHandle;
        TempBitmap.MaskHandle := ImgMaskHandle;
     
        // And copy the TBitmap to your Icon
        SystrayIcon.Icon.Assign(TempBitmap);
        SystrayIcon.Show;
     
      finally
        TempIntfImg.Free;
        TempBitmap.Free;
      end;
    end;

### Subversion

Located under components/trayicon/ on the latest subversion Lazarus. 

### Help, Bug Reporting and Feature Request

Please, post Bug Reports and Feature Requests on the [Lazarus Bugtracker](<http://bugs.freepascal.org/main_page.php>). 

Help requests can be posted on the Lazarus mailling list or on the Lazarus [Forum](<http://www.lazarus.freepascal.org/modules.php?op=modload&name=PNphpBB2&file=index>). 

### Change Log

  1. 17/01/2006 - Available as a preview on the Lazarus subversion. Still under heavy construction, however. 
  2. 24/01/2006 - Stable under win32, gnome and gtk1, but still waiting for gtk2 support. Lazarus 0.9.12 was release with this version. 
  3. 17/02/2006 - Added support for gtk2 on subversion. 
  4. July 2008 - Implments support for Qt 4 
  5. July 2008 - Implements support for Carbon through [PasCocoa](<PasCocoa.md> "PasCocoa")



### Technical Details

A difficulty on the development of this component was the many differences on the system tray implementation on various OSes and even Window Managers on Linux. To solve this, the component tries to implement the minimal set of features common to all target platforms. Below is a list of the features implemented on each platform: 

**Windows** \- Multiple system tray icons per application are supported. The image of the icon can be altered using a HICON handle. Events to the icon are sent via a special message on the user reserved space of messages (>= WM_USER) to the Window which owns the Icon. No paint events are sent to the Window. 

[![Note-icon.png](https://wiki.freepascal.org/images/b/be/Note-icon.png)](</File:Note-icon.png>)

**Note:** for some odd reason the environment by default does not support WM_USER+ messages, you will need to add "-dPassWin32MessagesToLCL" (without quotations) to support the messaging code. The steps are, click Tools -> Configure "Build Lazarus"..., and add that compiler option to "Options". If you have any existing options, they are "space" delimited.

**Linux (Gnome, KDE, IceWM, etc)** \- Multiple system tray icons per application are supported. The image of the icon is actually a very small Window, and can be painted and receive events just like any other TForm descendant. 

**Linux (WindowMaker, Openbox, etc)** \- Does not support system tray icons out-of-the-box. However, There are at least two softwares that provides support for it: [Docker](<http://icculus.org/openbox/2/docker/>) and [WMSystray](<http://freshmeat.net/projects/wmsystray/>)

**Mac OS X** \- TTrayIcon support is implemented using the menu bar extras. Unfortunatelly the API to use menu bar extras is only available in Cocoa and not in Carbon, so we use the stable PasCocoa bindings in the Carbon interface to support menu bar extras even in older FPC compilers and in the Cocoa interface we will use the more modern Objective Pascal syntax. 

[![Mn menubaritems.jpg](https://wiki.freepascal.org/images/9/97/Mn_menubaritems.jpg)](</File:Mn_menubaritems.jpg>)

To read more about menu bar extras: 

  1. <http://developer.apple.com/documentation/UserExperience/Conceptual/OSXHIGuidelines/XHIGMenus/chapter_16_section_6.html>
  2. <http://developer.apple.com/mac/library/documentation/UserExperience/Conceptual/AppleHIGuidelines/XHIGMenus/XHIGMenus.html>
  3. <http://en.wikipedia.org/wiki/Menu_extra>



With this in mind an approach which supports all Platforms was created: 

  * Painting is done via a TIcon object. (Required by Windows) 



The following extra features are already available or will be, but they won´t work on all platforms. 

  * OnPaint event and Canvas property to draw the icon freely. Won´t work on Windows. 



### External Links

  * <http://www.codeproject.com/shell/ctrayiconposition.asp> \- Code and theory to find the tray icon position under Windows 
  * [http://cvs.gnome.org/viewcvs/gtk%2B/gtk/gtkstatusicon.c?rev=1.23&view=markup](<http://cvs.gnome.org/viewcvs/gtk%2B/gtk/gtkstatusicon.c?rev=1.23&view=markup>) \- Gtk2 code that implements gtkstatusicon 
  * <http://pasmontray.sourceforge.net> \- Open source program that uses TrayIcon to display CPU and memory utilisation in the system tray.

---

_Source: [https://wiki.freepascal.org/TrayIcon](https://web.archive.org/web/20170720132019/https://wiki.freepascal.org/TrayIcon)_
