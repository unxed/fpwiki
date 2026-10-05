# FPCDocs Tutorial

## Contents

  * 1 Overview
  * 2 Prerequisites
    * 2.1 Assumptions
  * 3 Generating documentation
  * 4 References



## Overview

This tutorial will guide you through getting the latest FreePascal docs sources and generating documentation from it. Please see the official documentation [3] for more details. 

November 2011: I'm having trouble generating docs, and finding documentation on it, so I decided to start this page to help me and others. Please feel free to update/add info (and remove this notice when it actually works ;) \--[BigChimp](</User:BigChimp> "User:BigChimp") 05:11, 22 November 2011 (CET) 

## Prerequisites

  * A working FreePascal environment must be installed (e.g. a stable version from a Debian repository). See [1] for more details.
  * Having a working Lazarus installed may not hurt
  * As the documentation needs FPC source files, you will need the latest source from git. See [1] on how to get that.
  * The fpcdocs:


    
    
    svn co http://svn.freepascal.org/svn/fpcdocs/trunk fpcdocs
    

  * Some tex programs: texlive, tex4ht, and dvi2tty if you want to generate plain-text documents (for DOS, mainly). The package names may differ depending on your distribution. The files a4.sty and moveverb.sty. 
    * Ubuntu 12: 
          
          apt-get install tex4ht texlive-binaries texlive-latex-base texlive-latex-recommended texlive-latex-extra
          




### Assumptions

I'm assuming you're running on Linux; apparently generating docs on Windows is not supported. Also, this tutorial assumes you have a stable system-wide installation of FPC and Lazarus, and a working SVN version of FPC with proper fpc.cfg. Finally, you have your SVN FPC source and fpcdocs directories set up like this: 
    
    
    ~/fpc/
    ~/fpc/fpcdocs       #contains SVN http://svn.freepascal.org/svn/fpcdocs/trunk
    ~/fpc/fpcsrc        #contains FPC sources, e.g. http://svn.freepascal.org/svn/fpc/branches/fixes_2_6
    ~/fpc/fpcsrc/compiler/utils/msg2inc.pp  #contains FPC msg2inc tool
    

## Generating documentation
    
    
    make clean
    sh fixdocs.sh
    

write me ;) 

## References

  * [1] [Installing Lazarus](<Installing_Lazarus.md> "Installing Lazarus")
  * [2] Build FAQ
  * [3] FPDoc manual

---

_Source: [https://wiki.freepascal.org/FPCDocs_Tutorial](https://web.archive.org/web/20250403104301/https://wiki.freepascal.org/FPCDocs_Tutorial)_
