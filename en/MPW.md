# MPW

│ **English (en)** │    
****

[![Logo OSX.png](https://wiki.freepascal.org/images/7/7a/Logo_OSX.png)](</File:Logo_OSX.png>)

This article applies to [Mac OS Classic](</Category:Mac_OS_Classic> "Category:Mac OS Classic") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

MPW stands for Macintosh Programmers Workshop, a unix inspired environment, created by Apple Computer in the early eighties, to facilitate development of classic Macintosh programs. Previously a commerial product, it is today free to download and use, <http://developer.apple.com/tools/mpw-tools/>. MPW or ToolServer is required if one wants to develop fpc programs for [Target MacOS](<Target_MacOS.md> "Target MacOS"). (ToolServer is a stripped down engine for running MPW tools without MPW, and is included within MPW.) 

Native Macintosh files have CR as line ending and also have a file type and creator code, which are not visible to the user. 

MPW can also open files with LF (unix) line ending, and will also preserve them when saving the file. It also opens files without a mac file type and creator. 

Some tools however, specifically PPCAsm, does not accept LF in its input, although it does not require the file to have a mac file type. 

MPW may execute any text file as a script, it doesn't need any extension. However, it requires the file to have a mac file type, and specifically the type 'TEXT'. 

With the following MPW command, file type and creator code can be changed: 
    
    
     SetFile -c 'MPS ' -t 'TEXT' filename
    

...which changes the file type to 'TEXT' and creator to 'MPS ' which is the creator code for MPW. 

Note: Watch out for the blank after MPS in the string 'MPS '. 

_As of 2012, MPW is no longer available from Apple and is rapidly disappearing from mirrors and archive sites. Googling to determine the exact name of the binary, and then Googling for this specific file, might produce results from a cache somewhere._

_Hopefully the download will unpack the Macbinary file discarding the .bin suffix. The resulting .img file will be automatically mounted when double-clicked, but it's probably necessary to copy the MPW.GM folder out of this to ensure that it's not locked._

---

_Source: [https://wiki.freepascal.org/MPW](https://web.archive.org/web/20250315183932/https://wiki.freepascal.org/MPW)_
