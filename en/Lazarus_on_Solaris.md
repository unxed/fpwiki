# Lazarus on Solaris

This howto explains the steps to get Lazarus svn and FPC svn up and running starting from a clean Solaris 10 system on either x86 or SPARC. It assumes that you have already used svn (Subversion) and have built FPC and Lazarus from source before. 

There are older notes referring only to the FPC compiler and Solaris 7 at [Solaris Port](<Solaris_Port.md> "Solaris Port"). While the FPC compiler will run on Solaris 8 with a little bit of library fiddling, it's unlikely that Lazarus will run on it due to missing prerequisite libraries. 

## Contents

  * 1 Install the FPC Compiler Solaris Binaries
  * 2 Install Required Libraries
  * 3 A note on gld (Intel architecture only)
  * 4 A note on paths (all architectures)
  * 5 Get and Build Sources (Intel architecture)
  * 6 Get and Build Sources (SPARC architecture)
  * 7 Solaris 11 Express
  * 8 Interim note: Lazarus 1.0 and successors
  * 9 See also



## Install the FPC Compiler Solaris Binaries

_For x86:_

Get and install fpc 2.4.4 from <ftp://193.224.143.39/pub/fpc/dist/2.4.4/i386-solaris/fpc-2.4.4.i386-solaris.tar.gz> and the install script <ftp://193.224.143.39/pub/fpc/dist/2.4.4/i386-solaris/install.sh>. 

_For SPARC:_

The most recent version available as binaries is 2.4.2, which can be downloaded from <ftp://193.224.143.39/pub/fpc/dist/2.4.2/sparc-solaris/fpc-2.4.2.sparc-solaris.tar.gz>. There is an install script corresponding to this version at <ftp://193.224.143.39/pub/fpc/dist/2.4.2/sparc-solaris/install.sh>, but see <http://62.166.198.202/view.php?id=18271> for patches that might be needed; there might be a more recent version of this script with the trunk sources available via Subversion. 

