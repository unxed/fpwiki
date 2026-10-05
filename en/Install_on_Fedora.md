# Install on Fedora

│ **English (en)** │  **[polski (pl)](</Install_on_Fedora/pl> "Install on Fedora/pl")** │    
****

**Please See[Installing Lazarus on Linux](<Installing_Lazarus_on_Linux.md> "Installing Lazarus on Linux") \- a page that covers most of what you need for most Linux Distributions.**

## Fedora 22 and newer
    
    
    sudo dnf install lazarus
    

The latest packages that are included by default on Fedora 38 are: 

  * lazarus 2.2.6
  * fpc 3.2.2



## Fedora 21 and below
    
    
    sudo yum install lazarus
    

## Install latest packages

Download the [latest RPM packages from SourceForge](<https://sourceforge.net/projects/lazarus/files/Lazarus%20Linux%20x86_64%20RPM>). You need to download the appropriate packages for: 

  * fpc
  * fpc-src
  * lazarus



Navigate to your download folder and install the packages in the following order: 
    
    
    sudo dnf install fpc-3.2.2-1.x86_64.rpm
    sudo dnf install fpc-src-3.2.2-1.x86_64.rpm
    sudo dnf install lazarus-2.2.6-0.x86_64.rpm
    

Of course you need to replace the filenames in the example above with the files you downloaded before.

---

_Source: [https://wiki.freepascal.org/Install_on_Fedora](https://web.archive.org/web/20231128164247/https://wiki.freepascal.org/Install_on_Fedora)_
