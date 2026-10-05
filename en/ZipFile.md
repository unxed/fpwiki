# ZipFile

## Contents

  * 1 About
  * 2 Author
  * 3 License
  * 4 Contact
  * 5 Download
  * 6 Change Log
  * 7 Status
  * 8 Roadmap
  * 9 Platforms
  * 10 Dependencies / System Requirements
  * 11 Installation
  * 12 Usage
  * 13 See also



### About

TZipFile is an object that encapsulates a zip file so you can access it as if it's a filesystem. TZipFile is released under the LGPL license. TZipFile comes with a Lazarus package, so it's very easy to install it. The component is tested under Win32 and Linux_x86_32. File compression is currently not implemented although this feature is under development, so check SVN regularly. Currently only uncompressed files can be read and written to and from a zipfile. 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** For people needing compression: TZipper reads/writes zip files and supports various compression algorithms. See [paszlib#TZipper](<paszlib.md> "paszlib")

### Author

Darius Blaszijk 

### License

LGPL 

### Contact

You can contact me directly by email or go to the #lazarus-ide channel on freenode. 

### Download

  * [ZipFile package](<http://sourceforge.net/project/showfiles.php?group_id=92177&package_id=211018>)
  * SVN: <https://modelbuilder.svn.sourceforge.net/svnroot/modelbuilder/src/trunk/mcl/zipfile>



### Change Log

  * Version 0.1 9-Nov-2006: Initial release of the package.



### Status

  * Basic file operations - stable



### Roadmap

  * Stress testing
  * Testing on more platforms (please add your platform on this page if it's not listed)
  * Adding more tests
  * Implementing deflate and inflate algorithm



### Platforms

  * Windows XP - i386
  * SuSe Linux 10.0 - i386



### Dependencies / System Requirements

  * Lazarus 0.9.20+ and FPC 2.0.4+ (most probably older versions will work too)
  * Status: Beta
  * Issues: None known.



### Installation

  * Download the package from Sourceforge and unzip it anywhere you want.
  * Open Lazarus
  * Open the package ZipFilePkg.lpk with Component/Open package file (.lpk)
  * (Click on Compile only if you don't want to install the component into the IDE)
  * Click on Install and answer 'Yes' when you are asked about Lazarus rebuilding. A new tab named 'MB' will be created in the components palette.



### Usage

Drop TZipFile on a form. Set the filename property and set active to True. That's all. See the provided example for more advanced usage. 

### See also

The [paszlib](<paszlib.md> "paszlib") unit provided with FPC has support for many zip compression and decompression formats.

---

_Source: [https://wiki.freepascal.org/ZipFile](https://web.archive.org/web/20240910055915/https://wiki.freepascal.org/ZipFile)_
