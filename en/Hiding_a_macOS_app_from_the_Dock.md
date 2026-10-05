# Hiding a macOS app from the Dock

│ **English (en)** │ 

## Contents

  * 1 Overview
  * 2 Agent applications
  * 3 Background only applications
  * 4 See also
  * 5 External links



## Overview

While there is no way to hide an application from the Dock at runtime, this can be done at design-time if the application declares itself to be an agent or a background application. These two application categories and their attributes are explained below. 

## Agent applications

Agent applications do not appear in the Dock or in the Force Quit window; but they _are visible_ to utilities like Activity Monitor, Unix command-line tools (eg top), as well as macOS API functions.. If the agent key exists and is set to YES in the application's .plist file, Launch Services runs the application as an agent. Although they typically run as background applications, they can come to the foreground to present a user interface if desired. A click on a window belonging to an agent application brings that application forward to handle events. 

Agents are often shown in the macOS menu bar, in which case the agent application should provide a StatusBar (TTrayIcon) user interface and hotkeys if it needs a menu-like user interface. 

For example, startlazarus is an agent application, launched by the Lazarus IDE when rebuilt. Similarly, the Dock and Login Window are also two applications that run as agents. 

To declare an application to be an **agent application** , use Xcode to open the application's bundle Info.plist file: 

  * add a property _UIElement_ (or "Application is agent");
  * set the _UIElement_ property's value to True;
  * save the Info.plist file.



[![infoplist agent.png](https://wiki.freepascal.org/images/1/10/infoplist_agent.png)](</File:infoplist_agent.png>)

## Background only applications

A background only application runs only in the background. If background application key exists and is set to YES, Launch Services runs the application in the background only. You can use this key to create faceless background applications. You should also use this key if your application uses higher-level frameworks that connect to the window server, but are not intended to be visible to users. 

As background applications are _never shown_ in the macOS menu bar, you should be launch/terminate the application with another GUI application. 

To declare an application to be a **background application** , use Xcode to open the application's bundle Info.plist file: 

  * add a property _LSBackgroundOnly_ (or "Application is background only");
  * set the _LSBackgroundOnly_ property's value to True;
  * save the Info.plist file.



[![infoplist bkgrndapp.png](https://wiki.freepascal.org/images/9/94/infoplist_bkgrndapp.png)](</File:infoplist_bkgrndapp.png>)

## See also

  * [Application disable resize over Dock](<Application_disable_resize_over_Dock.md> "Application disable resize over Dock")
  * [Bounce Application Icon in Dock](<Bounce_Application_Icon_in_Dock.md> "Bounce Application Icon in Dock")
  * [macOS Application Dock Menu](<macOS_Application_Dock_Menu.md> "macOS Application Dock Menu")
  * [Show Badge on Application Icon in Dock](<Show_Badge_on_Application_Icon_in_Dock.md> "Show Badge on Application Icon in Dock")



## External links

  * [Apple: Technical Note TN2083 - Daemons and Agents](<https://developer.apple.com/library/archive/technotes/tn2083/_index.html#/apple_ref/doc/uid/DTS10003794>)
  * [Apple: Pre Login Agents](<https://developer.apple.com/library/archive/samplecode/PreLoginAgents/Introduction/Intro.html#//apple_ref/doc/uid/DTS10004414>)
  * [Apple: Daemons and Services Programming Guide](<https://developer.apple.com/library/archive/documentation/MacOSX/Conceptual/BPSystemStartup/Chapters/Introduction.html#//apple_ref/doc/uid/10000172i-SW1-SW1>)

---

_Source: [https://wiki.freepascal.org/Hiding_a_macOS_app_from_the_Dock](https://web.archive.org/web/20240914125759/https://wiki.freepascal.org/Hiding_a_macOS_app_from_the_Dock)_
