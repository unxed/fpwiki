# helper tools

Currently FPC uses various sets of tools from the current packages 

\- GNU Binutils. \- GNU Make \- GNU Fileutils \- GNU Diffutils \- GNU Shellutils 

\- Fileutils, Shellutils, and Textutils are combined in [coreutils](<http://www.gnu.org/software/coreutils/>) since 2005 

This page is meant to track the versioning of these utils. Please, if you update external version, consider only formal releases of mingw or djgpp. If there are notable other builds, note them in a bullet list UNDER the table. This allows some overview. 

## Contents

  * 1 Go32v2
  * 2 Win32
  * 3 OS/2
  * 4 Unix



## Go32v2

**file** | **package** | **our version** | **external version** | **remarks**  
---|---|---|---|---  
ar| binutils| 2.17| 2.25r2  
as| binutils| 2.17| 2.25r2|  (ours packed w/ buggy UPX 2.93)  
ld| binutils| 2.17| 2.25r2|  (ours packed w/ buggy UPX 2.93)  
objdump| binutils| 2.17| 2.25r2  
strip|  binutils | 2.17| 2.25r2  
gdb|  \- | 6.1.1| 7.10  
gdb (in ide)|  \- | 6.1.1  
make|  \- | 3.79.1| 4.1r2  
cmp|  diffutils | 2.8.7| 3.3r3  
diff|  diffutils | 2.8.7| 3.3r3  
cp|  fileutils | 4.1| 4.1r2  
ginstall|  fileutils | 4.1| 4.1r2  
gmkdir|  fileutils | 4.1| 4.1r2  
mv|  fileutils | 4.1| 4.1r2  
rm|  fileutils | 4.1| 4.1r2  
gdate|  shutils | 2.0.11| 2.0.11r2  
gecho|  shutils | 2.0.11| 2.0.11r2  
pwd|  shutils |  2.0.11 | 2.0.11r2  
grep|  |  2.5.3 | 2.22  
patch|  |  2.5.9 | 2.7.1r2  
zip|  |  2.32 | 2.32| 3.0 builds from sources  
cwsdpmi| djgpp| r7 2010| r7 2010  
wmemu387| djgpp| beta 1.11| beta 1.11|  (aka, /current/v2misc/wmemu21[bs].zip)   
  
## Win32

**file** | **package** | **our version** | **external version** | **remarks**  
---|---|---|---|---  
ar| binutils| 2.22  
as| binutils| 2.22  
ld| binutils| 2.22  
dlltool| binutils| 2.21  
objdump| binutils| 2.22  
strip|  binutils | 2.22  
windres| binutils| 2.21.1 20050827  
gdb|  \- | 7.2  
make|  \- | 3.82  
cmp|  diffutils | 2.7  
diff|  diffutils | 2.7  
cp|  fileutils | 4.0a  
ginstall|  fileutils | 3.16| | Needs manifest on Vista, added in 2.4.0   
gmkdir|  fileutils | 3.16  
mv|  fileutils | 3.16  
rm|  fileutils | 3.16  
gcc|  gcc | 2.95  
cpp|  gcc | 2.95 (19990728)  
gdate|  shutils | 1.13  
gecho|  shutils | 1.9.4  
pwd|  shutils |  \- | |  doesn't understand --version, no version info with strings  
gorc|  |  0.90.3c   
grep|  |  2.5.1   
patch|  |  2.5 | | Needs manifest on Vista.   
unzip|  |  5.42 (jan 2001)   
zip|  |  2.3 (november 1999)   
  
## OS/2

file | package | our version | external version | remarks  
---|---|---|---|---  
ar| binutils| 2.9.1  
as| binutils| 2.14| | A newer AS version is necessary for support of newer CPU instructions (e.g. SSE3), but the OS/2 port has a wider set of dependencies (e.g. a different libc) and thus it is not included right now. Port available on <http://os2ports.smedley.info> (version 2.25) has been tested successfully.   
ld| binutils| 2.6| | EMX  
objdump| binutils| 2.9.1  
strip| binutils| 2.9.1  
gdb|  \- | 4.16  
make|  \- | 3.76.1| | Port of version 3.81 built by kmk with Innotek port of GCC and klibc 0.63 has been tested successfully, but it is not included due to different libc not included with FPC.   
diff| diffutils| 2.8  
cp| fileutils| 3.13  
ginstall| fileutils| 3.13  
mkdir| fileutils| 3.13  
mv| fileutils| 3.13  
rm| fileutils| 3.13  
gdate| shutils| 1.12  
pwd| shutils| 1.12  
      
    
         LD for EMX
         EMXBIND 0.9d
         NM 2.9.1 for EMX
         GASP 1.2 for EMX
         ObjCopy 2.9.1 for EMX
         ObjDump 2.9.1 for EMX
         Strip 2.9.1 for EMX
         RANLIB 2.9.1 for EMX
         Make 3.76.1
         RM 3.13
         CP 3.13
         MV 3.13
         ChMod 3.13
         PWD 1.12
         Install 3.13
         GDate 1.12
         Echo 1.12
         Diff 2.8
     gdbemx.zip      contains the GNU Debugger 4.16 for EMX,
                     PMGDB (Presentation Manager add-on for GDB) and GPROF 2.9.1
    

## Unix

Unices usually use the tools as packaged by their distribution (or external GNU distribution, if a commercial Unix). On GNU tools are sometimes prefixed with a "g" to distinguish them from the standardly installed tools. (gmake,gld,gas etc)

---

_Source: [https://wiki.freepascal.org/helper_tools](https://web.archive.org/web/20211203161354/https://wiki.freepascal.org/helper_tools)_
