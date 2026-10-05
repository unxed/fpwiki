# FPC JVM

│ **English (en)** │  **[русский (ru)](<../ru/FPC_JVM.md> "FPC JVM/ru")** │    
****

## Contents

  * 1 Overview
  * 2 Detailed information
  * 3 Download
  * 4 Quick start
    * 4.1 Java or Android
    * 4.2 Compile a test program
    * 4.3 Run it on Windows
    * 4.4 Run it on Unix-like platforms
    * 4.5 More details
  * 5 See also



## Overview

The FPC backend for the Java Virtual Machine (JVM) generates Java byte code that conforms to the specifications of the **JDK 1.5 (and later)** , and also to the Dalvik VM from the **Android** platform. While not all FPC language features work when targeting the JVM, most do (or will in the future) and we have done our best to introduce as few differences as possible. 

## Detailed information

  * [Basic usage information](<FPC_JVM/Usage.md> "FPC JVM/Usage")
  * [Supported language constructs and other programming information](<FPC_JVM/Language.md> "FPC JVM/Language")
  * [Help with debugging the Java class files](<FPC_JVM/Debugging.md> "FPC JVM/Debugging")
  * [Building the compiler and Java utilities from source](<FPC_JVM/Building.md> "FPC JVM/Building")
  * [Information about internal changes to the compiler and RTL](<FPC_JVM/Internals.md> "FPC JVM/Internals") (mainly interesting to compiler/RTL developers)



## Download

Please check the official [download page](<http://www.freepascal.org/download.var>) for your platform to see whether a pre-built release of the JVM cross-compiler is available. 

If your platform is not listed above, or if you are only interested in building the compiler/RTL from source, a separate archive that only contains the compiled Java components (Jasmin, javapp, BCEL) is also available. Building instructions for the compiler and are available via the "Building" link above. 

  * [FPC JVM utilities](<ftp://ftp.freepascal.org/pub/fpc/contrib/jvm/fpcjvmutilities.zip>) (you do **not** need this if you already downloaded an official release)



## Quick start

### Java or Android

By default, the compiler will generate code suitable for running on a Java Virtual Machine. If you wish to create Java class files that can be translated using the Android SDK into Dalvik code, add the _-Tandroid_ compiler command line parameter. 

For more detailed information about the development of apps for Android see [here](<FPC_JVM_Android_Development.md> "FPC JVM Android Development"). 

### Compile a test program

The used example is <https://gitlab.com/freepascal.org/fpc/source/-/blob/main/tests/test/jvm/trange1.pp>
    
    
     ppcjvm -O2 -g trange1
    

### Run it on Windows

_Note: the path to the units has changed since the previous snapshots!_
    
    
     java -cp C:\full\path\to\fpcjvm\units\jvm-java\rtl;. trange1
    

Replace _C:\full\path\to\fpcjvm\units\jvm-java\rtl_ with the full path to the _units\jvm-java\rtl_ directory from the snapshot archive. 

### Run it on Unix-like platforms

_Note: the path to the units has changed since the previous snapshots!_
    
    
     java -cp /full/path/to/fpcjvm/units/jvm-java/rtl:. trange1
    

Replace _/full/path/to/fpcjvm/units/jvm-java/rtl_ with the full path to the _units/jvm-java/rtl_ directory from the snapshot archive. 

### More details

See the [usage information](<FPC_JVM/Usage.md> "FPC JVM/Usage") page for more details. 

## See also

  * [Lazarus JVM](<Lazarus_JVM.md> "Lazarus JVM") Using Lazarus to build Java applications.

---

_Source: [https://wiki.freepascal.org/FPC_JVM](https://web.archive.org/web/20250422001947/https://wiki.freepascal.org/FPC_JVM)_
