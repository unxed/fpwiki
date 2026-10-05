# SvnClasses

## Contents

  * 1 About
  * 2 Author
  * 3 License
  * 4 SVN
  * 5 Download
  * 6 Change Log
  * 7 Dependencies / System Requirements
  * 8 Status
  * 9 Issues
  * 10 Installation
  * 11 Examples
  * 12 See also



### About

_SvnClasses_ is a Lazarus package to help writing applications that interact with a Subversion (svn) repository. It requires an installed command line svn executable for accessing a svn repository and/or a working directory. I used these classes for writing a tool to mirror the Lazarus svn repository to Sourceforge. 

### Author

[Vincent Snijders](</User:Vincent> "User:Vincent")

### License

[LGPL](<http://www.opensource.org/licenses/lgpl-license.php>) with the linking exception, same as the LCL. 

### SVN

The package is available in Lazarus CCR SVN repository at <https://lazarus-ccr.svn.sourceforge.net/svnroot/lazarus-ccr/components/svn>

### Download

The latest stable release can be found on _link to the lazarus-ccr sf download location_. 

### Change Log

  * Version 0.1 2007-03-07



### Dependencies / System Requirements

  * Requires fpc 2.1.1 or later, because of the use of TProcess enhancements.
  * Requires an svn executable on the path or in default location in windows.
  * Although this package can be used for command line applications, it depends on the LCL for some functions from the FileUtils class



### Status

Status: Alpha 

### Issues

  * The ExecuteSvnCommand procedure doesn't capture the output on StdErr
  * The ExecuteSvnCommand procedure hasn't configurable verbosity



### Installation

  * extract the zip file
  * open the svnpkg.pkg and compile the package.



### Examples

For use see included unit tests. 

  * open the test\fpcunitsvnpkg.lpi project and run it.



I have written these classes to support [fpsvnsync](<fpsvnsync.md> "fpsvnsync"), a tool similar to [svnsync](<http://svn.collab.net/repos/svn/trunk/notes/svnsync.txt>) to mirror the lazarus svn repository to [SourceForge](<http://lazarus.svn.sourceforge.net/viewvc/lazarus/>). The svnsync tool doesn't work, because Sourceforge doesn't allow revision property changes. 

### See also

  * [fpcup](<fpcup.md> "fpcup") contains unified classes for subversion, git, and mercurial which may be an alternative. They are currently focussed on checkout/update operations but basic support has been added for committing.

---

_Source: [https://wiki.freepascal.org/SvnClasses](https://web.archive.org/web/20250418110902/https://wiki.freepascal.org/SvnClasses)_
