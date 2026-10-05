# Release engineering

This article discusses how to make a release of the FPC compiler on various platforms. 

## Contents

  * 1 Migration of existing SVN checkout
  * 2 Create libgdb.a and or binutils
  * 3 General notes about release building
  * 4 Convert the faq.adp to faq.htm and faq.txt
  * 5 OS/CPU specific release engineering
    * 5.1 FreeBSD
    * 5.2 Linux
      * 5.2.1 Building an rpm
      * 5.2.2 Building a deb
    * 5.3 Dos installer
    * 5.4 Windows Inno setup
      * 5.4.1 Windows to x86_64 cross compiler
      * 5.4.2 Windows combined 32-bit and x86_64 cross compiler in a single package
      * 5.4.3 Windows to Android cross compiler
      * 5.4.4 Windows to i8086-msdos cross compiler
    * 5.5 Darwin (Mac OS X) - Fink
      * 5.5.1 Files
      * 5.5.2 32 bit vs 64 bit
      * 5.5.3 Bootstrap Compiler
      * 5.5.4 Usually Required Adjustments for New Releases
      * 5.5.5 History
    * 5.6 Haiku
  * 6 Documentation building and LaTeX limits
  * 7 Packaging system
  * 8 Building the official fpc-packages
    * 8.1 Major versions
    * 8.2 Create directories on ftp
    * 8.3 Update the version-number
      * 8.3.1 Version number of the Makefiles
      * 8.3.2 Version number of the fpmake files
      * 8.3.3 Version number in the source & documentation files
    * 8.4 Tag version
    * 8.5 Create the source zips
    * 8.6 Create bootstrap compiler archives
    * 8.7 Documentation
  * 9 Release schedule
  * 10 Testing a release
  * 11 Testers template
    * 11.1 Previous releases



## Migration of existing SVN checkout

The existing checkout of the 'fixes_2_2' branch can be switched to the release tag using SVN command "switch", specifically its first variant (see below). However, it's recommended to build in exported sources, not in checked out version. 
    
    
    ~/fpc>>> svn sw --help
    switch (sw): Update the working copy to a different URL.
    usage: 1. switch URL [PATH]
           2. switch --relocate FROM TO [PATH...]
    
    
    
     1. Update the working copy to mirror a new URL within the
     repository.
        This behaviour is similar to 'svn update', and is the way to move
        a working copy to a branch or tag within the same repository.
    
    
    
     2. Rewrite working copy URL metadata to reflect a syntactic change
     only.
        This is used when repository's root URL changes (such as a scheme
        or hostname change) but your working copy still reflects the same
        directory within the same repository.
    

## Create libgdb.a and or binutils

  * If you have problems building recent binutils or libgdb's on any *nix, and the error is related to intl.h or libintl.a, reconfigure using ../<srcdir>/configure --disable-nls (assuming you use a separate builddir)
  * Some cygwin hints: 
    * ensure you've installed gcc, bison and flex
    * use   
`./configure --disable-ide --disable-libgui --disable-install-libgui`  
to configure



## General notes about release building

To create a build, you always have to start with compiler from the last previous major release (e.g. 2.0.0 for all 2.0.x and 2.2.0 releases, 2.2.0 for 2.2.x, etc.), or the last previous minor release (i.e. you can use 2.0.2 for building 2.0.4). 

## Convert the faq.adp to faq.htm and faq.txt

The faq's in fpcbuild/install/doc are generated from the faq.adp in the html-repository. (It's a good idea too check if the faq.adp is still up-to-date before converting it.) To do the conversion, check out the html-repository and do a 'make all'. Then create the faq.htm and .txt by: 
    
    
     ./adp2html faq.adp -m external-master.adp -p maindir=<http://www.freepascal.org/> -p x=.var > faq.htm
     w3m -dump faq.htm > faq.txt
    

It could be that you have to install w3m first. If the conversions are succesfull update these files in the fpcbuild-repository. 

## OS/CPU specific release engineering

### FreeBSD

[FreeBSD specific Release Engineering](<FreeBSD_specific_Release_Engineering.md> "FreeBSD specific Release Engineering")

### Linux

Get sources, download the fpcbuild from ftp and extract it or use a command like: 
    
    
     git clone --branch release_3_2_2 --depth 1 --recurse-submodules --remote-submodules --shallow-submodules <https://gitlab.com/freepascal.org/fpc/build.git> fpcbuild
     rm -rf fpcbuild/.git &&  rm -rf fpcbuild/fpcsrc/.git && rm -rf fpcbuild/fpcdocs/.git
    

  
