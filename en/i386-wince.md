# i386-wince

[![WinCE Logo.png](https://wiki.freepascal.org/images/8/86/WinCE_Logo.png)](</File:WinCE_Logo.png>)

This article applies to [Windows CE](</Category:WinCE> "Category:WinCE") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

This page has been created to collect all information regarding i386-wince target.This page only describes usage of Win32 as a development Host. Although most of today's PDAs, Phones, Tablets use ARM CPUs - dealing with i386 version of CE might make sense as: 

  * There exists a certain amount of industrial controllers running under i386-wince. Some of them originally where XPe but have been migrated to CE. Some of them have been created by the Alliance SIEMENS/Microsoft.
  * As Intel tries to regain market share in the embedded area (also Mobile/Tablet), it might be that more mobile/tablet will use X86 platforms ([Atom 2460 for example](<http://www.anandtech.com/show/5365/intels-medfield-atom-z2460-arrive-for-smartphones>)) again.



## Contents

  * 1 Prerequisites
  * 2 Setup Lazarus/FPC for compiling i386-wince
  * 3 Test Applications
    * 3.1 Simple graphical Hello
  * 4 Useful 3rd Party Tools, Libraries, Links



## Prerequisites

  * A Windows-CE Emulator (i386 Version!)  
For example [Windows CE 5.0 Device Emulator](<http://www.microsoft.com/download/en/details.aspx?id=18481>)(V 8.2.160.0).Install this to a folder. Create a Shortcut to executable(to be able to call it with command line parameters). Edit command line of link to something like  
"{path to exe}Emulator_500.exe" /ceimage NK.cem /ethernet virtualswitch  
This will startup the emulator using the NK.cem image and creates a bridged ethernet connection(set IP in emulator to fit your network if you have no DHCP server running). Also this emulator allows a shared folder between Host-PC and emulator(This can be your Lazarus project's working folder)
  * FPC Source code (I used [fpcbuild-2.6.0.zip](<http://gd.tuwien.ac.at/languages/pascal/fpc/dist/2.6.0/source/fpcbuild-2.6.0.zip>))
  * Lazarus (I used a snapshot of 20120225 [<ftp://ftp.hu.freepascal.org/pub/lazarus/snapshots/Lazarus-0.9.31-35595-fpc-2.6.1-20120226-win32.exe> Lazarus-0.9.31-35589-fpc2.6.1-20120215-win32.exe). The daily shapshot I used has been removed, but that one in the link should work too.



## Setup Lazarus/FPC for compiling i386-wince

**1) Install Lazarus from above Link (just as for compiling Win32 apps)**  
We use PATH=C:\Prog\lazarus\931\ in the example {lazfolder}  
**2) Unzip the fpc sources to a convenient folder**  
We use C:\Prog\fpc\2.6.0src\ in this example  
**3) Create a batch file (mkCE.bat) with the following content:**  

    
    
    PATH=C:\Prog\lazarus\931\fpc\2.6.0\bin\i386-win32;C:\Prog\fpc\2.6.0src\fpcsrc\compiler
    make clean all OS_TARGET=wince CPU_TARGET=i386 PP=ppc386.exe
    

Save this batchfile to C:\PROG\FPC\2.6.0src\fpcsrc. Start this batchfile by doubleclick. (It should run from the command line too, but in my setup, from the commandline there was an error message thrown (*** no libgdb.a found, supply NOGDB=1 to disable debugger support....) - but run from the Windows GUI this run just fine. The popping up console window will show compile progress for some time until it closes again. Then you will find a new file 'build-stamp.i386-wince' in the fpsrc folder. (In this case you can be happy that all worked OK)  
**4) Now there are lots of compiled subfolders in the fpc tree, that need to be copied over into (other positons) of the Lazarus folders.**  


  * **packages\\{packagename}\units\i386-wince** goes to **{lazfolder}\fpc\2.6.0\units\i386-wince\\{packagename}**  




where no units subfolder has been created during build, omit this folder  


  * **rtl\units\i386-wince** goes to **{lazfolder}\fpc\2.6.0\units\i386-wince\rtl**  




the packages are: bzip2,chm,fastcgi,fcl-base,fcl-db,fcl-fpcunit,fcl-image,fcl-js,fcl-json,fcl-net,fcl-passrc,fcl-process,fcl-registry,fcl-res,fcl-web,fcl-xml,fftw,fpmkunit,fppkg,hash,hermes,httpd22,ibase,libgd,mysql,numlib,odbc,oggvorbis,openssl,oracle,pasjpeg,paszlib,postgres,regexpr,rtl,sdl,sqlite,symbolic,tcl,unzip,winceunits,zlib 

(I will provide a batchfile soon for this...) 

## Test Applications

If your system is a core WindowsCE and not a Windows Mobile device, you might run into an error message:  
_Cannot find {your app name } (or one of it's components). Make sure the path and filename are correct..._  
In this case, this might be most probable a missing aygshell.dll (See [WinCE_Programming_Tips](<WinCE_Programming_Tips.md> "WinCE Programming Tips")). aygshell.dll might be found in the web, but most/all of them will be for ARM or MIPS. So to get hold of one, you will have to install the Windows CE 5.0 Trial version and build a sample device. 

<work in progress, will be updated...>

### Simple graphical Hello

As WinCE by default has no console, it's easier to try a graphical hello tesprogram.(for the console app you would have to install a console program or enable it first by a registry hack) <Sourcecode> <Screenshot> <prebuilt application>

  * runs on WinCE 4.2 (YES/NO)
  * runs on WinCE 5.0 (YES/NO)
  * runs on WinCE 6.0 (YES/NO)



## Useful 3rd Party Tools, Libraries, Links

  * Check [WinCE_port_Hints](<WinCE_port_Hints.md> "WinCE port Hints") for more details on programming under Windows CE platform.
  * [lNet](<http://lnet.wordpress.com/>) TCP/IP library seems to work fine (tested Visual/Nonvisual Lazarus packet and a Telnet client connecting from emulator to telnet host on Host-PC

---

_Source: [https://wiki.freepascal.org/i386-wince](https://web.archive.org/web/20240920213339/https://wiki.freepascal.org/i386-wince)_
