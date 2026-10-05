# DebugServer

## Overview

Official documentation: [doc:fcl/dbugintf](<http://lazarus-ccr.sourceforge.net/docs/fcl/dbugintf> "doc:fcl/dbugintf")

You can send debug output from your program to a debug server application. A debug server application is included with Lazarus; you can compile 
    
    
    $(LazarusDir)/tools/debugserver/debugserver.lpi
    

and set it up as an external tool: 
    
    
    $(LazarusDir)/tools/debugserver/debugserver$(ExeExt)
    

[![DebugServer.png](https://wiki.freepascal.org/images/5/53/DebugServer.png)](</File:DebugServer.png>)

Alternatively, FPC has a text mode debug server which can also be used; see documentation. 

To use it, add `dbugintf` to the uses clause of the units you want to debug, then use [SendDebug](<http://lazarus-ccr.sourceforge.net/docs/fcl/dbugintf/senddebug.html> "doc:fcl/dbugintf/senddebug.html") to send messages to the debug server. 

For more details, please see the documentation. 

**\--[DD](</index.php?title=User:DD&action=edit&redlink=1> "User:DD \(page does not exist\)") 05:38, 3 October 2015 (CEST): Does not seem to be thread safe!**

## See also

  * [TEventLog documentation](<http://lazarus-ccr.sourceforge.net/docs/fcl/eventlog/teventlog.html> "doc:fcl/eventlog/teventlog.html") Built in support for logging in FPC/Lazarus
  * [LazLogger](<LazLogger.md> "LazLogger")
  * [MultiLog](<MultiLog.md> "MultiLog")
  * [log4delphi](<log4delphi.md> "log4delphi")

---

_Source: [https://wiki.freepascal.org/DebugServer](https://web.archive.org/web/20250601000000/https://wiki.freepascal.org/DebugServer)_
