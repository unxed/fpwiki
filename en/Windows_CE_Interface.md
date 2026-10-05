# Windows CE Interface

[![WinCE Logo.png](https://wiki.freepascal.org/images/8/86/WinCE_Logo.png)](</File:WinCE_Logo.png>)

This article applies to [Windows CE](</Category:WinCE> "Category:WinCE") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

│ **English (en)** │  **[русский (ru)](<../ru/Windows_CE_Interface.md>)** │

## Contents

  * 1 Introduction
  * 2 Setting Up the Windows CE interface
    * 2.1 Using the stable add-on installer (Recommended Method)
    * 2.2 Using the snapshot add-on installer
    * 2.3 Setting up the Windows CE interface manually
    * 2.4 Compiling Windows CE project files with Lazarus IDE
    * 2.5 Debugging Windows CE software on the Lazarus IDE
    * 2.6 Installing and Using the Pocket PC Emulator
      * 2.6.1 Windows Mobile 5.0 Emulator
      * 2.6.2 Windows Mobile 6.5 in Windows Vista or superior
        * 2.6.2.1 Connecting the Windows Mobile 6.5 emulator in Vista or Superior to the computer
        * 2.6.2.2 Setting up a shared folder in the emulator
      * 2.6.3 Links to Emulators and Images
        * 2.6.3.1 Emulators
        * 2.6.3.2 Images
      * 2.6.4 Running an application on the emulator
  * 3 How to add a new control
  * 4 Gallery
    * 4.1 Wireless Orders for Mini Bar Cafe
  * 5 Other Interfaces
    * 5.1 Platform specific Tips
    * 5.2 Interface Development Articles
  * 6 See Also



## Introduction

The Windows CE Interface was started by [Felipe Monteiro de Carvalho](</User:Sekelsenmat> "User:Sekelsenmat") and latter extended by Roozbeh and other contributors. Development of the interface was started in 2006, when the Windows CE FPC compiler was still under development. In 2007 a stable compiler was released, which made it possible to release a WinCE add-on installer for the Lazarus 0.9.24 release. 

Because of the bad experience with code sharing between similar interfaces in the past with the gtk/gtk2 interface, it was decided to start a clean code for WinCE. Because the APIs are very similar, a lot of the code of the WinCE interface is copied from the Win32/64 Interface. 

## Setting Up the Windows CE interface

To set up the Windows CE interface you will need to either use the add-on installer or set up the interface manually. Both options are described below in detail. Using the add-on installer is, of course, much easier. 

### Using the stable add-on installer (Recommended Method)

With the first stable release of Free Pascal for Windows CE the installation of a WinCE development environment was never easier. The step-by-step guide below is all you need to install and configure Lazarus to compile wince applications. 

Step-by-step guide: 

  * Install the latest Lazarus on Windows normally from SourceForge: <http://sourceforge.net/project/showfiles.php?group_id=89339>


  * Download and install the add-on installer (cross-arm-wince) for the same Lazarus version you just downloaded. It's on the same Source Forge Download page.


  * You can now compile arm-wince application from within Lazarus by changing in the compiler options: 
    * _Project - > Project Options -> Compiler Options -> Paths -> LCL Widget Type: wince_ or maybe _Project - > Project Options -> Compiler Options -> Paths -> Select another widget set (Macro LCLWidgetType) --> Add --> Set LCLWidgetType --> Value "wince"_
    * _Project - > Project Options -> Compiler Options -> Config and Target -> Target OS: wince_
    * _Project - > Project Options -> Compiler Options -> Config and Target -> Target CPU Family: arm_


  * To reduce build filesize, in the _Project - > Project Options -> Compiler Options -> Debugging_ section, check _Strip symbols from executable (-Xs)_ and uncheck all the rest.



### Using the snapshot add-on installer

Instead of using the stable release, it's also possible to use the latest development version of the Windows CE interface through the daily snapshot. It's untested, but may be helpful if you need fixes which were added after the latest stable release and consider building everything yourself too hard or too much work. 

Step-by-step guide: 

  * Install the latest Lazarus snapshot on Windows normally from here. Make sure you choose a Win32 snapshot with the exact same FPC version as the WinCE snapshot: <http://www.hu.freepascal.org/lazarus/>


  * Download and install the add-on installer for the same Free Pascal version you just downloaded. It's on the same page.


  * The rest of the steps are similar to the step-by-step guide using the stable add-on installer.



### Setting up the Windows CE interface manually

To verify which FPC-version can be used with each Lazarus version please check [LCL_Internals#Minimum_Toolkit_versions](<LCL_Internals.md> "LCL Internals"). 

Here are instruction to set up the Windows CE interface manually: 

**Step 1** \- To start with you will need to recompile the compiler on Windows to create a Windows CE - ARM Cross-compiler. There are instructions here: [WinCE port](<WinCE_port.md> "WinCE port"). 

  * Use TortoiseSVN to checkout the latest 2.2.5: <http://svn.freepascal.org/svn/fpc/branches/fixes_2_2/>


  * Then create a batch script like this one to build your compiler starting from your installed FPC 2.2.4 (either from Lazarus or from separate install):


    
    
    cd compiler
    PATH=C:\Programas\lazarus220\fpc\2.2.4\bin\i386-win32
    make cycle CPU_TARGET=arm OS_TARGET=wince
    cd ..
    pause
    

**Step 2** \- You also need to compile the FCL (Free Component Library) with the newly built compiler. Instructions [here](<WinCE_port.md> "WinCE port"). 

  * The following clever batch script makes this easier (don't forget to fix the paths to match those on your machine). OPT="-FU... will put all compiled units on the same place, which is very convenient, (but can cause [problems](<http://bugs.freepascal.org/view.php?id=11347>). It is better to use a make clean all install instead):


    
    
    cd packages
    PATH=C:\Programas\lazarus220\fpc\2.2.5\bin\i386-win32
    make clean all CPU_TARGET=arm OS_TARGET=wince PP=ppcrossarm.exe OPT="-FUC:\Programas\lazarus220\fpc\2.2.5\units\arm-wince"
    cd ..
    pause
    

**Step 3** \- Put the batch file below on the root of your subversion Lazarus directory and run it 
    
    
    PATH=C:\Programas\lazarus22\fpc\2.2.5\bin\i386-win32;c:\Programas\arm
    make lcl LCL_PLATFORM=wince PP=ppcrossarm.exe CPU_TARGET=arm OS_TARGET=wince
    pause
    

This should compile LCL for Windows CE. 

**Step 4** \- Cross-compile the LazarusPackageIntf in order to be able to use 3rd party visual components. Go to lazarus\packager\registration and do: 
    
    
    PATH=C:\Programas\lazarus22\fpc\2.2.5\bin\i386-win32;c:\Programas\arm
    make PP=ppcrossarm.exe CPU_TARGET=arm OS_TARGET=wince
    pause
    

NOTE: you need to specify $(LazarusDir)\packager\units\$(TargetCPU)-$(TargetOS)\ into your project's unit path (adding FCL as requirement should do that). 

**Step 5** \- Now you can use the Lazarus IDE to design, compile and debug your applications. 

  * You can also use scripts similar to the one below for compiling your applications:


    
    
    PATH=C:\Programas\lazarus22\fpc\2.2.5\bin\i386-win32;c:\Programas\arm
    ppcrossarm.exe -Twince -FuC:\Programas\fpc\rtl\units\arm-wince -FDC:\Programas\arm -XParm-wince- test.pas
    ppcrossarm.exe -Twince -FuC:\programas\lazarus\lcl\units\arm-wince -FuC:\programas\lazarus\lcl\units\arm-wince\wince -FuC:\Programas\fpc\rtl\units\arm-wince -FDC:\Programas\arm -XParm-wince- windowtest.pas
    pause
    

### Compiling Windows CE project files with Lazarus IDE

NOTE: recently a "wincemenures.or" file is reported missing on linking. You just need to copy this file from "lazarus/lcl/interfaces/wince" to "lazarus/lcl/units/arm-wince" and everything will be fine. 

Everything is just as you do with other interfaces and OSes. Make sure you have selected wince as widgetset in in **Compiler Options- >Paths** and in **Code** tab page select **WinCE** as target os and **arm** as Target CPU. 

If you compiled your own compiler or installed it without using the Lazarus add-on installer, you also need to change **compiler path** in **Environment options** to point to your ppcrossarm.exe compiler. In other cases the compiler should already be set to the correct one, which is fpc.exe, which is a front end which chooses the correct back end compiler for the target CPU. In this case the path will be similar to C:\lazarus\fpc\2.2.2\bin\i386-win32\fpc.exe varying with the version of your installed FPC. 

Now, the IDE is ready to compile your files. 

### Debugging Windows CE software on the Lazarus IDE

You can also debug applications created within Lazarus IDE. 

**Step 1** \- In Lazarus IDE go to the menu **Environment- >Debugger Options**. Change the debugger path to the directory with gdb for wince.you can get it from here <ftp://ftp.freepascal.org/pub/fpc/contrib/cross/gdb-6.4-win32-arm-wince.zip>

Also read the notes on <http://bugs.freepascal.org/view.php?id=21061> for information on additional setup. 

And you also need to have ActiveSync installed for gdb to work. You can get ActiveSync here: <http://www.microsoft.com/windowsmobile/en-us/help/synchronize/activesync45.mspx>

  
**Step 2** \- If you are using Microsoft Device Emulator Preview, launch(or restore)it. Make sure you've started emulator with 128MB of RAM. Select a path for shared folders in emulator, add copy command to your .bat file used for building your application to copy the compiled exe file to your shared path. (as you can see in my .bat file). Here is a .bat file you can use to launch the emulator with 128 of ram. 
    
    
    start deviceemulator.exe ".\ppc_2003_se\PPC_2003_SE_WWE_ARMv4.bin" /memsize 128 /skin 
    ".\ppc_2003_se\PocketPC_2003_Skin.xml"
    

After that, just do 'save and exit' whenever you want to quit emulator and, launch it again from shortcuts created in your start menu. (The shortcuts with '(restore)'). 

**Step 3** \- Run Device Emulator Manager and in available emulators right click on the name of emulator and do cradle. Now Microsoft ActiveSync will be launched. If not in Microsoft ActiveSync, Go to the menu **File- >Get connected**. If ActiveSync still doesn't recognize the emulator, Uncradle and Cradle the emulator again. 

**Step 4 - Optional** \- This step is optional and if you don't do it, gdb will do this for you, but it might take some 5 minutes to do it. Copy your executable file with File Explorer program in your emulator to the directory "\gdb". If there is no gdb directory in the root folder, create it. 

**Step 5** \- Now you can safely debug your application.gdb for wince will be launched.it will copy arm-wince-pe-stub.exe to \gdb folder and check if your application.exe file is there and will launch the program. If you encountered an error a) make sure the \gdb folder is created and b) that both arm-wince-pe-stub.exe and your .exe file are present. Also most of the times because of big size of a .exe file, you can not copy that into your \gdb file. So, you'll have to call Microsoft Device Emulator Preview with 128mg ram instead of the default 64mg ram. 

**Known bugs**

There are some known issues with debugging, for which some have a work-around. Please read the bug tracker: [[1]](<http://bugs.freepascal.org/view.php?id=21061>) and [[2]](<http://bugs.freepascal.org/view.php?id=24126>)

**Some Hints**

  1. You can change the remote directory from /gdb to yours by add gdb parameter --eval-command="set remotedirectory ...


    
    
    --eval-command="set remotedirectory \Storage Card\Program Files\My Program\bin"
    

  1. You can reduce the size of .exe file by checking "Use external gdb file debug symbols file -Xg", and "Strip Symbols -Xs", the file myapp.gdb will be generated which can then be used with gdb.



### Installing and Using the Pocket PC Emulator

#### Windows Mobile 5.0 Emulator

1 - You can download a Pocket PC Emulator from Microsoft [here](<http://www.microsoft.com/downloads/details.aspx?FamilyId=C62D54A5-183A-4A1E-A7E2-CC500ED1F19A&displaylang=en>). First, download and install the file V1Emulator.zip. 

2 - Next, be careful that there is a wrong information on the website. It will say that you need to install the Virtual Machine Network Driver, but the link provided on the website is broken. Instead, download Virtual PC 2007 [here](<http://www.microsoft.com/downloads/details.aspx?FamilyID=04d26402-3199-48a3-afa2-2dc0b40a73b6&DisplayLang=en>). This will install the necessary driver too. 

3 - Now, go back to the first website and download and install the efp.msi file 

Now you should have a fully functional PocketPC Emulator which can be utilized together with Lazarus to develop applications. 

To run a Lazarus application on the emulator you can either use GDB via ActiveSync, or you can also just execute it directly. 

#### Windows Mobile 6.5 in Windows Vista or superior

1 - Download and install the [Microsoft Device Emulator 3.0](<http://www.microsoft.com/downloads/details.aspx?familyid=A6F6ADAF-12E3-4B2F-A394-356E2C2FB114&displaylang=en>)

2 - Download and install the [Windows Mobile 6.5 Developer Tool Kit (with localized Images)](<http://www.microsoft.com/downloads/details.aspx?displaylang=en&FamilyID=20686a1d-97a8-4f80-bc6a-ae010e085a6e>)

3 - There is no more ActiveSync in Vista or newer. For newer Windows version it is named "Windows Mobile Device Center. So, download and install it (this download requires the Genuine Windows check): 

3.1 - For Windows Vista or higher (32-bits): <http://www.microsoft.com/en-us/download/details.aspx?id=14>

3.2 - For Windows Vista or higher (64-bits): <http://www.microsoft.com/en-us/download/details.aspx?id=3182>

  
For information about cradling and connecting to a network, read: [[3]](<http://www.petenetlive.com/KB/Article/0000241.htm>)

##### Connecting the Windows Mobile 6.5 emulator in Vista or Superior to the computer

To connect the emulator to the computer, follow these steps: 

1 - Start the Emulator 

2 - Start the Device Emulator Manager. Previously it had a nice start menu icon, but in Vista+ it is well hidden. You have to run the file dvcemumanager.exe, which is probably located in C:\Program Files\Microsoft Device Emulator\1.0 

[![device manager.gif](https://wiki.freepascal.org/images/9/90/device_manager.gif)](</File:device_manager.gif>)

3 - You will see the devices GUID > Right click it and select "Cradle". 

[![cradle.gif](https://wiki.freepascal.org/images/3/39/cradle.gif)](</File:cradle.gif>)

4 - Run the "Windows Mobile Device Center". It should have a start menu icon by now. When it opens, select "Mobile Device Settings" and change the "Allow connections to one of the following" from "Bluetooth" to "DMA" 

[![mobile center dma.gif](https://wiki.freepascal.org/images/e/e6/mobile_center_dma.gif)](</File:mobile_center_dma.gif>)

After closing the dialog it should then connect. 

##### Setting up a shared folder in the emulator

The shared folder is a folder in your computer which is mounted in the device with the path "\Storage Card\" and it works in the device as if it was a memory card. To set this up, click in File -> Configure in the emulator and set the path to the folder, as shown in the screen shot below: 

[![wince emulator shared folder.png](https://wiki.freepascal.org/images/3/30/wince_emulator_shared_folder.png)](</File:wince_emulator_shared_folder.png>)

#### Links to Emulators and Images

##### Emulators

[Microsoft Device Emulator 2.0](<http://www.microsoft.com/downloads/details.aspx?displaylang=en&FamilyID=dd567053-f231-4a64-a648-fea5e7061303>)

[Microsoft Device Emulator 3.0](<http://www.microsoft.com/downloads/details.aspx?familyid=A6F6ADAF-12E3-4B2F-A394-356E2C2FB114&displaylang=en>)

##### Images

[Windows Mobile 6.0 Localized Emulator Images](<http://www.microsoft.com/downloads/details.aspx?familyid=38C46AA8-1DD7-426F-A913-4F370A65A582&displaylang=en>)

[Windows Mobile 6.1 Emulator Images (USA only)](<http://www.microsoft.com/downloads/details.aspx?familyid=3D6F581E-C093-4B15-AB0C-A2CE5BFFDB47&displaylang=en>)

[Windows Mobile 6.1.4 Emulator Images (USA only)](<http://www.microsoft.com/downloads/details.aspx?familyid=1A7A6B52-F89E-4354-84CE-5D19C204498A&displaylang=en>)

[Windows Mobile 6.5 Developer Tool Kit (with localized Images)](<http://www.microsoft.com/downloads/details.aspx?displaylang=en&FamilyID=20686a1d-97a8-4f80-bc6a-ae010e085a6e>)

#### Running an application on the emulator

1 - Go the the Windows Programs Menu --> "Windows Mobile Emulator Images" --> "PocketPC" 

2 - If you never configured your shared folder for the emulator, do so now, by clicking on the menu File --> Configure once the emulator opens. Set it to a folder where you can access your Windows CE executable created with Lazarus. 

3 - On the emulator click: Start --> Programs. Now select "File Explorer". Then select "Storage Card". Navigate until you find your executable and double click it to execute it. 

## How to add a new control

For example TButton. 

TButton is defined in lcl/buttons.pp. This is the platform independent part of the LCL, which is used by the normal LCL programmer. 

Its widgetset class is in lcl/widgetset/wsbuttons.pp. This is the platform independent base for all widgetsets (qt, carbon, gtk, win32, ...). 

It's wince interface class is in lcl/interfaces/wince/wincewsbuttons.pp: 
    
    
     TWinCEWSButton = class(TWSButton)
     private
     protected
     public
       class function  CreateHandle(const AWinControl: TWinControl; const AParams: TCreateParams): TLCLIntfHandle; override;
     end;
    

Every WS class, that actually implements something, must be registered. See the initialization section at the end of the wincewsXXX.pp unit: 
    
    
     RegisterWSComponent(TButton, TWinCEWSButton);
    

  
Also notice that DestroyHandle should be implemented to clean up memory utilized by the control. 

## Gallery

Below are screenshots of applications created with Lazarus for Windows CE: 

  
Virtual Moon Atlas - A free software for Moon observation or survay ( <http://ap-i.net/avl/en/start> ): 

[![Vmap1 west east.jpg](https://wiki.freepascal.org/images/b/bc/Vmap1_west_east.jpg)](</File:Vmap1_west_east.jpg>)

  
LazCalendar - A simple calendar application: 

[![Calendar Wince App.png](https://wiki.freepascal.org/images/b/b8/Calendar_Wince_App.png)](</File:Calendar_Wince_App.png>)

  
[germesorders](<germesorders.md> "germesorders") \- A simple database application using sqlite and RxLib 

[![germes2.png](https://wiki.freepascal.org/images/b/b2/germes2.png)](</File:germes2.png>)

  
[ZzOo](<http://deprecated.cocea.biz/blog1.php/2008/08/26/mobile-fun>), pronounced /'zi:zu:/, is a free Western Zodiac sign finder. You can install it directly and free of charge on your mobile phone by visiting the [GetJar](<http://www.getjar.com/mobile/51934/zzoo/>) app store. 

[![zzoo-wince.png](https://wiki.freepascal.org/images/8/8d/zzoo-wince.png)](</File:zzoo-wince.png>)

  
[Mini SQLite Viewer](<https://sourceforge.net/projects/sqlviewer.minilib.p>) \- Simple SQlite viewer/manager for WinCE and Win32 

[![MiniSQLiteViewerApp.png](https://wiki.freepascal.org/images/3/36/MiniSQLiteViewerApp.png)](</File:MiniSQLiteViewerApp.png>)

  
[STAR Manager](<http://yk-cv.com/software-developing/?id=starmgr>) \- advanced system manager that allows to adjust backligth and sound volume of ARM processor-based WinCE device, set desktop wallpapers, controll and manage system pocesses and memory load, contol battery charge and see device configuration. 

[![Progr-starmgr-1.jpg](https://wiki.freepascal.org/images/7/7b/Progr-starmgr-1.jpg)](</File:Progr-starmgr-1.jpg>)

  


### Wireless Orders for Mini Bar Cafe

Win32 TCP/IP Application Server, Win32 TCP/IP Client, WinCE TCP/IP Client. Using Lazarus and lNet we develop wireless ordering system for Mini Bar - Cafe. Print receipts directly to Cash Mashine. 

More Info ( <http://www.cforce.gr/orders.htm> ) Demo ( <http://www.cforce.gr/downloads/setupwodemo.exe> ) 

[![order.jpg](https://wiki.freepascal.org/images/7/7a/order.jpg)](</File:order.jpg>) [![Example.jpg](https://wiki.freepascal.org/images/a/a9/Example.jpg)](</File:Example.jpg>) [![table.jpg](https://wiki.freepascal.org/images/5/5a/table.jpg)](</File:table.jpg>) [![mtrl.jpg](https://wiki.freepascal.org/images/0/0a/mtrl.jpg)](</File:mtrl.jpg>)

## Other Interfaces

  * [Lazarus known issues (things that will never be fixed)](<Lazarus_known_issues_\(things_that_will_never_be_fixed\).md> "Lazarus known issues \(things that will never be fixed\)") \- A list of interface compatibility issues
  * [Win32/64 Interface](<Win32/64_Interface.md> "Win32/64 Interface") \- The Windows API (formerly Win32 API) interface for Windows 95/98/Me/2000/XP/Vista/10, but not CE
  * Windows CE Interface \- For Pocket PC and Smartphones
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
  * [Windows CE Development Notes](<Windows_CE_Development_Notes.md> "Windows CE Development Notes") \- For Pocket PC and Smartphones
  * [Adding a new interface](<Adding_a_new_interface.md> "Adding a new interface") \- How to add a new widget set interface
  * [LCL Defines](<LCL_Defines.md> "LCL Defines") \- Choosing the right options to recompile LCL
  * [LCL Internals](<LCL_Internals.md> "LCL Internals") \- Some info about the inner workings of the LCL
  * [Cocoa Internals](<Cocoa_Internals.md> "Cocoa Internals") \- Some info about the inner workings of the Cocoa widgetset



## See Also

  * [Windows CE Interface Development notes](<Windows_CE_Development_Notes.md> "Windows CE Development Notes")
  * [Alternative Lazarus Windows CE tutorial using Win64](</User:CCRDude> "User:CCRDude")
  * [Roadmap for the Windows CE interface](<Roadmap.md> "Roadmap")
  * Mini framework for WindowsCE applications: [pkMiniGUI.pas](<http://ccrdude.net/files/fpc/pkMiniGUI.pas>) [Documentation](<http://ccrdude.net/docs/pkCEStuff.htm>)
  * [WinCE port of KOL GUI library](<KOL-CE.md> "KOL-CE") \- compact applications for WinCE/Win32.
  * [WinCE port](<WinCE_port.md> "WinCE port") \- About the compiler specific parts of the port

---

_Source: [https://wiki.freepascal.org/Windows_CE_Interface](https://web.archive.org/web/20240920204109/https://wiki.freepascal.org/Windows_CE_Interface)_
