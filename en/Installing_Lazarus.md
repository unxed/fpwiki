# Installing Lazarus

│ **[Deutsch (de)](</Installing_Lazarus/de> "Installing Lazarus/de")** │  **English (en)** │  **[español (es)](</Installing_Lazarus/es> "Installing Lazarus/es")** │  **[suomi (fi)](</Installing_Lazarus/fi> "Installing Lazarus/fi")** │  **[français (fr)](</Installing_Lazarus/fr> "Installing Lazarus/fr")** │  **[magyar (hu)](</Installing_Lazarus/hu> "Installing Lazarus/hu")** │  **[日本語 (ja)](</Installing_Lazarus/ja> "Installing Lazarus/ja")** │  **[한국어 (ko)](</Installing_Lazarus/ko> "Installing Lazarus/ko")** │  **[polski (pl)](</Installing_Lazarus/pl> "Installing Lazarus/pl")** │  **[português (pt)](</Installing_Lazarus/pt> "Installing Lazarus/pt")** │  **[русский (ru)](<../ru/Installing_Lazarus.md> "Installing Lazarus/ru")** │  **[slovenčina (sk)](</Installing_Lazarus/sk> "Installing Lazarus/sk")** │  **[Tiếng Việt (vi)](</Installing_Lazarus/vi> "Installing Lazarus/vi")** │  **[中文（中国大陆） (zh_CN)](</Installing_Lazarus/zh_CN> "Installing Lazarus/zh CN")** │    
****

## Contents

  * 1 Overview
  * 2 Lazarus system requirements
  * 3 Operating system specific guides
    * 3.1 FreeBSD
    * 3.2 Haiku
    * 3.3 Linux
    * 3.4 macOS
    * 3.5 Raspbian
    * 3.6 Windows
  * 4 Multiple Lazarus installs
  * 5 Troubleshooting
  * 6 Lazarus FAQ
  * 7 Installing old versions
  * 8 See also



## Overview

For people who simply want to install Lazarus and start using it for programming, the easiest approach is to download and install a recent, reasonably stable binary release (such as a Linux ".rpm" package, a Windows ".exe" installer, or a macOS ".dmg" disk image or installer ".pkg" package). 

For those who want to participate in the development of the compiler or the Lazarus IDE, or for those who want the most up-to-date tools, an installation from source files is necessary. 

The Lazarus IDE provides two main parts: 

  * LCL - the Lazarus Component Library
  * IDE - the RAD tool itself



These in turn are dependent on: 

  * FPC - the Free Pascal Compiler
  * FCL - the Free Pascal Component library, containing most of the non-graphic components used by the Lazarus IDE.



## Lazarus system requirements

  1. A Free Pascal Compiler, packages, and sources. (*Important*: of the same version/date)
  2. A supported widget set: 

    

Win32/Win64
    The native Win32 API can be used, or the Qt widgetset.
Linux/BSD
    GTK+ 2.x or Qt : Most Linux distributions and *BSDs already install the GTK+ 2.x libraries. You can also find them at <http://www.gtk.org>.   
Qt is also supported with all distributions (auto installed if you prefer KDE).
macOS
    You need the Apple Xcode developer tools. For macOS versions before 10.15 (Catalina), the 32 bit Carbon or 64 bit Cocoa widget sets can be used.   
For macOS 10.15 onwards, the 64 bit Cocoa widget set must be used as all 32 bit support has been removed by Apple.  
Qt can be used too, but it requires much more effort.



    

    The Qt widget set is supported on Linux 32/64, Win 32/64, macOS 32/64, FreeBSD 32/64, Haiku and embedded Linux (qtopia) platforms. For more details about the installation of Qt, see the [Qt Interface](<Qt_Interface.md> "Qt Interface") article.

## Operating system specific guides

  * Remember that the Free Pascal Compiler and the Lazarus IDE are separate products, you almost certainly need to install FPC, FPC Source and Lazarus (maybe in that order!).


  * Some people recommended using the [fpcUP](<fpcup.md> "fpcup") updater-installer for first time users of Lazarus, which installs Free Pascal and Lazarus in one go into a single subdirectory structure ( ~/development ).



