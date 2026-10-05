# macOS extensions

[![macOSlogo.png](https://wiki.freepascal.org/images/1/15/macOSlogo.png)](</File:macOSlogo.png>)

This article applies to [macOS](</Category:macOS> "Category:macOS") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

│ **English (en)** │ 

The package is intended to provide additional macOS **Cocoa** specific features. 

**Note** : It's expected that those features will not work on any other system or as well as for deprecated **Carbon** API on macOS. However, it's expected that a project using the package would still succeffully compile and run on any other target. 

## Contents

  * 1 General Info
  * 2 Extensions
    * 2.1 MacOSFormProp
      * 2.1.1 Appearance
      * 2.1.2 DocEdited
    * 2.2 MacOSUIProc
      * 2.2.1 PromptUserSheet
  * 3 To be Implemented
  * 4 See Also



## General Info

Author: [Dmitry 'skalogryz' Boyarintsev](</User:Skalogryz> "User:Skalogryz")

License: [modified](<http://svn.freepascal.org/svn/lazarus/trunk/COPYING.modifiedLGPL>) [LGPL](<http://svn.freepascal.org/svn/lazarus/trunk/COPYING.LGPL>) (same as the FPC RTL and the Lazarus LCL). You can contact the author if the modified LGPL doesn't work with your project licensing. 

Repository: <https://github.com/skalogryz/macosext>

## Extensions

After installation a new "macOS" tab is added to the components palette. All additional components are available in this tab: 

[![macosext mainbar.PNG](https://wiki.freepascal.org/images/4/42/macosext_mainbar.PNG)](</File:macosext_mainbar.PNG>)

### MacOSFormProp

The component is introduced to control MacOS NSWindow specific attributes and features, not available via LCL API. 

#### Appearance

[![macosformprop apppropeditor.PNG](https://wiki.freepascal.org/images/7/75/macosformprop_apppropeditor.PNG)](</File:macosformprop_apppropeditor.PNG>)

[NSAppearance](<https://developer.apple.com/documentation/appkit/nsappearance?language=objc>) class was introduced in macOS 10.9 for the first time. It controls window appearance options. The fact that it's per window configuration makes it different from Themes - that's system wide configuration. 

**Appearance** property of TmacOSFormProp component is used to adjust the appearance. 

Apple suggests that there are 3 appearance styles currently available in the system: 
    
    
    * NSAppearanceNameAqua
    * NSAppearanceNameVibrantDark
    * NSAppearanceNameVibrantLight
    

The property itself is a string. It is not restricted to these 3 options only. It's **case-sensitive** name of the appearance [as provided by macOS](<https://developer.apple.com/documentation/appkit/nsappearance/standard_appearances?language=objc>). 

The value of the property can be changed in design time as well as run-time. As soon as the value is changed, it's expected that the appearance of the form, is changed. 

**Note:** if the form that's hosts TmacOSFormProp is embedded into another form, then the behavior is not defined. However, it's expected the parent form appearance would change. 

Using TmacOSFormProp is not desired on any component other than form. I.e. it's possible to use it on a frame, but in that case, the appearance of the hosting would be changed. 

In case multiple TmacOSFormProp are used, the later action would be used as preference. Multiple TmacOSFormProp are not desired for use, and might forbidden in future versions. 

  
The implementation is checking for the presence of NSAppearance class dynamically. Thus, if the code is launched on an earlier version of macOS, it would still be able to run, even though will not do anything. 

This is how the default **Aqua** style might look like: 

[![macosformprop aqua.PNG](https://wiki.freepascal.org/images/c/cc/macosformprop_aqua.PNG)](</File:macosformprop_aqua.PNG>)

This is how the **Dark** style might look like: 

[![macosformprop dark.PNG](https://wiki.freepascal.org/images/3/39/macosformprop_dark.PNG)](</File:macosformprop_dark.PNG>)

(Note that some controls are not affected by Appearance - TMemo. This is macOS default behavior and expected result) 

The actual style look as well style availability depends on a certain macOS version, and might change overtime at Apple's decision 

You can refer to **appearance** sample application. 

#### DocEdited

The property indicates if the window should reflect the document as "edited" (true) or not (false). 

In systems other than macOS, a change in a document is typically indicated by adding asterisk character to the end of the window caption. 

macOS provides a special UI behavior in this case. 

[![macosformprop editted.png](https://wiki.freepascal.org/images/4/42/macosformprop_editted.png)](</File:macosformprop_editted.png>)

### MacOSUIProc

The unit provides additional macOS UI utilities. 

#### PromptUserSheet

The function shows a simple user dialog as a sheet (sheets are modal to a window, not to the entire application). Does nothing on any other platform. (todo - it's possible to implement it as modal forms). Parameters are similar (while not identical) to PromptUser function: 

  * DialogCaption: string
  * DialogMessage: string
  * DialogType - type of the dialog. idDialogXXX (declared at LCLType)
  * Buttons - array of button types that should be shown (refer to idButtonXXX list at LCLType)
  * DefaultIndex - default button index
  * AParentForm: TCustomForm - a form where the sheet should be shown



## To be Implemented

  * NSToolbar support - TToolbar doesn't correspond to macOS UI guidelines



## See Also

  * [Cocoa Interface](<Cocoa_Interface.md> "Cocoa Interface") \- LCL implementation of cocoa widgetset
  * [NSAppearance](<https://developer.apple.com/documentation/appkit/nsappearance?language=objc>) \- NSAppearance class reference in Apple documentation

---

_Source: [https://wiki.freepascal.org/macOS_extensions](https://web.archive.org/web/20240423015342/https://wiki.freepascal.org/macOS_extensions)_
