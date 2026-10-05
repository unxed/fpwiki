# SimpleIPC

**Simple IPC** is a Free Pascal unit (and Lazarus components) that allows interprocess communication (IPC) between Free Pascal programs. 

It allows two executables to communicate. 

## Contents

  * 1 Advantages of SimpleIPC
    * 1.1 Hint : Multiuser Systems
  * 2 Use Cases
  * 3 Send Messages to Other Programming Languages
  * 4 See also



## Advantages of SimpleIPC

You can create communication systems without requiring low level sockets programming of your own. SimpleIPC does the grunt work of IPC for you and makes a nice high level wrapper around lower level IPC mechanisms, so you don't have to program them yourself. 

SimpleIPC can be used to communicate between programs to make simple communication systems, plugin systems and much more, 

### Hint : Multiuser Systems

On multiuser systems, eg, Unix systems, in Global Mode, note that the pipe that IPC creates must have a distinctive name between individual users. Its usual to set the ServerID to the application name so, add the user name to that - 
    
    
    CommsServer  := TSimpleIPCServer.Create(Nil);
    CommsServer.ServerID:='MyAppName' {$ifdef UNIX} + '-' + GetEnvironmentVariable('USER'){$endif}; 
    CommsServer.OnMessageQueued:=@CommMessageReceived;
    CommsServer.Global:=True;                  
    CommsServer.StartServer({$ifdef WINDOWS}False{$else}True{$endif});  // start listening, threaded
    

Without the user name being added, you risk all instances of the App trying to use the same named pipe and, obviously permissions do not allow that. 

## Use Cases

The [Lazarus help system](<Help_protocol.md> "Help protocol") itself, uses SimpleIPC. 

Some [fpGUI](<fpGUI.md> "fpGUI") tools and demos use IPC, for example, to detect if the program is already running (single instance) 

## Send Messages to Other Programming Languages

For IPC between Free Pascal programs and other programs written in any language (C++, Delphi, GoLang, etc.) see [SimpleIPC Library](<SimpleIPC_Library.md> "SimpleIPC Library") which allows you to use SimpleIPC not just in FPC and Lazarus, but in multiple programming languages. 

## See also

  * [Official FCL documentation for SimpleIPC unit](<https://www.freepascal.org/docs-html/current/fcl/simpleipc/index.html>)

---

_Source: [https://wiki.freepascal.org/SimpleIPC](https://web.archive.org/web/20230924164820/https://wiki.freepascal.org/SimpleIPC)_
