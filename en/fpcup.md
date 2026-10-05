# fpcup

│ **English (en)** │

## Contents

  * 1 About
  * 2 Ready to run
  * 3 Documentation
  * 4 Tips
    * 4.1 Anchordocking
    * 4.2 Building only FPC
      * 4.2.1 Settings
    * 4.3 Avoid lengthy lcl.chm documentation generation
  * 5 OS-specific notes
    * 5.1 Running on Raspberry Pi Raspbian
    * 5.2 Running on Windows 2000
  * 6 Cross compilers
    * 6.1 Status
    * 6.2 AIX PowerPC crosscompiler
      * 6.2.1 Required files
      * 6.2.2 Settings
      * 6.2.3 Creating and using the cross compiler
    * 6.3 Android ARM crosscompiler
      * 6.3.1 Required files
      * 6.3.2 Settings
      * 6.3.3 Creating and using the cross compiler
    * 6.4 Linux ARM cross compiler
      * 6.4.1 Required files
      * 6.4.2 Settings
      * 6.4.3 Creating and using the cross compiler
    * 6.5 Linux i386 cross
      * 6.5.1 Required files
      * 6.5.2 Settings
      * 6.5.3 Creating and using the cross compiler
    * 6.6 DOS crosscompiler
      * 6.6.1 Required files
      * 6.6.2 Settings
      * 6.6.3 Creating and using the cross compiler
  * 7 Download
  * 8 License
  * 9 Troubleshooting
  * 10 Known issues
    * 10.1 No rule to make target ...
    * 10.2 Strange errors running clean/distclean
    * 10.3 Info: Error running fpcup. Technical details: error executing sequence helplazarus; line: 4, param: helplazarus
    * 10.4 Error finding (e.g.) system.ppu when compiling with FPC/Lazarus
  * 11 Support and bug reporting
  * 12 Supported platforms
  * 13 Changelog
  * 14 External links
  * 15 See also



## About

fpcup is a tool that automates running a subversion client to build FPC and/or Lazarus in a self-contained directory structure. It is meant as a thin wrapper around the regular manual svn up/make/make install build process. 

