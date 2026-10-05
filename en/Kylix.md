# Kylix

│ **English (en)** │

**Kylix** ® is an [Object Pascal](<Object_Pascal.md> "Object Pascal") [compiler](<Compiler.md> "Compiler") and IDE released by Borland International for the [Linux](<Linux.md> "Linux") [operating system](<operating_system.md> "operating system"). It is an attempt to provide the equivalent functionality of their [Delphi](<Delphi.md> "Delphi")® Object [Pascal](<Pascal.md> "Pascal") product for the Microsoft Windows operating system. 

## Contents

  * 1 Kylix 3 Open Edition
  * 2 Installing Kylix Help in the Lazarus IDE
    * 2.1 Files needed from Kylix3
    * 2.2 The hyperhelp.sh script
    * 2.3 My directory tree
    * 2.4 Custom Tool setup in IDE
    * 2.5 In summary



## Kylix 3 Open Edition

Borland used to distribute a version of Kylix free for personal use that was called "Kylix 3 Open Edition", but this version unfortunately can no longer be legally distributed. 

**NOTE** :  
Kylix 3 is rather hard to install and get working with the latest Linux distros (this issue is only related to the Kylix IDE, not the applications created with it). 

It is therefore highly recommended that you rather install Free Pascal Compiler and the Lazarus IDE for any new development work. They are similar to Kylix and Delphi, but a lot more up to date than Kylix. Free Pascal and Lazarus are excellent tools and are Open Source, so they will always be available - without activation requirements. 

There is another cross-platform GUI toolkit you can try with Free Pascal; it is called [fpGUI Toolkit](<fpGUI.md> "fpGUI"). 

  * Free Pascal: <https://www.freepascal.org>
  * Lazarus: <https://lazarus-ide.org>
  * fpGUI Toolkit: <https://sourceforge.net/projects/fpgui/>



## Installing Kylix Help in the Lazarus IDE

For users who have a legal copy of Kylix, it is possible to add Kylix 3 context-sensitive help files to Lazarus IDE running on Linux x86. The benefit from those help files is that they are very detailed, and also contain the Object Pascal Language Reference help - most of which is applicable to FPC's Object Pascal syntax or the Lazarus LCL. Note that this uses the proprietary HyperHelp program, so is not portable to other platforms. 

### Files needed from Kylix3

These are the files and directories needed from the `${HOME}/kylix/help` directory. 

(I suspect that the help system from the free version of Kylix may work as well. ) 
    
    
     bin/           k3clx.hlp     k3dbd.cnt    k3.hlp
     foo            k3.cnt        k3dbd.fts    k3indy.cnt
     foo~           k3copts.cnt   k3dbd.hlp    k3indy.fts
     hhintro.txt    k3copts.fts   k3dbx.cnt    k3indy.hlp
     hoh.cnt        k3copts.hlp   k3dbx.fts    k3iota.cnt
     hoh.fts        k3crtl.cnt    k3dbx.hlp    k3iota.fts
     hoh.hlp        k3crtl.fts    k3derrs.cnt  k3iota.hlp
     hyperhelp.sh*  k3crtl.hlp    k3derrs.fts  k3new.cnt
     k3.als         k3ctools.cnt  k3derrs.hlp  k3new.fts
     k3cerrs.cnt    k3ctools.fts  k3dlr.cnt    k3new.hlp
     k3cerrs.fts    k3ctools.hlp  k3dlr.fts    k3prog.cnt
     k3cerrs.hlp    k3cw.cnt      k3dlr.hlp    k3prog.fts
     k3clr.cnt      k3cw.fts      k3dopts.cnt  k3prog.hlp
     k3clr.fts      k3cw.hlp      k3dopts.fts  lib/
     k3clr.hlp      k3dap.cnt     k3dopts.hlp
     k3clx.cnt      k3dap.fts     k3.ftg
     k3clx.fts      k3dap.hlp     k3.fts
    

Most of the above files may not pertain to Lazarus, such as the k3c* files which relate to C++, but they are harmless to keep around. 

The bare minimum files needed for the context help are these: 
    
    
    bin/           k3.als     k3clx.fts  k3.cnt  k3.fts  lib/
    hyperhelp.sh*  k3clx.cnt  k3clx.hlp  k3.ftg  k3.hlp
    

### The hyperhelp.sh script

