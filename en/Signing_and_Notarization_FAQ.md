# Signing and Notarization FAQ

│ **English (en)** │

[![macOSlogo.png](https://wiki.freepascal.org/images/1/15/macOSlogo.png)](</File:macOSlogo.png>)

This article applies to [macOS](</Category:macOS> "Category:macOS") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

  
****

## Questions and Answers

**Question:** Official Mac page (which assumes Xcode) says one must bind to 10.9 SDK but can deploy/target older macOS releases for hardening/notarization: <https://developer.apple.com/documentation/xcode/notarizing_macos_software_before_distribution>

So has anyone here tried? If I target 10.9 instead of 10.8 (which I currently do to avoid some compilation problems when targeting older) I believe I will hinder my software in running on old macOS versions. 

**Answer:** I don't have any idea about the necessity in terms of notarisation, but it's perfectly possible to link against a newer SDK (-XR) while targeting an older macOS version (-WM). The two have been decoupled by Apple since quite a while now. 

_Source:<https://forum.lazarus.freepascal.org/index.php/topic,47637.0.html>_

* * *

**Question:** Is notarization/hardening necessary at present in Catalina? 

**Answer:** You can still open non-codesigned/non-notarized applications on Catalina in exactly the same way as on older macOS versions: via right-click or control-click -> Open. The main difference in Catalina is that codesigning is basically useless now without notarization, because it will still result in a warning and the requirement for the right-click/control-click workaround. 

_Source:<https://forum.lazarus.freepascal.org/index.php/topic,47636.0.html>_

* * *

**Question:** Code signing fails with _errSecInternalComponent_. What's wrong? 

**Answer:** If you are using an ssh session to the Apple computer, this occurs when the login keychain is locked. Before attempting code signing, unlock the login keychain with this command: 
    
    
     $ security unlock-keychain login.keychain
    

* * *

**Question:** My application is notarised successfully but is still denied by Gatekeeper. Why? 

**Answer:** Notarising your application is necessary to pass Gatekeeper, but it’s not sufficient. Gatekeeper has its own array of checks, and it’s easy to trip over one of those checks even though notarisation is successful. It’s not always easy to work out why Gatekeeper is unhappy. See [macOS Gatekeeper Debugging Tips](<macOS_Gatekeeper_Debugging_Tips.md> "macOS Gatekeeper Debugging Tips"). 

_Source:<https://forums.developer.apple.com/thread/125567>_

* * *

**Question:** My notarised application was working without complaint on macOS 10.15 but not on macOS 10.14. Why? 

**Answer:** You should always set _CFBundlePackageType_ to _APPL_ in your application `Info.plist ` file. 

_Source:<https://developer.apple.com/forums/thread/77430>_

* * *

## See also

  * [Notarization for macOS 10.14.5+](<Notarization_for_macOS_10.14.md> "Notarization for macOS 10.14.5+")
  * [Code Signing for macOS](<Code_Signing_for_macOS.md> "Code Signing for macOS")
  * [macOS Gatekeeper Debugging Tips](<macOS_Gatekeeper_Debugging_Tips.md> "macOS Gatekeeper Debugging Tips")

---

_Source: [https://wiki.freepascal.org/Signing_and_Notarization_FAQ](https://web.archive.org/web/20230322005252/https://wiki.freepascal.org/Signing_and_Notarization_FAQ)_