It was made by and maintained by the late Reinier Olislagers (BigChimp). His version can still be found at [fpcup](<https://bitbucket.org/reiniero/fpcup>)

Current maintained version can be found at [new fpcup](<https://github.com/LongDirtyAnimAlf/Reiniero-fpcup>)

Lazarus 1.st time users will probably 

  * run the downloaded fpcup binary (e.g. **fpcup_linux_x64** on a Linux 64 bit). Once that installed a working Lazarus IDE, one could
  * download the fpcup Pascal project (i.e. the sources, in addition to the above binary) and
  * compile those to be able to run the native fpcup GUI that comes with that fpcup Pascal project.
  * Later, one may want to switch to one's favourite SVN GUI (possibly kdesvn), using 'Apache Subversion' with the SVN URL: **http://svn.freepascal.org/svn/fpc/trunk**
    * in the Linux app kdesvn you can _OPEN_ the URL **https://svn.code.sf.net/p/lazarus-ccr/svn** and once loaded, bookmark it. Then _CHECKOUT_ stuff you need to your local hard drive.



  
On Windows, it can also serve as a "bare metal" installer: it can download a subversion client, binutils (make.exe etc). 

Think of it as a batch file on steroids that does the boring svn work for you. 

fpcup allows maintaining multiple different FPC/Lazarus combinations. 

It is a third party tool, not part of FPC or Lazarus. 

The fpcup GUI which is provided with the source code: [![fpcupgui.png](https://wiki.freepascal.org/images/b/b8/fpcupgui.png)](</File:fpcupgui.png>)

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** Here is a confortable successor to fpcup 

  * [fpcupdeluxe](<fpcupdeluxe.md> "fpcupdeluxe")



## Ready to run

fpcup is ready to run out of the box. It will download and install FPC and Lazarus. On Windows, this directory layout is used: 
    
    
    C:\development\config_lazarus # Default primary-config-path directory 
                                  # (contains Lazarus settings)
    C:\development\cross          # Place to store cross compiling binutils and libs if wanted
    C:\development\extras         # Third party modules (see fpcup.ini) such as fpcdocs
    C:\development\fpcbootstrap   # Binutils (make.exe etc) and bootstrap compiler
    C:\development\fpc            # FPC fixes 2.6 by default
    C:\development\lazarus        # Lazarus trunk by default
    

On Linux/macOS/BSD, a similar layout is used under ~ (the user's home directory). 

fpcup will create a shortcut to the installed Lazarus on your desktop. 

Just run fpcup, e.g. on Windows: 
    
    
    fpcup
    

On UNIX: 
    
    
    #replace fpcup_linux_x86 with your actual fpcup filename of course if not on linux x86, e.g. 64-bit Linux or other variant
    
    chmod u+rx fpcup_linux_x86 
    #make executable (one time only action)
    
    ./fpcup_linux_x86 
    #run
    

Next time, you can either run the exact same command, or run the fpcup_update desktop shortcut or home directory shell script/batch file. 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** fpcup is set up to compile FPC 3.0 fixes and Lazarus 1.4 fixes by default

## Documentation

fpcup has 

  * a -h option
  * a [readme.txt](<https://raw.githubusercontent.com/LongDirtyAnimAlf/Reiniero-fpcup/master/Readme.TXT>)



See the [source code repository](<https://github.com/LongDirtyAnimAlf/Reiniero-fpcup>) for the latest readme.txt. 

To make it easier to get user contributions for documentation/tips, this wiki page has been created. Please feel free to add your own examples etc. 

## Tips

### Anchordocking

One way to enable anchordocking is by using settings.ini: open your settings file or add a new file `settings.ini`
    
    
    [anchordocking]
    ; Just a plain install except enable anchordocking package
    anchordocking=1
    

This will use default settings but also enable the anchordocking package in the IDE. 

Of course, you can use the anchordocking=1 module setting in any .ini section. 

You can also just run fpcup with just a single module: fpcup --only="anchordocking". 

### Building only FPC

You can build FPC without Lazarus if you want (or the other way round). The example below builds FPC from the fixes 2.6 SVN branch. 

#### Settings

Open your settings file or add a new file, `settings.ini`: 
    
    
    [fixes26]
    ; FPC 2.6 fixes (no Lazarus):
    fpcdir=c:\development\fpcfixes26
    ; replace e.g. with 
    ; http://svn.freepascal.org/svn/fpc/trunk
    ; for FPC trunk
    fpcurl=http://svn.freepascal.org/svn/fpc/branches/fixes_2_6
    ; Let's specify Lazarus info in case we ever want to add Lazarus
    lazdir=c:\development\lazarus12_fixes26
    ; Just an example: build Lazarus 1.2 - if we're building Lazarus
    lazurl=http://svn.freepascal.org/svn/lazarus/tags/lazarus_1_2
    primary-config-path=c:\development\configlazarus12_fixes26
    ; If we ever build Lazarus, do create a link/shortcut:
    lazlinkname=lazarus12_fixes26
    ; Do not create a batch file with shortcut
    fpcuplinkname=""
    ; We can use the binutils/bootstrap dirs that we usually use:
    binutilsdir=c:\development\fpcbootstrap
    fpcbootstrapdir=c:\development\fpcbootstrap
    ; Just install/update, no questions asked:
    noconfirm=true
    ; In case you want to submit patches, it's nice to be able to update
    ; without overwriting your fixes:
    keeplocalchanges=true
    ; Specify we only want FPC, not Lazarus
    ; in case of fpc patches FPCBuildOnly
    only=FPC,FPCCrossWin32-64
    skip=helplazarus,lazarus,lazbuild,bigide,useride
    

Then get FPC and build it with: 
    
    
    fpcup --inifile=settings.ini --inisection=fixes26
    

Now you can run your compiler: 
    
    
    c:\development\fpcfixes26\bin\i386-win32\fpc.exe -h
    

Cross-compile for win64, e.g.: 
    
    
    c:\development\fpcfixes26\bin\i386-win32\fpc.exe -Px86_64 yourfile.pas
    

### Avoid lengthy lcl.chm documentation generation

By default, fpcup tries to compile the `$(lazarusdir)\docs\chm\lcl.chm` Lazarus help file every day or so. This can take a long time due to the fpdoc tool being quite slow. 

You can avoid compiling lcl.chm by generating it one time (or copying over an existing file) and making the file read-only. 

## OS-specific notes

Specific notes for operating systems are given below if they are different from the general instructions. 

### Running on Raspberry Pi Raspbian

Open your settings file (e.g. `settings.ini` and include _at least_ these lines 
    
    
    ; ARMHF needs FPC >= 3.0; 2.6.x does not work (unless it is a patched version)
    fpcurl=http://svn.freepascal.org/svn/fpc/trunk
    ; if using armhf, you MUST specify -dFPC_ARMHF in the FPC options
    fpcopt="-dFPC_ARMHF -CaEABIHF -CpARMV6 -CfVFPV2"
    

### Running on Windows 2000

Normally, fpcup tries to download an svn client if it cannot find an existing client. This client will not work on Windows 2000 due to missing libraries, so please download and install an svn command line client - such as SlikSVN 1.6.16. 

## Cross compilers

fpcup not only lets you install native FPC/Lazarus, but will also build cross compilers for you. 

First, you should have set up your normal FPC (and optionally Lazarus) using fpcup. Then you let fpcup generate the cross compiler. 

For most cross compilers, you do need to have the relevant binutils that are specific for the host<>target system (so not the normal binutils but cross binutils). 

Often, you will also need libraries in target system format if you want to compile programs (e.g. Linux Qt or Gtk libraries for the relevant CPU if you're crosscompiling for Linux). 

When you run a crosscompiler, fpcup briefly checks if it can find required binutils and libraries and will warn you if they can't be found. 

### Status

  * Built in means no additional files are needed
  * Needs binutils/libs: means that you need to supply those (see above)
  * N/A: means non-applicable/impossible
  * No: means no plans to implement this. However, contributions are welcome, as always

From\To  | DOS  | GO32v2  | Windows x86  | Windows x64  | Linux x64  | Linux x86  | Linux mipsel  | Linux arm  | Android ARM  | FreeBSD x64  | FreeBSD x86  | macOS  | AIX powerpc32   
---|---|---|---|---|---|---|---|---|---|---|---|---|---  
Any  |  |  |  |  |  | needs binutils,libs  | needs binutils,libs  | needs binutils,libs  | needs binutils,libs  |  |  |  | needs binutils,libs   
Windows x86  | needs FPC trunk, binutils  | yes, built in  | N/A  | yes  | needs binutils,libs  | needs binutils,libs  | see Any  | see Any  |  |  |  |  | See Any   
Windows x64  | no  | no due to FPC limitations  | needs FPC trunk  | N/A  | needs binutils,libs  | needs binutils,libs  | see Any  | see Any  |  |  |  |  | See Any   
Linux x64  | no  |  | yes, built in  | yes, built in  | N/A  |  | see Any  | see Any  |  | no plans  | no plans  |  | See Any   
Linux x86  |  |  | yes, built in  | yes, built in  |  | N/A  | see Any  | see Any  |  | no plans  | no plans  |  | See Any   
Linux mipsel  |  |  |  |  |  |  | N/A  | see Any  |  |  |  |  | See Any   
Linux arm  |  |  |  |  |  |  | see Any  | N/A  |  |  |  |  | See Any   
Android ARM  |  |  |  |  |  |  | see Any  | see Any  | N/A  |  |  |  | See Any   
FreeBSD x64  | no  | no plans  | yes, built in  |  |  |  | see Any  | see Any  |  | N/A  |  |  | See Any   
FreeBSD x86  |  | no plans  | yes, built in  |  |  |  | see Any  | see Any  |  |  | N/A  |  | See Any   
macOS  |  |  | verify  |  |  |  | see Any  | see Any  |  |  |  | N/A  | See Any   
  
(With thanks to [FPSpreadsheet](<FPSpreadsheet.md> "FPSpreadsheet")'s spreadsheet to wikitable conversion tool, see e.g. examples\fpsgrid) 

### AIX PowerPC crosscompiler

Once you have set up a regular FPC trunk/2.7.1 compiler, you can add an [FPC AIX Port](<FPC_AIX_Port.md> "FPC AIX Port") cross compiler. 

Limitations: 

  * FPC trunk/2.7.1+ only
  * untested; don't have target platform libs (or access to target platform). Test reports welcome.



#### Required files

  * Crossbinutils: see wiki article mentioned earlier. Win32 cross binutils are provided; for *nix, you have to compile your own.
  * Libs: copy over from target device; see wiki article as well.



#### Settings

Open your settings file or add a new file, settings.ini: 
    
    
    [aix]
    ; AIX on powerpc32 cross compiler. Also runs on 64 bit powerpc
    ; a full fpcup run must have been done before
    ; please adjust paths etc
    fpcdir=c:\development\fpctrunk
    fpcurl=http://svn.freepascal.org/svn/fpc/trunk
    fpcopt=""
    fpcuplinkname=""
    lazlinkname=""
    binutilsdir=c:\development\fpcbootstrap
    fpcbootstrapdir=c:\development\fpcbootstrap
    noconfirm=true
    keeplocalchanges=true
    cputarget=powerpc
    fpcopt=""
    crossopt=""
    ostarget=aix
    ; Perhaps best to always clean... can be removed for speed
    only=FPCCleanOnly,FPCBuildOnly
    skip=FPCGetOnly,lazbuild,bigide,useride
    

See also: 

  * any (cross compiler) page about your device
  * your AIX documentation



#### Creating and using the cross compiler

Then generate the cross compiler with 
    
    
    fpcup --inifile=settings.ini --inisection=aix
    

If, like me, you don't have the libs from the target platform and run with `--verbose`, you'll see something like: 
    
    
    C:\development\cross\bin\powerpc-aix\powerpc-aix-ld.exe: cannot find \lib\crt0.o
    C:\development\cross\bin\powerpc-aix\powerpc-aix-ld.exe: cannot find -lm
    C:\development\cross\bin\powerpc-aix\powerpc-aix-ld.exe: cannot find -lc
    C:\development\cross\bin\powerpc-aix\powerpc-aix-ld.exe: cannot find -lbsd
    

If the compiler generation went ok, you can run a test compile, compiling a file test.pas with 

  * option -CN: avoid reading for null pointers on AIX; see wiki article


    
    
    fpc -Ppowerpc -Taix -CN test.pas
    

(your fpc.cfg will have been modified to add some settings when cross-compiling to Android ARM) 

  


### Android ARM crosscompiler

Once you have set up a regular FPC trunk/2.7.1 compiler, you can add an [Android](<Android.md> "Android") cross compiler. 

Limitations: 

  * only tested on FPC trunk
  * only builds FPC cross compiler, Android does not have "regular" LCL support so not enabled
  * patches for getting customdrawn support going are welcome



#### Required files

Download the Android NDK (e.g. a file like android-sdk_r22.3-windows.zip) for your platform and extract the crossbinutils, e.g. from a path like 
    
    
    android-ndk-r9c\toolchains\arm-linux-androideabi-4.8\prebuilt\windows-x86_64\bin
    

The result is something _like_ these files (adjust for your platform etc): 
    
    
    arm-linux-androideabi-addr2line.exe
    arm-linux-androideabi-ar.exe
    arm-linux-androideabi-as.exe
    arm-linux-androideabi-c++.exe
    arm-linux-androideabi-c++filt.exe
    arm-linux-androideabi-cpp.exe
    arm-linux-androideabi-elfedit.exe
    arm-linux-androideabi-g++.exe
    arm-linux-androideabi-gcc-4.8.exe
    arm-linux-androideabi-gcc-ar.exe
    arm-linux-androideabi-gcc-nm.exe
    arm-linux-androideabi-gcc-ranlib.exe
    arm-linux-androideabi-gcc.exe
    arm-linux-androideabi-gcov.exe
    arm-linux-androideabi-gdb-orig.exe
    arm-linux-androideabi-gdb.exe
    arm-linux-androideabi-gprof.exe
    arm-linux-androideabi-ld.bfd.exe
    arm-linux-androideabi-ld.exe
    arm-linux-androideabi-ld.gold.exe
    arm-linux-androideabi-ld.mcld.exe
    arm-linux-androideabi-nm.exe
    arm-linux-androideabi-objcopy.exe
    arm-linux-androideabi-objdump.exe
    arm-linux-androideabi-ranlib.exe
    arm-linux-androideabi-readelf.exe
    arm-linux-androideabi-size.exe
    arm-linux-androideabi-strings.exe
    arm-linux-androideabi-strip.exe
    

Put these files in your cross binutils directory for the right platform - e.g. on a standard Windows fpcup install: 
    
    
    C:\development\cross\bin\arm-android
    

Libs: extract the libs from a path like 
    
    
    android-ndk-r9c\platforms\android-19\arch-arm\usr\lib
    

into 
    
    
    C:\development\cross\lib\arm-android
    

The libs in question can be _like_ this list: 
    
    
    crtbegin_dynamic.o
    crtbegin_so.o
    crtbegin_static.o
    crtend_android.o
    crtend_so.o
    libandroid.so
    libc.a
    libc.so
    libdl.so
    libEGL.so
    libGLESv1_CM.so
    libGLESv2.so
    libGLESv3.so
    libjnigraphics.so
    liblog.so
    libm.a
    libm.so
    libm_hard.a
    libOpenMAXAL.so
    libOpenSLES.so
    libstdc++.a
    libstdc++.so
    libthread_db.so
    libz.so
    rs
    

#### Settings

Open your settings file or add a new file, settings.ini: 
    
    
    [android]
    ; Android ARM cross compiler
    ; a full fpcup run must have been done before
    ; please adjust paths etc
    fpcdir=c:\development\fpctrunk
    fpcurl=http://svn.freepascal.org/svn/fpc/trunk
    fpcopt=""
    fpcuplinkname=""
    lazlinkname=""
    binutilsdir=c:\development\fpcbootstrap
    fpcbootstrapdir=c:\development\fpcbootstrap
    cputarget=arm
    ostarget=android
    ; You can specify exact instruction set and floating point support etc:
    crossopt="-CpARMV7 -CfVFPV3"
    ; ... or something more conservative/compatible:
    ;crossopt="-CpARMV6"
    verbose=0
    ; We only need an FPC here; assume FPC has been updated by regular update
    only=FPCBuildOnly
    ; No LCL support on Android yet
    

See also: 

  * [ARM compiler options](<ARM_compiler_options.md> "ARM compiler options")
  * any (cross compiler) page about your device
  * your ARM device documentation



#### Creating and using the cross compiler

Then generate the cross compiler with 
    
    
    fpcup --inifile=settings.ini --inisection=android
    

Now you can run a test compile, compiling a file test.pas with 
    
    
    fpc -Parm -Tandroid test.pas
    

(your fpc.cfg will have been modified to add some settings when cross-compiling to Android ARM) 

### Linux ARM cross compiler

Once you have set up a regular FPC compiler, you can add a Linux on ARM cross compiler (e.g. for a Raspbian/Raspberry Pi). 

#### Required files

  * Linux/Unix build or install the relevant crossbinutils (Linux, unix)
  * Windows: download the binutils from <http://svn2.freepascal.org/svn/fpcbuild/binaries/i386-win32> (the files starting with arm-linux-) Put them into 
        
        C:\development\cross\bin\arm-linux
        




On Windows, the result is something _like_ these files (adjust for your platform etc): 
    
    
    arm-linux-addr2line.exe
    arm-linux-ar.exe
    arm-linux-as.exe
    arm-linux-ld.exe
    arm-linux-objcopy.exe
    arm-linux-objdump.exe
    arm-linux-strip.exe
    

Libs: extract the libs from your device - e.g. this on Raspbian: 

  * all .so files in /lib
  * all .so files in /usr/lib
  * all .so files in /usr/lib/arm-linux-gnueabihf



Note: some .so files are actually scripts; check 
    
    
    grep -i "ld script" *
    

Remove those .so and copy over (or symlink) the files the script point to (e.g. .so.x files) to .so so that the linker can find them 

Examples: 

  * libpthread.so
  * libc.so
  * libgcc_s.so



Put the libs into your cross lib path, e.g. on Windows 
    
    
    C:\development\cross\lib\arm-linux
    

#### Settings

Open your settings file or add a new file, settings.ini. 

As you can see in the comments below, you can build an ARMHF cross-compiler which works on e.g. Raspbian, requires an FPU and requires ARMHF libraries, but only on FPC 2.7+. 
    
    
    [linuxarm]
    ; a full fpcup run must have been done before (e.g. win32full)
    ; does not update packages
    ; please adjust paths etc
    fpcdir=c:\development\fpctrunk
    fpcurl=http://svn.freepascal.org/svn/fpc/trunk
    lazdir=lazdir=c:\development\lazarus
    primary-config-path=c:\development\lazarussettings
    lazlinkname=""
    fpcuplinkname=""
    ; NO FPIC: not supported on 2.6.x
    ; http://bugs.freepascal.org/view.php?id=19729
    ; otherwise Fatal: Internal error 200502052
    ; If using -CaEABIHF in crossopt, specify -dFPC_ARMHF here (only works on 2.7.x)
    ; more info see below
    fpcopt=""
    binutilsdir=c:\development\fpcbootstrap
    fpcbootstrapdir=c:\development\fpcbootstrap
    noconfirm=true
    keeplocalchanges=true
    cputarget=arm
    ; if using armhf, you MUST specify -dFPC_ARMHF in the FPC options; 
    ; specifying in crossopt is not enough.
    ; -fPIC not supported on FPC 2.6.x, only on 2.7.x; 
    fpcopt="-dFPC_ARMHF"
    
    ; safe default but slow:
    ;crossopt="-CpARMV6 -CaEABI -CfSOFT"
    
    ; Raspbian for Raspberry Pi safe
    ; ARMv6 instruction set
    ; if using arm hf (hard float/floating point), please specify -dFPC_ARMHF
    ; copy over /lib, /usr/lib 
    ; /usr/lib/arm-linux-gnueabihf (Raspbian) => note: 
    ; some .so files are actually scripts; check
    ; grep -i "ld script" *
    ; Remove those .so and copy over (or symlink) the .so.x to .so in order for the linker to find them
    ; Examples:
    ; libpthread.so
    ; libc.so
    ; libgcc_s.so
    ; to cross lib dir (try compiling and keep copying until no errors left)
    ; Note: -CpARMV6Z parameter not supported on 2.6.x, only on 2.7.x
    ; Note: -CaEABIHF parameter not supported on 2.6.x, only on 2.7.x
    ; Note: don't use -fPIC; will give segmentation fault
    crossopt="-CaEABIHF -CpARMV6 -CfVFPV2"
    
    ; beagleboard black with hardfloat:
    ; if using armhf, also specify -dFPC_ARMHF (in fpcopt) to avoid gtk scrollbar errors
    ; Note: -CaEABIHF not supported on 2.6.x, only on 2.7.x
    ;crossopt="-CaEABIHF -Cparmv7 -CfVFPv3"
    ostarget=linux
    ; Only build FPC
    only=FPCCleanOnly,FPCBuildOnly
    skip=FPCGetOnly,lazbuild,bigide,useride
    

See also: 

  * [ARM compiler options](<ARM_compiler_options.md> "ARM compiler options")
  * any (cross compiler) page about your device
  * your ARM device documentation



#### Creating and using the cross compiler

Then generate the cross compiler with 
    
    
    fpcup --inifile=settings.ini --inisection=linuxarm
    

Now you can run a test compile, compiling a file test.pas with 
    
    
    fpc -Parm -Tlinux test.pas
    

(your fpc.cfg will have been modified to add some settings when cross-compiling to Android ARM) 

### Linux i386 cross

On an x64 Linux system (or any system with support for i386 Linux libraries/cross binutils), once you have set up a regular FPC compiler, you can add a Linux i386 cross compiler. 

#### Required files

  * Linux 
    * build or install the relevant crossbinutils
    * build or install the relevant i386 libraries for your code (e.g. libc)
  * other systems: not tested yet



#### Settings

Open your settings file or add a new file, settings.ini. 
    
    
    [linuxi386]
    ; a full fpcup run must have been done before
    ; does not update packages
    ; please adjust paths etc
    fpcdir=~/development/fpctrunk
    fpcopt=""
    fpcurl=http://svn.freepascal.org/svn/fpc/trunk
    lazdir=lazdir=~/development/lazarus
    primary-config-path=~/development/lazarussettings
    lazlinkname=""
    fpcuplinkname=""
    fpcbootstrapdir=~/development/fpcbootstrap
    noconfirm=true
    keeplocalchanges=true
    cputarget=i386
    ostarget=linux
    only=FPCCleanOnly,FPCBuildOnly
    skip=FPCGetOnly,lazbuild,bigide,useride
    

  


#### Creating and using the cross compiler

Then generate the cross compiler with 
    
    
    fpcup --inifile=settings.ini --inisection=linuxi386
    

Now you can run a test compile, compiling a file test.pas with 
    
    
    fpc -Pi386 -Tlinux test.pas
    

(your fpc.cfg will have been modified to add some settings when cross-compiling to Android ARM) 

### DOS crosscompiler

Using 32 bit fpcup.exe on Windows, once you have set up a regular FPC trunk/2.7.1 compiler, you can add a [DOS](<DOS.md> "DOS") cross compiler. 

#### Required files

Download the required binaries as mentioned in [DOS](<DOS.md> "DOS"), currently: 
    
    
    msdos-nasm.exe
    msdos-wlib.exe
    msdos-wlink.exe
    nasm.exe
    wlibd.dll
    wlinkd.dll
    wlsystem.lnk
    

and put them in your cross binutils directory for the right platform - e.g. on a standard Windows fpcup install: 
    
    
    c:\development\cross\bin\i8086-msdos
    

There are no files needed in the corresponding library directory, 
    
    
    C:\development\cross\lib\i8086-msdos
    

#### Settings

Open your settings file or add a new file, settings.ini: 
    
    
    [dos]
    ; dos cross compiler
    ; a full fpcup run must have been done before
    ; please adjust paths etc
    fpcdir=c:\development\fpctrunk
    fpcurl=http://svn.freepascal.org/svn/fpc/trunk
    fpcopt=""
    fpcuplinkname=""
    lazlinkname=""
    binutilsdir=c:\development\fpcbootstrap
    fpcbootstrapdir=c:\development\fpcbootstrap
    cputarget=i8086
    ostarget=msdos
    verbose=0
    ; We only need an FPC here; assume FPC has been updated by regular update
    only=FPCBuildOnly
    

#### Creating and using the cross compiler

Then generate the cross compiler with 
    
    
    fpcup --inifile=settings.ini --inisection=dos
    

Now you can run a test compile, compiling a file test.pas with 
    
    
    fpc -Pi8086 -TMSDOS test.pas
    

(your fpc.cfg will have been modified to use DOS medium memory model, smartlinking etc) 

## Download

The current maintained version can be found at: <https://github.com/LongDirtyAnimAlf/Reiniero-fpcup>

The original site is at: <https://bitbucket.org/reiniero/fpcup/downloads> (not maintained at the moment) 

[![Warning-icon.png](https://wiki.freepascal.org/images/b/b2/Warning-icon.png)](</File:Warning-icon.png>)

**Warning:** Windows users, please download the 32 bit fpcup version even if you are on 64 bit Windows unless you know what you are doing. See [this warning about FPC 2.6.x x64.](<Windows_Programming_Tips.md> "Windows Programming Tips")

## License

fpcup is open source software released under the LGPL with linking exception (same as FreePascal/LCL). 

## Troubleshooting

When you hit errors with fpcup: 

  * RTFM (read the fine manual)
  * If compiling Lazarus, make sure lazarus and lhelp are not running.
  * If you have a working Lazarus, run Tools > Rescan FPC Source Directory
  * Try running fpcup with `--verbose` (look for fpcup: error etc) to see what's going on exactly.
  * Look into the error message fpcup generates; it also shows some troubleshooting steps you may perform.
  * Sometimes, the FPC or Lazarus build process leaves incorrect files; you can (e.g. with fpcupgui) delete all .ppu files from your FPC and Lazarus directories (including subdirectories) and run fpcup again.
  * Try removing all intermediate files by running fpcup with the `--clean` option
  * fpcup is a wrapper around svn/make/lazbuild etc. It cannot fix errors in those underlying systems. The svn information/database in your (fpc, lazarus, external modules) directory may be corrupted. Try a manual `svn up` to update and fix any error messages first (e.g. running `svn cleanup`).
  * Try running 
        
        svn revert THEDIRECTORY --recursive
        

followed by 
        
        svn update THEDIRECTORY
        

for your Lazarus and FPC directories to remove local modifications, if any.
  * Remove possible stale files: 
    * the bootstrap compiler and any binutils: remove all files and directories in the fpcbootstrap directory
    * if you haven't modified fpcup.ini, remove it and let fpcup regenerate it
    * same for settings.ini
    * same for fpc.cfg in $(fpcdir)\bin\<architecture>
    * same for your Lazarus primary config path/configuration directory
  * See if there's a newer fpcup version available that may have fixed your problem



If the steps before do not work: you can use the `--uninstall` option to remove all files including your FPC and Lazarus directories, then run fpcup with the same commands. 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** Of course, errors introduced in FPC and Lazarus source code/repository may also influence builds. There's nothing fpcup can do to fix incorrect code breaking the build. However, some of the steps mentioned above (deleting .ppu files, fpc.cfg, Lazarus config) may fix Lazarus/FPC build problems..

## Known issues

Known issues can be found in the issue tracker: [https://bitbucket.org/reiniero/fpcup/issues?status=new&status=open](<https://bitbucket.org/reiniero/fpcup/issues?status=new&status=open>)

### No rule to make target ...

If you see errrors like `make.exe: *** No rule to make target `all'. Stop.` on Windows, this may be caused by an invalid make version. Make sure you're using the right make.exe version. 

FPC 2.6.x needs make 3.80 (distributed with FPC 2.6.x) 

FPC 2.7.1+ needs at least make 3.82 (packet name "remake" at the debian repo) (distributed with e.g. Lazarus 1.2.4) 

### Strange errors running clean/distclean

This is a symptom of issues with the FPC build system (not fpcup). 

On Windows, make 3.80 which is distributed with FPC 2.6.x has problems when there are spaces in your path. This may lead to strange error messages when cleaning. 

FPC 2.6.x needs make 3.80 (distributed with FPC 2.6.x) 

FPC 2.7.1+ needs at least make 3.82 (distributed with e.g. Lazarus 1.2.4) 

### Info: Error running fpcup. Technical details: error executing sequence helplazarus; line: 4, param: helplazarus

HelpLazarus is often the first module that will be built after building lazbuild and the other core IDE units. However, the actual problem may be that lazbuild itself was not generated due to an error. Try running with `--verbose` to pinpoint where the problem is and follow the troubleshooting steps above. 

### Error finding (e.g.) system.ppu when compiling with FPC/Lazarus

First try running Tools > Rescan FPC Source Directory. Normally, Lazarus does this automatically, in the background but if you are too fast it won't have finished in time. 

Errors like this may also be caused by an fpc.cfg from an older/different version of FPC that remains in your fpc/bin/<platform> directory. Because you can customise fpc.cfg, fpcup will not just replace it. 

If you have not customised fpc.cfg, you can remove it and let fpcup regenerate it when rebuilding FPC. 

## Support and bug reporting

Please post support requests on the forum so other users can help you. 

Please report fpcup bugs on the bug tracker <https://bitbucket.org/reiniero/fpcup/issues>

If you report bugs, **after** running the troubleshooting steps above, please be sure to mention: 

  1. operating system
  2. fpcup version (as printed in its output)
  3. description of what went wrong/error message
  4. run fpcup with the `--verbose --noconfirm` arguments (or the equivalents in settings.ini) as well as the other arguments you used, redirect the output to file, and attach the file, e.g.: 
         
         fpcup --verbose --noconfirm > fpcupverbose.txt
         

.



## Supported platforms

  * Support for Windows 2000-7 (Windows 8: untested, probably works). Bare metal install currently needs Windows XP or later for the subversion client. On Windows 2000, manually installing a subversion client (e.g. SlikSVN 1.6.16) in advance will work.
  * Linux x86: works
  * Linux x64: works
  * Linux ARMHF: lightly tested; seems to work
  * macOS 10.?: lightly tested; seems to work
  * FreeBSD 9: lightly tested; seems to work



fpcup can be compiled on all platforms that support Lazarus+[Synapse](<Synapse.md> "Synapse")

## Changelog

This log tracks fpcup development. Note that not all binaries on the download page may be updated at the same time; fpcup prints a version string at the beginning which you can check. 

The commit log is located at <https://github.com/LongDirtyAnimAlf/Reiniero-fpcup/commits/master>

## External links

  * [Original project page - No longer available](<https://bitbucket.org/reiniero/fpcup>)
  * [Updated Repository](<https://github.com/LongDirtyAnimAlf/Reiniero-fpcup>)
  * [Binary downloads for several platforms](<https://github.com/LongDirtyAnimAlf/Reiniero-fpcup/tree/master/bin>)
  * [Additional install scripts](<https://github.com/LongDirtyAnimAlf/Reiniero-fpcup/tree/master/bin/i386-win32>)
  * [BlackDuck OpenHub/Ohloh code statistics](<https://www.openhub.net/p/fpcup>)



## See also

  * [Installing Lazarus](<Installing_Lazarus.md> "Installing Lazarus") Official way to install Lazarus
  * [Lazarus Manager](<Lazarus_Manager.md> "Lazarus Manager") Another installer for Lazarus/FPC
  * [Small Virtual Machines](<Small_Virtual_Machines.md> "Small Virtual Machines") See fpcup used in two virtual machines

---

_Source: [https://wiki.freepascal.org/fpcup](https://web.archive.org/web/20250416000007/https://wiki.freepascal.org/fpcup)_
