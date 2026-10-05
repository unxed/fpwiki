# Allegro.pas tutorial 0

This page explains everything you need to install Allegro.pas correctly. 

## Contents

  * 1 Before installing Allegro.pas
    * 1.1 The Allegro library
    * 1.2 The Pascal compiler
  * 2 Install Allegro.pas
    * 2.1 Testing
    * 2.2 Fix problems
  * 3 Installing the library



## Before installing Allegro.pas

Before to install [Allegro.pas](<FPC_and_Allegro.md> "FPC and Allegro") you need to install the original library ([Allegro](<http://liballeg.org/>)) and a compatible Pascal compiler (i.e. Free Pascal). 

### The Allegro library

You should remember: _“Allegro.pas is a wrapper to use the Allegro library with Pascal compilers”_. Allegro and Allegro.pas are different and independent projects and it's important to not mix them. So you need the original Allegro library to make Allegro.pas works. 

Last Allegro.pas version is compatible with the 5.2 branch of the Allegro library. That means it can link with any version that starts with numbers 5.2 (i.e. 5.2.0 RC, 5.2.0, 5.42.1, etc.) but not with previous (4.2, 3.0) or more modern ones (4.9, 5.0). 

If you're a Windows user you can get a pre-compiled version of Allegro from [its web site](<http://liballeg.org/download.html>). 

Linux users' best option is to download the sources from Allegro's site and compile by themselves. There are complete instructions to do this in the “docs/build” subdirectory. 

### The Pascal compiler

**Note:** Current version should be compatible with Delphi, but it wasn't fully tested. 

The best compiler to use Allegro.pas is Free Pascal (AKA: FPC) which can be installed separately or with the Lazarus IDE. 

Both FPC and Lazarus are easy to install. Just go to its webpage, select the Download section, get the package that fits better with your system and follow the instructions. 

Remember that Lazarus includes FPC so there's no need to download it. 

## Install Allegro.pas

Once you have the compiler and the Allegro library installed, you're prepared to install Allegro.pas. 

### Testing

Just to be sure you have everything you need you should compile the examples and the demo game. To do this open a console (cmd on Windows, bash or XTerm on GNU/Linux, etc.). Then go to the directory where you have unzipped Allegro.pas. For example, type: 
    
    
    cd allegro.pas
    

Note you should modify "allegro.pas" by the name of the directory. 

Now run the fix script (on Windows type `fix.bat` or `fix.cmd` and press intro, on GNU/Linux type `./fix.sh` and press intro) and follow on-screen instructions. 

Once it says _“Done”_ just type `make` and press intro. This should compile everything. If you have error messages then read below. 

Now try to run the examples and the demo game. 

### Fix problems

If compiling the demo or the examples you get an error telling `”ld: cannot find -lalleg”` or similar, first be sure you'd installed Allegro properly (read above). If problem persist, may be your CMake script didn't updated the library cache. So try to run `ldconfig` from console or reboot your computer. This should fix the problem. 

If you can't fix this please ask at the [Pascal Game Development](<http://www.pascalgamedevelopment.com/>) forums, at [Lazarus forums](<http://forum.lazarus.freepascal.org/index.php>) or at the [maling list](<https://sourceforge.net/p/allegro-pas/mailman/>). 

## Installing the library

You can copy the Allegro.pas library to the FPC's unit subdirectory so it will be available to all your projects without need to reconfigure it or add command options to the compilation line. 

First localize where FPC has the RTL units: 

  * On Windows systems it should be inside the directory where you installed FPC or Lazarus. If you installed Lazarus there should be a subdirectory named “`fpc`”.
  * On GNU/Linux systems a common place is the `/usr/share/lib` subdirectory.



In any case you should look for a subdirectory named `fpc/<version_number>/units/<platform>/` or similar (`<version_number>` is the version of your FPC compiler and `<platform>` is the name of your system platform, such as _i386_). 

Now create a subdirectory named `allegro` or `allegro5` and copy the contents of the lib subdirectory (may be you'll need administrator permissions to do this).

---

_Source: [https://wiki.freepascal.org/Allegro.pas_tutorial_0](https://web.archive.org/web/20190826093021/https://wiki.freepascal.org/Allegro.pas_tutorial_0)_
