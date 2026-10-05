# Install on Ubuntu

[![Logo-ubuntu cof-orange-hex.svg](https://upload.wikimedia.org/wikipedia/commons/thumb/a/ab/Logo-ubuntu_cof-orange-hex.svg/50px-Logo-ubuntu_cof-orange-hex.svg.png)](</File:Logo-ubuntu_cof-orange-hex.svg>)

This article applies to [Ubuntu](</Category:Ubuntu> "Category:Ubuntu") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

**See[Installing Lazarus on Linux](<Installing_Lazarus_on_Linux.md> "Installing Lazarus on Linux") \- this page covers most of what you need for most Linux Distributions.**

The easiest way to install lazarus and corresponding versions of [FPC](<FPC.md> "FPC") is by means of the **Ubuntu Software Center** that will install the lazararus/fpc version that is supported by the Ubuntu version. 
    
    
    sudo apt install lazarus
    

## Contents

  * 1 15.10
  * 2 20.04
  * 3 21.10
  * 4 from .deb
  * 5 git



## 15.10

On Ubuntu 15.10 Lazarus 1.4.0 / FPC 2.6.4 will be installed. 

## 20.04

On Ubuntu FPC 3.0.4 will be installed in `/usr/bin` while Lazarus 2.0.6 will be installed in `~/.lazarus`

## 21.10

FPC 3.2.2 will be installed in `/usr/bin` while Lazarus 2.0.12 will be installed in `~/.lazarus`

## from .deb

Install a specific lazarus version from .deb: 
    
    
    sudo apt install gdebi
    sudo gdebi lazarus-project_2.0.12-0_amd64.deb
    

## git
    
    
    git clone <https://gitlab.com/freepascal.org/lazarus/lazarus.git> .lazarus2
    cd .lazarus2
    make
    ./lazarus --pcp=~/.lazarus2

---

_Source: [https://wiki.freepascal.org/Install_on_Ubuntu](https://web.archive.org/web/20250417143841/https://wiki.freepascal.org/Install_on_Ubuntu)_
