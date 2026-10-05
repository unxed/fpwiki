# Lazarus Installer

There are various official installers/packages for different platforms/OSes. 

## Contents

  * 1 Lazarus Windows Installer
    * 1.1 Creating your own installer
  * 2 Linux packages
    * 2.1 RPM package
    * 2.2 deb package
  * 3 macOS image/package



## Lazarus Windows Installer

The Lazarus Windows installer is written for the [Inno Setup](<Inno_Setup.md> "Inno Setup") compiler. 

See [here](<http://wiki.lazarus.freepascal.org/Inno_Setup#Examples>) for a link to the location of the .iss file in your Lazrus directory. 

### Creating your own installer

You may want to create your own installer; good instructions are provided by the readme in $(LazarusDir)\tools\install\win\ 

Below is a list of commands interspersed with instructions/comments that is based on that readme and that was sucesfully used to build an installer from Lazarus SVN trunk (1.1 at the time) and FPC 2.6.1 (fixes 2.6). 

I had installed the binutils, Lazarus and FPC first using [fpcup](<Projects_using_Lazarus.md> "Projects using Lazarus") so the paths below reflect the default fpcup paths. Of course, you can modify to your situation. 
    
    
    rem A.1 Inno Setup
    rem I used Inno Setup version 5.5.1 (unicode). You can use Unicode Inno setup from Lazarus r38111 (thanks, Martin Friebe!)
    rem but it won't support Win9x
    rem If you want to support Win9x, use an Ansi Inno Setup lower than 5.5.0
    
    rem A.3 FPC build sources:
    rem Using fixes 2.6 here (the fixes to the latest stable version at this time)
    cd /d c:\development
    mkdir c:\development\fpcbuild
    svn checkout http://svn.freepascal.org/svn/fpcbuild/branches/fixes_2_6_0/ c:\development\fpcbuild
    
    rem A.4 Lazarus sources:
    rem I've got my Lazarus sources in c:\development\lazarus (downloaded by fpcup)
    
    rem A.5 Lazarus binaries
    rem Hum, where is the svn for that? Let's see:
    mkdir c:\development\lazbinaries
    svn checkout http://svn.freepascal.org/svn/lazarus/binaries/ c:\development\lazbinaries\
    
    rem A.6 Latest release fpc compiler
    rem I have a 2.6.0 i386 compiler in c:\development\fpcbootstrap\ppc386.exe (downloaded by fpcup)
    
    rem B adjust C:\Development\lazarus\tools\install\win\create_installer.bat
    rem didn't do that, instead set environment variables to my situation
    mkdir c:\development\lazarusbuild
    
    rem Set BUILDDIR to c:\development\lazarusbuild, by setting LAZTEMPBUILDDIR
    SET LAZTEMPBUILDDIR=c:\development\lazarusbuild
    
    rem set SVN to my situation: 
    SET SVN="D:\Program Files\SlikSvn\bin\svn.exe"
    
    rem set ISCC to my situation:
    SET ISCC="D:\Program Files (x86)\Inno Setup 5\ISCC.exe"
    
    rem check:
    SET
    
    rem C run the script
    cd /d C:\Development\lazarus\tools\install\win
    create_installer.bat c:\development\fpcbuild c:\development\lazarus c:\development\lazbinaries c:\development\fpcbootstrap\ppc386.exe
    

The log ends up in C:\Development\lazarus\tools\install\win\installer.log The output setup.exe ends up in a subdirectory Output 

## Linux packages

### RPM package

Lazarus is packaged as an RPM package. 

### deb package

Lazarus is packaged as a deb package. 

## macOS image/package

Lazarus is packaged as an macOS .dmg disk image (32 bit) or .pkg installation package (64 bit).

---

_Source: [https://wiki.freepascal.org/Lazarus_Installer](https://web.archive.org/web/20250122190725/https://wiki.freepascal.org/Lazarus_Installer)_
