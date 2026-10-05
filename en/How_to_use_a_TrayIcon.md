# How to use a TrayIcon

│ **English (en)** │

## Contents

  * 1 About
  * 2 Linux may not display the Icon
    * 2.1 Force traditional SysTray GTK2 ONLY
    * 2.2 UnityWSCtrls.GlobalUseAppInd GTK2 ONLY
    * 2.3 AppIndicator Libraries
    * 2.4 gnome-shell-extension-appindicator
    * 2.5 Checking before invoking TrayIcon
    * 2.6 On GTK3
    * 2.7 QT5 on Linux
    * 2.8 Showing the Icon before establishing its Menu
  * 3 Documentation
    * 3.1 Methods
    * 3.2 Properties
    * 3.3 Events
  * 4 Authors
  * 5 License
  * 6 Download
  * 7 Example 1 - Using TIcon
  * 8 Example 2 - Creating the icon with TLazIntfImage
  * 9 Subversion
  * 10 Help, Bug Reporting and Feature Request
  * 11 Change Log
  * 12 Technical Details
  * 13 External Links



### About

**[TTrayIcon](<TTrayIcon.md> "TTrayIcon")** is a multiplatform System Tray component. You can find TrayIcon on the [Additional tab](<Additional_tab.md> "Additional tab") of the [Component Palette](<Component_Palette.md> "Component Palette") (0.9.23+). 

It works reliably on Windows, MacOS and Linux (and similar) with all desktops except the Gnome Desktop. It can be made work with Gnome but perhaps that's despite the Gnome Developers efforts. 

In use, drag the TrayIcon onto your form, assign an icon to it, assign a popup menu (with at least one menu item) to it and call its Show method 

To start quickly, please read [the demonstration program](<TrayIcon.md> "TrayIcon"). 

### Linux may not display the Icon

Some Linux systems will have trouble displaying the GTK TrayIcon, in general this relates to a move away from the System Tray model that Lazarus TrayIcon is based on. This problem is a particular issue with Gnome but some other Desktops may also suffer. There are now two ways the GTK2 SysTray component tries to show its icon, the traditional System Tray and an AppIndicator library. 

Lazarus 2.2 (or fixes, main), now looks for an AppIndicator first and uses that if it it finds one. Only if that fails will it try for a traditional System Tray. This has several consequences - 

  * The AppIndicator is intentionally a much simpler user interface. One click and it shows up a Popup Menu. **No double click, no drag, no right click, no hint.** If your app depends on one of those secondary responses, and you don't have a Gnome Desktop, you may try to force Lazarus to use the traditional SysTray, see below.


  * An AppIndicator library must be installed, until recently, that was libappindicator3 but now many distros are moving to libayatana. A number of prominent distributions appear to be pre installing a AppIndicator but you cannot assume.


  * On Gnome desktops, an additional item is needed, gnome-shell-extension-appindicator must be installed and **enabled**. See below.


  * Developers of new (Linux) applications are advised to design their program around TrayIcon using only a popUpMenu !



Mageia Enlightenment also requires an AppIndicator library and enabling System Tray in Settings->Modules and then adding that System Tray to a Shelf (all terms familiar to Enlightenment users). Unfortunately, even that gets 'an' icon, not the one you carefully select for your application. 

#### Force traditional SysTray GTK2 ONLY

This is a purely GTK2 behavior, not for GTK3, QT5 nor any non Linux widget set. 

As some systems do still have a functional traditional SysTray and it does offer a richer interface, you may wish to still use it. As of Lazarus 2.0.6, GTK2 LCL apps will look at an environment variable, LAZUSEAPPIND before deciding to use the AppIndicator, if its set to NO then the traditional SysTray will be used but no error is reported (or even detectable) if its not available. If set to INFO (or anything else than NO) it will report, to the console, what it is trying to do. (The YES option became irrelevant with Lazarus 2.2.) 
    
    
    LAZUSEAPPIND=NO myapp <enter>
    

#### UnityWSCtrls.GlobalUseAppInd GTK2 ONLY

Lazarus version 3.0 and later have an additional way to predetermine which of the two TrayIcon models will be used. This has been made necessary because of the growing inconsistency of TrayIcon models in Linux Distributions. For example, at present, apparently, no 32bit Linux will work with AppIndicator model and any Gnome based distro will only work with the AppIndicator. You can set UnityWSCtrls.GlobalUseAppInd depending on some checks about the system or config file setting in the startup code of your app. 

