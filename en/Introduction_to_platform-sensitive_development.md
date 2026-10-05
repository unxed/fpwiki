# Introduction to platform-sensitive development

│ **English (en)** │    
****

[![Comparison of platform-independent with platform-sensitive applications on Mac Leopard. All shown applications are platform-independent, but only the programs in the right half of the screen are platform-sensitive.](https://wiki.freepascal.org/images/6/66/platform-independent_vs._platform-sensitive_applications.png)](</File:platform-independent_vs._platform-sensitive_applications.png>)

[](</File:platform-independent_vs._platform-sensitive_applications.png> "Enlarge")

Comparison of platform-independent with platform-sensitive applications on Mac Leopard. All shown applications are platform-independent, but only the programs in the right half of the screen are platform-sensitive. Note that the platform-insensitive X Window programs in the left half of the screen have an alien look. They tend to confuse the users with atypical behaviour and ambiguous elements like a double menu bar.

**Platform-sensitive development** is the next step in [cross-platform development](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide"). It extends the process of porting an application by implementing platform-specific functions and user-interface style guides. 

Thanks to the "[Write Once - Compile Anywhere](<Write_once_compile_anywhere.md> "Write once compile anywhere")" principle of Lazarus and Free Pascal it is very easy to develop a program that runs out of the box on multiple platforms. To make a finished application that adheres to the respective GUI style guidelines (and even the requirements of app stores) warrants additional steps, however. 

Platform-sensitive development is more than adherence to user-interface style guides. It also addresses aspects that are as different as memory management, file handling and interprocess communication. 

This article provides the reader with essential information and hints for platform-sensitive development. 

## Contents

  * 1 Prelude
  * 2 Platform-specific hints
    * 2.1 Android
      * 2.1.1 See also
      * 2.1.2 External Links
    * 2.2 FreeBSD
      * 2.2.1 See also
      * 2.2.2 External links
    * 2.3 iOS
      * 2.3.1 See also
      * 2.3.2 External Links
    * 2.4 Linux
      * 2.4.1 See also
      * 2.4.2 External links
    * 2.5 macOS
      * 2.5.1 See also
      * 2.5.2 External Links
    * 2.6 Windows
      * 2.6.1 See also
      * 2.6.2 External links
  * 3 See also



## Prelude

[![Stock-dialog-warning.svg](https://upload.wikimedia.org/wikipedia/commons/thumb/b/b3/Stock-dialog-warning.svg/50px-Stock-dialog-warning.svg.png)](</File:Stock-dialog-warning.svg>)

This article applies to [platform-sensitive development](</Category:Platform-sensitive_development> "Category:Platform-sensitive development") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

Most adaptations for specific target platforms are performed automatically by the [LCL](<LCL.md> "LCL"), [FCL](<FCL.md> "FCL"), [RTL](<RTL.md> "RTL") and the respective [widgetsets](<Widgetset.md> "Widgetset"). Often, only minor modifications are needed to create a finished application. Using conditional compilation by means of [compiler directives](</Category:Compiler_directives> "Category:Compiler directives") helps to keep your code portable. 

The following example demonstrates this technique. The procedure `AdaptMenus` adjusts the display of the menu bar according to the target platform. This is necessary as the positions of the "About" and "Preferences" items are in different locations in macOS than they are in Windows, Linux or FreeBSD. Additionally, the menu shortcuts are invoked by the command key on macOS (in the LCL referred to as Meta key), while they are triggered with the control key on most other operating systems. 

The main menu of the form contains some platform-specific items, `WinAboutItem` (it opens an info box) and `WinPreferencesItem` (for program settings). They are intended for use on non-macOS operating systems. On the other side, it contains an Apple Menu for macOS. Depending from the target platform, the code hides unneeded entries (and corresponding dividers) and shows those that are expected by the users of the respective platform. 
    
    
    uses
      LCLType;
    
    procedure AdaptMenus;
    { Adapts Menus and Shortcuts to the interface style guidelines
      of the respective operating system }
    var
      modifierKey: TShiftState;
    begin
      {$IFDEF Darwin}
      MainForm.WinAboutItem.Visible := False;
      MainForm.Divider_5_1.Visible := False;
      MainForm.Divider_2_2.Visible := False;
      MainForm.WinPreferencesItem.Visible := False;
      MainForm.AppleMenu.Visible := True;
      {$ELSE}
      MainForm.WinAboutItem.Visible := True;
      MainForm.Divider_5_1.Visible := True;
      MainForm.Divider_2_2.Visible := True;
      MainForm.WinPreferencesItem.Visible := True;
      MainForm.AppleMenu.Visible := False;
      {$ENDIF}
      MainForm.NewMenuItem.ShortCut := ShortCut(VK_N, ssModifier); //ssModifier equals ssMeta on Mac, ssCtrl on other platforms
      MainForm.OpenMenuItem.ShortCut := ShortCut(VK_O, ssModifier);
      MainForm.CloseMenuItem.ShortCut := ShortCut(VK_W, ssModifier);
      MainForm.SaveItem.ShortCut := ShortCut(VK_S, ssModifier);
      MainForm.PrintItem.ShortCut := ShortCut(VK_P, ssModifier);
      MainForm.QuitMenuItem.ShortCut := ShortCut(VK_Q, ssModifier);
      MainForm.UndoMenuItem.ShortCut := ShortCut(VK_Z, ssModifier);
      MainForm.CutMenuItem.ShortCut := ShortCut(VK_X, ssModifier);
      MainForm.CopyMenuItem.ShortCut := ShortCut(VK_C, ssModifier);
      MainForm.PasteMenuItem.ShortCut := ShortCut(VK_V, ssModifier);
      MainForm.SelectAllMenuItem.ShortCut := ShortCut(VK_A, ssModifier);
    end;
    

In older Lazarus versions (before 1.0) ssModifier is not supported. The following code is compatible with Lazarus 0.9.x, albeit more complex than the example above: 
    
    
    uses
      LCLType;
    
    procedure AdaptMenus;
    { Adapts Menus and Shortcuts to the interface style guidelines
      of the respective operating system }
    var
      modifierKey: TShiftState;
    begin
      {$IFDEF LCLcarbon}
      modifierKey := [ssMeta];
      MainForm.WinAboutItem.Visible := False;
      MainForm.Divider_5_1.Visible := False;
      MainForm.Divider_2_2.Visible := False;
      MainForm.WinPreferencesItem.Visible := False;
      MainForm.AppleMenu.Visible := True;
      {$ELSE}
      modifierKey := [ssCtrl];
      MainForm.WinAboutItem.Visible := True;
      MainForm.Divider_5_1.Visible := True;
      MainForm.Divider_2_2.Visible := True;
      MainForm.WinPreferencesItem.Visible := True;
      MainForm.AppleMenu.Visible := False;
      {$ENDIF}
      MainForm.NewMenuItem.ShortCut := ShortCut(VK_N, modifierKey);
      MainForm.OpenMenuItem.ShortCut := ShortCut(VK_O, modifierKey);
      MainForm.CloseMenuItem.ShortCut := ShortCut(VK_W, modifierKey);
      MainForm.SaveItem.ShortCut := ShortCut(VK_S, modifierKey);
      MainForm.PrintItem.ShortCut := ShortCut(VK_P, modifierKey);
      MainForm.QuitMenuItem.ShortCut := ShortCut(VK_Q, modifierKey);
      MainForm.UndoMenuItem.ShortCut := ShortCut(VK_Z, modifierKey);
      MainForm.CutMenuItem.ShortCut := ShortCut(VK_X, modifierKey);
      MainForm.CopyMenuItem.ShortCut := ShortCut(VK_C, modifierKey);
      MainForm.PasteMenuItem.ShortCut := ShortCut(VK_V, modifierKey);
      MainForm.SelectAllMenuItem.ShortCut := ShortCut(VK_A, modifierKey);
    end;
    

[![Note-icon.png](https://wiki.freepascal.org/images/b/be/Note-icon.png)](</File:Note-icon.png>)

**Note:** To see if LCL functionality is supported on your target widgetset, you can use the Restriction Browser (menu View/Restriction Browser)

## Platform-specific hints

### Android

Android is an atypical Linux variant, therefore many considerations for usual Linux distributions do not apply. Furthermore, development for Android is faced by the challenge that the software landscape is fragmented in a large field of different system versions and by a considerable heterogeneity of hardware among supported devices. 

#### See also

  * [Android Interface/Native Android GUI](<Android_Interface/Native_Android_GUI.md> "Android Interface/Native Android GUI")
  * [Android Portal](<Portal_Android.md> "Portal:Android")



#### External Links

  * [Android Design](<http://developer.android.com/design/index.html>)
  * [Android User Interface Guidelines](<http://developer.android.com/guide/practices/ui_guidelines/index.html>)



### FreeBSD

[FreeBSD](<https://www.freebsd.org>) is an operating system for a variety of platforms which focuses on features, speed, and stability. It is derived from BSD, the version of UNIX® developed at the University of California, Berkeley. It is developed and maintained by a large community. It is used to power modern servers, desktops, and embedded platforms. A large community has continually developed it for more than thirty years. Its advanced networking, security, and storage features have made FreeBSD the platform of choice for many of the busiest web sites and most pervasive embedded networking and storage devices. 

#### See also

  * [FreeBSD Portal](<Portal_FreeBSD.md> "Portal:FreeBSD") for comprehensive lists of links for development techniques, programming hints and tips for creating FreeBSD programs.



#### External links

  * [FreeBSD: Developers' Handbook](<https://www.freebsd.org/doc/en_US.ISO8859-1/books/developers-handbook/>).
  * [FreeBSD: Porters Handbook](<https://www.freebsd.org/doc/en_US.ISO8859-1/books/porters-handbook/>).
  * [FreeBSD: Handbook](<https://www.freebsd.org/doc/en_US.ISO8859-1/books/handbook/>).



### iOS

iOS is a pruned variant of macOS for mobile devices. Recent additions to the Lazarus evironment help to develop applications with [Free Pascal](<Free_Pascal.md> "Free Pascal") for iOS now. The development for iOS differs in some sense from that for other platforms, however thanks to the [iOS Designer](<iOS_Designer.md> "iOS Designer") it is possible to graphically design the GUI. For submission to Apple's App Store it is mandatory to implement the iOS Human Interface Guidelines. 

#### See also

  * [iOS Portal](<Portal_iOS.md> "Portal:iOS")



#### External Links

  * [Apple iOS Human Interface Guidelines](<http://developer.apple.com/library/ios/#documentation/UserExperience/Conceptual/MobileHIG/Introduction/Introduction.html>)
  * [Apple Style Guide](<https://help.apple.com/asg/mac/>)
  * [App Distribution Guide](<https://developer.apple.com/library/mac/#documentation/IDEs/Conceptual/AppDistributionGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40012582>)]
  * [High Resolution Guidelines for macOS](<https://developer.apple.com/library/mac/#documentation/GraphicsAnimation/Conceptual/HighResolutionOSX/Introduction/Introduction.html#//apple_ref/doc/uid/TP40012302>)



### Linux

Linux is less standardized than other operating systems. This is a consequence of both heterogeneity of distributions and the fact that there is no "standard" desktop environment for Linux. GNOME, KDE, LXDE and other desktop environments differ in their recommendations for GUI design. 

#### See also

  * [Linux portal](<Portal_Linux.md> "Portal:Linux")



#### External links

  * [GNOME human interface guidelines](<https://developer.gnome.org/hig-book/stable/>)
  * [KDE human interface guidelines](<http://techbase.kde.org/Projects/Usability/HIG>)
  * [Canonical Ubuntu App design guides](<http://design.ubuntu.com/apps>)
  * [LXDE Design principles for software](<http://wiki.lxde.org/en/Design_Principles>)



### macOS

Among all operating systems the GUI of the Mac operating system relies on the richest tradition. It dates back to classical Mac OS from 1984 and its less successful predecessors Lisa OS (1983), Xerox Star (1981) and Xerox Alto (1973). Many regard the Mac operating system GUI as delivering the best user experience of all platforms. This is balanced by a more complex development process, however. 

Compared with other operating systems many issues have been differently implemented on Mac computers. This covers design and location of the menu bar, the position of some standard menus and the style of buttons and icons. Also Mac computer non-GUI features differ, e.g. the architecture of programs, the location of preferences files and interprocess communication. 

Many of these issues are automatically taken care of by the [Carbon](<Carbon_Interface.md> "Carbon Interface") (32 bit only and now eliminated by Apple in the macOS 10.15 Catalina release in late 2019) and [Cocoa](<Cocoa_Interface.md> "Cocoa Interface") (64 bit only) widgetsets, so that it is easy to develop a simple program for macOS, often without any change to the source code. Some fine-tuning is still necessary to develop a finished Mac application, however. In particular, see [Apple-specific UI elements](<Apple-specific_UI_elements.md> "Apple-specific UI elements"). 

#### See also

  * [Mac Portal](<Portal_Mac.md> "Portal:Mac") for comprehensive lists of links for development techniques, programming hints and tips for creating macOS applications.



#### External Links

  * [macOS Human Interface Guidelines](<https://developer.apple.com/library/mac/#documentation/UserExperience/Conceptual/AppleHIGuidelines/Intro/Intro.html#//apple_ref/doc/uid/20000957>)
  * [Apple Style Guide](<https://help.apple.com/asg/mac/>)
  * [App Distribution Guide](<https://developer.apple.com/library/mac/#documentation/IDEs/Conceptual/AppDistributionGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40012582>)]
  * [High Resolution Guidelines for macOS](<https://developer.apple.com/library/mac/#documentation/GraphicsAnimation/Conceptual/HighResolutionOSX/Introduction/Introduction.html#//apple_ref/doc/uid/TP40012302>)



### Windows

Windows is one of the oldest platforms supported by Lazarus. Therefore, most applications generated by the IDE are automatically in good agreement with the requirements of usability engineering. However, the spectrum of Windows implementations is very wide today, covering mobile operating systems like Windows CE or Windows mobile, desktop environments like Windows 2000, Windows 7 and Windows 10, plus server platforms. Therefore, it may be advisable to fine-tune applications for special environments and their GUI guidelines. 

#### See also

  * [Multiplatform Programming Guide: Windows specific issues](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")
  * [Windows Portal](<Portal_Windows.md> "Portal:Windows")



#### External links

  * [Windows user experience interaction guidelines](<http://msdn.microsoft.com/en-us/library/windows/desktop/aa511258.aspx>)



## See also

  * [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")
  * [The LCL in various platforms](<The_LCL_in_various_platforms.md> "The LCL in various platforms")

---

_Source: [https://wiki.freepascal.org/Introduction_to_platform-sensitive_development](https://web.archive.org/web/20230128005815/https://wiki.freepascal.org/Introduction_to_platform-sensitive_development)_
