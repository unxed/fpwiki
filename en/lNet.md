# lNet

│ **English (en)** │  **[русский (ru)](<../ru/lNet.md>)** │

## Contents

  * 1 About
  * 2 Download
  * 3 Usage
    * 3.1 Events of TLNetComponent
  * 4 Documentation
  * 5 Installation
  * 6 Package contents
  * 7 Testing
  * 8 History
  * 9 Stability
  * 10 Roadmap
  * 11 Requirements
    * 11.1 Ubuntu special notice
  * 12 API changes
  * 13 lNet for FreeBSD, Linux, macOS and Windows
  * 14 See also



## About

**lNet** or **Lightweight Networking Library** is a collection of classes and components to enable event-driven TCP or UDP networking. 

Authors: 

  * Ales Katona ([User: Almindor](</User:Almindor> "User:Almindor"))
  * Micha Nelissen



License: modified LGPL license (permits static linking). 

## Download

There are two variants of the library, which are no longer completely synchronised: 

  * [lNet package](<http://sourceforge.net/projects/lazarus-ccr/files/lNet/>). Packaged as utilities for the use of the FPC project itself. They are not recommended for general use.
  * At <http://lnet.wordpress.com>, however this advises that the project has been moved to GitHub <https://github.com/almindor/lnet>.



You can contact the author directly by email or go to #lnet channel on freenode. 

## Usage

Drop a TLTCPComponent or TLUDPComponent on a form. Assign OnAccept, OnError, OnReceive, OnConnect and OnDisconnect event handlers. Note that this is optional but in most cases required for functionality. 

Client connection is initialized with the Connect call. Arguments are IP address/hostname and port respectivly. Non-blocking connect is used, this means that you **DON'T** know that you're connected until OnConnect event is fired. 

Server connection is initialized with the Listen* call. Argument is port. 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** address is a string representation of IP eg: '127.0.0.1' or a hostname like 'localhost'.

Sending data to the other side is accomplished with the Send/SendMessage method. 

### Events of TLNetComponent

  * OnError - this event is fired whenever a connection error occurs. If no handler is assigned an excepion with "msg" is raised. Arguments are: msg - contains string and numerical representation of error message formatted in native encoding or UTF8 (depending on ForceUTF8 property). aSocket is the socket at which the error occured, nil means that the error is not a socket specific one.


  * OnConnect - This event is fired when the client succesfuly connects. aSocket is the socket which connected. It should serve as the "start" point for clients.


  * OnAccept - this event is fired whenever a connection is accepted on a server. If no handler is assigned nothing is done. aSocket contains the socket which accepted the connection.


  * OnDisconnect - this event is fired whenever a connection is lost. If no handler is assigned nothing is done. aSocket contains the socket which got disconnected.


  * OnReceive - This event is fired whenever new data is **ready to be received**. If no handler is assigned nothing is done, but a system buffer fill may happen which will in turn fire an OnError event. aSocket is the socket at which the data got received. Data is read by Get/GetMessage method.


  * OnCanSend - This event is fired whenever new data can be sent again on a socket, after a call to send function failed, returning 0. It can be used to automate sending of big chunks. aSocket is socket on which send can be called.



## Documentation

Documentation of lNet is partially done. You can also use Jesus' diagrams of classes and units as basic reference. 

Documentation of eventer and TCP/UDP protocols: <http://members.chello.sk/ales/docs> ([Archived version at archive.org](<https://web.archive.org/web/20230000000000*/http://members.chello.sk/ales/docs>)}  


Overview diagrams can be found here: <http://members.chello.sk/ales/docs/diagrams/overview.html>  
Pictures:   
[New class diagram](<http://members.chello.sk/ales/docs/diagrams/inheritance_composition.png>)  
[All classes](<http://members.chello.sk/ales/docs/diagrams/extra/allclasses.png>)  
[lNet units](<http://members.chello.sk/ales/docs/diagrams/extra/lnetunits.png>)  
[lNet units 2](<http://members.chello.sk/ales/docs/diagrams/extra/lnetunits2.png>)  


Docs Mirror and Ongoing Notes: <http://z505.com/cgi-bin/powtils/docs/1.6/idx.cgi?file=lnetlufdoc>

## Installation

LNet 0.5+: 

  * In the lazarus/components directory, unzip the files from lnet-<version>.zip file. The lnet-<version> folder will be created.
  * Open Lazarus
  * Open the package lnet-<version>/lazaruspackage/lnetvisual.lpk with Package/Open package file (.lpk)
  * Click on Use/Install and answer 'Yes' when you are asked about Lazarus rebuilding. A new tab named 'lNet' will be created in the components palette.



![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** users who don't want to depend on LCL but use LNet in Lazarus (non-visually), can just compile lnetbase.lpk, which adds all the base units to path.

LNet 0.4: 

  * In the lazarus/components directory, unzip the files from lnet-<version>.zip file. The lnet-<version> folder will be created.
  * Open Lazarus
  * Open the package lnet-<version>/lazaruspackage/lnetpackage.lpk with Package/Open package file (.lpk)
  * Click on Compile
  * Open the package lnet-<version>/lazaruspackage/ide/lnetidepackage.lpk with Package/Open package file (.lpk)
  * Click on Use/Install and answer 'Yes' when you are asked about Lazarus rebuilding. A new tab named 'lNet' will be created in the components palette.



![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** older lNet is extraced to "lnetpackage" dir. Since 0.4.0 the "visual" and "non visual" packages are in one distribution file.

## Package contents

The package consists of base lNet units library, lTelnet for telnet protocol, lFTP for ftp protocol and lNetComponents libraries for providing visual and non-visual components for networking. As of 0.4.0 lHTTP and lSMTP components have been added. Since 0.5.1 there's a MIME support as part of the library mainly for creation of multipart messages for SMTP. Since 0.6.0 lNet supports SSL in a modular way, and SMTP protocol has been extended to full support of SSL/TLS (including runtime STARTTLS negotiation). HTTPS client side is supported too. 

## Testing

The non-visual console components were tested on Win32, Win64, Linux_x86_32, Linux_x86_64, Linux_PPC, Linux_PPC_64, Linux_ARM* (2.1.4+) and FreeBSD_x86_32. The visual (Lazarus packages) components were tested in Win32, Win64, ARM/WinCE, Linux_x86_32, Linux_x86_64 and FreeBSD_x86_32. (gtk1 and gtk2 on unix platforms) 

## History

  * March 2011: A new version of lNet was just released. 0.6.5 completes SSL and IPv6 support including server-side and per-socket (for SSL) setting. SSL and IPv6 bugs were fixed as well as some remaining disconnect problems related to server bind errors and lingering sockets. DecodeURL bug introduced in 0.6.4 was also fixed.


  * May 2010: A new version of lNet was just released. 0.6.4 adds Qt4 support inside Lazarus (lazarus latest trunk needed, 0.9.30 should be 1st stable with it). There were also numerous fixes (http URL parsing, FTP problems) and one major bug fixed (WinCE compilation with FPC 2.4.0+). Grab it at SVN, packages will be uploaded ASAP.


  * February 2010: Well there’s a skip in dates. Anyways version 0.6.3 of lNet has been released, you can find it in the downloads section. There’s been various bugfixes and file renames to avoid FPC conflicts. I didn’t find the time to do the QT4 port as I wanted but it’s in the works now. Some SSL fixes were made as well.


  * May 2008: Where to begin? This is a HUGE critical bugfix release. There was about 5 big bugs fixed in SSL section, one logical oversight in Session support. OpenSSL unit was added so WinCE compiles again and SMTP and FTP got bugs fixed in regards to possible desynchronization. I'd urge everyone who uses 0.6.x to update ASAP, but some bugs are also from 0.5.x (specifically the SMTP and FTP event deadlock problem).


  * May 2008: It seems that due to an oversight in what's packaged with FPC and for which platform, lNet 0.6.x doesn't currently compile on WinCE due to missing OpenSSL unit. The unit is in fpc 2.2.0, but is not compiled for that platform yet (it is in 2.2.1+). I will add the unit to lNet and release 0.6.2 shortly, but until then, WinCE users can work around this by copying [this file](<http://svn.freepascal.org/svn/fpc/trunk/packages/openssl/src/openssl.pas>) into their lnetdir/lib directory. (same will be done in 0.6.2 so don't worry)


  * April 2008: 0.6.1 has been released today. This release fixes an ugly SSL logical bug in regards to the mandatory requirements of Certificate Authority List file and Keyfile proprties of SSLSession. They are no longer needed for normal SSL to work. A minor change in HTTP and handling of SSLSession are also present.



## Stability

  * Base lNet TCP/UDP - stable
  * Telnet Client - stable*
  * FTP Client - stable*
  * SMTP Client - stable
  * HTTP Client - stable
  * HTTP Server - experimental
  * MIME streams - stable
  * SSL support (client-side) - stable
  * SSL support (server-side) - stable (0.6.5+)
  * IPv6 support - stable (0.6.5+)



* FTP and Telnet still need some additional features like authentication methods. 

On Linux, since you start using LNet, GDB can crash randomly (upon compilations) with diverse kind of reports. Setting debugger to Automatic in both lnetbase and lnetvisual package's options fixes the issue. 

## Roadmap

Items that are ~~struck through~~ have been completed in the indicated version: 

  * Stress testing [always]
  * ~~Addition of Telnet and FTP clients (experimental level) [0.3]~~ (done)
  * ~~Cementing of TCP/UDP API [0.3]~~ (done)


  * Addition of ICMP Ping/Traceroute protocol [na] (cancelled due to various reasons*)
  * ~~Addition of SMTP, HTTP components [0.4]~~ (done)
  * ~~Stabilization of Telnet and FTP clients [0.5]~~ (done)
  * ~~Addition of MIME streams [0.5.1]~~ (done)
  * Addition of Telnet and FTP servers (experimental level) [???]
  * ~~Addition of SSL [0.6+]~~ (done)
  * Addition of Carbon and/or Cocoa widget set support [0.7+]
  * ~~Addition of Qt4 widget set support [0.6.4+]~~ (done)
  * Cleanup of SSL/HTTP (so pure HTTP doesn't depend on TLSSLSocket) [0.6.x?]



* PING requires raw sockets which are only accessible to root on unix/linux platforms. It was decided that lNet won't support this, users who need PING should use the ping and ping6 programs via TProcess. 

## Requirements

LNet 0.6+: 

  * Lazarus 0.9.24+ and FPC 2.2.0+
  * Status: Beta
  * Issues: Completely tested on Windows (WinXP), Linux (Ubuntu 7.10). FastCGI considered broken for now.



LNet 0.5: 

  * Lazarus 0.9.22+ and FPC 2.0.4+
  * Status: Beta
  * Issues: Completely tested on Windows (WinXP), Linux (Ubuntu 6.10) and FreeBSD 5.5. FastCGI considered broken for now. **WinCE requires Lazarus 0.9.23+ to compile!**



LNet 0.4: 

  * Lazarus 0.9.18+ and FPC 2.0.4+
  * Status: Beta
  * Issues: Completely tested on Windows (WinXP), Linux (Ubuntu 6.06LTS) and FreeBSD 6.1. HTTP server has issues under high load with fastcgi usage. FastCGI works only in Linux for now.



### Ubuntu special notice

Some "missing files" are, in fact, in the "/net" subfolder and are not found by the fpc compiler. Simply copy missing files from the subfolder (lnet.pas, tomwinsock.pas, lfunc.inc...) "/usr/lib/lazarus/components/lnetpackage/lnet" in the "/usr/lib/lazarus/components/lnetpackage" folder (adjust paths as necessary). Then complete the setup by following the installation procedure. (by Steph12358) 

## API changes

**0.1 to 0.2**

  * There have been some API changes from 0.1 to 0.2. These were required and should improve the usability of packages. I know API changes are always a PITA but I'll try to keep them minimal from now on. Version 0.3 will be the first beta from which API will remain unchanged (only adding new stuff will be possible).
  * OnRecieve() changed to OnReceive()
  * Accept() changed to Listen()
  * All events' arguments changed. SocketNumber (int) got changed into aSocket(TLSocket) because of logical and speed reasons



**0.2 to 0.3**

  * Added OnCanSend and OnConnect. Connect is now non-blocking.
  * Removed buffersize and maxmsgs. Buffersize is no longer required because no internal buffer is used.



**0.3 to 0.4**

  * Lots of additions. Only "breakage" is that TLSocket.Port got changed into TLSocket.LocalPort and TLSocket.PeerPort for consistency reasons.



**0.4 to 0.5**

  * We did some API cleanup which resulted in API breakage.
  * All "connection" classes now descend from TLComponent
  * TLSocket now has a .Creator which is a TLComponent property specifying the creator (eg: TLTcp), so you can see now which socket in an event belongs to which connection.
  * Added "Connect()" and "Listen()" parameterless calls to all connections. This is mostly to enable Lazarus visual style, so people can use .Host and .Port properties and repeatedly just call ".Connect()" for example.
  * Added "StatusSet" set to SMTP and FTP components. This enables you to monitor specific FTP or SMTP actions and their results with events OnSuccess and OnFailure. You set the events to be watched in the set, and get results in one of the callbacks.
  * **Changed ALL callbacks which didn't use TLSocket as sender to use it. This means all FTP, SMTP and HTTP events which had "Sender: TL <HigherType>" were reverted to basic Socket style callbacks!**



**0.5.4- to 0.5.5+**

  * Some minor API changes were made which shouldn't break anything but I list them to be on the safe side.
  * .Timeout property on eventers and connections was set to Integer (from DWord) to support -1 as "block until an event happens"



**0.5 to 0.6**

  * No changes in public API, only additions.



**Note for 0.4.0** : In version 0.4.0, I forgot to move lHTTPSettings unit from lib to the http example. It was NOT supposed to be a part of the library, just the example. DON'T use this, it's not a "library", it's a part of the example (http server will get it's own project later) 

## lNet for FreeBSD, Linux, macOS and Windows

  * [lNet for FreeBSD, Linux, macOS and Windows](<https://github.com/trevoz/lnet>)
    * Many changes so that the library will now compile on the above with FPC 3.2.2 or FPC 3.3.1.
    * TLSv1.3 added
    * Linux TLS is not working due to an epoll problem in the file `lib/sys/lepolleventer.inc`
    * The console examples compile (I have tested the SMTP example which was what I was after).
    * The visual component examples do not compile for macOS - there are no Cocoa, or even defunct Carbon, interface bindings.



## See also

  * [lNet examples](<lNet_examples.md> "lNet examples")

---

_Source: [https://wiki.freepascal.org/lNet](https://web.archive.org/web/20250504104539/https://wiki.freepascal.org/lNet)_