To use UnityWSCtrls.GlobalUseAppInd you must include 'UnityCtrls' in the uses section of a unit, it can be set to one of the UnityWSCtrls.TUseAppIndInstruction values, (UseAppIndYes, UseAppIndNo, UseAppIndAuto). It MUST be set before your TrayIcon is created, so, put your code in the project's LPR file ahead of Application.CreateForm(TMainForm, MainForm) line, or, if you prefer, manually create your TrayIcon later in the startup process. 

For example, this might appear in your LPR file - 
    
    
    uses
        .....
        {$ifdef LCLGTK2}
        , unitywsctrls
        {$endif};
    
    {$R *.res}
    
    
    // ----------------------------------------------------------------------------
    //   This about reading the command line and env for instructions re TrayIcon
    //   It applies to ONLY gtk2 and Lazarus >= 3.0.0 !
    // ----------------------------------------------------------------------------
    {$ifdef LCLGTK2}
     function GetUseAppInd() : UnityWSCtrls.TUseAppIndInstruction;
     var
        EnvVar : string;
     begin
     Result := UnityWSCtrls.GlobalUseAppInd;
     if Application.HasOption('useappind') then begin              // Command line
        if Application.GetOptionValue('useappind') = 'yes' then    // if not set, leave it alone.
            Result := UseAppIndYes
        else if Application.GetOptionValue('useappind') = 'no' then
            Result := UseAppIndNo;                            // Anything other than yes or no is ignored
     end else begin
        EnvVar := Application.EnvironmentVariable['LAZUSEAPPIND'];   // EnvironmentVariable
        if EnvVar = 'YES' then
            Result := UseAppIndYes
        else if EnvVar = 'NO' then
            Result := UseAppIndNo;
        end;
    end;
    {$endif}
    
    begin
        Application.Scaled := True;
        Application.Title := 'tomboy-ng';
        RequireDerivedFormResource:=True;
        Application.Initialize;
        {$ifdef LCLGTK2 }
            {$ifdef CPUi386}             // Note: unless Ayatana fix their problem, no option for Gnome users
                                        // https://github.com/AyatanaIndicators/libayatana-appindicator/issues/76
            UnityWSCtrls.GlobalUseAppInd := UnityWSCtrls.UseAppIndNo;     // 32bit must be a 'no'.
            {$endif}
            UnityWSCtrls.GlobalUseAppInd := GetUseAppInd();   // Set before creating TrayIcon
        {$endif}
        Application.CreateForm(TMainForm, MainForm);    
        ....
    

You most certainly do not have to set UnityWSCtrls.GlobalUseAppInd, in many case, its default settings will work. But it may not work for everyone, it does give you, the programmer, some control over the situation. 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** At present, late 2023, every 32bit Linux I tested would generate an Access Violation when calling a GTK2 TrayIcon.Show setup to use the AppIndicator. So, better wrap it with a Try .. Except on E: EAccessViolation do... 

#### AppIndicator Libraries

There are currently two possible AppIndicator libraries, Canonical's LibAppIndicator3-1 or libayatana-appindicator3-1, some systems have both, Bullseye has only the Ayatana one. Lazarus trunk supports Ayatana after 22 May 2021, r65122 and in 2.2.0. See External Links, below about the shift to Ayatana. 

One or the other library can generally be installed from your distribution's normal package library. However, it is packaged with a number of different names. Sigh ... 

  * libappindicator3 - Slackware, NetBSD
  * libappindicator3-1 - openSUSE and earlier Debian [-based]
  * libappindicator-gtk3 - Arch, ALT, CentOS and Fedora, RedHat
  * libappindicator3_1 - all Mandrake/Mandriva derivatives -- Mageia, OpenMandriva, PCLinuxOS, Rosa
  * libayatana-appindicator3-1 - Debian Bullseye and most other Linuxs that offer Ayatana.



#### gnome-shell-extension-appindicator

This plugin is necessary for all current Gnome based systems. It is pre installed on some Gnome Desktops and only works with the an AppIndicator (not traditional SysTray). It is KNSI inspired so no fancy right clicks allowed, just assign it a menu. 

