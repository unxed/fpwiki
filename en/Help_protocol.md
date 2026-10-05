# Help protocol

## Contents

  * 1 Overview
  * 2 Required switches
  * 3 IPC protocol
  * 4 Current protocol
  * 5 See also



## Overview

This page describes the protocol used to communicate between the Lazarus IDE (as well as Lazarus applications that include chm help) and the help viewer. This protocol is used to provide context-sensitive help. Currently, the lhelp chm viewer supports this protocol. 

This page can be helpful in adapting alternative viewers (e.g. the one delivered with [fpGUI](<fpGUI.md> "fpGUI")) to be used more easily with Lazarus. 

## Required switches

The viewer program needs to support these command-line switches as the IDE will start the viewer with them: 

  * `--ipcname <name>`: name is the name of the IPC server started by the IDE



## IPC protocol

  * The IDE sets up a [SimpleIPC](<SimpleIPC.md> "SimpleIPC") connection to the viewer and may send these commands (constant names): 
    * `rtFile`: asks the viewer to open a help file (which is specified in a `TFileRequest`)
    * `rtURL`: asks the viewer to open an URL (specified in a `TURLRequest`)
    * `rtContext`: asks the viewer to open up a context/specific help item name? (specified in a `TContextRequest`)
  * There is an additional IPC channel between viewer and IDE that carries data from viewer to IDE.
  * The viewer responds success or failure (codes: see below) after receiving and processing a command from the IDE.



As per July 2013, the viewer part is implemented in e.g. ($lazarusdir)\components\chmhelp\lhelp\lhelpcore.pas; the request constants are defined in ($lazarusdir)\components\chmhelp\packages\help\lhelpcontrol.pas 

## Current protocol

Current protocol version is 2.1 (implemented in Lazarus trunk revision, 16 October 2014) 

  * The IDE passes an `--ipcname` starting with a server dependent string constant [1] then string representation of the last 5 digits of the application processID/PID padded with 00000 at the right [2] so having multiple IDE/lhelp sets does not interfere with communication.
  * The viewer must support a `--hide` command-line option: it should not show the contents of loading/loaded help files in the GUI but allow e.g. loading of help files and receiving further instructions from the IDE.
  * After sending a command, the IDE must not send another command until: 
    * it receives a response from the client (see above) or
    * a timeout has been reached.
  * Additional commands: 
    * `version` (IDE=>viewer): passes help protocol version (e.g. 1). If the client does not support it, it should return `TLHelpResponse.srError`, else `TLHelpResponse.srSuccess`.
    * `close` (IDE=>viewer): client may respond `TLHelpResponse.srSuccess` or not send a response, and then close itself. Used when the IDE is closed.
    * `TLHelpResponse.srSuccess` (viewer=>IDE): viewer has succesfully completed the last command issued by the IDE.
    * General error code `TLHelpResponse.srError`, or the more specific `TLHelpResponse.srNoAnswer`, `TLHelpResponse.srInvalidFile`, `TLHelpResponse.srInvalidURL`, `TLHelpResponse.srInvalidContext` (viewer=>IDE): viewer could not successfully complete the command.



[1] The string constant may contain only ['a'..'z', 'A'..'Z', '_'] characters. In the Lazarus implementation, this is ~~the value specified in tools/options/help options/chm help viewer/HelpLabel, default~~ lazhelp 

[2] code would be something like this: 
    
    
    copy(inttostr(GetProcessID)+'00000',1,5)
    

## See also

  * [Creating IDE Help](<Creating_IDE_Help.md> "Creating IDE Help")
  * [chmhelp](<chmhelp.md> "chmhelp")
  * [SimpleIPC documentation](<http://lazarus-ccr.sourceforge.net/docs/fcl/simpleipc/index.html>)

---

_Source: [https://wiki.freepascal.org/Help_protocol](https://web.archive.org/web/20220626234210/https://wiki.freepascal.org/Help_protocol)_
