# Linux Programming Tips

│ **English (en)** │

[![Crystal 128 penguin.png](https://upload.wikimedia.org/wikipedia/commons/1/1c/Crystal_128_penguin.png)](</File:Crystal_128_penguin.png>)

This article applies to [Linux](</Category:Linux> "Category:Linux") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

## Contents

  * 1 How to...
    * 1.1 Write a daemon
    * 1.2 Get the list of mounted partitions
    * 1.3 Perform text-to-speech (TTS) or how to let my computer speak
    * 1.4 Install Lazarus on Raspberry Pi
    * 1.5 Test whether an application is already running
  * 2 Other Interfaces
    * 2.1 Platform specific Tips
    * 2.2 Interface Development Articles



## How to...

### Write a daemon

To write a daemon (service application), please see [Daemons and Services](<Daemons_and_Services.md> "Daemons and Services")

### Get the list of mounted partitions

Read and parse the file /etc/mtab 

An example of such a file: 
    
    
    /dev/sda5 / ext4 rw,commit=0 0 0
    none /proc proc rw 0 0
    /dev/sda7 /home ext4 rw,commit=0 0 0
    none /proc/sys/fs/binfmt_misc binfmt_misc rw 0 0
    gvfs-fuse-daemon /home/felipe/.gvfs fuse.gvfs-fuse-daemon rw,nosuid,nodev,user=felipe 0 0
    truecrypt /tmp/.truecrypt_aux_mnt1 fuse.truecrypt rw,nosuid,nodev,allow_other 0 0
    /dev/mapper/truecrypt1 /media/truecrypt1 vfat rw,uid=500,gid=500,umask=077 0 0
    

### Perform text-to-speech (TTS) or how to let my computer speak

See: 

  * [examples for calling espeak](<http://lazarus.freepascal.org/index.php/topic,19723.0/topicseen.html>).



**to do: incorporate those examples**

### Install Lazarus on Raspberry Pi

  * [Lazarus on Raspberry Pi](<Lazarus_on_Raspberry_Pi.md> "Lazarus on Raspberry Pi")



### Test whether an application is already running

See [AppIsRunning](<AppIsRunning.md> "AppIsRunning"). 

## Other Interfaces

  * [Lazarus known issues (things that will never be fixed)](<Lazarus_known_issues_\(things_that_will_never_be_fixed\).md> "Lazarus known issues \(things that will never be fixed\)") \- A list of interface compatibility issues
  * [Win32/64 Interface](<Win32/64_Interface.md> "Win32/64 Interface") \- The Windows API (formerly Win32 API) interface for Windows 2000/XP/Vista/10, but not CE
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
  * Linux Programming Tips \- How to execute particular programming tasks in Linux
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

---

_Source: [https://wiki.freepascal.org/Linux_Programming_Tips](https://web.archive.org/web/20250601000000/https://wiki.freepascal.org/Linux_Programming_Tips)_