### FreeBSD

  * See [Installing Lazarus on FreeBSD](<Installing_Lazarus_on_FreeBSD.md> "Installing Lazarus on FreeBSD")



### Haiku

  * See [Installing Lazarus on Haiku](<Installing_Lazarus_on_Haiku.md> "Installing Lazarus on Haiku")



### Linux

**See[Installing Lazarus on Linux](<Installing_Lazarus_on_Linux.md> "Installing Lazarus on Linux") which covers most of what you need for most Linux Distributions.**

  * The command to start Lazarus from a console is [startlazarus](<startlazarus.md> "startlazarus"). If you installed it from a Debian package, you should have a Lazarus menu entry under Application/Programming.
  * Issue: there is an ambiguity with a program also called "lazarus" from a `tct` package available for Ubuntu.
  * For a fully working Lazarus installation, older versions of the FPC compiler, FPC source or Lazarus can be a problem if present.
  * Some people recommended using [fpcUP](<fpcup.md> "fpcup") updater-installer for first time users of Lazarus, which installs Free Pascal and Lazarus in one go into a single subdirectory structure (~/development).



Some distribution specific pages exist, but they may not be as up to date as the [Installing Lazarus on Linux](<Installing_Lazarus_on_Linux.md> "Installing Lazarus on Linux") guide. 

  * [Install on aarch64 Arch or Manjaro](<Install_on_aarch64_Arch_or_Manjaro.md> "Install on aarch64 Arch or Manjaro")
  * [Install on Fedora](<Install_on_Fedora.md> "Install on Fedora")
  * [Scientific Linux](<Scientific_Linux.md> "Scientific Linux")



**Ubuntu/Debian Linux notes**

  * The Debian Testing Repository, unlike Ubuntu Releases, often contains a current or near current version of FPC and Lazarus. Feedback is needed and appreciated; please send your comments to Carlos Laviola <claviola@debian.org>
  * Building debs the easy way - A possible way to get a current working installation of Lazarus is to download and build your own .deb packages by following the instructions at [How to setup a FPC and Lazarus Ubuntu repository](<How_to_setup_a_FPC_and_Lazarus_Ubuntu_repository.md> "How to setup a FPC and Lazarus Ubuntu repository")



### macOS

  * See [Installing Lazarus on macOS](<Installing_Lazarus_on_macOS.md> "Installing Lazarus on macOS")



### Raspbian

  * See [Lazarus on Raspberry Pi](<Lazarus_on_Raspberry_Pi.md> "Lazarus on Raspberry Pi")



### Windows

  * See [Installing Lazarus on Windows](<Installing_Lazarus_on_Windows.md> "Installing Lazarus on Windows")



## Multiple Lazarus installs

Please see [Multiple Lazarus](<Multiple_Lazarus.md> "Multiple Lazarus") for details on having more than one Lazarus version installed on one system. We cover issues that can arise due to multiple Lazarus installs here, because they can also happen when installing over a previous version. 

## Troubleshooting

Troubleshooting details that should (hopefully) be applicable across platforms may be found in the article [Installation Troubleshooting](<Installation_Troubleshooting.md> "Installation Troubleshooting"). 

## Lazarus FAQ

The Lazarus FAQ - Frequently Asked Questions - page is available [here](<Lazarus_Faq.md> "Lazarus Faq"). 

## Installing old versions

See [Installation hints for old versions](<Installation_hints_for_old_versions.md> "Installation hints for old versions")

## See also

  * A real "in depth" build guide is [here](<http://www.stack.nl/~marcov/buildfaq.pdf>).
  * [Getting Lazarus](<Getting_Lazarus.md> "Getting Lazarus").

---

_Source: [https://wiki.freepascal.org/Installing_Lazarus](https://web.archive.org/web/20250330014543/https://wiki.freepascal.org/Installing_Lazarus)_