Versions **3.0.x and below and some special targets** are using libgdb inside the ide to be able to debug. For those versions you have to get libgdb for your CPU from <ftp://ftp.freepascal.org/pub/fpc/contrib/libgdb/>, extract it into `fpcbuild/fpcsrc/` preserving pathes. Before building please check for files **libiberty.a** , **libopcode.a** and **libbfd.a**. These files are often installed in directories like /usr/lib. The compiler **must not** be able to find them! The reason is that otherwise the compiler might use the preinstalled ones instead of the ones you put in the libgdb directory, and the preinstalled ones are not compatible with libgdb.a. If the compiler finds and uses these libraries in /usr/lib, the IDE will crash on any debugging attempt. 

Get the most recent docs package from the ftp site, it's called doc-pdf.tar.gz and copy it into the `fpcbuild/` directory. 

Change into `fpcbuild/`. Now **become root** , or use **fakeroot** , then execute 
    
    
     install/makepack
    

#### Building an rpm

To build an RPM, change into `fpcbuild` and execute 
    
    
     make rpm
    

the documentation will be built as well and that requires a working Tex installation. To prevent the documentation from being built, execute 
    
    
     make rpm NODOCS=1
    

#### Building a deb

To build an Debian Package, please refer to [Debian Packaging](<Debian_Packaging.md> "Debian Packaging") page. Here are quick hints : 

\- Ensure that you have at least installed the following debian packages: dpkg-dev, fakeroot 

\- Modify the `fpcbuild/install/debian/changelog` file adding the following 
    
    
    fpc (x.y.z-b) unstable; urgency=low
    
      * release candidate build
    
     -- First Last <mail@address.ext>  Thu, 07 Jul 2006 23:44:00 +0100
     
    

where b is package building number. Normally if **b** = 0, then packages will be named substituting **b** by the building date, otherwise it will be conserved. But this is changed because b>0 is reserved for official Debian-builds. So in fact we should always use b=0. The value for b is only replaced by the date when SNAPSHOT=1. Please note that **z** could be a number but also a word like 2rc1 

\- change into `fpcbuild` and execute 
    
    
     make deb [GDBLIBDIR=<path/${OS_TARGET}/${CPU_TARGET}>]
    

the documentation will be built as well and that requires a working Tex installation. To prevent the documentation from being built, execute 
    
    
     make deb NODOCS=1
    

### Dos installer

(See also [go32v2 development](<go32v2_development.md> "go32v2 development")) 

The basis is the fpcbuild repository, with the last release compiler as starting compiler, as always. 

