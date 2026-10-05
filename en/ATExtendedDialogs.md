# ATExtendedDialogs

## Contents

  * 1 About
  * 2 ATFontDialog
    * 2.1 Properties
  * 3 Download



## About

ATExtendedDialogs is a package of units with extended dialogs. Currently only font dialog in the ATFontDialog unit. 

Author: Alexey Torgashin 

License: dual licensed, MPL 2.0 / LPGL. 

## ATFontDialog

ATFontDialog contains a form which mimics the good features from the GTK2/Win32 font dialogs. The form has many options (beginning with the Opt prefix so as not to mess up the existing LCL properties), you can see them in the demo program. Almost all dialog parts can be hidden (except the button panel). 

[![ATFontDialog.png](https://wiki.freepascal.org/images/5/5b/ATFontDialog.png)](</File:ATFontDialog.png>)

Mimics GTK2 feature: when user clicks font size bigger than about 20, preview panel height is increased (together with the form height). 

### Properties

  * OptFont: TFont - Chosen font.
  * OptPreviewText: string - String to show in Preview panel.
  * OptSizeMin: integer - Minimal size of font (if it's limited) for input field.
  * OptSizeMax: integer - Maximal size of font (if it's limited).
  * OptSizeLimited: boolean - Allow limit for font size.
  * OptShowNames: boolean - Show listbox with font names.
  * OptShowColor: boolean - Show color-box for font color.
  * OptShowStyles: boolean - Show listbox with font basic styles (bold, italic).
  * OptShowStylesEx: boolean - Show checkboxes for font extended styles (underline, crossed).
  * OptShowSizes: boolean - Show listbox with font sizes.
  * OptShowPreview: boolean - Show Preview panel.
  * OptShowApply: boolean - Show Apply button.
  * OptSelectName: boolean - Initially select font name in listbox of names.
  * OptSelectStyle: boolean - Initially select font style in listbox of styles.
  * OptSelectSize: boolean - Initially select font size in listbox of sizes.
  * OptSelectColor: boolean - Initially select font color in color-box.
  * OnClickApply: TNotifyEvent - Called on pressing Apply button.



## Download

GitHub: <https://github.com/Alexey-T/ATExtendedDialogs>

---

_Source: [https://wiki.freepascal.org/ATExtendedDialogs](https://web.archive.org/web/20250126120319/https://wiki.freepascal.org/ATExtendedDialogs)_
