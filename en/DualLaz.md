# DualLaz

## long story short

fpcUP _deluxe_ = [fpcupdeluxe](<fpcupdeluxe.md> "fpcupdeluxe") is the newest alternative among GUI installers and pretty convenient, fpcGUI is older and less powerful. 

setup fpcGUI like in pic below (possibly make your alterations) and hit run button. Should all work like a charm. 

## Two simultaneous Lazarus copies operational in Linux 64 bit

Typically, you have a primary install via fpcUP in ~/development.   
But you also want a secondary test version in /workdir/LazCC or somewhere independently, possibly for release testing of Lazarus ver. 1.4   
Windows users, see [Multiple Lazarus](<Multiple_Lazarus.md> "Multiple Lazarus") install info. 

Here is a run-through to achieve that. Let's assume: 

  * We use the Linux distro: mint 17.1 KDE 64 bit of 2015 (Kubuntu should work too, I guess)
  * **primary** Lazarus resides in **/home/you/development** installed via fpcUP
  * **secondary** Lazarus will reside at **/workDIR/LazCC**
  * no other Lazarus installed via a debian-package installer or whatever, which could interfere somehow (e.g. /usr/local/lib/fpc - so we delete it )
  * /home/you below refers to your ~ aka home-directory aka $HOME aka /home/michael or whatever name you chose.



  
By default, Lazarus uses _~/development/config_lazarus/_ as the **primary-config-path**  
We will override that in the second script: 

In ~ create two shell scripts : runL1.sh and runL2.sh containing 
    
    
    #!/bin/sh
    /home/you/development/lazarus/startlazarus --primary-config-path=/home/you/development/config_lazarus
    

and 
    
    
    #!/bin/sh
    /workDIR/LazCC/laz/startlazarus --primary-config-path=/workDIR/LazCC/pcp4fup
    

respectively. Use an explorer like dolphin etc. or type sth. like sudo mkdir /workDIR/LazB and chown to make it accessible to you as a non-root user. 

Upon executing **runL1.sh** the "Configure Lazarus IDE" screen (pic below) pops up in case that your pcp-dir is new or emptied (i.e. for alternative settings set). In such case we choose 
    
    
    /home/you/development/fpc/bin/x86_64-linux/**fpc.sh**
    

as the **compiler**. Do not choose 
    
    
    /home/you/development/fpc/bin/x86_64-linux/fpc
    

because it will give you run time errors aplenty (system.ppu not found etc.). 

  


  
fpc.sh contains 
    
    
    #!/bin/sh
    # This script starts the fpc compiler installed by fpcup
    # and ignores any system-wide fpc.cfg files
    # Note: maintained by fpcup; do not edit directly, your edits will be lost.
    /home/you/development/fpc/bin/x86_64-linux/fpc  -n @/home/you/development/fpc/bin/x86_64-linux/fpc.cfg "$@"
    

and will obviously be path-adapted for the secondary Lazarus. This is done automatically by fpcUP. We'll run it via fpcupGUI, which is contained in the source zip-archive of fpcUP. Compile and run it with a prior Lazarus install you made. The settings below will keep fpcUP from using undesired fallback-paths and instruct it to put it where we want, i.e. the subdirectory **/workDIR/LazCC** which takes 7 minutes roundabout 

[![fpcGUI LazCC.png](https://wiki.freepascal.org/images/0/00/fpcGUI_LazCC.png)](</File:fpcGUI_LazCC.png>)

upon running abovementioned **runL2.sh** we should complete the path setup   
in the popup window [![LazCFG.png](https://wiki.freepascal.org/images/4/49/LazCFG.png)](</File:LazCFG.png>)

  
  
according to this pic: [![LazPaths.png](https://wiki.freepascal.org/images/1/19/LazPaths.png)](</File:LazPaths.png>)

  
... and we're done. We have 2 independent Lazarusses on disk ! Now you can repeat the routine n times with new paths, if you so desire. 

The option **lazlinkname=lazarus_fpctrunk** actually produced a link on the desktop named 
    
    
    /home/you/Desktop/fpcup-lazarus_fpctrunk.desktop      (i.e. not in /workDIR because the option was used twice, the latter being effective) executing
    
    
    
    /workDIR/LazCC/laz/lazarus --pcp="/workDIR/LazCC/pcp4fup"
    

which is OK. We don't necessarily need our above runL2.sh and can use the desktop link instead. 

* * *

for copying, here is the settingsCC.ini: 
    
    
    ; settings for fpcup              CC ver
    ; use e.g.    fpcup --inifile=settingsCC.ini --inisection=generalCC
    ; to compile with the options specified in that section  [generalCC]
    [generalCC]
    installdir=/workDIR/LazCC
    primary-config-path=/workDIR/LazCC/pcp4fup
    fpcdir=/workDIR/LazCC/fpc
    lazdir=/workDIR/LazCC/laz
    lazlinkname=run-lazCC
    ;lazlinkname=lazarus_fpctrunk  - just the name of the Desktop link
    ;lazlinkname=/workDIR/LazCC/lazrunner.sh  
    lclplatform=gtk2
    ; Full FPC+Lazarus download & compile
    ; Here use FPC trunk instead of the version tagged  "default fixes"
    fpcurl=http://svn.freepascal.org/svn/fpc/trunk 
    ; Compiles with debug options:
    fpcopt="-g -gl -O1"
    lazopt="-g -gl -O1"
    ; Use our own names to avoid the default values
    fpcuplinkname=fpcuptrunkdebug
    ; Keeps changes you made to the svn checkout:
    keeplocalchanges=true
    ; You can enable/disable modules specified in fpcup.ini:
    anchordocking=0

---

_Source: [https://wiki.freepascal.org/DualLaz](https://web.archive.org/web/20230924201514/https://wiki.freepascal.org/DualLaz)_
