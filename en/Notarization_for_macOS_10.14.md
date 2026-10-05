# Notarization for macOS 10.14.5+

│ **English (en)** │  **[русский (ru)](<../ru/Notarization_for_macOS_10.14.md>)** │

[![macOSlogo.png](https://wiki.freepascal.org/images/1/15/macOSlogo.png)](</File:macOSlogo.png>)

This article applies to [macOS](</Category:macOS> "Category:macOS") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

  
****

## Contents

  * 1 Overview
  * 2 Gatekeeper dialogs
  * 3 Notarization requirements
    * 3.1 Apple Statement 23 December 2019
  * 4 Notarizing an application
    * 4.1 Step 1 - Create disk image
    * 4.2 Step 2 - Code sign the disk image
    * 4.3 Step 3 - Generate an app-specific password
    * 4.4 Step 4 - Upload the disk image to the notary service
    * 4.5 Step 5 - Check the notarization process
    * 4.6 Step 6 - Staple the ticket to the disk image
    * 4.7 Step 7 - Verify notarization of the disk image
    * 4.8 Step 8 - Verify notarization of the application
  * 5 Notarizing a command line tool
    * 5.1 Inserting an Info.plist in a command line executable
    * 5.2 Signing a command line executable
    * 5.3 Submitting a command line executable for notarization
    * 5.4 Checking the notarization progress
    * 5.5 Verifying notarization of a command line executable
  * 6 Troubleshooting
    * 6.1 Notarization upload process
    * 6.2 Notarization process failures
    * 6.3 Notarization check failures
  * 7 See also
  * 8 External links



## Overview

Beginning with macOS 10.14.5 (Mojave), software signed with a new Apple developer certificate and all new or updated kernel extensions must be notarized to run. Beginning in macOS 10.15 (Catalina), all software built after June 1, 2019, and code signed with an Apple developer certificate must be notarized. _Notarization is not required for software distributed through the Apple App Store_ because the App Store submission process already includes equivalent security checks. 

In November 2023, Apple amended the Notarizing process and rules, and changed the utility program to notarytool. These changes are now shown below. 

Notarization is designed by Apple to give users more confidence that Apple developer-signed software has been checked by Apple for malicious components. Notarization is not an App Review. The Apple notary service is an automated system that scans software for malicious content, checks for code-signing issues, and returns the results. If there are no issues, the notary service generates a ticket to staple to the software; the notary service also publishes that ticket online where Gatekeeper can find it. 

When the user first installs or runs software, the presence of a ticket (either online or attached to the executable) tells Gatekeeper that Apple notarized the software. Gatekeeper then places descriptive information in the initial launch dialog to help the user make an informed choice about whether to launch the app. 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** **Even if an application has not been notarized, it can still be run by control-clicking on the app, or right clicking for the context menu, and choosing _Open_ \- see the Gatekeeper "bypass" dialog in the gallery [below](<Notarization_for_macOS_10.14.md> "Notarization for macOS 10.14.5+").**

Code signing an application without also notarizing it is now a totally pointless exercise _unless_ you are only distributing your application via the App Store. 

## Gatekeeper dialogs

  * [![Gatekeeper Dialog before Catalina where an app that is not notarized is first opened \(Mojave macOS 10.14 dialog\)](https://wiki.freepascal.org/images/e/e1/macos-mojave-app-downloaded-from-internet-alert-dark.jpg)](</File:macos-mojave-app-downloaded-from-internet-alert-dark.jpg> "Gatekeeper Dialog before Catalina where an app that is not notarized is first opened \(Mojave macOS 10.14 dialog\)")

Gatekeeper Dialog before Catalina where an app that is not notarized is first opened (Mojave macOS 10.14 dialog) 

  * [![Gatekeeper dialog where an app which is signed and notarized is first opened \(Catalina macOS 10.15.1 dialog\)](https://wiki.freepascal.org/images/c/c6/Screenshot_2020-01-02_at_22.42.01.png)](</File:Screenshot_2020-01-02_at_22.42.01.png> "Gatekeeper dialog where an app which is signed and notarized is first opened \(Catalina macOS 10.15.1 dialog\)")

Gatekeeper dialog where an app which is signed and notarized is first opened (Catalina macOS 10.15.1 dialog) 

  * [![Gatekeeper dialog where an app is not signed nor notarized is first opened \(Catalina macOS 10.15.1 dialog\)](https://wiki.freepascal.org/images/6/6c/gatekeep_no.png)](</File:gatekeep_no.png> "Gatekeeper dialog where an app is not signed nor notarized is first opened \(Catalina macOS 10.15.1 dialog\)")

Gatekeeper dialog where an app is not signed nor notarized is first opened (Catalina macOS 10.15.1 dialog) 

  * [![The Gatekeeper "bypass" dialog, accessed by control-clicking on the app \(Catalina macOS 10.15.3 dialog\)](https://wiki.freepascal.org/images/3/33/macos-catalina-alert-unsigned-override.jpg)](</File:macos-catalina-alert-unsigned-override.jpg> "The Gatekeeper "bypass" dialog, accessed by control-clicking on the app \(Catalina macOS 10.15.3 dialog\)")

The Gatekeeper "bypass" dialog, accessed by control-clicking on the app (Catalina macOS 10.15.3 dialog) 




## Notarization requirements

Notarization requires the Xcode command line tools from Xcode 10 or later. Building a new app for notarization requires macOS 10.13.6 or later. Stapling an app requires macOS 10.12 or later. 

Apple's notary service requires you to: 

  * Enable code-signing for all of the executables you distribute.
  * Enable the [Hardened Runtime](<Hardened_runtime_for_macOS.md> "Hardened runtime for macOS") capability for your application and command line targets.
  * Use a “Developer ID” application, kernel extension, or installer certificate for your code-signing signature.
  * Include a secure timestamp with your code-signing signature (include the --timestamp option when running the codesign tool).
  * Don’t include the com.apple.security.get-task-allow entitlement with the value set to any variation of true. If your software hosts third-party plug-ins and needs this entitlement to debug the plug-in in the context of a host executable, see [Avoid the Get-Task-Allow Entitlement](<https://developer.apple.com/documentation/xcode/notarizing_macos_software_before_distribution/resolving_common_notarization_issues#3087731>).
  * Link against the macOS 10.9 or later SDK.



On 3 September 2019 Apple announced that until January 2020 developers could get new versions of applications notarized even though they were not [hardened](<Hardened_runtime_for_macOS.md> "Hardened runtime for macOS") or fully compliant with the normal requirements. **That deadline was later extended to 3 February 2020. The above notarization requirements are now in full effect.**

### Apple Statement 23 December 2019

In June, we announced that all Mac software distributed outside the Mac App Store must be notarized by Apple in order to run by default on macOS Catalina. In September, we temporarily adjusted the notarization prerequisites to make this transition easier and to protect users on macOS Catalina who continue to use older versions of software. Starting February 3, 2020, all submitted software must meet the original notarization prerequisites. 

If you haven’t yet done so, upload your software to the notary service and review the developer log for warnings. These warnings will become errors starting February 3 and must be fixed in order to have your software notarized. Software notarized before February 3 will continue to run by default on macOS Catalina. 

As a reminder, all installer packages must be signed since they may contain executable code. Disk images do not need to be signed, although signing them can help your users verify their contents. 

## Notarizing an application

These instructions assume you have completed the other preparatory steps: [code signed](<Code_Signing_for_macOS.md> "Code Signing for macOS") the application, implemented the [hardened runtime capability](<Hardened_runtime_for_macOS.md> "Hardened runtime for macOS"), [code signed](<Code_Signing_for_macOS.md> "Code Signing for macOS") the installer pkg (if you have one) etc. 

You can't upload the .app bundle directly to the notary service, you need to create a compressed archive containing the application or you can put applications, kernel extensions, and other software in a container, like a disk image, and notarize the container. The notary service accepts disk images (UDIF format), signed flat installer packages, and ZIP archives. It processes nested containers as well, like packages inside a disk image. 

### Step 1 - Create disk image

Create a disk image by opening a Terminal and running the following command: 
    
    
    hdiutil create -volname MyApp -srcfolder /path/to/MyApp.app -ov -format UDBZ MyApp.dmg
    

### Step 2 - Code sign the disk image

Disk images must be signed with your Developer ID Application Certificate and include a secure timestamp. 

Code sign the disk image by opening a Terminal and running the following command: 
    
    
    codesign -s "Developer ID Application: Your Name (Team ID)" --timestamp MyApp.dmg
    

Note: See [Code Signing for macOS](<Code_Signing_for_macOS.md> "Code Signing for macOS") for more details relevant to code signing especially if doing this on aarch64 for an x86_64 binary. 

### Step 3 - Generate an app-specific password

To generate an app-specific password, refer to this [Apple Support article](<https://support.apple.com/en-us/HT204397>). _Note:_ this is a password that will be specific to the notarization application (xcrun notarytool) and not to the application being notarized. You therefore only need to do this once, but make sure you copy the generated password and save it somewhere. 

[![Note-icon.png](https://wiki.freepascal.org/images/b/be/Note-icon.png)](</File:Note-icon.png>)

**Tip:** Save the password to your keychain with:   
  
`xcrun notarytool store-credentials "notarytool-password" --apple-id "<AppleID>" --team-id <DeveloperTeamID> --password <secret_2FA_password>`   
  
and then you can use `"@keychain:<name for pwd>"` in the following commands.

### Step 4 - Upload the disk image to the notary service

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** When you notarize the container disk image, notarytool also notarizes the application inside, so you can skip the step of notarizing the application itself.

Upload the disk image file to the Apple notary service by opening a Terminal and running the following command: 
    
    
    xcrun notarytool submit --apple-id "me@mysoftware.com" --password <app-specifc-password>  --team-id <your 10 character team ID>  /Path/to/MyApp.dmg
    

The response includes an _id_ (session-ID) that we use later to keep track of this submission with the notarization service. It will be used again to check on the status. 

If the upload is successful you should receive output similar to the following: 
    
    
    Conducting pre-submission checks for MyApp.dmg and initiating connection to the Apple notary service...
    Submission ID received
      id: 43e04940-1601-4959-a40c-79b83172d22b
    Upload progress: 100,00% (3,97 MB of 3,97 MB)   
    Successfully uploaded file
      id: 43e04940-1601-4959-a40c-79b83172d22b
      path: /Path/to/MyApp.dmg
    

  


### Step 5 - Check the notarization process

The notarization process generally takes less than an hour, and usually just few minutes. They no longer issue emails, so you will need to check its progress from time to time by opening a Terminal and running the following command: 
    
    
    xcrun notarytool info --apple-id "me@mysoftware.com" --password <app-specifc-password>  --team-id <your 10 character team ID>  session-ID
    

When the notarization process completes successfully the above command will return information similar to the following: 
    
    
    Looking up :  43e04940-1601-4959-a40c-79b83172d22b
    Successfully received submission info
      createdDate: 2024-05-06T10:16:46.448Z
      id: 43e04940-1601-4959-a40c-79b83172d22b
      name: MyApp.dmg
      status: Accepted
    

The possible status results are: In process, Accepted or Failure. 

Accepted means success in this context. If the status is failure, then further details can be obtained with the "log" command: 
    
    
    xcrun notarytool log --apple-id "me@mysoftware.com" --password <app-specifc-password>  --team-id <your 10 character team ID>  session-ID
    

  


### Step 6 - Staple the ticket to the disk image

The notarization process produces a ticket that tells Gatekeeper that your application is notarized. After notarization completes successfully, the next time any user attempts to run your application on macOS 10.14 or later, Gatekeeper will find the ticket online. This includes users who downloaded your application before notarization. 

After step 5 receives the "Package Approved" status message, you should also attach the ticket to the disk image file using the stapler tool, so that future distributions include the ticket. This ensures that Gatekeeper can find the ticket even when a network connection is not available. 

To staple the ticket to the disk image file, open a Terminal and run the following command: 
    
    
    xcrun stapler staple /path/to/MyApp.dmg
    

If the command completes successfully, the output should be similar to: 
    
    
    Processing: /Path/to/MyApp.dmg
    Processing: /Path/to/MyApp.dmg
    The staple and validate action worked!
    

Note: While you can staple an installer package and disk image directly, if notarization used a zip file, the application bundle needs to be unzipped and then it can be stapled and rezipped. 

### Step 7 - Verify notarization of the disk image

To verify the notarization of the disk image, open a Terminal and run the following command: 
    
    
    spctl -a -vv -t install MyApp.dmg
    

A successful verification of the notarization process should produce output similar to the following: 
    
    
    MyApp.dmg: accepted
    source=Notarized Developer ID
    origin=Developer ID Application: <Developer Name> (<TEAM_ID>)
    

### Step 8 - Verify notarization of the application

To verify the notarization of the application, install the application, open a Terminal and run the following command: 
    
    
    spctl -a -vv /Applications/MyApp.app
    

A successful verification of the notarization process should produce output similar to the following: 
    
    
    /Applications/MyApp.app: accepted
    source=Notarized Developer ID
    origin=Developer ID Application: <Developer Name> (<TEAM_ID>)
    

**Important:** test the application by putting it in the Applications directory. It's treated differently by the Gatekeeper when in the "installed" location. 

Alternatively, the Apple-recommended way to verify the notarization of an application is to open a Terminal and run the following command: 
    
    
    xcrun stapler validate MyApp.app
    

A successful verification of the notarization process should produce output similar to the following: 
    
    
    Processing: MyApp.app
    The validate action worked!
    

## Notarizing a command line tool

Command line tools can be code signed, but they cannot be easily notarized because they do not have an [Application Bundle](<Application_Bundle.md> "Application Bundle") and a standalone `Info.plist` [property list file](<macOS_property_list_files.md> "macOS property list files"). You can relatively easily insert an `Info.plist` file in your command line tool executable. 

### Inserting an Info.plist in a command line executable

Here is a template that you can customise for your command line utility. I extracted it from a command line utility executable written in C named "simplereach" which I built with Xcode 11.3.1 on Mojave after enabling the "Create Info.plist Section in Binary" under the "Packaging" options (I have omitted the unnecessary Xcode-related build info from it). 
    
    
    <!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
    <plist version="1.0">
    <dict>
    	<key>BuildMachineOSBuild</key>
    	<string>18G8022</string>
    	<key>CFBundleDevelopmentRegion</key>
    	<string>English</string>
    	<key>CFBundleDocumentTypes</key>
    	<array>
    		<dict>
    			<key>CFBundleTypeOSTypes</key>
    			<array>
    				<string>fold</string>
    				<string>disk</string>
    				<string>****</string>
    			</array>
    		</dict>
    	</array>
    	<key>CFBundleExecutable</key>
    	<string>simplereach</string>
    	<key>CFBundleIdentifier</key>
    	<string>com.mycompany.simplereach</string>
    	<key>CFBundleInfoDictionaryVersion</key>
    	<string>6.0</string>
    	<key>CFBundleName</key>
    	<string>simplereach</string>
    	<key>CFBundlePackageType</key>
    	<string>APPL</string>
    	<key>CFBundleShortVersionString</key>
    	<string>1.0</string>
    	<key>CFBundleSupportedPlatforms</key>
    	<array>
    		<string>MacOSX</string>
    	</array>
    	<key>CFBundleVersion</key>
    	<string>1</string>
    	<key>LSMinimumSystemVersion</key>
    	<string>10.14</string>
    </dict>
    </plist>
    

You will need to modify a few key strings in the above file. Substitute your own: 

  * **BuildMachineOSBuild** string can be obtained from a Terminal with the command `sw_vers`. The "18G8022" strong is for Mojave 10.14.6.
  * **CFBundleDevelopmentRegion** string if it is not "English".
  * **CFBundleExecutable** string with your utility name.
  * **CFBundleIdentifier** string (conventionally your domain name in reverse followed by utility name; feel free use anything unique).
  * **CFBundleName** string (use the same value as for CFBundleExecutable).
  * **CFBundleShortVersionString** string with your version number.
  * **LSMinimumSystemVersion** string with the minimum macOS version on which your utility will run.



You can then insert the `Info.plist` file into your application by telling the linker to add it as a __TEXT section named __info_plist: 
    
    
    fpc -k"-sectcreate __TEXT __info_plist Info.plist" my_utility.pas
    

To check whether it has been successfully inserted you can run the following command in a Terminal: 
    
    
    otool -X -v -s __TEXT __info_plist my_utility
    

### Signing a command line executable

Now that the `Info.plist` file has been inserted in the executable, you are ready to code sign it by executing the following commands in a Terminal: 
    
    
    cd /path/to/my_utility
    mkdir bin
    mv my_utility bin/
    codesign -f --strict --timestamp --options=runtime -s "Developer ID Application: YOUR NAME (TEAM_ID)" bin/my_utility
    

It is _critical_ that you create the `bin` directory and move your utility to it before signing. Failure to do so will result in a `_CodeSignature directory` being created which is not appropriate for a command line utility and the notarisation process will fail. 

To verify this was successful you can run the following command in a Terminal: 
    
    
    codesign -vv bin/my_utility
    

If code signing was successful, the output should be something similar to: 
    
    
    my_utility: valid on disk
    my_utility: satisfies its Designated Requirement 
    

### Submitting a command line executable for notarization

As you cannot notarize a bare executable, you need to incorporate it into a container (.dmg - Disk Image, .pkg - Installer Package, or a ZIP archive file). A ZIP archive is the simplest so in a Terminal execute the following command: 
    
    
    zip my_utility.zip bin/my_utility
    

You are now ready to upload your command line utility to Apple's servers for notarization. In a terminal execute the following command: 
    
    
    xcrun notarytool submit --apple-id "me@mysoftware.com" --password <app-specifc-password>  --team-id <your 10 character team ID> my_utility.zip
    

If you do not already have an app specific password, see above for how to generate one. 

The response includes an _id_ (session-ID) that we use later to keep track of this submission with the notarization service. It will be used again to check on the status. 

If the upload is successful you should receive output similar to the following: 
    
    
    Conducting pre-submission checks for my_utility.zip and initiating connection to the Apple notary service...
    Submission ID received
      id: 43e04940-1601-4959-a40c-79b83172d22b
    Upload progress: 100,00% (3,97 MB of 3,97 MB)   
    Successfully uploaded file
      id: 43e04940-1601-4959-a40c-79b83172d22b
      path: /Path/to/my_utility.zip
    

### Checking the notarization progress

The notarization process generally takes less than an hour, and usually just few minutes. They no longer issue emails, so you will need to check its progress from time to time by opening a Terminal and running the following command: 
    
    
    xcrun notarytool info --apple-id "me@mysoftware.com" --password <app-specifc-password>  --team-id <your 10 character team ID>  session-ID
    

When the notarization process completes successfully the above command will return information similar to the following: 
    
    
    Looking up :  43e04940-1601-4959-a40c-79b83172d22b
    Successfully received submission info
      createdDate: 2024-05-06T10:16:46.448Z
      id: 43e04940-1601-4959-a40c-79b83172d22b
      name: MyApp.dmg
      status: Accepted
    

The possible status results are: In process, Accepted or Failure. Accepted means success in this context. If the status is failure, then further details can be obtained with the "log" command: 
    
    
    xcrun notarytool log --apple-id "me@mysoftware.com" --password <app-specifc-password>  --team-id <your 10 character team ID>  session-ID
    

  
In case of a notarization failure, consult Troubleshooting below for help. 

### Verifying notarization of a command line executable

To verify the notarization of a command line tool, open a Terminal and run the following command: 
    
    
    spctl -a -t open --context context:primary-signature -vv my_utility 
    

A successful verification of a properly notarized command line executable should produce output similar to the following: 
    
    
    my_utility: accepted
    source=Notarized Developer ID
    origin=Developer ID Application: Your Name (Z9HBGNDZZ9)
    

## Troubleshooting

### Notarization upload process

If the notarisation upload process does not go well, you should be able to find a compressed log file in your temporary directory which may shed some light on what happened. You can view it by opening a Terminal and running the following commands (substitute the name of your log file which includes your application bundle-id): 
    
    
      cd ${TMPDIR}
      gzcat c51ca05f-cf49-405f-be57-26873faab5f6_3073718126753816760org.sentry.myapp-20191231004646-273.itmsp6022059143057318377.log
    

which, for a successful notarization upload, should look similar to: 
    
    
     [2019-12-31 19:46:46 AEDT] <main>  INFO: Starting upload for packageName: org.sentry.myapp-20191231004646-273.itmsp
     [2019-12-31 19:46:46 AEDT] <main>  INFO: Thread Pool Configuration: {processors=12, perCoreThreadMultiplier=1.65, coreThreads=6, maxThreads=14, minConnections=6, maxConnections=13, rateLimit=100000000000 KBs}
     [2019-12-31 19:46:46 AEDT] <main>  INFO: Reservation CPUSource113/v4/93/41/ce/9341ce96-e7bc-2577-eaac-4528c888a70d/metadata.xml created for file: (metadata.xml), part count:(1), file size: (538)
     [2019-12-31 19:46:46 AEDT] <main>  INFO: Reservation CPUSource113/v4/93/41/ce/9341ce96-e7bc-2577-eaac-4528c888a70d/MyApp_31122019.dmg created for file: (MyApp_31122019.dmg), part count:(1), file size: (1400933)
     [2019-12-31 19:46:46 AEDT] <RLT-002>  INFO: Starting upload for reservation: (CPUSource113/v4/93/41/ce/9341ce96-e7bc-2577-eaac-4528c888a70d/metadata.xml) file: (metadata.xml/1) size: 538
     [2019-12-31 19:46:46 AEDT] <RLT-003>  INFO: Starting upload for reservation: (CPUSource113/v4/93/41/ce/9341ce96-e7bc-2577-eaac-4528c888a70d/myapp_31122019.dmg) file: (MyApp_31122019.dmg/1) size: 1400933
     [2019-12-31 19:46:47 AEDT] <RLT-002>  INFO: Finished part upload for: (metadata.xml/1) 2.91 KB/s in 0 seconds
     [2019-12-31 19:46:47 AEDT] <RLT-002>  INFO: Finished upload for reservation: (CPUSource113/v4/93/41/ce/9341ce96-e7bc-2577-eaac-4528c888a70d/metadata.xml) file: (metadata.xml)
     [2019-12-31 19:46:47 AEDT] <pool-8-thread-1>  INFO: Reservation committed: (CPUSource113/v4/93/41/ce/9341ce96-e7bc-2577-eaac-4528c888a70d/metadata.xml) file: (metadata.xml)
     [2019-12-31 19:46:49 AEDT] <RLT-003>  INFO: Finished part upload for: (MyApp_31122019.dmg/1) 781.33 KB/s in 1 second
     [2019-12-31 19:46:49 AEDT] <RLT-003>  INFO: Finished upload for reservation: (CPUSource113/v4/93/41/ce/9341ce96-e7bc-2577-eaac-4528c888a70d/MyApp_31122019.dmg) file: (MyApp_31122019.dmg)
     [2019-12-31 19:46:49 AEDT] <main>  INFO: Waiting on reservation commit(s) to finish.
     [2019-12-31 19:46:49 AEDT] <pool-8-thread-1>  INFO: Reservation committed: (CPUSource113/v4/93/41/ce/9341ce96-e7bc-2577-eaac-4528c888a70d/MyApp_31122019.dmg) file: (MyApp_31122019.dmg)
     [2019-12-31 19:46:49 AEDT] <pool-8-thread-1>  INFO: All reservation commit(s) finished.
     [2019-12-31 19:46:49 AEDT] <main>  INFO: 456.36 KB/s in 3 seconds
    

### Notarization process failures

If the when you check you find that the notarization process has failed. For example: 
    
    
    Date                      RequestUUID                          Status  Status Code Status Message   
    ------------------------- ------------------------------------ ------- ----------- ---------------- 
    2020-02-29 13:54:08 +0000 fa2e5d79-cba0-469d-bb27-859a8f081821 invalid 2           Package Invalid  
    

You can retrieve the URL for the notarization log file with the command: 
    
    
    xcrun notarytool log --apple-id "me@mysoftware.com" --password <app-specifc-password>  --team-id <your 10 character team ID>  session-ID
    

which should yield information similar to the following: 
    
    
    No errors getting notarization info.
    
              Date: 2020-02-29 13:54:08 +0000
              Hash: 1b08ea5cdf97eb6fed9b959a549d4b325eb5e90f16538139a29a83f75609d87b
        LogFileURL: https://osxapps-ssl.itunes.apple.com/itunes-assets/[...]
       RequestUUID: fa2e5d79-cba0-469d-bb27-859a8f081821
            Status: invalid
       Status Code: 2
    Status Message: Package Invalid
    

You can then cut and paste the LogFileURL into your web browser and retrieve the notarization process log file which will look something like this: 
    
    
    {
      "logFormatVersion": 1,
      "jobId": "fa2e5d79-cba0-469d-bb27-859a8f081821",
      "status": "Invalid",
      "statusSummary": "Archive contains critical validation errors",
      "statusCode": 4000,
      "archiveFilename": "test.dmg",
      "uploadDate": "2020-02-29T13:54:08Z",
      "sha256": "1b08ea5cdf97eb6fed9b959a549d4b325eb5e90f16538139a29a83f75609d87b",
      "ticketContents": null,
      "issues": [
        {
          "severity": "error",
          "code": null,
          "path": "test.dmg/MyApp.app/Contents/MacOS/MyApp",
          "message": "The executable does not have the hardened runtime enabled.",
          "docUrl": null,
          "architecture": "x86_64"
        }
      ]
    }
    

In the above failure case, it was because the executable had not been [hardened](<Hardened_runtime_for_macOS.md> "Hardened runtime for macOS"). 

It is also worthwhile mentioning that it is _always_ worth retrieving the notarization log file to check for warnings which are not yet errors so that you can fix them before they do become errors which prevent successful notarization. 

Here is an example of a notarization process log from a successful notarization: 
    
    
    {
      "logFormatVersion": 1,
      "jobId": "fa2e5d79-cba0-469d-bb27-859a8f081822",
      "status": "Accepted",
      "statusSummary": "Ready for distribution",
      "statusCode": 0,
      "archiveFilename": "test.dmg",
      "uploadDate": "2020-03-01T01:50:55Z",
      "sha256": "1945554c24da3bb1d587b7c9f968dcbb925b726f2f663b816301928c06593dd9",
      "ticketContents": [
        {
          "path": "test.dmg",
          "digestAlgorithm": "SHA-256",
          "cdhash": "1d4d033edbecc2baf7ff8ce0fa16691be990595f"
        },
        {
          "path": "test.dmg/MyApp.app",
          "digestAlgorithm": "SHA-256",
          "cdhash": "7f9d54e76f25807e2b425a09684aa8cb54dcd17f",
          "arch": "x86_64"
        },
        {
          "path": "test.dmg/MyApp.app/Contents/MacOS/MyApp",
          "digestAlgorithm": "SHA-256",
          "cdhash": "7f9d54e762c7907e2b425a09684aa8cb54dcd17f",
          "arch": "x86_64"
        },
        {
          "path": "test.dmg/MyApp.app/Contents/Resources/MyApp.help",
          "digestAlgorithm": "SHA-256",
          "cdhash": "b544a32a216578e7805ca93b968377cdd92f7ec6",
          "arch": "i386"
        },
        {
          "path": "test.dmg/MyApp.app/Contents/Resources/MyApp.help",
          "digestAlgorithm": "SHA-256",
          "cdhash": "b544a32a216578e7805ca93b968377cdd92f7ec6",
          "arch": "x86_64"
        },
        {
          "path": "test.dmg/MyApp.app/Contents/Resources/MyApp.help",
          "digestAlgorithm": "SHA-256",
          "cdhash": "b544a32a216578e7805ca93b968377cdd92f7ec6",
          "arch": "x86_64h"
        }
      ],
      "issues": null
    }
    

### Notarization check failures

If the notarization process succeeds, but when you check whether your application is properly notarized and it does not pass the notarization check successfully, you can use the `Console.app` to inspect the log files for information about the rejection (after trying to launch the blocked application). Be aware you should open the `Console.app` _before_ trying to open your blocked application, otherwise not all messages may be logged. You should choose your device in the left sidebar of the `Console.app` and check for the process **XprotectService** in the logs. 

## See also

  * [Apple Developer Certificates](<Apple_Developer_Certificates.md> "Apple Developer Certificates")
  * [Code Signing for macOS](<Code_Signing_for_macOS.md> "Code Signing for macOS")
  * [Hardened runtime for macOS](<Hardened_runtime_for_macOS.md> "Hardened runtime for macOS")
  * [Signing and Notarization FAQ](<Signing_and_Notarization_FAQ.md> "Signing and Notarization FAQ")



## External links

  * [Apple: Notarizing your app before distribution](<https://developer.apple.com/documentation/security/notarizing_your_app_before_distribution>)
  * [Apple: Notarizing macOS Software Before Distribution](<https://developer.apple.com/documentation/xcode/notarizing_macos_software_before_distribution>)
  * [Apple: Customizing the Notarization Workflow](<https://developer.apple.com/documentation/xcode/notarizing_macos_software_before_distribution/customizing_the_notarization_workflow>)
  * [Apple: Common notarization issues](<https://developer.apple.com/documentation/xcode/notarizing_macos_software_before_distribution/resolving_common_notarization_issues>)
  * [Apple: Using app-specific passwords](<https://support.apple.com/en-us/HT204397>)
  * [Notarize FPC](<https://github.com/neurolabusc/NotarizeFPC>) A script to notarize a command line utility and package it in an installer .pkg file.

---

_Source: [https://wiki.freepascal.org/Notarization_for_macOS_10.14.5%2B](https://web.archive.org/web/20241212103500/https://wiki.freepascal.org/Notarization_for_macOS_10.14.5%2B)_
