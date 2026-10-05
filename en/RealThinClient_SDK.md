# RealThinClient SDK

## Contents

  * 1 About
  * 2 Author
  * 3 License
  * 4 Download
  * 5 Change Log
  * 6 Dependencies / System Requirements
  * 7 Supported targets
  * 8 Installation
  * 9 The Example Application



### About

Delphi/Lazarus components for writing stable HTTP(S) Servers, ISAPI extensions and Clients. 

The download contains the components and demo applications. 

### Author

"RealThinClient" is a small privately held company from Germany, with main focus on Delphi components development, marketing and customer support. It was founded by Danijel Tkalcec in November 2004, when development of "RealThinClient" components has started. 

### License

BSD License (obsolete version), commercial license 

### Download

The lastest opensource release (SDK3 BSD License, 2009-11-20) can be found on <http://sourceforge.net/projects/rtcsdk/>. 

The lastest commercial release can be found on <http://www.realthinclient.com/files/>. 

### Change Log

  * Last commercial version: 6.04, 2012-09-18
  * Last opensource version: 3.0, 2009-11-20



### Dependencies / System Requirements

  * None



### Supported targets

  * Windows
  * Linux
  * MacOSX



### Installation

  * On Windows, the installation is very easy. Start up Lazarus, then open, compile and install the "rtcsdk_fpc" packege from RTC SDK Lib folder. Lazarus will restart and you will see the "RTC Client" and "RTC Server" tabs in Lazarus components palette.


  * If you are using uBuntu or some other Debian Linux distribution, do NOT use the Lazarus package from uBuntu and do NOT enable installation of community-supported (universe) packages, or you might end up installing files from Lazarus 0.9.24 and FPC 2.2.0 instead of what you have downloaded. In order to install the RTC SDK package, you will have to start Lazarus as a superuser "root". To do this, open up a Terminal window and use the "su" command to enter "superuser" mode (you will have to enter your password when asked), move to the Lazarus folder (it should be located at "usr/lib/lazarus" and execute "lazarus-ide". If you see Lazarus 0.9.26 popup window and Lazarus opens, you can download the latest RTC SDK package, unpack it somewhere in your "home" folder, open up the "rtcsdk_fpc" package in Lazarus, compile and install it. After Lazarus recompiles and starts up again, check if you see "RTC Server" and "RTC Client" tabs in the Lazarus components palette. They should now be there. Now that the RTC SDK is installed, you should close Lazarus and the Terminal window, then start Lazarus up again by using the standard "Lazarus" link under "Applications / Programming" in the ubuntu menu. When Lazarus starts up, you should be able to use the RTC SDK components.


  * On MacOSX, the installation is relatively easy because there are installation files for PowerPC and Intel-based Macs. If you are the Admin on MacOSX, you will not need to use a Terminal window to compile RTC SDK and have Lazarus built. Simply start up Lazarus (using the "Start Lazarus" icon) and you can open up, compile and install the "rtcsdk_fpc" package from the RTC SDK Lib folder. Lazarus will restart and you will see the "RTC Client" and "RTC Server" tabs in Lazarus components palette.



### The Example Application

  * First install the package (see *Installation*).
  * Open Demos folder and choose any Lazarus demo
  * compile
  * run

---

_Source: [https://wiki.freepascal.org/RealThinClient_SDK](https://web.archive.org/web/20190919110440/https://wiki.freepascal.org/RealThinClient_SDK)_
