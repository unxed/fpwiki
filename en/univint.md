# univint

[![macOSlogo.png](https://wiki.freepascal.org/images/1/15/macOSlogo.png)](</File:macOSlogo.png>)

This article applies to [macOS](</Category:macOS> "Category:macOS") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

│ **English (en)** │

## Overview

The **univint** package contains Pascal translations of various macOS framework headers. 

In FPC 2.4.0 and earlier, these headers are only available for PowerPC and Intel 32 bit. Most headers in these versions are limited to their state in Puma 10.1, although some are newer (a few have even been updated to Leopard 10.5). 

In later FPC versions, they are also available for 64 bit platforms and for iOS/ARM, and have been updated to Yosemite 10.10 (2014). Since not all APIs are available to 64 bit and iOS applications in the C headers, the same is of course true for the Pascal translation. 

See [Apple's documentation](<https://developer.apple.com/documentation/>) for details about which APIs are available on which platforms and in which operating system versions. 

## Carbon framework

The Carbon framework is not available at all on the iOS platform, is only partially available on 64 bit macOS platforms up to macOS 10.14 Mojave, and is not available at all on macOS 10.15 Catalina released in 2019. Catalina has dropped support for all 32 bit applications - they will not execute. The future for macOS is the 64 bit [Cocoa framework](<Cocoa_Interface.md> "Cocoa Interface"). 

Therefore, while on 32 bit platforms the MacOSAll unit contains a _{$linkframework Carbon}_ statement so that any application using that unit automatically links in the Carbon framework, we have opted not to do this for 64 bit platforms. On those platforms, the MacOSAll unit will by default only link against the _CoreFoundation_ framework. If you wish to use Carbon functionality from a 64 bit application, you must explicitly link it in by adding the _{$linkframework Carbon}_ statement to your program. 

A side-effect of this policy is that the the _GetKeys_ routine is not available in the 64 bit version of MacOSAll. The reason is that this function is implemented using a wrapper, which in turn calls the real _GetKeys_ routine from the Carbon framework. As a result, if smart linking/dead code stripping is not enabled, then the presence of this wrapper in your program (via the MacOSAll unit) makes your program depend on the Carbon framework, which as mentioned above is not very nice for 64 bit applications. Therefore this one function is available in a separate unit called KeyEvents when compiling for 64 bit. For 32 bit applications, this unit also exists, but is empty (simply to avoid requiring ifdefs in case you need this routine in 64 bit). 

## See also

  * [macOS Catalina changes for developers](<macOS_Catalina_changes_for_developers.md> "macOS Catalina changes for developers")

---

_Source: [https://wiki.freepascal.org/univint](https://web.archive.org/web/20230131225301/https://wiki.freepascal.org/univint)_
