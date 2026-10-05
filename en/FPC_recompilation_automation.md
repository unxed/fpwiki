# FPC recompilation automation

When you want to change something in FPC, you will have to repeat several commands many times to rebuild FPC and test your changes. Here is a script to automate this process. 

  


## Contents

  * 1 FPC rebuilding script for Windows
  * 2 FPC rebuilding script for Linux



  


# FPC rebuilding script for Windows

Save the following script as **rebuild-trunk.bat**. Tested on Windows 10 x64, FPC 3.3.1 with 3.2.2 bootstrap, and 32 bit FPCUPDELUXE 1.8.2u. 
    
    
    @echo off
    
    echo ***************************************************************************
    echo *                                                                         *
    echo *  TRUNK REBUILDER 1.0                                                    *
    echo *  by Zeljko Avramovic (c) 2021                                           *
    echo *                                                                         *
    echo *  used for easy FPC RTL changes recompilation automation                 *
    echo *                                                                         *
    echo *  1. Download FPC and LAZ trunks with FPCUPDELUXE                        *
    echo *  2. Set FPCUPDELUXEDIR, BOOTSTRAPDIR and THREADS variables in BAT file  *
    echo *  3. Change RTL or any other FPC source files                            *
    echo *  4. Run this BAT file to rebuild FPC                                    *
    echo *  5. Rebuild Lazarus if needed (only if your changes affect it)          *
    echo *                                                                         *
    echo ***************************************************************************
    
    REM adjust these variables according to your system:
    set BOOTSTRAPDIR=c:\Prg\Lazarus\fpc.3.2.2\bin\i386-win32
    set FPCUPDELUXEDIR=c:\Prg\Lazarus\TrunkAll
    set THREADS=8
    
    set FPCDIR=%FPCUPDELUXEDIR%\fpc
    set STARTDIR=%cd%
    set STARTTIME=%time%
    
    cd /d %FPCUPDELUXEDIR%\fpcsrc
    
    echo.
    echo *****************
    echo *  1. CLEAN     *
    echo *****************
    echo. 
    %BOOTSTRAPDIR%\make.exe distclean -j %THREADS% FPMAKEOPT="-T %THREADS%"
    set ERR=1 (CLEAN)
    if %ERRORLEVEL% neq 0 goto :error
    
    echo.
    echo *****************
    echo *  2. MAKE      *
    echo *****************
    echo.
    %BOOTSTRAPDIR%\make.exe all -j %THREADS% FPMAKEOPT="-T %THREADS%"
    set ERR=2 (MAKE)
    if %ERRORLEVEL% neq 0 goto :error
    
    echo.
    echo *****************
    echo *  3. INSTALL   *
    echo *****************
    echo.
    %BOOTSTRAPDIR%\make.exe install INSTALL_PREFIX=%FPCDIR% -j %THREADS% FPMAKEOPT="-T %THREADS%"
    set ERR=3 (INSTALL)
    if %ERRORLEVEL% neq 0 goto :error
    
    echo.
    echo *****************
    echo *  4. FINISHED  *
    echo *****************
    echo.
    echo %STARTTIME%
    echo %TIME%
    set ERR=0 (OK)
    goto :end
    
    :error 
    echo {{{ ERROR %ERR% }}}
      
    :end
    cd %STARTDIR%
    
    REM beep
    echo �
    

# FPC rebuilding script for Linux

T.B.D.

---

_Source: [https://wiki.freepascal.org/FPC_recompilation_automation](https://web.archive.org/web/20241213014348/https://wiki.freepascal.org/FPC_recompilation_automation)_
