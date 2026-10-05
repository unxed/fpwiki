# go32v2 development

This page is about development wrt the GO32V2 port. 

## Contents

  * 1 DOSBox-X
  * 2 Compiling under XP
  * 3 Memory issues under Vista
  * 4 See also



## DOSBox-X

DOSBox-X is a fork of DOSBox that supports long file names. It can be used for go32v2 development on modern systems. 

<https://dosbox-x.com/>

To enable the long file name support, the following command should be used inside the DOSBox-X command prompt: 
    
    
    VER SET 7 0
    

Snapshot building fails, due to this bug: 
    
    
    <https://github.com/joncampbell123/dosbox-x/issues/2375>
    

## Compiling under XP

use 
    
    
      make <arguments> RUNBATCH="c:\windows\system32\cmd.exe /c"
    

this avoids some lfn command.com issues. 

If there are errors with removing the so called "pack dir", hunt for $(DELTREE) statements that don't have a - (to ignore errors) in front of them. A work around can be to use a batchfile that pauses before rm-rfing: 
    
    
      ping -n 2 localhost > nul
      rm -rf %1 %2 %3
    

And pass it using DELTREE=c:\path\to\abovebatchfile.bat 

With fpmake, most of these removal operations have moved into fpmake, and fpmake has been hardened for a few of these issues, to retry a few times with pauses. 

## Memory issues under Vista

It seems that Vista standardly allocates only 1MB extended memory to the dosbox. I found the below bit, but wasn't able to get more than 15MB free. (probably 16MB total) 

You can change these settings by running explorer as Administrator or you can also change it from Safe Mode. To run explorer as Administrator, open up a new elevated command window by right clicking cmd.exe -> Run as administrator, then open Task Manager and End Process on explorer, then from the elevated command prompt type explorer.exe (DO NOT EXIT TASK MANGER). Explorer is now running elevated, navigate to C:\WINDOWS\System32\command.com and right click -> properties, proceed to change the setting in the memory tab. After you are completed to return explorer to a standard user process context goto task manager and end the explorer process, then goto File->New Task(Run...) and type explorer.exe, explorer is now running in a standard user process. 

<http://www.trnicely.net/misc/vista.html>

## See also

  * [DOS](<DOS.md> "DOS")
  * [GO32V2](<GO32V2.md> "GO32V2")

---

_Source: [https://wiki.freepascal.org/go32v2_development](https://web.archive.org/web/20250315170247/https://wiki.freepascal.org/go32v2_development)_
