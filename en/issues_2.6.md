# Issues 2.6.2

## Contents

  * 1 Issues with 2.6.2 release candidates
    * 1.1 Issues with 2.6.2-rc1
    * 1.2 Merge requests
    * 1.3 Post rc1 fixes



# Issues with 2.6.2 release candidates

This page is supposed to contain all issues discovered while testing release candidates and their status. Make sure to include information about the platform where the problem appears (if applicable). Once the issue gets resolved, you should strike it over and add number of SVN revision fixing it. If the revision number refers to a SVN repository different from "fpc" (i.e. "fpcbuild" or "fpcdocs"), include information about the SVN repository too. 

Example: 

  1. ~~cpu-os: IE209101221 while compiling XYZ.pas~~ \- r123456
  2. docs: missing description of XYZ
  3. ~~readme.txt: wrong version number~~ \- r654321 (fpcbuild)



## Issues with 2.6.2-rc1

  1. ufclndos.zip is created and included for go32v2 target although it has no real content (there are no units supported for go32v2 in this package) and contains garbage polluting the installation directory instead (problem existed already with 2.6.0 at least)
  2. Full release packages for GO32v2 and OS/2 miss sources for our installer
  3. Installer (fpc-2.6.2rc1.i386-win32.exe) wil not run on Win9x platform (downgrade Inno like we did vor Lazarus 1.0??) 
     1. Win9x was never supported for 2.6.x. The support in 2.6.0 was third party. ([Marcov](</User:Marcov> "User:Marcov"))
     2. The installer from the official download for 2.6.0 from both <http://freepascal.org/down/i386/win32-netherlands.var> and <http://sourceforge.net/projects/freepascal/files/Win32/2.6.0/fpc-2.6.0.i386-win32.exe/download> run just fine on WinMe. They were created with Inno 5.4.2 (a). The current installer is created with Inno 5.5.2 (a), suggesting this is still a non-unicode installer, but it will not load on WinMe. ([Bart](</User:Bart> "User:Bart")) 
        1. As mentioned repeatedly it is no surprise 5.5.2 won't work on 9x: "The latest version that can create installers for Windows versions before Windows 2000 is version 5.4." <http://wiki.lazarus.freepascal.org/Inno_Setup> (includes link to Inno changelog/docs) --[BigChimp](</User:BigChimp> "User:BigChimp") 07:14, 13 December 2012 (UTC)
  4. On Windows the readme.txt is almost unreadable in Notepad (in which it is opened by default).
  5. The Linux/i386 distro-independent file (fpc-2.6.2rc1.i386-linux.tar.gz) is a simple archive containing all files, instead of a tar file containing an install script (that, a.o., creates a configuration file) and all individual files packed in separate .tgz files.



## Merge requests

  * maybe <http://bugs.freepascal.org/view.php?id=22130> r23336 resolver bug? Only seems to fix a case where the filename was not set, and the fallback default wasn't used for the fileage check.



## Post rc1 fixes

  * r23106 (some methods made virtual in fcl-db.
  * r23216 (removal of a debug writeln)
  * sockets and synchronization fixes for OS/2 by Tomas. (trunk revs r22983, r22984,r23251)
  * **r23218** [bug 23476](<http://bugs.freepascal.org/view.php?id=23476>)
  * (better 4GB+ zip support) [bug 23482](<http://bugs.freepascal.org/view.php?id=23482>) Mostly changing longints to longwords, and a few (local) variables to int64 to force 64-bit seek.
  * svn revs 23202,23219,23205 hkey_reg constants need to be sign extended and dedupe registry constants on windows platforms (win64 fix)

---

_Source: [https://wiki.freepascal.org/issues_2.6.2](https://web.archive.org/web/20201001234935/https://wiki.freepascal.org/issues_2.6.2)_
