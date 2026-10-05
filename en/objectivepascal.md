# objectivepascal

[![Warning-icon.png](https://wiki.freepascal.org/images/b/b2/Warning-icon.png)](</File:Warning-icon.png>)

**Warning:** This information would be appear to be out of date.

## Contents

  * 1 Version References
  * 2 Downloading Sources
  * 3 Building the compiler for Mac OS X development
    * 3.1 Cocoa Headers
  * 4 Building the compiler for iOS development
    * 4.1 iOS Headers
    * 4.2 Compiling iOS Sources



## Version References

This article makes reference to various versions (in scripts and commands) that may not be the most current. Please verify they are correct on your system before trying any of the commands. 

  * 2.4.2 - The base install of FPC (used for bootstrapping the installation of the latest version)
  * 2.5.1 - The latest version of FPC used for compiling Objective Pascal (macOS and iOS)
  * iPhoneOS4.2.sdk - The latest iOS SDK which is compatible with the translated headers.



## Downloading Sources

Before building any compilers you must download the FPC sources from SVN. This page references the standard location of `/Developer/ObjectivePascal/fpc` for all the FPC sources and you may need to create the `/Developer/ObjectivePascal` directory before going any further. 

To check out the FPC sources for the first time use the command: 
    
    
    svn co <http://svn.freepascal.org/svn/fpc/trunk> /Developer/ObjectivePascal/fpc
    

To update the FPC sources from a previous download use the command: 
    
    
    svn up <http://svn.freepascal.org/svn/fpc/trunk> /Developer/ObjectivePascal/fpc
    

## Building the compiler for Mac OS X development

1) Download the latest version of FPC and Xcode developer tools then install. 

  * <http://www.freepascal.org/down/i386/macosx-ftp.freepascal.org.var>
  * <http://developer.apple.com/devcenter/mac>



2) Download the latest version of the sources (see #Downloading Sources) 

3) Build the Intel compiler to `/usr/local/lib/fpc/2.5.1/ppc386`
    
    
    cd /Developer/ObjectivePascal/fpc
    sudo make distclean -j 3
    sudo make FPC=/usr/local/lib/fpc/2.4.2/ppc386 OPT="-ap" distclean all -j 2
    sudo make FPC=`pwd`/compiler/ppc386 install
    

4) Build the PPC compiler to `/usr/local/lib/fpc/2.5.1/ppcppc`. 
    
    
    cd /Developer/ObjectivePascal/fpc
    sudo make distclean -j 3
    sudo make FPC=/usr/local/lib/fpc/2.4.2/ppc386 OPT="-ap" distclean CPU_TARGET=powerpc all -j 2
    sudo make FPC=`pwd`/compiler/ppcppc CROSSINSTALL=1 install
    sudo mv /usr/local/lib/fpc/2.5.1/ppcrossppc /usr/local/lib/fpc/2.5.1/ppcppc
    

5) Build the 64-bit compiler to `/usr/local/lib/fpc/2.5.1/ppcx64`. 

cd /Developer/ObjectivePascal/fpc sudo make distclean -j 3 make FPC=/usr/local/lib/fpc/2.4.2/ppc386 OPT="-ap" CPU_TARGET=x86_64 all -j 2 sudo make FPC=`pwd`/compiler/ppcx64 CROSSINSTALL=1 install sudo mv /usr/local/lib/fpc/2.5.1/ppcrossx64 /usr/local/lib/fpc/2.5.1/ppcx64 

### Cocoa Headers

The most recent version of the translated Cocoa headers (which are guaranteed to compile with that version) are location in the FPC distribution at `/Developer/ObjectivePascal/fpc/packages/cocoaint/src`. If you need newer headers you will need to parse them manually using the framework parser located at `/Developer/ObjectivePascal/fpc/packages/cocoaint/utils/parser.php`. 

## Building the compiler for iOS development

Follow the steps below to install the ARM compiler and 386 compiler for iOS simulator. In the examples below the _iPhoneOS4.2.sdk_ SDK is used but you should change this to the SDK version which you installed from Xcode. 

1) Download the latest version of **FPC** and **Xcode Developer Tools** (with iOS SDK) then install. Please note you must start with a standard (unchanged) installation of Xcode developer tools and iOS SDK. 

If you installed Xcode into another location than the default location `/Developer` you will need to change the paths below and in any other scripts you may run. Make sure if you installed into another location the path does not contain spaces. 

  * <http://www.freepascal.org/down/i386/macosx-ftp.freepascal.org.var>
  * <http://developer.apple.com/devcenter/mac> (Xcode Developer Tools)



