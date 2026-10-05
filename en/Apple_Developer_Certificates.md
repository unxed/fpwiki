# Apple Developer Certificates

[![macOSlogo.png](https://wiki.freepascal.org/images/1/15/macOSlogo.png)](</File:macOSlogo.png>)

This article applies to [macOS](</Category:macOS> "Category:macOS") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

[![Apple iOS new.svg](https://upload.wikimedia.org/wikipedia/commons/thumb/4/48/Apple_iOS_new.svg/50px-Apple_iOS_new.svg.png)](</File:Apple_iOS_new.svg>)

This article applies to [iOS](</Category:iOS> "Category:iOS") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

## Contents

  * 1 Overview
  * 2 Xcode 11
    * 2.1 Apple Development Certificate
    * 2.2 Apple Distribution Certificate
  * 3 macOS app distribution via the Mac App Store
    * 3.1 Mac Development Certificate
    * 3.2 Mac App Distribution Certificate
    * 3.3 Mac Installer Distribution Certificate
  * 4 macOS app distribution outside the Mac App Store
    * 4.1 Developer ID Application Certificate
    * 4.2 Developer ID Installer Certificate
  * 5 Expired or revoked certificates
    * 5.1 Mac App Distribution Certificate and Mac Installer Distribution Certificate (Mac App Store)
    * 5.2 Developer ID Application Certificate (Mac apps)
    * 5.3 Developer ID Installer Certificate (Mac apps)
  * 6 See also
  * 7 External links



## Overview

This article deals with the various certificates available to someone who has signed up and paid for the [Apple Developer Program](<https://developer.apple.com/programs/>) and describes which certificate to use for what. Given the number and names of the various certificates it can be somewhat daunting to figure out which certificate is used for what. 

Finally, there is a description of the consequences of expired and revoked certificates. 

[![Warning-icon.png](https://wiki.freepascal.org/images/b/b2/Warning-icon.png)](</File:Warning-icon.png>)

**Warning:** It is not possible to use certificates from third-party providers like Comodo and DigiCert because they will not pass Gatekeeper which requires an Apple developer issued certificate. Also note that you cannot sign Windows applications with the Apple developer certificate (this time you do need a third-party Comodo, DigiCert etc certificate).

## Xcode 11

Xcode 11 supports the new _Apple Development_ and _Apple Distribution_ certificate types. These certificates support building, running, and distributing apps on any Apple platform. Preexisting iOS and macOS development and distribution certificates continue to work, however, new certificates you create in Xcode 11 use the new types. Previous versions of Xcode don’t support these certificates. 

### Apple Development Certificate

This certificate is used to sign development versions of your iOS, macOS, tvOS, and watchOS applications. For use in Xcode 11 or later. 

### Apple Distribution Certificate

This certificate is used to sign your applications for submission to the App Store for distribution. For use with Xcode 11 or later. 

## macOS app distribution via the Mac App Store

### Mac Development Certificate

This certificate is used to sign development versions of your Mac applications for testing and debugging. It enables certain app services during development and testing. 

### Mac App Distribution Certificate

This certificate is used to code sign your application and configure a Distribution Provisioning Profile for submission to the Mac App Store. 

### Mac Installer Distribution Certificate

This certificate is used to sign your application's Installer Package for submission to the Mac App Store. 

## macOS app distribution outside the Mac App Store

The certificate types for distribution of macOS applications outside the Apple Mac App Store: 

### Developer ID Application Certificate

This certificate is used to code sign your application for distribution outside of the Mac App Store. Note that kernel extensions require a special certificate and that they are now deprecated anyway. 

### Developer ID Installer Certificate

This certificate is used to sign your application's Installer Package (if any) for distribution outside of the Mac App Store. 

## Expired or revoked certificates

### Mac App Distribution Certificate and Mac Installer Distribution Certificate (Mac App Store)

If your Apple Developer Program membership is valid, your existing apps on the Mac App Store will not be affected. However, you will no longer be able to upload new apps or updates signed with the expired or revoked certificate to the Mac App Store. 

### Developer ID Application Certificate (Mac apps)

If your certificate expires, users can still download, install, and run versions of your Mac applications that were signed with this certificate. However, you will need a new certificate to sign updates and new applications. If your certificate is revoked, users will no longer be able to install applications that have been signed with this certificate. If your Mac application utilizes a Developer ID provisioning profile to take advantage of advanced capabilities such as CloudKit and push notifications, you must ensure your Developer ID provisioning profile is valid in order for installed versions of your application to run. 

### Developer ID Installer Certificate (Mac apps)

If your certificate expires, users can no longer launch installer packages for your Mac applications that were signed with this certificate. Previously installed apps will continue to run however new installations will not be possible until you have re-signed your installer package with a valid Developer ID Installer certificate. If your certificate is revoked, users will no longer be able to install applications that have been signed with this certificate. 

## See also

  * [Code Signing for macOS](<Code_Signing_for_macOS.md> "Code Signing for macOS")
  * [Notarization for macOS 10.14.5+](<Notarization_for_macOS_10.14.md> "Notarization for macOS 10.14.5+")



## External links

  * [Apple Guidelines and Resources](<https://developer.apple.com/app-store/resources/>)

---

_Source: [https://wiki.freepascal.org/Apple_Developer_Certificates](https://web.archive.org/web/20230605165408/https://wiki.freepascal.org/Apple_Developer_Certificates)_
