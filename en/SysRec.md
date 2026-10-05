# SysRec

│ **English (en)** │  **[español (es)](</SysRec/es> "SysRec/es")** │    
****

## Contents

  * 1 About
  * 2 License
  * 3 Author
  * 4 Download
  * 5 Usage
  * 6 Description
  * 7 See also



### About

  * The application currently supports all Windows Operating Systems using the [VFW](<Glossary.md> "Glossary") [API](<Glossary.md> "Glossary") available on all 32 bit [Windows](<Glossary.md> "Glossary").
  * Supports Preview and Overlay modes with any capture device such as Cameras and TV Tuners, though it has been designed for use with a WebCam.
  * Allows silent movie capturing with any resolution and color depth, you can also chose a codec to record smaller files and a function has been added to allow grabbing single images, adding audio recording should not be a problem.
  * A caller application has been added to demonstrate message based Inter Process Comunication.
  * This application is designed with [Lazarus](<Glossary.md> "Glossary") but a few files and directives have been added to allow compilation in [Delphi](<Glossary.md> "Glossary")
  * The application uses the [Delphi-JEDI](<http://www.delphi-jedi.org>) [VFW](<Glossary.md> "Glossary") unit licensed under MPL, however you need some small modifications if you want to download it again, search for "FPC" in the current unit to see them. You can download the unit [here](<ftp://ftp.delphi-jedi.org/api/vfw.zip>).



### License

  * This application can be used under Lazarus modified LGPL and MPL, this allows making commercial projects that use this code but NO WARANTY, you are the only responsable if this code doesn't work or causes any problems to your computer.



### Author

[Razvan Adrian Bogdan](</User:Lightning> "User:Lightning")

### Download

The application can be found on the [Lazarus CCR Files page](<http://sourceforge.net/project/showfiles.php?group_id=92177&package_id=148359&release_id=331555>). 

### Usage

To use the application simply open the .lpi files from your File Browser or using the Open Project option from the Lazarus IDE the press Run, the Main form should display and if you have a Capture Driver installed you should be able to see some images, in the Status Bar you will see if your device supports Overlay or just Software Preview, in Overlay mode the application use less CPU and with more dynamic rendering.   
You should also see some buttons: Source, Format, Quality, Connect, Record/Stop.   
The first 3 buttons open various dialogs wich are dependant on your configuration.   
The Source button allows selecting and configuring a video source wich is capable of using a WDM driver.   
The Format button allows selecting the resolution and number of colors for the captured images.   
The Quality button allows selecting and configuring a codec, this is dependant on your installed codecs.   
The Connect button is a workaround for a Windows bug wich does not detect Overlay/Preview modes properly when changing the video source using Select.   
The Record button switches from Record to Stop when clicked and starts recording, the files are saved in the same directory and have the start and stop time in their name and the .avi extension.   


### Description

**If any of the links do not work ask MS why did they move the website again and why is it so sloooow :)**  
  
How does it work ?  


  * [VFW](<Glossary.md> "Glossary") is one of the simplest parts of the [Windows](<Glossary.md> "Glossary") [API](<Glossary.md> "Glossary"), this application simply creates a special window using [capCreateCaptureWindow](<http://msdn.microsoft.com/library/default.asp?url=/library/en-us/multimed/htm/_win32_capcreatecapturewindow.asp>), you can call various functions using the handle resulted by creating this window.
  * After creating the window you have to connect it to a valid driver, for newer devices this is the default WDM driver, use [capDriverConnect](<http://msdn.microsoft.com/library/default.asp?url=/library/en-us/multimed/htm/_win32_capdriverconnect.asp>) to connect the Capture window to a capture driver.
  * You will have to find if the driver supports hardware acceleration or **Overlay** , this is done by [capDriverGetCaps](<http://msdn.microsoft.com/library/default.asp?url=/library/en-us/multimed/htm/_win32_capdrivergetcaps.asp>), use the **fHasOverlay** member of the [TCapDriverCaps](<http://msdn.microsoft.com/library/default.asp?url=/library/en-us/multimed/htm/_win32_capdrivercaps_str.asp>) structure.
  * Next you have to decide if you want live preview from the capture device, to activate it use [capOverlay](<http://msdn.microsoft.com/library/default.asp?url=/library/en-us/multimed/htm/_win32_capoverlay.asp>) or [capPreview](<http://msdn.microsoft.com/library/default.asp?url=/library/en-us/multimed/htm/_win32_cappreview.asp>) depending on your driver capabilities, if you have overlay then set [capPreviewRate](<http://msdn.microsoft.com/library/default.asp?url=/library/en-us/multimed/htm/_win32_cappreviewrate.asp>) to 0 otherwise set it to a resonable value, you might also want the image to be stretched to the window size, to do this use [capPreviewScale](<http://msdn.microsoft.com/library/default.asp?url=/library/en-us/multimed/htm/_win32_cappreviewscale.asp>), to stop the preview use **capOverlay** or **capPreview** (depending on **fHasOverlay** member) and pass **false** as the second parameter.
  * To record you should use [capFileSetCaptureFile](<http://msdn.microsoft.com/library/default.asp?url=/library/en-us/multimed/htm/_win32_capfilesetcapturefile.asp>), [capCaptureSequence](<http://msdn.microsoft.com/library/default.asp?url=/library/en-us/multimed/htm/_win32_capcapturesequence.asp>) and [capFileSaveAs](<http://msdn.microsoft.com/library/default.asp?url=/library/en-us/multimed/htm/_win32_capfilesaveas.asp>) if you did not use **capFileSetCaptureFile** , to stop recording you should use [capCaptureStop](<http://msdn.microsoft.com/library/default.asp?url=/library/en-us/multimed/htm/_win32_capcapturestop.asp>).
  * To get a single frame use [capGrabFrameNoStop](<http://msdn.microsoft.com/library/default.asp?url=/library/en-us/multimed/htm/_win32_capgrabframenostop.asp>) and [capEditCopy](<http://msdn.microsoft.com/library/default.asp?url=/library/en-us/multimed/htm/_win32_capeditcopy.asp>) or use the internal **CapGrabFrame** wich captures one frame as a **TBitmap** from the clipboard so you can process/save it later.
  * You will have to also disconnect the driver, to do so use [capDriverDisconnect](<http://msdn.microsoft.com/library/default.asp?url=/library/en-us/multimed/htm/_win32_capdriverdisconnect.asp>).
  * Before you close the program you also need to free the window with [DestroyWindow](<http://msdn.microsoft.com/library/default.asp?url=/library/en-us/winui/winui/windowsuserinterface/windowing/windows/windowreference/windowfunctions/destroywindow.asp>)
  * How to use a hook and process messages from various system windows or dynamic messages obtained with [RegisterWindowMessage](<http://msdn.microsoft.com/library/default.asp?url=/library/en-us/winui/winui/windowsuserinterface/windowing/messagesandmessagequeues/messagesandmessagequeuesreference/messagesandmessagequeuesfunctions/registerwindowmessage.asp>).



## See also

  * [Video Playback Libraries](<Video_Playback_Libraries.md> "Video Playback Libraries")

---

_Source: [https://wiki.freepascal.org/SysRec](https://web.archive.org/web/20250323133604/https://wiki.freepascal.org/SysRec)_