Most recent GDB for go32v2 is 6.1.1, you can fetch it from [gdb 6.1.1](<ftp://freepascal.stack.nl/pub/fpc/contrib/libgdb/v6.1.1/>). Older targets have no architecture designation, so put it in fpcsrc/libgdb/go32v2 

Then do a make go32v2zip in the fpcbuild/ dir. On XP this takes a really long time...... 

[b] Note this process will not work on Vista in the default configuration, since Vista only seems to allocate 16 or 32MB for DPMI use to a dosbox (and rumour is that W7 does too), which is too little. I've tried several of the workarounds, (see [go32v2 development](<go32v2_development.md> "go32v2 development")), but as of yet, to no avail.[/b] 

  
That is, the full procedure to build a release for go32v2 is, from fpcbuild directory: 

  1. <create the fpcsrc\libgdb\go32v2 directory and copy libgdb files there>
  2. make go32v2zip OPT="-dGDB_V601"
  3. make sourcezip
  4. make demozip
  5. make docsrc
  6. make utilities
  7. <now we have long file names packages>
  8. make shortnames



### Windows Inno setup

The situation for Windows is pretty much similar to makepack. However, instead of a script, [Inno Setup](<Inno_Setup.md> "Inno Setup") is used. Note that this is the rough principle only. For real, official releases, probably more steps (make sure install/doc/whatsnew is up to date etc, proper SVN tag) must be observed. 

Steps to do once only: 
    
    
    * install Inno Setup. For FPC 3.2.2 we are trialing version 5.6.1-unicode 
    * If support for Win9x/Win ME is needed, install an Ansi version lower than 5.5.0 (see 5.5.0 section in [Inno changelog](<http://www.jrsoftware.org/files/is5-whatsnew.htm>))   
    

Steps per build: 
    
    
    * checkout fpcbuild
    * export it to avoid lardering source zips with GIT dirs. (git can only export archives, but extracting on the fly with cygwin tar works: mkdir ../fpcexp and then git archive main |tar xC ../fpcexp )
    * make sure the last FPC release is first in your path (bootstrap)
    * ~~copy libgdb to its location  (fpcsrc/libgdb/win32)  (see e.g. http:/www.stack.nl/~marcov/mingw ). For this test release I used mingw.  Pass   OPT='-dUSE_MINGW_GDB'"  on the make inno commandline to to select the mingw debugger for the IDE.**obsolete: starting with 3.2.0, IDE uses MI interface on Windows**  ~~ 
    * get "doc-pdf.zip" from FTP in the relevant version. Put it in fpcbuild/
    * to build the installer I ran "make inno"
    * inno has options to speed up compression (multithreading), but these are not used yet.
    * .. _WAIT_ ... :-) 
    

Tips: 

  * if due to atypical Windows configuration the iscc.exe installation script compiler can't be found (e.g. because your program files dir is not detected properly), pass it with ISCCPROG="g:\Program Files\Inno Setup 5\ISCC.exe"
  * the Inno script is in fpcbuild/install/fpc.ist and is postprocessed by fpcsubst to become fpc.iss passed to Inno. fpcsubst fixes the builddir, the exact versions and optionally translates and passes NODOCS to build a release without docs.



Known Issues: 

  * No help/docs for ide. (chms must be added, and maybe even preinstalled)
  * If a FPC install has been done before, the installer suggests the old install dir. (often the previous release).



#### Windows to x86_64 cross compiler

  * Follow the same instructions as for Windows Inno setup, but instead of "make inno OPT=...", do a:


    
    
    make innox64
    

#### Windows combined 32-bit and x86_64 cross compiler in a single package

  * Follow the same instructions as for Windows Inno setup, but instead of "make inno OPT=...", do a:


    
    
    make innox86x64
    

#### Windows to Android cross compiler

  * Install the latest FPC release for Windows.
  * Install the Android NDK. 
    * FPC 3.2 still supports the **android-mips** target, but **mips** support is no longer available since Android NDK r17. In order to build the setup package you need to download NDK r16b from <https://developer.android.com/ndk/downloads/older_releases> and unpack it to some folder.
  * Follow the same instructions as for Windows Inno setup, but instead of "make inno OPT=...", do:


    
    
    make innoandroid NDK=<Path to the NDK, like C:\Users\joost\AppData\Local\Android\Sdk\ndk\r16b>
    

  * When finished you will get a setup file such as `fpc-3.2.0.i386-win32.cross.android.exe` in the top level folder of the fpcbuild's source tree.



#### Windows to i8086-msdos cross compiler

  * Install the latest stable version of the Open Watcom C++ compiler for Windows. As of now, this is **open-watcom-c-win32-1.9.exe**.
  * Follow the same instructions as for Windows Inno setup, but instead of "make inno OPT=...", do a:


    
    
    make innomsdos
    

  * Compilation takes a very long time (3-7 hours, depending on CPU speed), so be patient. The reason for that is, we build fpc 20 times (5 memory models for 4 subarchs - 8086, 80186, 80286 and 80386), using smartlinking (for which, an external assembler is launched for each function in the rtl and packages, and launching a process is slow on windows).



### Darwin (Mac OS X) - Fink

#### Files

  1. fpc-config.info
  2. fpc.info
  3. fpc-cross-*
  4. fpc-doc.info
  5. fpc-sources.info (needed by lazarus)



#### 32 bit vs 64 bit

Contrary to fpc from disk images, fpc is built as 64 bit resulting in 64bit ppcx64 as default. The main reason is that it fits better to the presence of all the 64bit C libraries in fink. Nevertheless, 32bit is still available as cross-compiler as fpc-cross-i386-darwin. 

#### Bootstrap Compiler

Building the fink packages is based on a bootstrap compiler. Whereas the official FPC releases still support 10.4 and later, starting with fpc 3.0.0 the package descriptions support only 10.7 and up. Therefore, only the 64-bit compiler and no universal compiler are required as bootstrap compiler. The 64-bit bootstrap compiler is build using strip and tar commands similar to below. The description below refers to fpc up to 2.6 and any system above 10.4. 

Separate bootstrap compilers for each cpu would be possible, but a universal bootstrap compiler has been choosen, since it makes the installation script simpler. The increased filesize (about twice the size of a single separate bootstrap compiler) of the bootstrap compiler is considered negligible compared to the size of the complete source tarball. Reverting this decision should be doable by well chosen names for the bootstrap compiler tar balls, such as fpc-%v.%m-darwin.bootstrap.tar.bz2, with %m being the cpu. The universal bootstrap compiler is created from the separate compilers using "lipo". The file size is reduced by applying "strip" on the binaries. Although it is possible to use the previous stable release of fpc as a bootstrap compiler, it is better to use the same version, because "make cycle" becomes faster, since only two instead of three cycles are performed. 

This is a typical sequence of commands to produce the tar ball of a universal bootstrap compiler on an IntelMac: 
    
    
     make all PP=PATH_TO_A_LAST_VERSION_COMPILER
     cp compiler/ppc386 $HOME/Desktop
     make distclean
     make all CPU_TARGET=x86_64 PP=$HOME/Desktop/ppc386
     cp compiler/ppcx64 $HOME/Desktop
     make distclean
     make all CPU_TARGET=powerpc PP=$HOME/Desktop/ppc386
     cp compiler/ppcppc $HOME/Desktop
     cd $HOME/Desktop
     strip ppc386
     strip ppcx64
     strip ppcppc
     lipo -create ppc386 ppcx64 ppcppc -output ppcuniversal
     tar -cjf fpc-VERSION.universal-darwin.bootstrap.tar.bz2 ppcuniversal
     md5sum *.universal-darwin.bootstrap.tar.bz2
    

#### Usually Required Adjustments for New Releases

These fields in the file fpc.info usually need changes (examples from version 2.1.2, a beta version of 2.2): 
    
    
    Version: 2.1.2
    Source: <ftp://ftp.freepascal.org/pub/fpc/beta/%v/source/fpcbuild-%v.source.tar.gz>
    Source-MD5: a2ee4db22301cdcbda07185a7b6679b2
    Source2: <ftp://ftp.freepascal.org/pub/fpc/beta/%v/source/fpc-%v.universal-darwin.bootstrap.tar.bz2>
    Source2-MD5: ca5e29e3683ff6d5caa491c0adf4635e
    

The MD5 checksums can be obtained from the error message when fetching the sources with fink or with the terminal command md5sum. 

The fpc-doc.info file also needs according changes in the Source and Source-MD5 fields. 

#### History

2.6.* more crosscompilers 

2.4.0 several crosscompilers (processors and systems) added. 

2.2.4 minor changes. 

2.2.2 extra package for the config file. cross compilers for win32 and linux. execution of tests enabled. 

2.2.1 interims version for lazarus 0.9.24 

2.2.0 first version with native Intel code 

2.0.4 first version in stable tree (powerpc and Rosetta emulation only) 

1.9.8 initial version in unstable tree 

### Haiku

[Haiku specific Release Engineering](<Haiku_specific_Release_Engineering.md> "Haiku specific Release Engineering")

## Documentation building and LaTeX limits

A lot of TeTeX distributions are configured to never use more than a very small amount of memory. The values chosen are more of the 486 era (several MBs, while now 256MB is considered "entry level"). 

The Free Pascal documentation is fairly large, and uses a lot of labels for convenient hyperlinked PDFs and thus hits this limit. 

This can be remedied by modiying the save_size line in texmf.conf to 
    
    
     save_size=10000
    

Usually, you can also do this by environment variable. 
    
    
    export save_size=10000
    pdflatex rtl.tex
    

## Packaging system

See [Packaging System and dividing FPC - Lazarus into packages](<Packaging_System_and_dividing_FPC_-_Lazarus_into_packages.md> "Packaging System and dividing FPC - Lazarus into packages")

## Building the official fpc-packages

### Major versions

Major versions warrant some extra attention during the release process. Primarily the problem is that trunk is never really released from, and any branch to branch from it has to potential to have (many) hidden issues. Also during betas and release candidates, more issues are found for a major branch, since some features receive wide testing for the first time. 

However since 2.2.2, most release engineering work for the fixes branch is performed in trunk, and then merged back to fixes, and in practice this improved this a lot. This however mainly improved the quality. The release engineering from the branchpoint from trunk till release is typically still 6-8 months. 

Some points to consider: 

  * Traditionally major versions have had a beta release before the first release candidate.
  * before branching a major branch, update all makefiles in trunk with a recent fpcmake. This avoids that when a rarely used package is updated, one gets an immense diff on the makefile.
  * This is the time to remove deprecated functions. See [major version to do](<major_version_to_do.md> "major version to do")



### Create directories on ftp

  1. Create the following structure on ftp: (check which platforms are released for this version) 
     * {version} 
       * docs
       * source 
         * separate 
           * tar
           * zip
           * short
       * arm-linux
       * arm-wince
       * i386-freebsd
       * i386-go32v2 
         * separate
       * i386-linux 
         * deb
         * rpm
       * i386-netware
       * i386-netwlibc
       * i386-os2 
         * separate
       * i386-win32
       * powerpc-linux
       * powerpc-macos
       * powerpc-morphos
       * powerpc64-linux
       * sparc-linux
       * x86_64-linux 
         * deb
         * rpm
       * x86_64-win64
  2. Copy the extra files (asld*.zip, gdb*.zip, make*.zip) in i386-os2\separate and i386-gov32v2\separate from previous release (unless updated with new versions)



### Update the version-number

Make sure that fpcmake points to the relevant version for the branch you are running this for. (usually from fixes) 

#### Version number of the Makefiles

For the final release, update the REQUIREDVERSION(2) in the toplevel Makefile.fpc to the new version and the last release. 

Update the version numbers in all Makefile.fpc files containing version=... (plus regenerate all corresponding Makefiles). Do this for the fpc and fpcbuild-repositories. This can be done using the updmkver script from within the root of both repositories. The FPC_DIR environment variable has to be set to the fpcsrc-path: 
    
    
     export FPCDIR=....../fpcbuild/fpcsrc
     cd fpcbuild
     install/updmkver {version} 
     cd ../fpc
     ../fpcbuild/install/updmkver {version}
    

Replace {version} with the version number, for example 2.2.4 or 2.2.4rc1 

#### Version number of the fpmake files

Update the version numbers in all fpmake.pp files in the packages directory (toplevel and build dir). These files are normal code-files so replacing the version numbers can in principle only be done manually, but there is a script updfpmkver which finds all occurences of "Version:='xxx'" where xxx can be anything. Note that this will also update all dependencies to the specified version number. 
    
    
     cd fpc
     ../../fpcbuild/install/updfpmkver {version}
    

Please check the results manually. The following command searches for all occurences of "version" in all fpmake.pp files 
    
    
     find -name fpmake.pp -exec grep -i version {} ';'
    

#### Version number in the source & documentation files

The version number has to be updated in the following places. 

FPCBuild: 

  * /install/doc/readme.txt
  * /install/doc/whatsnew.txt
  * /install/debian/changelog
  * /install/debian/controls contains versions too?
  * /install/doc/faq.htm (sections about latest version, on final release only)
  * /install/doc/faq.txt (sections about latest version, on final release only)



FPCDocs: (there is only a trunk-version) 

  * /docs/fpc.sty (macro fpcversion)



FPC: 

  * /installer/install.dat (header)
  * /installer/install.pas (installer version and copyright year)
  * /compiler/version.pas
  * packages/chm/src/chmbase.pas ~~and chmcmd.lpr~~ (now compiler macro)
  * check the year of copyright in compiler/msg/errore.msg
  * check the year of copyright in packages/ide/src/fpviews.pas



### Tag version

If the changes of the version-numbers are committed, use the createbranch script in fpcbuild to create a 'release_{version}' tag in svn. Use underscores in the version, instead of dots! 
    
    
     ./createbranch.sh tags/release_3_0_4
     ./createbranch.sh tags/release_3_0_4_rc1
    

### Create the source zips

A.s. Before doing this, update whatsnew,readme and faq in the fixes branch, so that changes can be merged from fixes to rc1/final branch. 

Create all the source zips. 

Once the version is tagged the makesource script in fpcbuild/install can create the fpc-{version}.source.tar.gz and fpc-{version}.source.zip. To build the fpcbuild-{version}.tar.gz and fpcbuild-{version}.zip use the makefpcbuild script. Before you run the scripts go to a temporary directory and become root or use fakeroot. If you want to create the sourcefile from a specific branch, you can provide it using the -svnpath parameter For example: 
    
    
     fakeroot
     fpcbuild/install/makesource 3.0.4
     fpcbuild/install/makefpcbuild -svnpath tags/release_3_0_4_rc1 3.0.4rc1
    

The zips for the "source/short" directory are made on a DOS / OS/2 platform (or MS Windows with an additional parameter "OS_SOURCE=go32v2" for all three commands) by executing: 
    
    
     make sourcezip "OS_TARGET=go32v2" NOGDB=1
     make sourcezip "OS_TARGET=os2"  NOGDB=1
     make shortsrc
    

The reason for multiple invocations of make sourcezip with different parameters is the fact that make sourcezip only build ZIP files for sources supported on the particular target platform by default. Since the short version of the source ZIP files is supposed to be used on OS/2 and GO32v2 targets only at the moment, these two platforms need to be listed explicitly as a general rule (obviously, slightly simpler version is possible when running it under OS/2 or native DOS, but the example above should work everywhere). 

Note, in 2.6.0 "make shortsrc" doesn't seem to work anymore. For 2.6.0 and 2.6.2 the following commands were used instead (and manually pushed "n" a few times) 
    
    
      make renametoshort NOGDB=1 "OS_SOURCE=os2" "OS_TARGET=os2" PKGPRE=os2- PKGUNITSPRE=units-  ZIPSUFFIX=.source ZIPSUFFIXSHORT=src
      make renametoshort NOGDB=1 "OS_SOURCE=go32v2" "OS_TARGET=go32v2" PKGPRE=go32v2- PKGUNITSPRE=units-  ZIPSUFFIX=.source ZIPSUFFIXSHORT=src
    

### Create bootstrap compiler archives

  * darwin (fink)



From ppc386 and ppcppc create ppcuniversal using lipo, create .tar.bz2 archive (full name: fpc-version.universal-darwin.bootstrap.tar.bz2) and place it in the source dir. 

  


### Documentation

Latex takes date/time formats from the locale. Make sure your linux distro/environment variables are set to e.g. the US locale to avoid non English month names in the docs. In my case, it was simply the matter of changing the locale in the Ubuntu GUI settings app, and latex adapted. 

  
Most formats: run 
    
    
      make alldist
    

in fpcdocs/ 

CHM: run fixdocs.sh but edit it first to correct the location of the fpcsrc repo. 

## Release schedule

  * [FPC Release Schedule](<FPC_Release_Schedule.md> "FPC Release Schedule").



## Testing a release

In general: 

  1. check all zip/deb/rpm etc files
  2. try the installer (if available)
  3. make sure readme.txt & whatsnew.txt are for the current version
  4. run all executables
  5. make cycle with newly installed binaries and sources
  6. run testsuite
  7. IDE 
     1. open the installed hello.pp in IDE
     2. make a minor change in the demo in IDE & save it
     3. view documentation in IDE, traverse 2-3 pages (at least one with screenshots)
     4. compile the demo file in IDE
     5. run the demo within the IDE (debugger)
  8. Documentation (open all files) 
     1. Check PDF documentation (if available)
     2. Check HTML documentation (if available)
     3. Check TXT documentation (if available)



Target-specific: 

  1. GO32v2 
     1. dos???.zip installation
     2. dos???full.zip installation (over the previously installed dos???.zip to simulate updates)
  2. OS/2 
     1. os2???.zip installation
     2. os2???full.zip installation (over the previously installed os2???.zip to simulate updates)
  3. Win32 
     1. Run installer
  4. x86_64-linux and i386-linux 
     1. Test a .rpm install
     2. Test a .deb install
  5. darwin (fink) 
     1. fink install fpc-config fpc
     2. fink install fpc-doc
     3. optional on Intel-Darwin: fink install fpc-cross-***



## Testers template

See [Testers Template](<Testers_Template.md> "Testers Template"). 

### Previous releases

[Testers 3.2.2](<Testers_3.2.md> "Testers 3.2.2") | [Testers 3.2.0](<Testers_3.2.md> "Testers 3.2.0") | [Testers 3.0.4](<Testers_3.0.md> "Testers 3.0.4") |   
---|---|---|---  
[Testers 2.6.4](<Testers_2.6.md> "Testers 2.6.4") | [Testers 2.6.2](<Testers_2.6.md> "Testers 2.6.2") | [Testers 2.6.0](<Testers_2.6.md> "Testers 2.6.0")  
[Testers 2.4.4](<Testers_2.4.md> "Testers 2.4.4") | [Testers 2.4.2](<Testers_2.4.md> "Testers 2.4.2") | [Testers 2.4.0](<Testers_2.4.md> "Testers 2.4.0")  
[Testers 2.2.4](<Testers_2.2.md> "Testers 2.2.4") | [Testers 2.0.4](<Testers_2.0.md> "Testers 2.0.4") |

---

_Source: [https://wiki.freepascal.org/Release_engineering](https://web.archive.org/web/20250124213422/https://wiki.freepascal.org/Release_engineering)_
