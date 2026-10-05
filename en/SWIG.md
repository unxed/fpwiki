# SWIG

## Contents

  * 1 Overview
  * 2 Object Pascal support
  * 3 Download source
  * 4 Compiling
  * 5 Download
  * 6 Development
  * 7 See also



## Overview

[SWIG](<http://swig.org/>) is a program that takes C++ .h and .cpp files and generates object-oriented language bindings for various languages. 

## Object Pascal support

The main SWIG tree does not support Delphi/Object Pascal. 

Some years ago, Stefano Moratti wrote a Delphi adapter for the SWIG code which allows automatic translation of C++ libraries into Delphi mode Object Pascal. [[1]](<http://downloadit.pf-control.de/dl.php?ref=swig>) His patches were not integrated into mainstream SWIG, but they have been used, e.g. to create a Delphi binding for GDAL: [[2]](<http://article.gmane.org/gmane.comp.programming.swig.devel/18297>)

In 2012, FPC mailing list user d.l.i.w. adapted the original Delphi patches to the more recent SWIG 2.0.8 source but unfortunately forgot to do it in a version-controlled repository. See here: [[3]](<http://www.mail-archive.com/fpc-pascal@lists.freepascal.org/msg30422.html>) and more about functionality: [[4]](<http://www.mail-archive.com/fpc-pascal@lists.freepascal.org/msg30349.html>)

Unfortunately, there have been no further efforts to incorporate this into current SWIG trunk. Additionally, FPC developers have indicated that they prefer C++ binding support to being in FPC itself but no work is being done on this currently. 

UPDATE 2017: There is a SWIG 3.0.11 for Delphi and Object Pascal: [[5]](<http://www.fmxexpress.com/create-wrapper-interfaces-for-c-and-c-libraries-using-swig-with-delphi-support/>)

UPDATE 2020: The version from this repo didn't build: [[6]](<https://github.com/FMXExpress/swig-delphi>)

But the one from this repo did: [[7]](<https://github.com/lynxnake/swig>)

I didn't test if it works or not. But I just want to share it with you. 

The last commit was from 2017, it seemed he abandoned it. 

Glad if it worked for you. But don't expect much from it! I think we need a paid professional fully working on it to bring it up to speed with upstream and eventually merge it to upstream. 

## Download source

The complete set of 2.0.8 files is uploaded at [[8]](<https://dliw.pfweb.eu/dl.php?ref=swig>) and [[9]](<https://bitbucket.org/reiniero/swigdelphi/src>). 

An on going effort of 3.0.11 files [[10]](<http://www.fmxexpress.com/wp-content/uploads/swig-delphi.7z>) and [[11]](<https://github.com/FMXExpress/swig-delphi>). 

Recent 4.0.0 try [[12]](<https://github.com/lynxnake/swig>)

## Compiling

In the configure step, it doesn't hurt to specify 
    
    
    --with-delphi=yes
    

, to indicate you want support for Object Pascal. 

On Linux, do e.g. this (run on Debian Linux; adjust paths etc as necessary: 
    
    
    su -
    aptitude -y install build-essential yodl byacc mercurial
    # build-essential has compilers
    # yodl needed for man page install
    # byacc apparently needed at least on Debian x86
    # mercurial needed to get the swig sources from the bitbucket repository
    # alternatively download, extract etc
    exit #to regular user, called pascaldev on this system
    
    cd ~
    hg clone https://bitbucket.org/reiniero/swigdelphi
    cd ~/swigdelphi/swig-2.0.8
    
    # configure:
    chmod ug+rx configure
    # tell it to install in user's home directory; adjust as necessary
    mkdir ~/swig
    ./configure --with-delphi=yes --prefix=/home/pascaldev/swig
    
    #compile:
    make
    
    #install:
    chmod ug+rx Tools/config/install-sh
    make install
    
    #should be installed in ~/swig now; e.g. ~/swig/bin/swig should work
    ~/swig/bin/swig -delphi -version
    

## Download

Version for Delphi which cannot be compiled now: <https://github.com/FMXExpress/swig-delphi>

Version which can be compiled: <https://github.com/lynxnake/swig>

## Development

It would of course be very welcome if somebody 

  * got the current version into main SWIG so it is more easily usable
  * improve the code



Additionnally, binary releases for e.g. Linux and Windows are warmly welcome for upload to e.g. the BitBucket repository. Please contact forum user BigChimp for that. 

## See also

  * [SWIG project homepage](<http://swig.org/>)
  * [Pascal for C++ users](<Pascal_for_C++_users.md> "Pascal for C++ users")
  * [Latest SWIG 3.0 for Delphi development](<http://www.fmxexpress.com/create-wrapper-interfaces-for-c-and-c-libraries-using-swig-with-delphi-support/>)

---

_Source: [https://wiki.freepascal.org/SWIG](https://web.archive.org/web/20240308084944/https://wiki.freepascal.org/SWIG)_
