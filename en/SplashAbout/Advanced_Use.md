# SplashAbout/Advanced Use

* For installation, download, version and other information, see the [SplashAbout](<../SplashAbout.md> "SplashAbout") page



## Contents

  * 1 SplashAbout properties tester Application
  * 2 Properties
    * 2.1 Personalising the Splash screen
    * 2.2 Personalising the About screen
    * 2.3 Personalising the License Screen
    * 2.4 Personalising the Credits Screen
  * 3 How To...
    * 3.1 How to show a shaped Splash screen
      * 3.1.1 MaskType
        * 3.1.1.1 How to make a BitMapMaskMono image
        * 3.1.1.2 How to make a BitmapMaskColor image
    * 3.2 How to show a license button on the About screen
      * 3.2.1 LicenseFile
        * 3.2.1.1 Standard licences (GPL etc)
        * 3.2.1.2 User licenses
    * 3.3 How to show a Credits button on the About screen
      * 3.3.1 ShowCreditButton
      * 3.3.2 CreditLines
      * 3.3.3 CreditTextFileName
    * 3.4 How to show a 'Powered by Lazarus' pre-splash graphic
    * 3.5 How to show different text in the Splash and About screens
      * 3.5.1 Description and ShowDescription
      * 3.5.2 TitleStyle and UserTitle
      * 3.5.3 Organisation
      * 3.5.4 FormSplashTextAlign and FormAboutTextAlign
      * 3.5.5 LicenseFile
      * 3.5.6 Application Version info
    * 3.6 How to show a graphic in the Splash and About screens
      * 3.6.1 BitmapBackground
      * 3.6.2 FormSplashWidth/Height and FormAboutWidth/Height
    * 3.7 How to set a custom Icon
      * 3.7.1 TitleStyle
      * 3.7.2 Icon
    * 3.8 How (or why) to avoid a bigger executable by using external graphic files
      * 3.8.1 Graphics
      * 3.8.2 ExternalFile<propertyname>
      * 3.8.3 ExternalFileOptions
    * 3.9 How to use a large background image
      * 3.9.1 ResizeMode
    * 3.10 How to deploy an application that uses the SplashAbout component
  * 4 Is it OK to...
    * 4.1 Is it OK to use SplashAbout in a commercial application?
    * 4.2 Is it OK to alter/change the code for my own version of SplashAbout?
  * 5 See Also



## SplashAbout properties tester Application

  * The distribution archive contains an example application with all the resources needed to experiment with advanced properties - pre-configured with Resource and External graphics, also sample license.txt and Credits
  * Click the [Help] button to view the readme



