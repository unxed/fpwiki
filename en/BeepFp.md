# BeepFp

## Contents

  * 1 About
  * 2 Author
  * 3 License
  * 4 Download
  * 5 Change Log
  * 6 Dependencies / System Requirements
  * 7 Issues
  * 8 Installation
  * 9 The _BEEP_Listen_ Server Example Application
  * 10 The _BEEP_Client_ Client Example Application



### About

_BeepFp_ is a Free Pascal componentset to allow development of custom protocols using _BEEP_ in native Object Pascal. 

_BeepFp_ does not implement _BEEP_ itself, but rather builds an easy to use componentset on top of a stable and proven library. The library, [_LibVortex_](<http://www.aspl.es/vortex>), is a full RFC3080 implementation. 

_BEEP_ is a network application framework protocol. It is not a complete protocol but only provides the building blocks common to most network protocols to ease custom protocol design and speed up implementation. To understand exactly what _BEEP_ is, see [www.beepcore.org](<http://www.beepcore.org>) and RFC3080. 

The main characteristics of _BeepFp_ are : 

  * Built on a proven library
  * Event driven communication
  * Blocking and non-blocking modes
  * Requires only a few event handlers to implement full-blown network protocol
  * Multiple channels on a single TCP/IP connection



  
The main characteristics of _BEEP_ are : 

  * Common parts of network protocols are in the specification
  * Provides for authentication using SASL
  * Provides for encryption using TLS
  * Protocols can be defined in a peer-to-peer mode, client-server mode or a combination thereof
  * Each socket connection between peers runs multiple concurrent channels.
  * Each channel can deliver messages synchronously or asynchronously.



### Author

Wimpie Nortje ([User:Wimpie](</index.php?title=User:Wimpie&action=edit&redlink=1> "User:Wimpie \(page does not exist\)")) 

### License

[modified](<http://svn.freepascal.org/svn/lazarus/trunk/COPYING.modifiedLGPL>) [LGPL](<http://svn.freepascal.org/svn/lazarus/trunk/COPYING.LGPL>) (same as the FPC RTL and the Lazarus LCL). You can contact the author if the modified LGPL doesn't work with your project licensing. 

The LibAxl and LibVortex shared libraries is under _LGPL_. Contact the authors at [www.aspl.es](<http://www.aspl.es>) with any questions. 

### Download

The latest stable release can be found on <http://sourceforge.net/projects/lazarus-ccr/files/BeepFp/>. 

### Change Log

  * Release 1.1 _31 March 2010_
    * Events are triggered in main application thread.
    * Lazarus component to manage thread synchronisation.
    * Numerous fixes.
  * Release 1.0 _14 August 2009_
    * Initial release with most of the basic BEEP functionality



### Dependencies / System Requirements

  * [LibAxl](<http://www.aspl.es/axl>) version 0.5.7 or later
  * [LibVortex](<http://www.aspl.es/vortex>) v1.1.2 or later.



  
Status: _Stable_

  


### Issues

  * Denying a channel request doesn't work properly
  * Requesting unsopported profiles causes problems
  * The server cannot initiate a connection close. It hangs when trying to.



### Installation

  * Obtain the [LibAxl](<http://www.aspl.es/axl>) and [LibVortex](<http://www.aspl.es/vortex>) libraries.
  * Install and test the libraries following the included transactions.
  * Open the libaxl package, compile
  * Open the libvortex package, compile
  * Open the BeepFp package, compile
  * In a project, add BeepFp package as requirement



### The _BEEP_Listen_ Server Example Application

  * Open example/BEEP_Listen.lpi
  * compile
  * run



### The _BEEP_Client_ Client Example Application

  * Open example/BEEP_Client.lpi
  * compile
  * run

---

_Source: [https://wiki.freepascal.org/BeepFp](https://web.archive.org/web/20250417140108/https://wiki.freepascal.org/BeepFp)_
