# VirtualTreeview

│ **English (en)** │

## Contents

  * 1 About
  * 2 Author
  * 3 License
  * 4 Download
  * 5 Repository
    * 5.1 Version 8
  * 6 Bug report / Feature request
  * 7 Change Log
  * 8 Dependencies / System Requirements
  * 9 Installation
  * 10 Help
  * 11 See also



### About

VirtualTreeview is a [TTreeView](<TTreeView.md> "TTreeView") control built from ground up. 

Its main characteristics are : 

  * it is extremely fast. Adding one million nodes takes only ~700 milliseconds
  * very small memory foot print. by only allocating about 60 bytes per node
  * optimized for high speed access. It takes as few as 0.5 seconds to traverse one million nodes
  * Multiselection is supported
  * Drawing the entire tree to a bitmap or the printer is supported
  * fixed background image can be used
  * Hot style for nodes is supported
  * Nodes can have individual heights
  * Sorting via compare [callback](<callback.md> "callback")
  * Support for unicode
  * Multiple columns are supported
  * ... and many more



This component was designed for cross-platform applications. 

[![Anivt.gif](https://wiki.freepascal.org/images/a/a4/Anivt.gif)](</File:Anivt.gif>)

### Author

Author: Mike Lischke  
Old LCL Port: Joerg Thaler,[Christian Ulrich](</User:Christian> "User:Christian")  
New LCL Port: [Luiz Américo](</User:Luizmed> "User:Luizmed")

### License

[LGPL](<http://www.opensource.org/licenses/lgpl-license.php>) or [Mozilla Public Licence 1.1](<http://opensource.org/licenses/mozilla1.1.php>)

### Download

See the project [releases page](<https://github.com/blikblum/VirtualTreeView-Lazarus/releases>)

Note that v5.5.3.1 is included in Lazarus v2.0+, there is not need for a separate download. However, to avoid naming conflicts, a "laz" prefix was added to package, unit and components: 

| Official version | Lazarus version   
---|---|---  
**Package** | virtualtreeview_package.lpk | laz.virtualtreeview_package.lpk   
**Main unit** | virtualtrees.pas | laz.virtualtrees.pas   
**Components** | TVirtualStringTree   
TVirtualDrawTree   
TVTHeaderPopupMenu | TLazVirtualStringTree   
TLazVirtualDrawTree   
TLazVTHeaderPopupMenu   
  
### Repository

You can checkout the source code from [GitHub](<https://github.com/blikblum/VirtualTreeView-Lazarus>)

It's possible to use both Subversion and Git clients 

Subversion: 
    
    
    svn co <https://github.com/blikblum/VirtualTreeView-Lazarus/branches/lazarus-v5>
    

Replace lazarus_v5 by lazarus_v4 or lazarus_master to get a different version. For more info how to use Subversion, see [GitHub help](<https://help.github.com/articles/support-for-subversion-clients/>)

Git: 
    
    
    git clone <https://github.com/blikblum/VirtualTreeView-Lazarus.git>
    

#### Version 8

An experimental lcl port of VirtualTreeView version 8 is also available [here](<https://github.com/salvadorbs/VirtualTreeView-Lazarus>). 

### Bug report / Feature request

[Here](<https://github.com/blikblum/VirtualTreeView-Lazarus/issues>)

For patches / pull requests, [here](<https://github.com/blikblum/VirtualTreeView-Lazarus/pulls>). Newcomers to GitHub may want to read the [help](<https://help.github.com/articles/using-pull-requests/>)

### Change Log

  * 02/06/2016 - [4.8.7 R4 and 5.5.3 R1](<http://forum.lazarus.freepascal.org/index.php?topic=32856.0>) \- First release of 5x branch + moved to GitHub
  * 20/10/2012 - [4.8.7 LCL R2](<http://www.lazarus.freepascal.org/index.php/topic,18640.0.html>) \- Compatibility with Lazarus 1.0 + 64 bit support
  * 18/02/2011 - [4.8.7 LCL R1](<http://www.lazarus.freepascal.org/index.php/topic,12172.msg62067.html>) \- Sync with 4.8 branch + misc fixes
  * 11/02/2010 - [4.8.6](<http://www.lazarus.freepascal.org/index.php/topic,8601.msg41542.html>) \- First stable release of the new port



### Dependencies / System Requirements

Versions 4.x or 5.x 

  * Lazarus 1.6 or newer
  * fpc 2.6.4 or newer
  * LCL Extensions 0.6 or newer



Version 6.x (lazarus_master branch) 

  * Lazarus 1.6 or newer
  * fpc 3.1 (trunk) or newer
  * LCL Extensions 0.6 or newer



### Installation

  * In Lazarus v2.0+ the virtualtreeview package is installed by default. Note, however, that everything has been renamed with a "laz" prefix (see above table) to avoid possible naming conflicts. It is subsequently assumed that you want to install one of the github versions in addition to the built-in version.
  * If new to Lazarus, read [Install Packages](<Install_Packages.md> "Install Packages")
  * In Lazarus v2.0 or older, uninstall the built-in version first: go to "Package" > "Install/uninstall packages", select "virtualtreeview_package 5.5.3.1" in the _left_ list, click "Uninstall selection" and "Save and rebuild IDE". Note: This is not required in Lazarus v2.2 or newer.
  * Download the LCL Extensions package and extract it to a directory (lazarus\components\lclextensions or other of your preference).
  * Download the Virtual Treeview package and extract it to a directory (lazarus\components\virtualtreeview or other of your preference).
  * Open lclextensions_package.lpk in LCL Extensions directory and click "Use / Add to project"
  * Open virtualtreeview_package.lpk in the package editor and click "Use / Install". Rebuild the IDE.



### Help

A comprehensive help file in .chm format(*) can be found within the GIT repository [[1]](<https://github.com/blikblum/VirtualTreeView-Lazarus>) in the subdirectory _Help_.  
A tutorial and numerous code samples can be found in this forum [VirtualTreeview Example for Lazarus](<VirtualTreeview_Example_for_Lazarus.md> "VirtualTreeview Example for Lazarus"), in the _Demos_ Subdirectory of the VirtualTreeView GIT [[2]](<https://github.com/blikblum/VirtualTreeView-Lazarus>), and in the SVN of freepascal.org [[3]](<https://svn.freepascal.org/cgi-bin/viewvc.cgi/trunk/examples/virtualtreeview/?root=lazarus>)  


(*)Beginning with Windows 7, additional security measures were introduced for .chm Files, if you either download a .chm file directly via the browser (without going through a .zip file), or open the file from a network drive. The lock causes the problem that after opening the .chm file you will see only the table of contents, while the actual contents are missing. To manually release the lock use the Windows Explorer (Right Mouse Click - Properties - "Unblock"). This works with Windows 10 (4/2021) as well. 

## See also

  * [TTreeView](<TTreeView.md> "TTreeView")

---

_Source: [https://wiki.freepascal.org/VirtualTreeview](https://web.archive.org/web/20250416082038/https://wiki.freepascal.org/VirtualTreeview)_
