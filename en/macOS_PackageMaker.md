# macOS PackageMaker

[![macOSlogo.png](https://wiki.freepascal.org/images/1/15/macOSlogo.png)](</File:macOSlogo.png>)

This article applies to [macOS](</Category:macOS> "Category:macOS") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

│ **English (en)** │    
****

  


  


[![Warning-icon.png](https://wiki.freepascal.org/images/b/b2/Warning-icon.png)](</File:Warning-icon.png>)

**Warning:** PackageMaker was discontinued after Xcode v4.4 - now use pkgbuild and productbuild command line tools or the third party GUI Packages (<http://s.sudre.free.fr/Software/Packages/about.html>) application

If you absolutely require that several files are installed in different locations, then (and only then!) should you create a package file (.pkg extension). This package file can then be moved, downloaded or emailed. Once the end user has it, they can double-click the package and the _Installer.app_ is launched, which will perform the installation. 

The installer .pkg file is actually a directory, as is the resulting application bundle (.app extension) that it copies to the Applications folder. To the user, .pkg and .app files look and act like ordinary files, but they're really directories whose details have been hidden from the user. You can see what's inside a .pkg or .app file by entering normal cd commands in a terminal window (for example, cd progname.app) or by Ctrl-clicking the file and choosing Show Package Contents from the popup menu. 

You create .pkg files using `PackageMaker`, which is installed along with the Xcode tools in /Developer/Applications/Utilities. With `PackageMaker`, you select the folder containing the files you want to package and set other installation options, for example whether a password must be entered to install the program. Note that the folder you select can be an .app file. Choose File | Create Package to create the .pkg file. You can also save your settings for future use in PackageMaker by choosing File | Save to create a .pmsp file that you name (.pmproj with later versions). 

To create a .dmg file, run the macOS Disk Utility, which is installed in /Applications/ Utilities. Select Images > New > Image from Folder and choose the folder containing your .pkg file and any other files you want to include in the disk image. In the Convert Image dialog, enter the name of the .dmg file to create, select where it should be saved, and select "compressed" as the image format. The .dmg file that Disk Utility creates is then ready for distribution.

---

_Source: [https://wiki.freepascal.org/macOS_PackageMaker](https://web.archive.org/web/20250315183201/https://wiki.freepascal.org/macOS_PackageMaker)_