[![splashabout properties tester144.png](https://wiki.freepascal.org/images/c/c1/splashabout_properties_tester144.png)](</File:splashabout_properties_tester144.png>)

* * *

## Properties

When dropped onto a new application mainform, the property defaults will show a Splash Screen and About window that look something like this: 

[![splashabout default splash.png](https://wiki.freepascal.org/images/e/e6/splashabout_default_splash.png)](</File:splashabout_default_splash.png>)

[![splashabout default about.png](https://wiki.freepascal.org/images/7/76/splashabout_default_about.png)](</File:splashabout_default_about.png>)

  * A normal (borderless rectangle) Splash screen displayed for 2 seconds (ShowSplash method) 
    * It will use the Application default icon (default=Lazarus icon)
    * Title will be the Application name (default='project1')
    * Colour will be clSkyBlue
    * No background image
    * No description
    * Window size = 320px x 240px
    * LCL and FPC build text
    * Compilation Date/Time text
  * The About screen will show (ShowAbout method) 
    * No License or Credits buttons
    * The same text as the default Splash Screen



The default screens are probably OK for a quick one-off app that is not for commercial use. It shows useful information for a developer. 

However; if you think they look a bit _uninspiring_ , the SplashAbout component is capable of considerable customisation from its default settings, and there are many properties available to help you achieve _a unique and very inspiring result_. 

Here are the SplashAbout properties you can change for each kind of customisation: 

### Personalising the Splash screen

  * Properties that affect the Splash Screen: 
    * The Application Title,Icon and VersionInfo (set in Project/Options)
    * BackGroundColor
    * BitMapBackground
    * BitmapMaskColor
    * BitmapMaskMono
    * DelaySeconds
    * Description
    * Font
    * FormSplashHeight
    * FormSplashTextAlign
    * FormSplashWidth
    * Graphics
    * Icon
    * MaskType
    * Organisation
    * ShowDecription
    * ShowPoweredBy
    * TitleStyle
    * UserTitle



### Personalising the About screen

  * Properties that affect the About Screen: 
    * The Application Title,Icon and VersionInfo (set in Project/Options)
    * Author
    * BackGroundColor
    * CreditLines
    * CreditsTextFileName
    * Description
    * Font
    * FormAboutHeight
    * FormAboutTextAlign
    * FormAboutWidth
    * Graphics
    * Icon
    * LicenseFile
    * Orgnisation
    * ShowCreditButton
    * ShowDecription
    * LicenseFile
    * SupportContact
    * TitleStyle
    * UserTitle



### Personalising the License Screen

  * Properties that affect the License Screen: 
    * Author
    * LicenseFile
    * Organisation
    * SupportContact



### Personalising the Credits Screen

  * Properties that affect the Credits Screen: 
    * CreditLines
    * CreditsTextFileName
    * ShowCreditButton



* * *

## How To...

### How to show a shaped Splash screen

Properties to set: 

  * BitmapMaskColor (Graphics=saResources)
  * BitmapMaskMono (Graphics=saResources)
  * MaskType
  * Graphics



#### MaskType

  * Default=saNoMask



The simple thing to do is to choose one of the built-in masks (saRoundedRect, saBigCog, saBigFlower and saBigSplash) If you choose saUserMask then you need to make a 2-colour mask graphic in the .BMP format. You can optionally also make a corresponding BitMapMaskColor image. Note: There are some example images deployed in the /exampleapp folder of the distribution archive 

##### How to make a BitMapMaskMono image

  * Start with a black (RGB:0,0,0)background
  * Make your contigious shape using a white (RGB:255,255,255) shape
  * Reduce the image to a 2-color palette
  * Save the file as a Windows Bitmap (.BMP) and use it to set the BitmapMaskColor property 
    * It should NOT be run-length encoded (RLE)



If you want a shaped colour splash screen then you need to make a corresponding BitMapColor image 

##### How to make a BitmapMaskColor image

You would normally do this before making the BitmapMaskMono image 

  * Make with an colour image that has a white (RGB:255,255,255) background 
    * You can use a selection tool to isolate the background then paint it white in a graphics application
  * Save the file as a .JPG and use it to set the BitmapMaskColor property
  * To make a corresponding BitmapMaskColor image: 
    * Invert the selection, and paint it black (RGB:0.0.0)
    * Reduce the image to a 2-color palette
    * Save the new image as a non-RLE Windows Bitmap file (.BMP) and use it to set the BitmapMaskMono property



* * *

### How to show a license button on the About screen

Properties to set: 

  * LicenseFile
  * Optional: 
    * Author
    * Organisation
    * SupportContact



#### LicenseFile

  * Default=saNone



##### Standard licences (GPL etc)

  * If you set LicenseFile to one of the standard license type (saGPL,saLGPL,saMIT or saModifiedGPL) then the License button will appear on the About dialog
  * When clicked, the License button shows a modal window with the appropriate license text
  * The text is stored as a resource, so no need for external files
  * The text of the standard license can be modified by setting the Author Organisation and SupportContact properties



##### User licenses

  * If you set LicenseFile to saUserFile then the License button will appear on the About dialog
  * When clicked, the License button shows a modal window with text
  * The text will come from a file you create yourself which must be called 'license.txt' and deployed in the same folder as your executable (see exampleapp)
  * You can use the following tokens in 'license.txt' 
    * All instances of <name of author> are replaced with the Author property
    * All instances of <contact> are replaced with the SupportContact property
    * All instances of <copyright holders> are replaced with the Organisation property
    * All instances of <year> are replaced with the compilation date



* * *

### How to show a Credits button on the About screen

Properties to set: 

  * ShowCreditButton
  * CreditLines
  * Optional
  * Font (inherited)
  * BackgroundColor (inherited) 
    * CreditTextFileName



#### ShowCreditButton

  * Default-False
  * If set to True, then the Credits button will appear on the About dialog
  * When clicked it will show either: 
    * The contents of CreditLines
    * or the contents of a text file (default name='credits.txt') deployed in the same folder as your executable
  * If both contain no text, then default text is shown
  * If both contain text, then 'credits'txt' will be shown in preference
  * The text will scroll upwards, and repeat until the user clicks the close button
  * The text will be centre-justified



#### CreditLines

  * default=(empty)
  * Use the Object inspector to write your credits text.



#### CreditTextFileName

  * This can be set so that is doesn't clash with any existing deployed text file



* * *

### How to show a 'Powered by Lazarus' pre-splash graphic

Properties to set: 

  * ShowPoweredBy



That's it! 

* * *

### How to show different text in the Splash and About screens

Properties to set: 

  * ShowDescription
  * Description
  * TitleStyle
  * UserTitle
  * Organisation
  * FormSplashTextAlign
  * FormAboutTextAlign
  * LicenseFile
  * Application VersionInfo



##### Description and ShowDescription

  * Use the Object Inspector to write a short description of your application and set ShowDescription=True
  * The Description text will show on both the Splash and About dialogs
  * The Description text will replace the Build and Platform information on both dialogs



##### TitleStyle and UserTitle

  * If TitleStyle=saUserTitle then the UserTitle text will appear on the Splash and About dialogs
  * If TitleStyle=saForm the Caption text of your mainform will be used
  * If TitleStyle=saApplication, the Application title (set in Project/Options) will be used 
    * The text will also appear as the caption text of all dialogs



##### Organisation

  * If set, then the text will appear below the UserTitle on the Splash and About dialogs



##### FormSplashTextAlign and FormAboutTextAlign

  * You might want to set these if you have a background image that has a blank area for text on one side



##### LicenseFile

  * If this is anything other than LicenseFile=saNone, then the text 'Released under <licensetype> license' will appear at the bottom of the Splash and About dialogs where <licensetype> is 
    * LicenseFile=saGPL: 'GPL'
    * LicenseFile=saLGPL: 'LGPL'
    * LicenseFile=saMIT: 'MIT'
    * LicenseFile=saModifiedGPL: 'Modified GPL'
    * LicenseFile=saUserFile: 'proprietry'



##### Application Version info

  * If you enable Version Information in the Project/Options then it will appear in both Splash and About dialogs



* * *

### How to show a graphic in the Splash and About screens

Properties to set: 

  * BitmapBackground
  * Optional: 
    * FormSplashWidth
    * FormSplashHeight
    * FormAboutWidth
    * FormAboutHeight



##### BitmapBackground

  * Use the Object Inspector to load a graphic file (most types supported)
  * The graphic will override the BackgroundColor setting
  * The graphic will stretch to fit the Splash and About dialogs



##### FormSplashWidth/Height and FormAboutWidth/Height

  * You can set these to any reasonable values and the controls and background will stretch/move as necessary



* * *

### How to set a custom Icon

Properties to set: 

  * Icon
  * TitleStyle



##### TitleStyle

  * To show a custom Icon then TitleStyle must be set to saUserTitle



##### Icon

  * Use the Object Inspector to set an appropriate icon 
    * It will be shown on all dialogs
  * If TitleStyle is set to saForm then the mainform's icon will always be shown
  * If TitleStyle is set to saApplication then the application's always icon will be shown
  * If no custom icons have been set for either, the the default Lazarus icon is displayed



* * *

### How (or why) to avoid a bigger executable by using external graphic files

SplashAbout has been designed from the start to be very lightweight. Splash and About dialogs are hardly the most important part of an application so the component is designed to add as little as possible in both filesize and effort. Lazarus/fpc are extremely efficient in storing resources in executables, however adding SplashAbout with embedded graphics will make your executable bigger. If you choose to set a huge Splash and/or About dialog with correspondingly huge graphics, maybe it will be worth 'going external' 

Here are the stats: 

  * Typical Win64 application using SplashAbout with every graphics property set and stored in the executable = 5MB
  * Typical Win64 application using SplashAbout with all graphics deployed externally=3.7MB



If this difference is important to your particular deployment, then read on... 

Properties to set: 

  * Graphics
  * ExternalFileBackgroundImage
  * ExternalFileIcon
  * ExternalFileMaskMonoImage
  * ExternalFileMaskColorImage
  * ExternalFileOptions



##### Graphics

  * Default=saUseResources
  * If Graphics=saExternalFiles then the ExternalFile<propertyname> properties will be used



##### ExternalFile<propertyname>

  * All ExternalFile properties accept a simple filename as text
  * All external files MUST be in the executable's deployed folder
  * SplashAbout will test at runtime to see if the appropriate file is present, and will display an error message if it is absent or in the wrong folder



##### ExternalFileOptions

  * Setting these will allow you to use some External files and some resources together



* * *

### How to use a large background image

  * By default, ShowSplash respects the dimensions you set in the Splash and About Formwidth and Formheight properties. It will stretch/compress any BackgroundImage to fit the window which is preferred behaviour. You may, however want to resize the Splash and About windows to fit your background graphic, or even avoid all resizing altogether.
  * Note: ResizeMode has no effect on shaped Splash screens - they are always resized to fit the mask.



Properties to set: 

  * ResizeMode
  * BackgroundImage



##### ResizeMode

  * Default=saResizeGraphic
  * If ResizeMode=saResizeWindow, then SplashAbout will resize the Splash and About windows to the loaded graphic dimensions.
  * If ResizeMode=saNoResize, then the Splash and About width/height properties will be respected, and the BackgroundImage will display without stretching or compressing.



* * *

### How to deploy an application that uses the SplashAbout component

  * If Graphics=saResources then no external files are necessary
  * If ShowCreditButton=True, CreditLines is empty then you will need to deploy a 'credits.txt' textfile (CreditsTextfileName can set the filename) in the same folder as your deployed executable. 
    * This could be a good option if you want to update the Credits screen without recompiling and re-deploying the executable



* * *

## Is it OK to...

Some information about licensing, updating the source etc. 

### Is it OK to use SplashAbout in a commercial application?

  * Splashabout is released under the LGPL licence, so the short answer is 'yes' I would hope for a mention in the Credits screen :)
  * Here is the relevant text of the LGPL license:


    
    
     This library is free software; you can redistribute it and/or modify it
     under the terms of the GNU Library General Public License as published by
     the Free Software Foundation; either version 2 of the License, or (at your
     option) any later version.
     This program is distributed in the hope that it will be useful, but WITHOUT
     ANY WARRANTY; without even the implied warranty of MERCHANTABILITY or
     FITNESS FOR A PARTICULAR PURPOSE. See the GNU Library General Public License
     for more details.
    

### Is it OK to alter/change the code for my own version of SplashAbout?

  * Of course! That's what open source software is all about
  * [Email me](<mailto:minesadorada@charcodelvalle.com%7C>) with your updates as a courtesy and perhaps I could incorporate them here
  * In the spirit of open source, please retain acknowledgement of my source code



  
Page written by [Torrenter](</index.php?title=User:Torrenter&action=edit&redlink=1> "User:Torrenter \(page does not exist\)") 20:06, 2 June 2014 (CEST) 

* * *

## See Also

[SplashAbout component main page](<../SplashAbout.md> "SplashAbout")

---

_Source: [https://wiki.freepascal.org/SplashAbout/Advanced_Use](https://web.archive.org/web/20250417143304/https://wiki.freepascal.org/SplashAbout/Advanced_Use)_
