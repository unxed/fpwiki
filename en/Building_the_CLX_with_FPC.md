# Building the CLX with FPC

## Contents

  * 1 Introduction
  * 2 Status
  * 3 Howto
    * 3.1 Building the DB units
  * 4 Todo



## Introduction

There are some efforts to build the (Free)CLX with FPC, here is a short explanation how to build it. For Win32 the Delphi 7 CLX is used while on linux the Kylix 3 CLX is used. 

Further, FPC from svn (2.3.1 or 2.2.1) is used. 

## Status

Current status is that all units except the database related units build. 

That's 39 of 44 units. 

Today (02.07.2007) I get perfectly compiled Qxxx units and test application (written with Kylix). fpc-2.0.4 (i386 rpm from lazarus snapshots build 20070701), fpc-2.1.5 (build 20070702) cannot do the job. 

## Howto

  * Build and install FPC from SVN sources: [How_to_start](<How_to_start.md> "How to start")
  * copy the clx sources to a newly created directory as well as the 
    * Win32 only: CommDlg.pas from Source/Rtl/Win/
    * Win32 only: CommCtrl.pas from Source/Rtl/Win/
    * Win32 only: ShlObj.pas from Source/Rtl/Win/
    * Win32 only: RegStr.pas from Source/Rtl/Win/
    * Win32 only: WinInet.pas from Source/Rtl/Win/
    * Win32 only: UrlMon.pas from Source/Rtl/Win/
    * Win32 only: WinSpool.pas from Source/Rtl/Win/
    * Linux only: XLib.pas from source/rtl/linux/
    * MaskUtils.pas from Source/Rtl/Common/
    * Contnrs.pas from Source/Rtl/Common/
    * HelpIntfs.pas from Sources/Common/
    * IniFiles.pas from Source/Rtl/Common/
    * Registry.pas from Source/Rtl/Common/
    * Linux only: QFileCtrls.res from Lib/ ~~and rename it to qfilectrls.res~~
    * ~~Linux only: rename DirSel.xfm to dirsel.xfm~~
    * QButtons.res from Lib/
    * QDialogs.res from Lib/
    * QDBCtrls.res from Lib/
    * QCheckLst.res from Lib/
    * QFileCtrls.res from Lib/
    * ~~Linux only: rename QFileDialog.res to qfiledialog.res~~
    * ~~Linux only: rename QDialogs.res to qdialogs.res~~
    * ~~Linux only: rename QDBPWDlg.xfm to qdbpwdlg.xfm~~
    * ~~Linux only: rename QDBLogDlg.xfm to qdblogdlg.xfm~~
  * ~~remove all references to *.xfm files, fpc can't handled them yet (fix this!!)~~
  * D7 CLX only: remove usage of sysinit namespace in qdialogs.pas; system can be used instead
  * ~~replace all RTLConsts with RTLConst (fix this!!) (should be fixed after 1.9.8)~~
  * D7 CLX only: fix ShlObj.pas: the lines 3554 to 3557 miss a semicolon at the end; FPC won't emulate bugs in the Delphi parser
  * ~~Linux ONLY : Add dynlibs unit after implementation (LoadLibrary & FreeLibrary needs it).~~ Compiling with the -Sk switch should be enough
  * Remove sysutils from uses clause of implementation section of QStyle.pas
  * 2.2.1 only: 
    * Change {$R *.xfm} to {$R *.res} in QFileDialog.pas
    * Convert QFileDialog.xfm to resource. It can be done with fpcres utility **from a 2.3.1 snapshot** : `fpcres -of res QFileDialog.xfm`
  * create a dummy unit to build the clx:  



    
    
    unit buildclx;
      interface
       uses
         BindHelp,   QDBActns,   QFileCtrls,
         QActnList,  QFileDialog,  QSearch,
         QButtons,   QForms,     QStdActns,
         QCheckLst,  QGraphics,  QStdCtrls,
         QClipbrd,   QGrids,     QStyle,
         QComCtrls,  QImgList,   QTypes,
         QConsts,    QDialogs,   QMask,
         QControls,  QExtCtrls,  QMenus,
         Qt,         QPrinters;
      implementation
    end.
    

  * build the CLX with


    
    
     fpc -Sd buildclx -Rintel 
    

on windows and 
    
    
     fpc -Sdk buildclx -Rintel 
    

on linux. 

### Building the DB units

Building of the DB units isn't possible yet, however, at least the following steps must be done: 

  * copy the following files into the directory where you put the clx source: 
    * DB.pas from Sources/Vcl
    * SqlTimSt.pas from Sources/Vcl
    * DBConsts.pas from Sources/Vcl



## Todo

As you might guess, there is also stuff to do: 

  * The ClassGrouping functions are only dummies in FPC (StartClassGroup et al.)

---

_Source: [https://wiki.freepascal.org/Building_the_CLX_with_FPC](https://web.archive.org/web/20230128011145/https://wiki.freepascal.org/Building_the_CLX_with_FPC)_
