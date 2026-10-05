# Uninstalling Lazarus on macOS

│ **English (en)** │ 

[![macOSlogo.png](https://wiki.freepascal.org/images/1/15/macOSlogo.png)](</File:macOSlogo.png>)

This article applies to [macOS](</Category:macOS> "Category:macOS") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

  
****

## Contents

  * 1 Installed using fink
  * 2 Installed using the official packages
    * 2.1 FPC 3.x.x and Lazarus 2.0.8 through 2.0.10
    * 2.2 FPC 3.x.x and Lazarus 2.0.11+ and 2.2.0
    * 2.3 Lazarus preference folder .lazarus in the home directory
    * 2.4 Third party package uninstaller



## Installed using fink

If you installed Lazarus + FPC using `fink`, the complete uninstall of all Lazarus + FPC packages is done with: 
    
    
    fink purge --recursive fpc-config
    

If you want to preserve changes to the preference file /sw/etc/fpc.cfg, do: 
    
    
    fink remove --recursive fpc-config
    

For the removal of the Lazarus preferences subdirectory in your home directory, and the files that it contains, see the end of the next section. 

## Installed using the official packages

Normally you uninstall an application on macOS simply by dragging it from the Applications folder to the trash. As Lazarus and Free Pascal are development tools, they install files in several folders that you don't normally see in Finder and so the normal approach does not work. 

You can usually install newer versions of Lazarus over older versions, while newer versions of Free Pascal are installed in unique directories. However, as with most software it's not a bad idea to clean out everything before you install a newer version. The two scripts below should give you a sense of where the various pieces of Lazarus and Free Pascal are located. 

You can copy and save the commands from one of the following scripts to a file, say `uninstallLaz.sh`, and run it if you need to uninstall Lazarus and Free Pascal. 

### FPC 3.x.x and Lazarus 2.0.8 through 2.0.10

The script below will delete all versions of FPC and Lazarus which have been installed in the targeted locations using the official Lazarus installation packages (which also install FPC). These locations are correct for FPC 3.x.x and Lazarus 2.0.8 through 2.0.10. 
    
    
    #!/bin/sh
    #
    # Uninstaller script for FPC 3.x.x and Lazarus 2.0.8 to 2.0.10
    #
    bin=/usr/local/bin
    private=/private/etc
    share=/usr/local/share
    lib=/usr/local/lib
    library=/Library
    receipts=/private/var/db/receipts
    fpcdoc="/Users/Shared/Free Pascal Compiler"
    manpages=/usr/local/share/man
    app=/Applications
    
    rm -fv $bin/pas2js
    rm -fv $bin/pas2jni
    rm -fv $bin/pas2fpm
    rm -fv $bin/mkx86ins
    rm -fv $bin/mkinsadd
    rm -fv $bin/mkarmins
    rm -fv $bin/mka64ins
    rm -fv $bin/makeskel.rsj
    rm -fv $bin/makeskel
    rm -fv $bin/json2pas
    rm -fv $bin/instantfpc
    rm -fv $bin/h2paspp
    rm -fv $bin/h2pas
    rm -fv $bin/fprcp
    rm -fv $bin/fppkg
    rm -fv $bin/fpdoc
    rm -fv $bin/fpcsubst.rsj
    rm -fv $bin/fpcsubst
    rm -fv $bin/fpcreslipo
    rm -fv $bin/fpcres
    rm -fv $bin/fpcmkcfg.rsj
    rm -fv $bin/fpcmkcfg
    rm -fv $bin/fpcmake
    rm -fv $bin/fpclasschart.rsj
    rm -fv $bin/fpclasschart
    rm -fv $bin/fpcjres
    rm -fv $bin/fpc
    rm -fv $bin/fd2pascal
    rm -fv $bin/delp
    rm -fv $bin/data2inc
    rm -fv $bin/compileserver
    rm -fv $bin/cldrparser
    rm -fv $bin/chmls
    rm -fv $bin/chmcmd
    rm -fv $bin/bin2obj
    rm -fv $bin/webidl2pas
    rm -fv $bin/unitdiff.rsj
    rm -fv $bin/unitdiff
    rm -fv $bin/unihelper
    rm -fv $bin/rstconv.rsj
    rm -fv $bin/rstconv
    rm -fv $bin/rmcvsdir
    rm -fv $bin/pyacc
    rm -fv $bin/ptop.rsj
    rm -fv $bin/ptop
    rm -fv $bin/ppumove
    rm -fv $bin/ppufiles
    rm -fv $bin/ppudump
    rm -fv $bin/ppdep
    rm -fv $bin/postw32
    rm -fv $bin/plex
    rm -fv $bin/pas2ut.rsj
    rm -fv $bin/pas2ut
    rm -fv $bin/ppcx64
    rm -fv $bin/ppc386
    rm -fv $bin/lazbuild
    
    rm -rfv $private/lazarus
    
    rm -rfv $share/fpcsrc
    rm -rfv $share/src/fpc-*
    rm -rfv $share/doc/fpc-*
    
    rm -rfv $lib/fpc
    
    rm -rfv $library/Lazarus
    
    rm -rfv $receipts/org.lazarus*
    rm -rfv $receipts/org.freepascal*
    
    rm -rfv "$fpcdoc"
    
    rm -fv $manpages/man1/bin2obj.1.gz
    rm -fv $manpages/man1/chmcmd.1.gz
    rm -fv $manpages/man1/chmls.1.gz
    rm -fv $manpages/man1/data2inc.1.gz
    rm -fv $manpages/man1/delp.1.gz
    rm -fv $manpages/man1/fd2pascal.1.gz
    rm -fv $manpages/man1/fpc.1.gz
    rm -fv $manpages/man1/fpcjres.1.gz
    rm -fv $manpages/man1/fpclasschart.1.gz
    rm -fv $manpages/man1/fpcmake.1.gz
    rm -fv $manpages/man1/fpcmkcfg.1.gz
    rm -fv $manpages/man1/fpcres.1.gz
    rm -fv $manpages/man1/fpcsubst.1.gz
    rm -fv $manpages/man1/fpdoc.1.gz
    rm -fv $manpages/man1/fppkg.1.gz
    rm -fv $manpages/man1/fprcp.1.gz
    rm -fv $manpages/man1/grab_vcsa.1.gz
    rm -fv $manpages/man1/h2pas.1.gz
    rm -fv $manpages/man1/h2paspp.1.gz
    rm -fv $manpages/man1/makeskel.1.gz
    rm -fv $manpages/man1/pas2fpm.1.gz
    rm -fv $manpages/man1/pas2jni.1.gz
    rm -fv $manpages/man1/pas2ut.1.gz
    rm -fv $manpages/man1/plex.1.gz
    rm -fv $manpages/man1/postw32.1.gz
    rm -fv $manpages/man1/ppc386.1.gz
    rm -fv $manpages/man1/ppcarm.1.gz
    rm -fv $manpages/man1/ppcppc.1.gz
    rm -fv $manpages/man1/ppcsparc.1.gz
    rm -fv $manpages/man1/ppcx64.1.gz
    rm -fv $manpages/man1/ppdep.1.gz
    rm -fv $manpages/man1/ppudump.1.gz
    rm -fv $manpages/man1/ppufiles.1.gz
    rm -fv $manpages/man1/ppumove.1.gz
    rm -fv $manpages/man1/ptop.1.gz
    rm -fv $manpages/man1/pyacc.1.gz
    rm -fv $manpages/man1/rmcvsdir.1.gz
    rm -fv $manpages/man1/rstconv.1.gz
    rm -fv $manpages/man1/unitdiff.1.gz
    rm -fv $manpages/man5/fpc.cfg.5.gz
    rm -fv $manpages/man5/fpcmake.5.gz
    rm -fv $manpages/man5/ptop.cfg.5.gz
    
    rm -fv /Applications/Lazarus.app  
    
    rm -fv /etc/fpc.cfg
    rm -fv ~/.fpc.cfg
    rm -rfv ~/.fppkg
    

### FPC 3.x.x and Lazarus 2.0.11+ and 2.2.0

The script below will delete all versions of FPC and Lazarus which have been installed in the targeted locations using the official Lazarus installation packages (which also install FPC). These locations are correct for FPC 3.x.x and Lazarus 2.0.11, 2.0.12 and 2.2.0. 
    
    
    #!/bin/sh
    # Uninstaller for FPC 3.2.x and Lazarus 2.0.11, 2.0.12 and 2.2.0
    #
    bin=/usr/local/bin
    share=/usr/local/share
    lib=/usr/local/lib
    receipts=/private/var/db/receipts
    private=/private/etc
    fpcdoc="/Users/Shared/Free Pascal Compiler"
    manpages=/usr/local/share/man
    
    rm -fv $bin/ptop.rsj
    rm -fv $bin/unitdiff.rsj
    rm -fv $bin/fpclasschart.rsj
    rm -fv $bin/makeskel.rsj
    rm -fv $bin/fpcsubst.rsj
    rm -fv $bin/rstconv.rsj
    rm -fv $bin/fpcmkcfg.rsj
    rm -fv $bin/pas2ut.rsj
    rm -fv $bin/fd2pascal
    rm -fv $bin/h2paspp
    rm -fv $bin/postw32
    rm -fv $bin/mkarmins
    rm -fv $bin/mka64ins
    rm -fv $bin/data2inc
    rm -fv $bin/msg2inc
    rm -fv $bin/mkx86ins
    rm -fv $bin/ppdep
    rm -fv $bin/mkinsadd
    rm -fv $bin/chmls
    rm -fv $bin/rmcvsdir
    rm -fv $bin/fpc
    rm -fv $bin/mkz80ins
    rm -fv $bin/delp
    rm -fv $bin/h2paschk
    rm -fv $bin/plex
    rm -fv $bin/pyacc
    rm -fv $bin/eawparser
    rm -fv $bin/gbpparser
    rm -fv $bin/ppufiles
    rm -fv $bin/ppumove
    rm -fv $bin/chmcmd
    rm -fv $bin/fpcsubst
    rm -fv $bin/mkx86inl
    rm -fv $bin/fprcp
    rm -fv $bin/wasmtool
    rm -fv $bin/instantfpc
    rm -fv $bin/bin2obj
    rm -fv $bin/h2pas
    rm -fv $bin/wasa
    rm -fv $bin/fpcreslipo
    rm -fv $bin/fpcmake
    rm -fv $bin/unihelper
    rm -fv $bin/rstconv
    rm -fv $bin/fpcjres
    rm -fv $bin/pas2jni
    rm -fv $bin/fpcres
    rm -fv $bin/fpcmkcfg
    rm -fv $bin/ppudump
    rm -fv $bin/unitdiff
    rm -fv $bin/makeskel
    rm -fv $bin/cldrparser
    rm -fv $bin/ihxutil
    rm -fv $bin/ptop
    rm -fv $bin/fpclasschart
    rm -fv $bin/webidl2pas
    rm -fv $bin/json2pas
    rm -fv $bin/pas2fpm
    rm -fv $bin/pas2ut
    rm -fv $bin/fppkg
    rm -fv $bin/pas2js
    rm -fv $bin/compileserver
    rm -fv $bin/fpdoc
    rm -fv $bin/ppca64
    rm -fv $bin/ppcx64
    rm -fv $bin/ppc386
    
    rm -rfv $private/lazarus
    rm -rfv /Applications/Lazarus
    rm -rfv $share/fpcsrc
    rm -rfv $share/src/fpc-*
    rm -rfv $share/doc/fpc-*
    rm -rfv $lib/fpc
    
    rm -rfv $receipts/org.lazarus*
    rm -rfv $receipts/org.freepascal*
    
    rm -rfv "$fpcdoc"
    
    rm -fv $manpages/man1/bin2obj.1.gz
    rm -fv $manpages/man1/chmcmd.1.gz
    rm -fv $manpages/man1/chmls.1.gz
    rm -fv $manpages/man1/data2inc.1.gz
    rm -fv $manpages/man1/delp.1.gz
    rm -fv $manpages/man1/fd2pascal.1.gz
    rm -fv $manpages/man1/fpc.1.gz
    rm -fv $manpages/man1/fpcjres.1.gz
    rm -fv $manpages/man1/fpclasschart.1.gz
    rm -fv $manpages/man1/fpcmake.1.gz
    rm -fv $manpages/man1/fpcmkcfg.1.gz
    rm -fv $manpages/man1/fpcres.1.gz
    rm -fv $manpages/man1/fpcsubst.1.gz
    rm -fv $manpages/man1/fpdoc.1.gz
    rm -fv $manpages/man1/fppkg.1.gz
    rm -fv $manpages/man1/fprcp.1.gz
    rm -fv $manpages/man1/grab_vcsa.1.gz
    rm -fv $manpages/man1/h2pas.1.gz
    rm -fv $manpages/man1/h2paspp.1.gz
    rm -fv $manpages/man1/makeskel.1.gz
    rm -fv $manpages/man1/pas2fpm.1.gz
    rm -fv $manpages/man1/pas2jni.1.gz
    rm -fv $manpages/man1/pas2ut.1.gz
    rm -fv $manpages/man1/plex.1.gz
    rm -fv $manpages/man1/postw32.1.gz
    rm -fv $manpages/man1/ppc386.1.gz
    rm -fv $manpages/man1/ppcarm.1.gz
    rm -fv $manpages/man1/ppcppc.1.gz
    rm -fv $manpages/man1/ppcsparc.1.gz
    rm -fv $manpages/man1/ppcx64.1.gz
    rm -fv $manpages/man1/ppdep.1.gz
    rm -fv $manpages/man1/ppudump.1.gz
    rm -fv $manpages/man1/ppufiles.1.gz
    rm -fv $manpages/man1/ppumove.1.gz
    rm -fv $manpages/man1/ptop.1.gz
    rm -fv $manpages/man1/pyacc.1.gz
    rm -fv $manpages/man1/rmcvsdir.1.gz
    rm -fv $manpages/man1/rstconv.1.gz
    rm -fv $manpages/man1/unitdiff.1.gz
    rm -fv $manpages/man5/fpc.cfg.5.gz
    rm -fv $manpages/man5/fpcmake.5.gz
    rm -fv $manpages/man5/ptop.cfg.5.gz
    
    rm -fv /etc/fpc.cfg
    rm -fv ~/.fpc.cfg
    rm -rfv ~/.fppkg
    

To run either of the above scripts, open an Applications > Utilities > Terminal, and enter: 
    
    
    cd <replace this text and angle brackets with the name of the directory where the script is stored>  
    chmod +x uninstallLaz.sh
    sudo ./uninstallLaz.sh
    

Then enter your password when prompted. 

### Lazarus preference folder .lazarus in the home directory

Lazarus also creates a .lazarus preferences subdirectory in your home directory where it stores environment settings and a list of recently opened projects and files. You can leave this folder alone if you want the new version of Lazarus you're installing to use your old settings. macOS normally doesn't show files or folders whose names start with a dot (.). To see this folder and its contents, open an Applications > Utilities > Terminal and enter the following in the Terminal window: 
    
    
    cd .lazarus
    ls -al
    

To remove it, execute the following commands in a Terminal: 
    
    
    cd
    rm -rfv .lazarus
    

### Third party package uninstaller

If you have installed Lazarus using the official package files, then you can also use a third party uninstaller like [UninstallPKG](<https://www.corecode.io/uninstallpkg/>) from Core Code. You can perform 5 uninstallations for free before you need to buy it ($US 9.99/Euro 9.99).

---

_Source: [https://wiki.freepascal.org/Uninstalling_Lazarus_on_macOS](https://web.archive.org/web/20250426005138/https://wiki.freepascal.org/Uninstalling_Lazarus_on_macOS)_
