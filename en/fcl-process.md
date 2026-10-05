# fcl-process

## Overview

**fcl-process** is a package that contains multiple units for executing and interfacing (by pipes) with other programs. The most prominent member is the **Process.TProcess** class that is used by Lazarus and described and documented elsewhere, like [TProcess](<Executing_External_Programs.md> "Executing External Programs") and has its own documentation in the FPC Free Component Library (FCL) manual. 

The **pipes** unit implements basic pipe stream classes for use by the other units. 

The dbug* units implement a simple remote logging/inspection interface. See also [DebugServer](<DebugServer.md> "DebugServer"). 

## Units

Unit | - | Comment   
---|---|---  
[process](</index.php?title=process&action=edit&redlink=1> "process \(page does not exist\)") | - | Contains TProcess class for executing external programs (and some related utility functions)   
[pipes](</index.php?title=pipes&action=edit&redlink=1> "pipes \(page does not exist\)") | - | Input and output pipe stream class used by process.   
[simpleipc](</index.php?title=simpleipc&action=edit&redlink=1> "simpleipc \(page does not exist\)") | - | Unit implementing one-way IPC between 2 processes. On Windows it apparently uses Windows messaging; see forum post [[1]](<http://forum.lazarus.freepascal.org/index.php/topic,15974.msg104374.html#msg104374>) that includes a C# example that connects with a FreePascal SimpleIPC server   
[dbugintf](</index.php?title=dbugintf&action=edit&redlink=1> "dbugintf \(page does not exist\)") | - | Debugserver client interface, based on SimpleIPC   
[dbugmsg](</index.php?title=dbugmsg&action=edit&redlink=1> "dbugmsg \(page does not exist\)") | - | Debugserver Client/Server common code.   
  
## Notes

  * To fool programs that check for a pty on Unix/Linux, forkpty might have to be used instead of fork. See [[2]](<http://forum.lazarus.freepascal.org/index.php?topic=22912.0;topicseen>). Openpty and forkpty (which uses openpty) are not POSIX calls though. On FreeBSD, these functions are not in libc but in libutil, and seem to be a shell over POSIX openpt and ptsname functions with some IOCTLs thrown in.

---

_Source: [https://wiki.freepascal.org/fcl-process](https://web.archive.org/web/20230604063652/https://wiki.freepascal.org/fcl-process)_
