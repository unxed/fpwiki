# Porting Free Pascal Example

This is an example of how to port Free Pascal to a new target. The target used here is i386-minix, which is a Unix based microkernel system. Host system is a i386-linux system. For more information about the port see [here](</index.php?title=Target_Minix&action=edit&redlink=1> "Target Minix \(page does not exist\)"). 

**this example is a work in progress**

## Contents

  * 1 Prerequisites
    * 1.1 Binutils
  * 2 Compiler
    * 2.1 Modifications
    * 2.2 Compiling
  * 3 Adding the new RTL
  * 4 See also



## Prerequisites

You'll need the source code of Free Pascal (preferably the most recent version in svn), a SVN client, a working last release of Free Pascal (currently 2.4.0) and a working version of GCC. 

It's best to create a new directory where you'll work in. 
    
    
    mkdir $HOME/minix-port
    cd $HOME/minix-port
    

Then either copy an existing Free Pascal working copy on your machine or load a fresh one from the repository. I'll use the second possibility here (for additional help see [here](<Getting_Lazarus.md> "Getting Lazarus")): 
    
    
    svn checkout <http://svn.freepascal.org/svn/fpc/trunk> fpc
    

### Binutils

The first compiler for a new target is always a cross compiler, so we need utilities that allow us to create executables for our target on our host system. 

Minix supports the GNU binutils. As the current binutils (2.20) are not "up to date" regarding the output format of Minix we will use the port provided by the Minix distribution. 
    
    
    svn --username anonymous checkout <https://gforge.cs.vu.nl/svn/minix/trunk/bigports/binutils-2.16.1> minix-binutils
    

We also need the source of Minix, cause we need two include files to build the binutils successfully. 
    
    
    svn --username anonymous checkout <https://gforge.cs.vu.nl/svn/minix/trunk/src> minix
    

Now copy the needed include files to a directory that is included by the makefiles: 
    
    
    cp minix/include/minix/a.out.h minix-binutils/include/minix-a.out.h
    cp minix/include/ansi.h minix-binutils/include/ansi.h
    

You also need to change the include line in _minix-binutils/bfd/i386minix.c_ from 
    
    
    #include <a.out.h>
    

to 
    
    
    #include <minix-a.out.h>
    

Now we can build the binutils: 
    
    
    cd minix-binutils
    ./configure --host=i686-minix --prefix=../minix-binutils-bin/
    make
    make install
    cd ..
    

## Compiler

### Modifications

The following diff shows the changes that were done in already existing files in _compiler_ and _utils_ directories: 
    
    
    Index: compiler/i386/cputarg.pas
    ===================================================================
    --- compiler/i386/cputarg.pas	(Revision 15151)
    +++ compiler/i386/cputarg.pas	(Arbeitskopie)
    @@ -80,6 +80,9 @@
         {$ifndef NOTARGETNATIVENT}
           ,t_nativent
         {$endif}
    +    {$ifndef NOTARGETMINIX}
    +      ,t_minix
    +    {$endif}
     
     {**************************************
                  Assemblers
    Index: compiler/systems.pas
    ===================================================================
    --- compiler/systems.pas	(Revision 15151)
    +++ compiler/systems.pas	(Arbeitskopie)
    @@ -151,6 +151,7 @@
                  system_mips_linux,         { 66 }
                  system_mipsel_linux,       { 67 }
                  system_i386_nativent,      { 68 }
    +             system_i386_minix          { 69 }
            );
     
          type
    Index: compiler/compiler.pas
    ===================================================================
    --- compiler/compiler.pas	(Revision 15151)
    +++ compiler/compiler.pas	(Arbeitskopie)
    @@ -113,6 +113,9 @@
     {$ifdef nativent}
       ,i_nativent
     {$endif nativent}
    +{$ifdef minix}
    +  ,i_minix
    +{$endif minix}
       ,globtype;
     
     function Compile(const cmd:string):longint;
    Index: utils/fpcm/fpcmake.ini
    ===================================================================
    --- utils/fpcm/fpcmake.ini	(Revision 15151)
    +++ utils/fpcm/fpcmake.ini	(Arbeitskopie)
    @@ -972,6 +972,14 @@
     SHORTSUFFIX=nativent
     endif
     
    +# Minix
    +ifeq ($(OS_TARGET),minix)
    +BATCHEXT=.sh
    +EXEEXT=
    +HASSHAREDLIB=0
    +SHORTSUFFIX=minix
    +endif
    +
     else
     # long version for 1.0.x - target specific extensions
     
    Index: utils/fpcm/fpcmmain.pp
    ===================================================================
    --- utils/fpcm/fpcmmain.pp	(Revision 15151)
    +++ utils/fpcm/fpcmmain.pp	(Arbeitskopie)
    @@ -70,7 +70,8 @@
             o_linux,o_go32v2,o_win32,o_os2,o_freebsd,o_beos,o_haiku,o_netbsd,
             o_amiga,o_atari, o_solaris, o_qnx, o_netware, o_openbsd,o_wdosx,
             o_palmos,o_macos,o_darwin,o_emx,o_watcom,o_morphos,o_netwlibc,
    -        o_win64,o_wince,o_gba,o_nds,o_embedded,o_symbian,o_nativent
    +        o_win64,o_wince,o_gba,o_nds,o_embedded,o_symbian,o_nativent,
    +        o_minix
           );
     
           TTargetSet=array[tcpu,tos] of boolean;
    @@ -92,14 +93,16 @@
             'linux','go32v2','win32','os2','freebsd','beos','haiku','netbsd',
             'amiga','atari','solaris', 'qnx', 'netware','openbsd','wdosx',
             'palmos','macos','darwin','emx','watcom','morphos','netwlibc',
    -        'win64','wince','gba','nds','embedded','symbian','nativent'
    +        'win64','wince','gba','nds','embedded','symbian','nativent',
    +        'minix'
           );
     
           OSSuffix : array[TOS] of string=(
             '_linux','_go32v2','_win32','_os2','_freebsd','_beos','_haiku','_netbsd',
             '_amiga','_atari','_solaris', '_qnx', '_netware','_openbsd','_wdosx',
             '_palmos','_macos','_darwin','_emx','_watcom','_morphos','_netwlibc',
    -        '_win64','_wince','_gba','_nds','_embedded','_symbian','_nativent'
    +        '_win64','_wince','_gba','_nds','_embedded','_symbian','_nativent',
    +        '_minix'
           );
     
           { This table is kept OS,Cpu because it is easier to maintain (PFV) }
    @@ -133,7 +136,8 @@
             { nds    }  ( false, false, false, false, false, true,  false, false, false, false, false, false, false, false),
             { embedded }( true,  true,  true,  true,  true,  true,  true,  true,  true , false, false, false, false, false),
             { symbian } ( true,  false, false, false, false, true,  false, false, false, false, false, false, false, false),
    -        { nativent }( true,  false, false, false, false, false, false, false, false, false, false, false, false, false)
    +        { nativent }( true,  false, false, false, false, false, false, false, false, false, false, false, false, false),
    +        { minix }   ( true,  false, false, false, false, false, false, false, false, false, false, false, false, false)
           );
     
         type
    

You also need to add the files i_minix.pas and t_minix.pas in compiler/systems directory. 

TODO 

### Compiling

## Adding the new RTL

TODO 

  


## See also

  * [Porting Free Pascal](<Porting_Free_Pascal.md> "Porting Free Pascal")

---

_Source: [https://wiki.freepascal.org/Porting_Free_Pascal_Example](https://web.archive.org/web/20250331235033/https://wiki.freepascal.org/Porting_Free_Pascal_Example)_