We do need to dress up the default help script provided with Kylix so that it will work as a standalone application under X. Note that the last line of the script sets some missing rendering parameters. Also note the -geometry which positions the window and sets its size. 
    
    
    #!/bin/bash
    HHHOME="${HOME}/Lazarus/DelphiHelp"
    XPPATH="${HHHOME}/xprinter"
    LANG=en_US
    LC_ALL=en_US
    locale=${LC_ALL:-${LC_CTYPE:-${LANG:-"C"}}}
    LD_LIBRARY_PATH+="lib/locale/"${locale}":lib:"
    XAPPLRESDIR="app-defaults"
    NLSPATH="lib/locale/%L/%N.cat"
    export XPPATH HHHOME LANG LC_ALL LD_LIBRARY_PATH XAPPLRESDIR NLSPATH
    cd $HHHOME
    bin/hyperhelp k3clx.hlp -m AL\($1,3,,main\)\
        -s 0 -d 32159 -display :0.0 -geometry 600x500+100+100 -w
    

### My directory tree

This is where I have placed the Kylix Help file directory. It is under my installation of Lazarus in the user home directory. The hyperhelp.sh script refers to these locations. 
    
    
    /home/tom/Lazarus/DelphiHelp/
    |-- bin
    `-- lib
       `-- locale
           |-- de_DE
           |-- fr_FR
           |-- ja_JP.SJIS
           |-- ja_JP.eucJP
           `-- ja_JP.eucjp
    

### Custom Tool setup in IDE

Finally, set up the new custom tool within Lazarus from the menu 'Tools|Configure custom tools|Add'. Replace '/home/tom/' with your user home directory path. This has to be an absolute path to the hyperhelp.sh script. 

[![edittool.png](https://wiki.freepascal.org/images/7/78/edittool.png)](</File:edittool.png>)

### In summary

Just place your cursor in the edit window on the word you want to find help for and press `Ctrl`+`F1`. The Kylix help will then open up the context sensitive help page for that keyword. 

Various [Pascal](<Pascal.md> "Pascal") [Compilers](<Compiler.md> "Compiler"):  [AAEC Pascal](</index.php?title=AAEC_Pascal&action=edit&redlink=1> "AAEC Pascal \(page does not exist\)") | [Alice Pascal](<Alice_Pascal.md> "Alice Pascal") | [Apple Pascal](<Apple_Pascal.md> "Apple Pascal") | [Borland Pascal](<Borland_Pascal.md> "Borland Pascal") | [Clascal](<Clascal.md> "Clascal") | [Delphi](<Delphi.md> "Delphi") | [Free Pascal Compiler (FPC)](<FPC.md> "FPC") | [GNU Pascal](<GNU_Pascal.md> "GNU Pascal") | Kylix | [Lisa Pascal](<Lisa_Pascal.md> "Lisa Pascal") | [Mac Pascal](<Mac_Pascal.md> "Mac Pascal") | [Metrowerks Pascal](</index.php?title=Metrowerks_Pascal&action=edit&redlink=1> "Metrowerks Pascal \(page does not exist\)") | [NBS Pascal](<NBS_Pascal.md> "NBS Pascal") | [OMSI Pascal](</index.php?title=OMSI_Pascal&action=edit&redlink=1> "OMSI Pascal \(page does not exist\)") | [PascalABC.net](<PascalABC.md> "PascalABC.net") | [P32](</index.php?title=P32&action=edit&redlink=1> "P32 \(page does not exist\)") | [Sibyl](<Sibyl.md> "Sibyl") | [Smart Pascal](</index.php?title=Smart_Pascal&action=edit&redlink=1> "Smart Pascal \(page does not exist\)") | [Stanford Pascal Compiler](<Stanford_Pascal_Compiler.md> "Stanford Pascal Compiler") | [Swedish Pascal](</index.php?title=Swedish_Pascal&action=edit&redlink=1> "Swedish Pascal \(page does not exist\)") | [THINK Pascal](<THINK_Pascal.md> "THINK Pascal") | [Turbo Pascal](<Turbo_Pascal.md> "Turbo Pascal") | [UCSD Pascal](<UCSD_Pascal.md> "UCSD Pascal") | [VAX Pascal](</index.php?title=VAX_Pascal&action=edit&redlink=1> "VAX Pascal \(page does not exist\)") | [Virtual Pascal](<Virtual_Pascal.md> "Virtual Pascal") | [winsoft PocketStudio](<winsoft_PocketStudio.md> "winsoft PocketStudio")  
---  
An extensive list of compilers was maintained at [Pascaland (Internet Archive Version)](<https://web.archive.org/web/20230529010938/https://www.pascaland.org/pascall.htm>) up to January 2018.   
  
  
****

---

_Source: [https://wiki.freepascal.org/Kylix](https://web.archive.org/web/20240920204050/https://wiki.freepascal.org/Kylix)_
