# Code Signing for Mac OS X

[![Logo OSX.png](https://wiki.freepascal.org/images/7/7a/Logo_OSX.png)](</File:Logo_OSX.png>)

This article applies to [Mac OS X](</Category:Mac_OS_X> "Category:Mac OS X") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

**[Code signing](<Code_signing.md> "Code signing")** ensures both authenticity and integrity of executables that have been downloaded from wide area networks like the internet. This discussion below, is aimed at AppStore distribution. 

  
For a discussion on the new Notarizing requirements, and for Developer website distributed app bundles, please see the forum thread here: <https://forum.lazarus.freepascal.org/index.php/topic,46256.0.html>

  
Code signing is required in [iOS](<Portal_iOS.md> "Portal:iOS"). On [Mac OS X](<Portal_Mac.md> "Portal:Mac") 10.7 and later, it enables programs downloaded from the Internet to be opened without any warnings (if they are signed with an Apple-issued certificate) and it is required there for using certain functionality (e.g. APIs used by debuggers; note that in this case a self-signed certificate that is marked as "trusted" suffices). 

This functionality is performed by Apple's Gatekeeper software. The user may entirely disable Gatekeeper in system preferences, but default settings only allow launching of applications with code signature or from the Mac App Store. 

  * [![](https://wiki.freepascal.org/images/9/9c/load_security_warning_dmg_leopard_en.png)](</File:load_security_warning_dmg_leopard_en.png>)

Simple warning after opening a downloaded application in Mac OS X Leopard. 

  * [![](https://wiki.freepascal.org/images/8/89/load_security_warning_dmg_mavericks_en.png)](</File:load_security_warning_dmg_mavericks_en.png>)

Default settings are more strict in Mac OS X Mavericks, where they prevent downloaded unsigned apps from being started via double-clicking. 

  * [![](https://wiki.freepascal.org/images/7/72/load_security_warning_mavericks_context_en.png)](</File:load_security_warning_mavericks_context_en.png>)

If the application is launched via context menu in Mavericks a simple warning is displayed similar to that of earlier versions of Mac OS X. 

  * [![](https://wiki.freepascal.org/images/7/7a/simple_load_security_warning_mavericks_en.png)](</File:simple_load_security_warning_mavericks_en.png>)

Gatekeeper shows a simplified warning, if a code-signed app has been downloaded and launched for the first time. In subsequent launches the program is started immediately. 

  * [![](https://wiki.freepascal.org/images/9/91/codesigning.png)](</File:codesigning.png>)

Overview of code-signing a program written with Lazarus and Free Pascal 




## Contents

  * 1 Overview
  * 2 Using codesign to sign your executable
  * 3 Running an app inside a sandbox in Mac OS X
  * 4 External links



## Overview

The basic steps to sign a program that has been written with Lazarus and/or Free Pascal are: 

  1. Obtain a Developer ID from Apple and install it in your system's key chain.
  2. Note the alphanumeric key of your Developer ID.
  3. Sign your executables with the _codesign_ command.



It is also possible to use signatures of third-party providers. Examples are DigiCert, Symantec and Thawte. 

## Using codesign to sign your executable

**Sign your application with:**

**
    
    
    codesign -f -s<signature> <path> -r="host => anchor apple and identifier com.apple.translate designated => identifier <BundleIdentifier>"

**

**Display basic information about the result of the signing process:**

**
    
    
    codesign -d -r-<path>

**

If your app was successfully signed this command will return the path to your executable and your identifier key. 

**Verify your signature:**

**
    
    
    codesign -vvvv<path>

**

If your app was successfully signed this command will return the strings "valid on disk" and "satisfies its Designated Requirement", respectively, after the path to your application. 

_< signature>_ denotes the 10-digit alphanumeric key of your developer ID.  
_< path>_ is the path to your program on your Mac's filesystem.  
_< BundleIdentifier >_ is an unambiguous URI of your executable, e.g. _com.my_company.my_application_. 

## Running an app inside a sandbox in Mac OS X

Every app released in the App Store must be code-signed and run in the sandbox, therefore it is useful to test if your application will work correctly when run inside the sandbox before submitting it for the App Store. 

To make sure that your app will be really running inside the sandbox, one useful trick is to create a button which will make a disallowed operation, such as creating a file. The code below is an example of how to do this. Remember to substitute an actual username in <<USERNAME>>
    
    
    procedure TMyDelegate.HandleButtonClick(sender: TObject {id for objective pascal apps});
    var
      lStrings: TStringList;
    begin
      lStrings := TStringList.Create();
      lStrings.Add('Line of text');
      lStrings.SaveToFile('/Users/<<USERNAME>>/forbitten_file.txt');
      lStrings.Free;
    end;

Run the application, click the button and verify that the file is created. Now delete the file. 

Create a permissions file, similar to the example below: 
    
    
    <?xml version="1.0" encoding="utf-8"?>
    <plist version="1.0">
      <dict>
        <key>com.apple.security.app-sandbox</key>
        <true/>
        <key>com.apple.security.files.user-selected.read-write</key>
        <true/>
        <key>com.apple.security.device.camera</key>
        <true/>
        <key>com.apple.security.print</key>
        <true/>
        <key>com.apple.security.app-protection</key>
        <true/>
        <key>com.apple.security.camera</key>
        <true/>
        <key>com.apple.security.documents.user-selected.read-write</key>
        <true/>
        <key>com.apple.security.temporary-exception.mach-lookup.global-name</key>
        <array>
          <string>com.apple.systemuiserver.screencapture</string>
        </array>
      </dict>
    </plist>

Now make sure that the application bundle does not use a symlink, which is actually the default for apps created in Lazarus. This is necessary, because otherwise code-signinig will fail. And codesign your application. The example below shows how to code-sign with a debug key for testing: 
    
    
     $ mv simplewindow simplewindow.app/Contents/MacOS/simplewindow
     $ codesign -s - -f --entitlements simplewindow.plist simplewindow.app/
    

For signing with a Apple developer number try: 
    
    
     $ /usr/bin/codesign -f --entitlements “foo.entitlements" -s "Mac Developer: My Name (ID)” foo.app/
    

But the debug key is enough. 

Next run the application as usual: 
    
    
     $ open simplewindow.app/
    

Then click in the button. An exception will be raised in the application. /Applications/Utilities/Console.app will show the following log about the error: 
    
    
     04/04/15 13:41:32,603 sandboxd[1675]: ([8980]) simplewindow(8980) deny file-write-create /Users/felipe/Programas/forbitten_file.txt
    

This is a success! The sandbox blocked the invalid operation. Now you can remove the button and test your application. You are sure that your application is running in the sandbox because it blocked the file write operation. If no other operations from your application are blocked, it passed the test and this is a very good sign towards it being compliant with the App Store rules. If you have other operations being blocked you will need to either add permissions to do these operations in your plist file, or you will need to redesign or remove some features, or maybe even give up the app store if your application cannot really work properly with the sandbox rules. 

## External links

  * [Apple Developer portal](<https://developer.apple.com/>)
  * [Code Signing Overview](<https://developer.apple.com/support/technical/code-signing/>)
  * [Apple's Code Signing Guide](<https://developer.apple.com/library/mac/documentation/Security/Conceptual/CodeSigningGuide/>)
  * [Information on code signatures by DigiCert](<http://www.digicert.com/code-signing/mac-os-codesign-tool.htm>)

---

_Source: [https://wiki.freepascal.org/Code_Signing_for_Mac_OS_X](https://web.archive.org/web/20190915100452/https://wiki.freepascal.org/Code_Signing_for_Mac_OS_X)_
