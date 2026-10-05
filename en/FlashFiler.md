# FlashFiler

## Contents

  * 1 About
  * 2 Installation
  * 3 Usage
  * 4 Screenshots
  * 5 State/changes of the Lazarus port
  * 6 ToDo
  * 7 Current Version



## About

This is a Lazarus port of TurboPower FlashFiler Database. Based on version tpflashfiler_2_13 from SourceForge (<https://sourceforge.net/projects/tpflashfiler/>). More port infos are in devdocs\LazConvertReadMe.txt 

Use [this thread](<http://forum.lazarus.freepascal.org/index.php/topic,34834.0.html>) in Lazarus forum for questions. Read the #readme.txt-files from zip-file for more information. 

License: same as TurboPower FlashFiler (MPL 1.1) 

Author of Lazarus port: Soner A. 

## Installation

  1. Download it (goto current version section for download links).
  2. Open and compile the runtime package lazff2.lpk from folder sources.
  3. Open, compile and install the designtime package lazff2_dt.lpk from folder sources.
  4. When you get sometimes errors like this "Error compiling ffllexp.pas, ffsrmgr can not found" at compiling lazff2_dt.lpk, then open lazff2.lpk and recompile it.



## Usage

1\. Start bin\i386-win32\ffserver.exe  
2\. Make 2 db-aliases in ffserver [ffserver-Menu > Config > Aliases ...]  

    
    
    Alias:		Path:
    mythicdb 	root folder\flashfiler\data\mythicdb
    Tutorial	root folder\flashfiler\data\demodb
    

I created them you must change the root folder.  
3\. Open FlashFiler Server General Configuration Dialog  
[ffserver-Menu > Config > General ...]  
4\. In configuration dialog Enter for Server name:   

    
    
    local  
    
    then Click Ok.  
    
    

5\. Now the server "local" appears in Servers listview. Click on it and start it.  
6\. Now open any example from examples-folder and compile, run and enjoy it.  


## Screenshots

Components: 

[![ffComps.png](https://wiki.freepascal.org/images/1/1a/ffComps.png)](</File:ffComps.png>)

Example programs: 

[![ffserverclients.png](https://wiki.freepascal.org/images/f/f8/ffserverclients.png)](</File:ffserverclients.png>)

## State/changes of the Lazarus port

10.12.2016: Client components are Working. Server-Engine component (TffServerEngine) has error so you need server binaries compiled with delphi. Tested with Windows 7, Lazarus 1.6.3, fpc 3.0. 

11.03.2017: Now the server components are working. I must test and change somethings and upload it to lazarus-ccr. 

20.06.2017: Server and clients components are working. Now, you can create client and server applications. (Tested with Windows 7, Xp, Lazarus 1.6.4, fpc 3.0.2 All 32Bit) 

21.06.2017: The programs flashfiler explorer (database gui) is half ported so if you select some menu items you will get error. The flashfiler server is not ported. You can use the originals from the bin folder or you can create your database in your program. You have also to write your own server if you don't want original server from the bin folder. I put referenz server in examples folder. 

21.06.2017-2: Packages files moved from packages to source folder. Empty Packages folder deleted. 

## ToDo

[11.03.2017 Done] _1\. Solve server-engine component (TffServerEngine) error. The error is located in fflldict.pas-file in _procedure_TffDataDictionary.ReadFromStream(S : TStream);_ _It is stream reading error with caused by functions ReadString and ReadInteger. I could not solve it, maybe someone with better skills can do it._  


20.06.2017: port some ide experts and test it more.  


## Current Version

tpflashfiler_2_13-20170621.7z (6,96 MB) [[1]](<https://1drv.ms/u/s!AvJGv-C_3b-WgQmtDFn1613VUBVS>)

When you downloaded the tpflashfiler_2_13-20170620 version then you can use this to upgrade it. (2,58 KB) [[2]](<https://1drv.ms/u/s!AvJGv-C_3b-WgQh0y4026vz-t8ef>)

---

_Source: [https://wiki.freepascal.org/FlashFiler](https://web.archive.org/web/20231206105249/https://wiki.freepascal.org/FlashFiler)_