2) Download the latest version of the sources (see #Downloading Sources) 

3) Make **sig_cpu.inc** (a file that can not be distributed without violating the Apple user agreement) by running the command below. Please note you may need to set the path to the iOS SDK you are using by editing the variables _IPHONEINSTALLDIR_ and _IPHONESDKDIR_. If the command was successful the file should exist at _/Developer/ObjectivePascal/fpc/rtl/darwin/arm/sig_cpu.inc_. 
    
    
    FPCSOURCEDIR=/Developer/ObjectivePascal/fpc
    IPHONEINSTALLDIR=/Developer
    IPHONESDKDIR=iPhoneOS4.2.sdk
    IPHONEPLATFORMBASEDIR="$IPHONEINSTALLDIR"/"Platforms/iPhoneOS.platform/Developer"
    INCLUDEDIR="$IPHONEPLATFORMBASEDIR"/"SDKs/$IPHONESDKDIR/usr/include"
    STRUCTSHEADER="$INCLUDEDIR"/arm/_structs.h
    /usr/bin/gcc -I"$INCLUDEDIR" -E -D__need_mcontext_t -D__DARWIN_UNIX03 "$STRUCTSHEADER" | sed -e 's/__uint32_t/cuint32/g' -e 's/_STRUCT_ARM_EXCEPTION_STATE/__darwin_arm_exception_state/' -e 's/_STRUCT_ARM_THREAD_STATE/__darwin_arm_thread_state/' -e 's/_STRUCT_ARM_VFP_STATE/__darwin_arm_vfp_state/' -e 's/__darwin_mcontext/mcontext_t/' > "$FPCSOURCEDIR"/rtl/darwin/arm/sig_cpu.h
    /usr/local/bin/h2pas -i -o "$FPCSOURCEDIR"/rtl/darwin/arm/sig_cpu.inc "$FPCSOURCEDIR"/rtl/darwin/arm/sig_cpu.h
    

4) Build the ARM cross-compiler to `/usr/local/lib/fpc/2.5.1/ppcrossarm`

  * Currently, if you are using Xcode 3.2.6+ or Xcode 4+, currently you first have to apply a patch to the FPC sources as explained at <http://bugs.freepascal.org/view.php?id=19196>.


    
    
    cd /Developer/ObjectivePascal/fpc
    export IPHONEPLATFORMBASEDIR=/Developer/Platforms/iPhoneOS.platform/Developer
    make FPC=/usr/local/lib/fpc/2.4.2/ppc386 CPU_TARGET=arm CROSSOPT="-FD${IPHONEPLATFORMBASEDIR}/usr/bin -XR${IPHONEPLATFORMBASEDIR}/SDKs/iPhoneOS4.2.sdk/ -ap -Cfvfpv2" all
    sudo make FPC=`pwd`/compiler/ppcrossarm OPT="-ap" CPU_TARGET=arm CROSSOPT="-FD${IPHONEPLATFORMBASEDIR}/usr/bin -XR${IPHONEPLATFORMBASEDIR}/SDKs/iPhoneOS4.2.sdk/ -ap" install CROSSINSTALL=1
    sudo ln -sf ../lib/fpc/2.5.1/ppcrossarm /usr/local/bin/ppcarm
    

5) Build the Intel compiler for iOS Simulator `/usr/local/lib/fpc/2.5.1/ppc386`
    
    
    cd /Developer/ObjectivePascal/fpc
    make FPC=/usr/local/lib/fpc/2.4.2/ppc386 OS_TARGET=iphonesim all
    sudo make FPC=/usr/local/lib/fpc/2.4.2/ppc386 OS_TARGET=iphonesim install CROSSINSTALL=1
    

### iOS Headers

The iOS headers must be manually generated using a script as they can not be legally distributed in translated Pascal form. The SDK used to parse the sources may not be the latest version of the SDK you installed from Xcode. Check that you have the SDK version used in the script _make-ios-headers.sh_ before trying this step. 
    
    
    cd /Developer/ObjectivePascal/fpc/packages/cocoaint/utils
    ./make-ios-headers.sh
    

The headers will be parsed and saved into `/Developer/ObjectivePascal/fpc/packages/cocoaint/utils/uikit-skel/src`. 

### Compiling iOS Sources

These are example commands you can use to compile iOS programs from the command line. Change the path of the iOS SDK to the version which you generated the headers for. 

  * Example command for iOS simulator


    
    
    /usr/local/lib/fpc/2.5.1/ppcross386 -XX -Tiphonesim -Fu/Developer/ObjectivePascal/fpc/packages/cocoaint/utils/uikit-skel/src -XR/Developer/Platforms/iPhoneSimulator.platform/Developer/SDKs/iPhoneSimulator4.2.sdk program.pas
    

  * Example command for iOS device


    
    
    /usr/local/lib/fpc/2.5.1/ppcrossarm -XX -Cfvfpv2 -Fu/Developer/ObjectivePascal/fpc/packages/cocoaint/utils/uikit-skel/src -FD/Developer/Platforms/iPhoneOS.platform/Developer/usr/bin -XR/Developer/Platforms/iPhoneOS.platform/Developer/SDKs/iPhoneOS4.2.sdk program.pas

---

_Source: [https://wiki.freepascal.org/objectivepascal](https://web.archive.org/web/20250115000000/https://wiki.freepascal.org/objectivepascal)_
