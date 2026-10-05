# Free Pascal Window Manager

│ **English (en)** │  **[español (es)](</Free_Pascal_Window_Manager/es> "Free Pascal Window Manager/es")** │    
****

## Contents

  * 1 History
  * 2 Guidelines
  * 3 Subversion
  * 4 Tutorials
    * 4.1 Installing Linux in a virtual machine inside Windows
    * 4.2 Configuring the login screen in Linux
      * 4.2.1 Adding XTerm as an option in the login screen
  * 5 Issues
  * 6 Lap-Tupniquim project
  * 7 External links



## History

[![](https://wiki.freepascal.org/images/e/ed/screenshot-fpwm.png)](</File:screenshot-fpwm.png>)

[](</File:screenshot-fpwm.png> "Enlarge")

fpwm running in Xnest (circa 2008)

The Free Pascal Window Manager was started in 2006 by Andrew Haines, Daniel Franzini and Felipe Monteiro de Carvalho. The main motivation for this project was to give a greater visibility for applications written in Free Pascal, which face difficulties to find adequate deployment in Linux because the Linux distributions prefer to deploy the standard programs which are packaged by each Window Manager, such as KDE or Gnome. FPWM should come as a simple, but easy to use Window Manager which also deploys a full set of useful applications. This set of applications is developed in the [Free Pascal Application Suite](<Free_Pascal_Application_Suite.md> "Free Pascal Application Suite") project. Unfortunately, due to the lack of free time, lack of enough knowledge of X11 and the general difficulties in finding information about how to write a window manager, the project soon stalled. 

In the second half of 2010 the University of São Paulo through Prof. Dr. Pedro Luiz Pizzigatti Corrêa has given its support for the project by making a team of 4 students available to help in its development for 1 semester as part of the engineering course in the university. This counts as executing a subject with 2 credits. In this project, with most of the code and fixes coming from Felipe, it was achieved the total conversion of the wm2 window manager to Pascal, making it fully functional by the beginning of 2011. 

The next step after achieving a functional window manager is modifying the windows to have a more normal layout. 

## Guidelines

FPWM should be simples, light-weight and easy to use. It should be usable for both tablets / touch-screen devices and for normal desktop computers with a mouse. It should provide a taskbar similar to Windows, as well as icons in the desktop. It should have larger borders and larger icons in its tablet mode, and a more common appearance in its desktop mode. It should provide a system tray icon area and respect the Free Desktop specifications for Window Managers. 

## Subversion
    
    
     svn co <https://fpwm.svn.sourceforge.net/svnroot/fpwm> fpwm
    

## Tutorials

### Installing Linux in a virtual machine inside Windows

  * First download Virtual PC: [Download the full version of Microsoft Virtual PC 2007](<http://www.microsoft.com/downloads/details.aspx?FamilyID=04d26402-3199-48a3-afa2-2dc0b40a73b6&DisplayLang=en>)


  * Tutorial to install Linux in Virtual PC: [Run Linux in Windows](<http://www.pcreview.co.uk/articles/Windows/Run_Linux_in_Windows/>)



### Configuring the login screen in Linux

#### Adding XTerm as an option in the login screen

For debugging purposes one can add the XTerm as a Window Manager in the Linux login screen. 

Then when logging in choose XTerm from the list of window managers. This will open an empty X11 with a single XTerm instance running which you can use to open a real Window Manager, fpwm for example. 

The following terminal commands can be utilized to to start fpwm: 
    
    
    cd /path/to/fpwm
    ./fpwm
    

To add XTerm in the list of choices, one should add the file 99xterm.desktop to a folder which contains one .desktop file per window manager. This file can be obtained from: [http://fpwm.svn.sourceforge.net/viewvc/fpwm/resources/99XTERM.desktop?revision=107&view=markup](<http://fpwm.svn.sourceforge.net/viewvc/fpwm/resources/99XTERM.desktop?revision=107&view=markup>)

And should be similar to this: 
    
    
    [Desktop Entry]
    Encoding=UTF-8
    Name=XTerm
    Comment=XTerm
    TryExec=xterm
    Exec=/usr/share/X11/xdm/Xsession xterm
    Icon=
    Type=Application
    

In Mandriva Linux with GDM (Gnome login manager) the folder to place this file is: /etc/X11/dm/Sessions/ In Mandriva Linux with KDM (KDE login manager) the folder to place this file is: /usr/share/apps/kdm/sessions/ 

## Issues

<<< March 2017 >>>

When compiling (latest snapshot from sourceforge) with Lazarus 1.6 and FPC 3.0.0 it show this error: 

**Compile Proyect, Target: fpwm: Output Code 256, errors: 1 hints:** 4 

  
fpwmrotated.pas(406,34) Error: Illegal type conversion: "ShortString" to "Pchar" 

The other hints are: 

fpwmgeneral.pas(65,17) Hint: Local variable "sAct" does not seem to be initialized 

fpwmgeneral.pas(21,6) Hint: Unit "xlib" not used in fpwmgeneral 

xproto.pas(74,6) Hint: Unit "x" not used in xproto 

xproto.pas(74,9) Hint: Unit "xmd" not used in xproto 

## Lap-Tupniquim project

This Window Manager might be utilized in the brazilian digital school desks if it proves to be good enough for it: 

  * Project Lap-Tupniquim: <http://www.youtube.com/watch?v=GvEv0aWo94Q>



## External links

  * [X11 Book: Xlib Programming Manual](<http://www.niksula.hut.fi/~jkirma/books/xlib.pdf>)
  * [Inter-Client Communication Conventions Manual](<http://tronche.com/gui/x/icccm/>)

---

_Source: [https://wiki.freepascal.org/Free_Pascal_Window_Manager](https://web.archive.org/web/20250324172634/https://wiki.freepascal.org/Free_Pascal_Window_Manager)_
