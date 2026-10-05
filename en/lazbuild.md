# lazbuild

│ **English (en)** │

lazbuild is a command line utility to compile Lazarus projects and packages, as well as the Lazarus IDE itself. When you built Lazarus yourself you can find the lazbuild executable in the Lazarus source directory together with the lazarus executable. 

## Contents

  * 1 Synopsis
  * 2 Description
  * 3 Usage
  * 4 Options
  * 5 .lpi and .lpk files



## Synopsis
    
    
    lazbuild [options] filename filename ...
    

## Description

lazbuild builds Lazarus projects (.lpi) and/or packages (.lpk). It automatically compiles required packages. It can also compile the IDE and install any compiled packages. 

## Usage

lazbuild is a command-line tool. It recursively checks all dependencies and compiles needed packages first. It uses the Free Pascal compiler (fpc) to compile. 

## Options

-h or --help
    write the list of options.

-B or --build-all
    build all files of project/package. Normally the compiler only compiles the main source and all sources with a newer date.

-r or --recursive
    apply build flags (-B) to dependencies, too.

-d or --skip-dependencies
    do not compile dependencies

\--build-ide=<options>
    Compile the IDE (i.e. the Lazarus executable). The list of installation packages is read from the config directory. The <options> are appended to the other IDE custom options. They can be empty. (With 0.9.30 and below the IDE profile custom options were replaced). Since at least Lazarus 1.0.6 (revision 38971): you can pass -dKeepInstalledPackages to build the IDE with the list of packages you had previously selected - analogous to make useride; handy when using --add-package. Build-ide uses the selected profile in the IDE. You can use another profile with --build-mode (see below).

  * Example for building with your default profile: --build-ide=
  * Example for building with a specific profile: --build-ide="Normal IDE"



-v or --version
    show version and exit

-q or --quiet
    be less verbose; can be given multiple times. Since 1.5: Passing quiet two times, will pass -vw-n-h-i-l-d-u-t-p-c-x- to the compiler.

\--verbose
    be more verbose; can be given multiple times

\--verbose-pkgsearch
    Write what package files are searched and found. Since 1.5.

\--add-package
    add package(s) to list of installed packages in the IDE (combine with --build-ide to rebuild IDE). Since 1.1.

Example: --build-ide= --add-package projecttemplates lazopenglcontext /path/to/your/pkg/name.lpk 

\--add-package-link=<.lpk file>
    Register the Lazarus package files (.lpk) and exit. Do not build. Since 1.7.

\--create-makefile
    Instead of compile package, create a simple Makefile. No support for build modes. Does not compile required packages.

\--primary-config-path=<path> or --pcp=<path>
    primary config directory, where Lazarus stores its config files. Default is $HOME/.lazarus

\--secondary-config-path=<path> or --scp=<path>
    secondary config directory, where Lazarus searches for config template files. Default is /etc/lazarus

\--operating-system=<operating-system> or --os=<operating-system>
    override the project operating system. e.g. win32 or linux.

\--widgetset=<widgetset> or --ws=<widgetset>
    override the project widgetset. e.g. gtk, gtk2, qt, win32, or carbon.

\--cpu=<cpu>
    override the project cpu. e.g. i386, x86_64, powerpc, powerpc_64 etc.

\--subtarget=<subtarget>
    override the project subtarget (since Lazarus 3.99)

\--build-mode=<project/ide build mode> or --bm=<project/ide build mode>
    override the project build mode. With --build-ide= it selects the IDE build profile. (With 0.9.30 and below you can not select the IDE build profile)

\--compiler=<ppcXXX>
    override the default compiler. e.g. ppc386, ppcx64, ppcppc etc.

\--language=<language>
    override language. For example --language=de. For possible values see files in the lazarus/languages directory.

\--lazarusdir=<Lazarus directory>
    directory, to be used as a basedirectory

\--max-process-count=<count>
    Maximum number of threads for compiling in parallel. Default is 0, which guesses the number of cores in the system. Since 1.5.

\--no-write-project
    Do not write updated project info file (.lpi) after build. If not specified, build number will be incremented if configured. Since 1.7.

## .lpi and .lpk files

If you want to create .lpi and .lpk files, it is recommended to use the Lazarus IDE. The files are simple XML and all settings can be edited via the IDE. There is no reason to edit them manually, except for very rare cases like renaming hundreds of files.

---

_Source: [https://wiki.freepascal.org/lazbuild](https://web.archive.org/web/20250113143847/https://wiki.freepascal.org/lazbuild)_