On Fedora, you can do this (note you don't need gnome-tweaks on Fedora 34) - 
    
    
    sudo dnf install libappindicator-gtk3  gnome-shell-extension-appindicator gnome-tweaks [enter]
    

Once installed, you need to restart the Desktop, just logout and back in. Then you must 'enable' the new plugin. On Fedora 33 thats easy, use the gnome-tweaks command, Extensions, enable "Kstatusnotifieritem/appindicator support". On Fedora 34, they have made it a bit harder by removing the Extensions section from Gnome Tweaks (too many happy users I expect). So, with Fedora 34, use the gnome-extensions command, it does not have a gui so issue the following command - 
    
    
     gnome-extensions enable appindicatorsupport@rgcjonas.gmail.com [enter]
    

Replace 'enable' with 'info' to get some diagnostic information. 

Note, three steps, install, restart desktop, enable plugin ! 

**Debian Bullseye** appears to arrive with the Ayatana library pre installed but the Gnome version still requires gnome-shell-extension-appindicator, install as above. 

Your application can enable the extension (after a desktop restart), if you are feeling brave see <https://github.com/tomboy-notes/tomboy-ng/blob/master/source/mainunit.pas>

**Note :** _Adding the plugin as a dependency of your application might seem attractive but do not do so ! On desktops other than Gnome, it will pull in, as dependencies, the full Gnome desktop. Your end users will not thank you._

#### Checking before invoking TrayIcon

Because your app may show the user only its TrayIcon and if that's not visible the user cannot interact, you may like to test to see if its going to work. Sadly a test previously recommend here has now proved to be unreliable (in Ubuntu 21.10) and its been removed from Lazarus. 

<https://github.com/tomboy-notes/tomboy-ng/blob/master/source/mainunit.pas> function TMainForm.CheckGnomeExtras() has code that asks the Gnome plugin manager if if the plugin is installed and enabled. And it enables it if necessary. And we really should not have to do things like that ! 

#### On GTK3

Trunk from October 2019 has a working GTK3 TrayIcon based, again, on the AppIndicator. The advice above relating to installing LibAppindicator3 applies to GTK3 as well. As the only way you can get a TrayIcon under GTK3 is the an AppIndicator, you can only use it to display a popup menu, don't expect to get a working OnClick event. 

#### QT5 on Linux

Some linux systems that won't display the GTK Tray Icon will display the Qt5 version, but only if they use XOrg. Any system using wayland apparently has other has issues with Qt5. Most Gnome desktops and some others use wayland by default but users can choose to use XOrg instead at logon time. 

#### Showing the Icon before establishing its Menu

It has long been known that the System Tray Icon, on Linux at least, will not show if its .show function is called before it's popup menu has been assigned. No error shown, just no Icon to be seen. 

However, in 2023 with the release of Ubuntu using Gnome Desktop 44.0, a new aspect appeared. If the popup menu assigned has, itself, no menu items when .show is called, the menu is never shown even after menu items are added. In the case of gtk2, we do get an error message (complete with spelling mistake) but we see no menu. With Qt5, no error message, we get a menu but in the wrong part of the screen, and, after a being displayed a few times, we loose even that. We do not see this issue on any other OS or even any other Linux Desktop. How very Gnome-ish ! 

So, in summary, if you are managing menu items dynamically, make sure you have at least one menu item added to the popup menu and that popup menu assigned to the TrayIcon BEFORE calling TrayIcon.show. 

note: chapter about 32bit carbon removed as irreverent. 

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

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** Does not work on win32.

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** Following events not available on Linux if using the LibAppIndicator3 model, its menu only.

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



![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** Windows: [Ozz Nixon](</User:Ozznixon> "User:Ozznixon")

### License

Modified LGPL. 

### Download

Status: Stable 

Can be located at Lazarus 0.9.22 or inferior at the directory: lazarus/components/trayicon 

And on Lazarus 0.9.23 or superior it is automatically installed with LCL 

### Example 1 - Using TIcon

As of Lazarus 0.9.26 TIcon has been fully implemented and it is no longer necessary to load the icon from a resource file on Windows. The icon can be loaded in the IDE or with usual code. 

Go to the Additional tab of components, and add a TTrayIcon to your form. Then change its **Name** property to SystrayIcon 

Next add a button to the form. Double click the button and add this code to it: 
    
    
    procedure MyForm.Button1Click(Sender: TObject);
    begin
      SystrayIcon.Icon.LoadFromFile('/path_to_icon/icon.ico');
      SystrayIcon.Hint := 'my tool tip';
    
      SystrayIcon.PopUpMenu := MyPopUpMenu;
    
      SystrayIcon.Show;
    end;
    

### Example 2 - Creating the icon with TLazIntfImage

You can use TLazIntfImage to draw your icon quickly, as in the example code below: 
    
    
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

Help requests can be posted on the Lazarus mailing list or on the Lazarus [Forum](<http://www.lazarus.freepascal.org/modules.php?op=modload&name=PNphpBB2&file=index>). 

### Change Log

  1. 17/01/2006 - Available as a preview on the Lazarus subversion. Still under heavy construction, however.
  2. 24/01/2006 - Stable under win32, gnome and gtk1, but still waiting for gtk2 support. Lazarus 0.9.12 was release with this version.
  3. 17/02/2006 - Added support for gtk2 on subversion.
  4. July 2008 - Implements support for Qt 4
  5. July 2008 - Implements support for Carbon through [PasCocoa](<PasCocoa.md> "PasCocoa")



### Technical Details

A difficulty on the development of this component was the many differences on the system tray implementation on various OSes and even Window Managers on Linux. To solve this, the component tries to implement the minimal set of features common to all target platforms. Below is a list of the features implemented on each platform: 

**Windows** \- Multiple system tray icons per application are supported. The image of the icon can be altered using a HICON handle. Events to the icon are sent via a special message on the user reserved space of messages (>= WM_USER) to the Window which owns the Icon. No paint events are sent to the Window. 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** for some odd reason the environment by default does not support WM_USER+ messages, you will need to add "-dPassWin32MessagesToLCL" (without quotations) to support the messaging code. The steps are, click Tools -> Configure "Build Lazarus"..., and add that compiler option to "Options". If you have any existing options, they are "space" delimited.

**Linux (Gnome, KDE, IceWM, etc)** \- Multiple system tray icons per application are supported. The image of the icon is actually a very small Window, and can be painted and receive events just like any other TForm descendant. Note that this does not apply to TrayIcons that are using the OS's AppIndicator interface. Certainly not Gnome, maybe not of desktops as time goes by. 

**Linux (WindowMaker, Openbox, etc)** \- Does not support system tray icons out-of-the-box. However, There are at least two softwares that provides support for it: [Docker](<http://icculus.org/openbox/2/docker/>) and [WMSystray](<http://freshmeat.net/projects/wmsystray/>)

**macOS** \- TTrayIcon support is implemented using the menu bar extras. Unfortunately the API to use menu bar extras is only available in Cocoa and not in Carbon, so we use the stable PasCocoa bindings in the Carbon interface to support menu bar extras even in older FPC compilers and in the Cocoa interface we will use the more modern Objective Pascal syntax. 

[![Mn menubaritems.jpg](https://wiki.freepascal.org/images/9/97/Mn_menubaritems.jpg)](</File:Mn_menubaritems.jpg>)

To read more about menu bar extras: 

  1. <https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/StatusBar/StatusBar.html>
  2. <https://developer.apple.com/design/human-interface-guidelines/macos/extensions/menu-bar-extras/>
  3. <https://developer.apple.com/design/human-interface-guidelines/macos/menus/dock-menus/>
  4. <https://developer.apple.com/design/human-interface-guidelines/macos/system-capabilities/dock/>
  5. <https://developer.apple.com/design/human-interface-guidelines/macos/menus/menu-anatomy/>
  6. <http://en.wikipedia.org/wiki/Menu_extra>



With this in mind an approach which supports all Platforms was created: 

  * Painting is done via a TIcon object. (Required by Windows)



The following extra features are already available or will be, but they won´t work on all platforms. 

  * OnPaint event and Canvas property to draw the icon freely. Won´t work on Windows.



### External Links

  * <http://www.codeproject.com/shell/ctrayiconposition.asp> \- Code and theory to find the tray icon position under Windows
  * [http://cvs.gnome.org/viewcvs/gtk%2B/gtk/gtkstatusicon.c?rev=1.23&view=markup](<http://cvs.gnome.org/viewcvs/gtk%2B/gtk/gtkstatusicon.c?rev=1.23&view=markup>) \- Gtk2 code that implements gtkstatusicon
  * <http://pasmontray.sourceforge.net> \- Open source program that uses TrayIcon to display CPU and memory utilisation in the system tray.
  * [Background to Debian's switch from libappindicator to Ayatana](<https://lists.debian.org/debian-devel/2018/03/msg00506.html>)
  * [Ayatana App Indicator](<https://github.com/AyatanaIndicators/libayatana-appindicator>)

---

_Source: [https://wiki.freepascal.org/How_to_use_a_TrayIcon](https://web.archive.org/web/20250125080848/https://wiki.freepascal.org/How_to_use_a_TrayIcon)_