In either case it is probably worth making sure that the binaries are no more than one release behind the svn trunk. At the time of writing (July '11) trunk is 2.4.5, so while the x86 binaries at 2.4.4 are OK it might be advisable to get, build and install the 2.4.4 release for SPARC before proceeding any further. As of mid 2015, version 2.7.1 revision 29398 is particularly robust. 

It might be necessary to treat ncurses from Sunfreeware (see below) as a prerequisite to recompilation. If necessary, apply the patch from <http://bugs.freepascal.org/view.php?id=19675>

## Install Required Libraries

There is an extensive freeware library containing GNU utilities and libraries at <http://sunfreeware.com/> (mirrored at e.g. <http://sunfreeware.saix.net/>), these are more recent and more complete than the libraries etc. on Sun's Solaris 10 "Companion" CD. Note that these are initially downloaded using a web browser rather than by a graphical package or update manager, so don't waste time hunting for one on your system. Refer to <http://www.sunfreeware.com/download.html> for general instructions. 

Install the following packages from SunFreeware or a mirror. This is a long list because it includes a complete set of GTK2 libraries and dependencies, some of which mandate updated GNU binutils which themselves imply an update of gcc, (g)make and so on. In addition a development system will likely require a copy of Subversion, which itself has extensive dependencies. These are the versions for both Intel and SPARC systems at the time of writing (July '11): 
    
    
    atk-1.18.0
    binutils-2.21.1
    cairo-1.4.10
    coreutils-8.11
    expat-2.0.1
    fontconfig-2.8.0 †
    freetype-2.4.2
    gcc-3.4.6
    gdb-6.8
    glib-2.25.13
    gtk+-2.12.0
    jpeg-8a
    libiconv-1.13.1
    libintl-3.4.0
    libpng-1.2.44
    make-3.82
    ncurses-5.7
    pango-1.18.2
    render-0.8
    renderproto-0.9.3
    tiff-3.9.4
    xft-2.1.2
    xrender-0.8.3
    zlib-1.2.5
    
    † Install this only if it's not already on the system as part of Solaris.
    

The actual file names will be qualified by OS version and architecture, e.g. render-0.8-sol10-intel.local.gz and render-0.8-sol10-sparc.local.gz. 

In addition, for a complete Subversion (i.e. including Apache-based server) installation: 
    
    
    apache-2.2.19
    neon-0.29.5
    openldap-2.4.24
    openssl-1.0.0d
    sasl-2.1.21
    subversion-1.6.12
    

It might be less disruptive to build Subversion from source, e.g. <http://subversion.tigris.org/downloads/subversion-1.6.17.tar.bz2>. According to the INSTALL file the actual dependencies for a client using the svn: (as distinct from http:) protocol are libapr, libapr-util, SQLite and libz; these are available from e.g. <http://subversion.tigris.org/downloads/subversion-deps-1.6.17.tar.bz2>: 
    
    
    gtar xjf subversion-1.6.17.tar.bz2
    gtar xjf subversion-deps-1.6.17.tar.bz2
    cd subversion-1.6.17  
    export LDFLAGS='-lintl -L/usr/local/lib -R/usr/local/lib'
    ./configure
    make
    make install
    
    # I suspect this leaves some symlinks in a parlous state, but it appears usable.
    

A client that is capable of using the http: protocol will also require an SSL library, this might be problematic on very old systems but a hack that might work is to build additional libraries from source packages from a Linux distro of the same age (e.g. Slackware 8.1 for Solaris 8). 

Adapt your PATH variable to get the gnu version of the binutils: 

$ export PATH=/usr/local/bin:$PATH 

$ export LD_LIBRARY_PATH=/usr/local/lib:$LD_LIBRARY_PATH 

Note that when FPC looks for an assembler and linker, it first scans the path looking for gas and gld, and then for as and ld. Make sure that the utilities that are found first are the ones from the binutils package installed above, other versions (i.e. the proprietary assembler and linker supplied with Solaris, or older GNU software) will probably not work as expected. Exact details might depend on what you've done with the system, which shell you're running and so on. 

Check your GTK2 installation: 

$ gtk-demo 

## A note on gld (Intel architecture only)

Gnu ld creates binaries that are not completely solaris abi compatible. malloc (libc) for example always returns a null pointer. FPC uses the solaris linker by using the -Xn compiler option (available from 2.4.2 onwards but not documented until 3.2). Solaris ld is in /usr/css/bin and gnu binutils has installed the link /usr/bin/ld : 

# rm /usr/bin/ld 

# ln -s /usr/css/bin/ld /usr/bin/ld 

Note that you need to specify the -Xn compiler option for all your fpc and lazarus programs. Lazarus programs linked without -Xn crash with the message GLib-ERROR ... Failed to allocate 42 bytes... core dump. 

## A note on paths (all architectures)

The most common causes of problems are missing directories in the current search path, or not being able to find required libraries (e.g. libcairo.so.2) when trying to start the Lazarus IDE or a program that uses the LCL. If running the Bash shell, make sure that the ~/local.profile file exports appropriate PATH and LD_LIBRARY_PATH variables: 
    
    
    #
    # Copyright (c) 2001 by Sun Microsystems, Inc.
    # All rights reserved.
    #
    # ident "@(#)local.profile      1.10    01/06/23 SMI"
    stty istrip
    PATH=/usr/local/bin:/usr/sfw/bin:/usr/bin:/usr/ucb:/etc:.
    MANPATH=/usr/sfw/man:/usr/man
    PAGER=/usr/bin/less
    export PATH MANPATH PAGER
    
    export LD_LIBRARY_PATH=/usr/local/lib:$LD_LIBRARY_PATH
    

## Get and Build Sources (Intel architecture)

$ svn co <http://svn.freepascal.org/svn/fpc/trunk> fpc 

$ cd fpc 

$ make clean all OPT="-Xn" 

$ su 

# make install INSTALL_PREFIX=/usr 

/etc/fpc.cfg installed with 2.4.4 is fine for your new 2.5.1 installation. 

$ svn co <http://svn.freepascal.org/svn/lazarus/trunk> lazarus 

$ cd lazarus 

$ make clean all OPT="-Xn -k-L/usr/local/lib" PP=/usr/lib/fpc/2.5.1/ppc386 

## Get and Build Sources (SPARC architecture)

This assumes that version 2.4.4 of the compiler has been built and installed. A later version of the compiler should work, but there are some cases where the Lazarus sources might have problems with 2.4.5/2.5.x due to conditional directives. 

In the FPC sources, make sure that packages/Makefile.fpc includes iconvenc and gtk2, if necessary running fpcmake to ensure changes "stick". As a minimum: 
    
    
    dirs_solaris=fv fcl-web fastcgi fcl-async ibase mysql ncurses zlib oracle odbc postgres sqlite pthreads imagemagick \
                  libpng x11 gdbm tcl syslog libcurl opengl cairo gtk1 bfd svgalib fcl-extra \
                  imlib utmp  fpgtk xforms fftw pcap ggi  openssl gnome1 httpd13 httpd20 httpd22 numlib iconvenc gtk2 cairo
    

If Lazarus is to be compiled with database units etc. (i.e. make bigide) then also check the following: 
    
    
    In packages/src/sqldb/Makefile.fpc:
    
      dirs_solaris=interbase postgres mysql odbc oracle sqlite
    

$ cd fpc 

$ gmake NOGDB=1 OPT='-O- -gl' all 

$ su 

# make install 

# rm /usr/local/bin/ppcsparc 

# ln -s /usr/local/lib/fpc/2.4.4/ppcsparc /usr/local/bin/ppcsparc 

# exit 

/etc/fpc.cfg installed with 2.4.4 is fine for your new 2.5.1 installation. 

$ svn co <http://svn.freepascal.org/svn/lazarus/trunk> lazarus 

$ cd lazarus 

Either: 

$ make clean all 

or: 

$ make clean bigide 

## Solaris 11 Express

**Dependencies**

Depending on the installation media, gnome is already installed or can easily be installed by installing the slim_install package. subversion and gnu-binutils can be installed from the solaris repository with "pkg install" or the graphical Package Manager. 

**gld**

solaris ld is already installed in /usr/bin. The use of -Xn is still mandatory. 

## Interim note: Lazarus 1.0 and successors

_Do not link to this section, it will be superseded._

As of September 2012, Lazarus 1.0 has been released. As with the preceding version (0.9.30), the appropriate version of FPC to use for compilation is 2.6.0; later versions might be better with FPC 2.7.1 as described below. 

In the case of SPARC, there was a small number of late-breaking patches to fix code generation issues. While these are documented as having been applied to the development versions of the compiler (2.6.1 and 2.7.1), the more significant ones were also applied to the stable version (2.6.0) shortly before release: see the summary at <http://lists.freepascal.org/lists/fpc-devel/2012-September/029845.html>. 

Refer to <http://www.freepascal.org/develop.var> for information on getting a fixes-branch daily snapshot of the compiler, or fetching it from the Subversion repository. 

As of some time in 2013/2014, SunFreeware has moved to a subscription model announcing: 

    Following 20 years of providing free pre-compiled Open Source packages to the Solaris community though sponsorship. Sunfreeware.com no longer provides free downloads as of September 30, 2013 as the sponsorship has terminated.

As of early 2015, the final FPC development trunk version of the 2.6 era (2.7.1 svn release 29398) compiles on SPARC Solaris 10 without problems. Lazarus trunk around svn release 48671 requires libraries etc. as discussed above, a usable mirror for Solaris 10 packages appears to be <http://sunfreeware.saix.net/>. 

Solaris appears to be more sensitive than Linux to whether something is being run as a privileged or unprivileged user. Subversion build might need to be done as root, and FPC installation may need to be done as a root login (i.e. not just using sudo). 

In the longer term, the viability of Solaris as an easily-obtained operating system is questionable. <http://opensxce.org/> might be worth investigating, this might prefer alternative packages from <http://mirror.opencsw.org/>. 

## See also

  * [Solaris Port](<Solaris_Port.md> "Solaris Port")

---

_Source: [https://wiki.freepascal.org/Lazarus_on_Solaris](https://web.archive.org/web/20250601000000/https://wiki.freepascal.org/Lazarus_on_Solaris)_
