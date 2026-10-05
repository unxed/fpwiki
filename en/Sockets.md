# Sockets

│ **English (en)** │

## Contents

  * 1 About
  * 2 Author
  * 3 License
  * 4 Download
  * 5 Change Log
  * 6 Roadmap
  * 7 Dependencies / System Requirements
  * 8 Installation
  * 9 Usage



### About

[![Warning-icon.png](https://wiki.freepascal.org/images/b/b2/Warning-icon.png)](</File:Warning-icon.png>)

**Warning:** **THIS IS DEPRECATED AND DEPENDS ON VERY OLD Lazarus 0.9.6 (FPC 1.9.8) VERSIONS.  
** DOCUMENTATION PURPOSE ONLY.  
  
****

Sockets is a modified-LGPL package for Lazarus. It defines the TSocketClient component for connecting applications to generic server sockets over TCP/IP networks. 

  
The download contains the component Pascal files, the Lazarus package file and resource files and the modified-LGPL license text files. 

This component was designed for applications running on Linux and Win32 platforms. 

### Author

[Antonio d'Avino](</index.php?title=User:Blacktony&action=edit&redlink=1> "User:Blacktony \(page does not exist\)")

### License

Modified LGPL (read COPYING.modifiedLGPL and COPYING.LGPL included in package). 

### Download

(As noted on 26-June-2012, the author's website does not work nor there is a "Sockets" package on SourceForge's download page) 

The latest stable release can be found on [Lazarus CCR Files page](<http://sourceforge.net/project/showfiles.php?group_id=92177>) or on author's web pages <http://infoconsult.homelinux.net>. 

  * As of September 2013 the download location is [Here.](<http://sourceforge.net/projects/lazarus-ccr/files/Code%20and%20Component%20Packages/Sockets%200.2.0/>)



### Change Log

  * Version 0.2.0 2005/06/01



### Roadmap

  * Implementation of asynchronous mode in write operations.
  * Adding of TSocketServer class to package



### Dependencies / System Requirements

  * Lazarus 0.9.6 (FPC 1.9.8)



Status: Beta 

Issues: Tested on Windows (Win2K) and Linux (Mdk 10.1). 

### Installation

  * In the lazarus/components directory, untar (unzip) the files from sockets-laz-package-<version>.tar.gz file. The sockets folder will be created.
  * Open lazarus
  * Open the package sockets_laz.lpk with Component/Open package file (.lpk)
  * (Click on Compile only if you don't want to install the component into the IDE)
  * Click on Install and answer 'Yes' when you are asked about Lazarus rebuilding. A new tab named 'Sockets' will be created in the components palette.



Note for Win32 users: In case you experience some difficults in compiling the Pascal files, you may need to add the following path: <some-drive>:<some-path>\lazarus\lcl\units\i386\win32 (ie: c:\programs\lazarus\lcl\units\i386\win32) to the 'Other unit files ...' in 'Paths' tab of 'Compiler Options' of the lazarus package manager window. 

### Usage

Drop a TSocketClient component on a form, for any different client socket connections your application needs. Mandatory property of this component you have to set are: 

  * Host : the TCP/IP address or the URL of the PC where the server sockets you want to connect to is running;
  * Port : the TCP/IP port on wich the server process is listening;



Optional properties you may set are: 

  * DosLf : If set to 'True', a CR (0x0D) is prepend to LF (0x0A) when a writeLineLf procedure is executed (see forward).
  * ReadCharsThereshold : the number of characters read from channel before a OnReadChars event is triggered (see forward). 0 is equivalent to 1.



Now you can set to 'True' the property Active for opening the channel. An exception is raised if connection fails (host unreachable, no server process listening on port, network errors). The connection is closed setting 'False' the active property. Also, you can test the Active property to verify if connection status is ON (the closing of the connection by the server side set false this property). 

**Public methods of the TSocketClient class.**

  * function bufferChars : Integer. It returns the number of chars received from the server process through the channel and available for reading.
  * function getBuffer (var buffer: String) : Integer. This function copy the chars from internal buffer of component into the parameter string 'buffer'. The number of characters copied is returned and the internal buffer is empty.
  * function readLine (var buffer: String) : Boolean. The characters in internal buffer are copied into the parameter string 'buffer', upto, but not including, the first LF (0x0A9) character occurrence. Any CR characters (0x0D) are discarded.



If a complete line can be copied, the function returns true. If not any LF char is encountered, the whole internal buffer is copied into the 'buffer' parameter and the function returns false. 

Note: the pending content in internal buffer is available also when the connection is closed. It will be lost only when the connection is reopen. 

  * function writeChar (buffer : Char ) : ShortInt. This function write a single char on channel, returning 0 in case of success or a value less than 0 if it fails.
  * function writeLine (buffer : String) : ShortInt. The content of parameter buffer is sent to server. The function returns 0 in case of success or a value less than 0 if it fails.
  * function writeLineLf (buffer : String) : ShortInt. Same of writeLine function, but a LF char (0x0A) is appended (a couple of CR/LF, if property DOSLf is true). The function returns 0 in case of success or a value less than 0 if it fails.



Note: at this moment, the write methods of the class are not asynchronous, so they may be blocking for your application in case of failing. This will be fixed in next release. 

**Events of TSocketClient class.**

  * OnBufferFull : this event is triggered when the internal buffer is full. No others characters are received from channel until characters are read from internal buffer.
  * OnEndOfLineReceive : this event occurs where a LF character (0x0A) is received from server.
  * OnReadChars : this event is triggered when a number of at least ReadCharsThereshold is pending in internal buffer.
  * OnStateChange : this event occurs any time the Active property change its value.

---

_Source: [https://wiki.freepascal.org/Sockets](https://web.archive.org/web/20250506082154/https://wiki.freepascal.org/Sockets)_
