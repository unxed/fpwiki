# Haiku specific Release Engineering

│ **English (en)** │  **[español (es)](</Haiku_specific_Release_Engineering/es> "Haiku specific Release Engineering/es")** │    
****

## Contents

  * 1 Creating a package for Haiku's package management system
  * 2 Getting fpcbuild repository
  * 3 Getting binaries for debugging support in the IDE
    * 3.1 Instructions used to compile above gdb libraries for Freepascal (probably deprecated)
  * 4 Getting documentation
  * 5 Building packages
    * 5.1 Build under older version of Haiku (before hrev46213)



#### Creating a package for Haiku's package management system

The goal is to have a package already available from HaikuDepot, the application that list available applications and can download them to your computer. Once the package is downloaded, you can launch your application directly (no lengthy install process under Haiku ;-) 

! Work in progress... 

For sometime, recipes are available to create an fpc package for Haiku's package management system (see <https://github.com/haikuports/haikuports/tree/master/dev-lang/fpc>). Those recipes were quite simple and not complete. At least, it was a first step to bootstrap : a package is required to build FreePascal from source. Those packages are build using existing binaries in the official distribution and do not need an existing compiler package to be build : <https://freepascal.org/down/i386/haiku.var>. 

The second step is to generate packages from source, using official fpcbuild's sources package (from <https://sourceforge.net/projects/freepascal/files/Source/3.0.2/> for example) 

#### Getting fpcbuild repository

Get sources, download the fpcbuild from ftp and extract it or use a command like: 
    
    
     svn export <http://svn.freepascal.org/svn/fpcbuild/tags/release_2_4_0> fpcbuild
    

Currently, it might be difficult to get the whole tree in one shot under Haiku. The root cause is not yet known (Haiku ? Subversion ? the network ?). 

An alternative is to checkout the fpcbuild repository. It will consume more disk space, but you can update the tree in case of partial download. 

#### Getting binaries for debugging support in the IDE

At the time of writing, a package containing the needed files is available here : <http://olivier.coursiere.free.fr/download/libgdb-6.3-i386-haiku.zip>

Extract this file in fpcbuild/fpcsrc/. 

On recent versions of Haiku (aka with package management), it might be necessary to install some development package in order to link with above GDB libraries : 

  * libncurses_devel



##### Instructions used to compile above gdb libraries for Freepascal (probably deprecated)

Keep for reference of required libraries under Haiku with package management. 

While you can compile gdb on your own, the easiest way (by far) under Haiku is to build Haiku yourself. Instructions are available at <http://www.haiku-os.org/development>. 

Then, you can collect all the needed files in the build tree : 

  * **libgdb.a** in `generated/objects/haiku/x86/release/bin/gdb/gdb`
  * **libbfd.a** in `generated/objects/haiku/x86/release/bin/gdb/bfd`
  * **libiberty.a** in `generated/objects/haiku/x86/release/bin/gdb/libiberty`
  * **libopcodes.a** in `generated/objects/haiku/x86/release/bin/gdb/opcodes`
  * **libreadline.a** in `generated/objects/haiku/x86/release/bin/gdb/readline`


  * **libintl.a** in `generated/objects/haiku/x86/release/bin/gawk/intl/`


  * **libgcc.a** in `/boot/develop/abi/x86/gcc2/tools/gcc-2.95.3-haiku-081024/lib/gcc-lib/i586-pc-haiku/2.95.3-haiku-081024` (you may have to adapt the path to the installed gcc version on your machine).



Put those files in a libgdb subfolder in fpcsrc (in the corresponding <system>/<architecture> folders) : 
    
    
    fpcbuild/fpcsrc/libgdb/haiku/i386
    

#### Getting documentation

Get the most recent docs package from the ftp site, it's called doc-pdf.tar.gz and copy it into the `fpcbuild/` directory. You should put this file in the root directory of the source repository, aka in fpcbuild. 

In case of an official release candidate or final version, you should ask the file to the release coordinator. 

#### Building packages

Change into `fpcbuild/`, then execute 
    
    
     install/makepack
    

##### Build under older version of Haiku (before hrev46213)

**pre-2.4.0 specific** : if haiku is not in the UNIXs variable at the begining of Makefile, you will have to regenerate them with fpcmake (at least from r14324). If not, the install script adapted for Haiku will not fully work. Here is the command line to do so : 
    
    
     fpcmake -Tall -r
    

Haiku is unix like enough to use the Freepascal's unix scripts to generate a binary distribution. Unfortunately, there is no `/usr/bin` path under Haiku. So, the usual shebang will not work. A workaround is to execute the script with `sh` directly. 

Change into `fpcbuild/`, then execute 
    
    
     sh install/makepack

---

_Source: [https://wiki.freepascal.org/Haiku_specific_Release_Engineering](https://web.archive.org/web/20250124212356/https://wiki.freepascal.org/Haiku_specific_Release_Engineering)_
