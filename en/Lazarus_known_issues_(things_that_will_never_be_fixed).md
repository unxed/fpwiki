# Lazarus known issues (things that will never be fixed)

│ **English (en)** │  **[日本語 (ja)](</Lazarus_known_issues_\(things_that_will_never_be_fixed\)/ja> "Lazarus known issues \(things that will never be fixed\)/ja")** │    
****

## Contents

  * 1 Win32/Win64 issues
    * 1.1 TComboBox.Height
    * 1.2 TButton's and TBitBtn's color cannot be changed
    * 1.3 TProgressBar.Height
    * 1.4 TTrayIcon.Hint
  * 2 GTK 1 issues
    * 2.1 Inconsistent internationalization support
    * 2.2 TTabSheet.TabVisible property
    * 2.3 TComboBox.DropDownCount property is ignored
    * 2.4 TStatusBar Panels
  * 3 GTK 2 issues
    * 3.1 TComboBox.DropDownCount property is ignored
    * 3.2 TStatusBar Panels
  * 4 Carbon issues
    * 4.1 No 64-bits support
  * 5 QT issues
    * 5.1 Qt license
    * 5.2 TOpenDialog/TSaveDialog filter limitations
  * 6 Win CE issues



## Win32/Win64 issues

### TComboBox.Height

It's impossible to set an arbitrary height to a TComboBox under Windows. Use AutoSize to make sure the size is good on all platforms. (Although [TComboBox](<TComboBox.md> "TComboBox").Height can be changed indirectly by changing TComboBox.Font size) 

### [TButton](<TButton.md> "TButton")'s and [TBitBtn](<TBitBtn.md> "TBitBtn")'s color cannot be changed

It's impossible to implement without making them owner drawn. 

### [TProgressBar](<TProgressBar.md> "TProgressBar").Height

The Height of a [TProgressBar](<TProgressBar.md> "TProgressBar") cannot be smaller than 10 if a Windows Theme is used. 

Associated bug report: <http://bugs.freepascal.org/view.php?id=10626>

MSDN Docs: <http://msdn2.microsoft.com/en-us/library/bb760816(VS.85).aspx>

### [TTrayIcon](<TTrayIcon.md> "TTrayIcon").Hint

[TTrayIcon](<TTrayIcon.md> "TTrayIcon").Hint may have a maximum length of 127 WideChars (will usually be more then 127 Bytes in a UTF-8 string, but may be less too, depending on the string). It's a Windows limitation, which can't be worked around. 

## GTK 1 issues

### Inconsistent internationalization support

GTK 1 always uses the default locale encoding to input and draw text and therefore adequate unicode support cannot be implemented for it. 

### TTabSheet.TabVisible property

[TTabSheet](</index.php?title=TTabSheet&action=edit&redlink=1> "TTabSheet \(page does not exist\)") under gtk1 can either be fully hidden (with Tab) or fully visible (with Tab). There is no possibility to show TabSheet without it tab. Look at gtk1 sources (gtknotebook.c) for details. 

### [TComboBox](<TComboBox.md> "TComboBox").DropDownCount property is ignored

What I consider a limitation in GTK1 & GTK2, but apparently it was a GTK design decision. For more details, see the following Mantis bug report: <http://bugs.freepascal.org/view.php?id=1933>

### [TStatusBar](<TStatusBar.md> "TStatusBar") Panels

The TStatusPanel.Bevel and TStatusPanel.Alignment properties are ignored by all platforms except Windows. Maybe Qt supports them too, but is not confirmed. See the following message thread for details. <http://lists.lazarus.freepascal.org/pipermail/lazarus/2010-January/048399.html>

## GTK 2 issues

### [TComboBox](<TComboBox.md> "TComboBox").DropDownCount property is ignored

What I consider a limitation in GTK1 & GTK2, but apparently it was a GTK design decision. For more details, see the following Mantis bug report: <http://bugs.freepascal.org/view.php?id=1933>

### [TStatusBar](<TStatusBar.md> "TStatusBar") Panels

The [TStatusPanel](</index.php?title=TStatusPanel&action=edit&redlink=1> "TStatusPanel \(page does not exist\)").Bevel and TStatusPanel.Alignment properties are ignored by all platforms except Windows. Maybe Qt supports them too, but is not confirmed. See the following message thread for details. <http://lists.lazarus.freepascal.org/pipermail/lazarus/2010-January/048399.html>

## Carbon issues

Go to [Carbon interface internals#Compatibility issues](<Carbon_interface_internals.md> "Carbon interface internals"). 

### No 64-bits support

Carbon cannot be used to write 64-bits applications. Apple decided not to release a 64-bits version of Carbon. The migration path is writing a new interface in Cocoa. 

## QT issues

### Qt license

  1. Qt up to and including version 4.4 could not be used to produce proprietary applications unless a Qt license is bought.
  2. Qt starting with 4.5 is now also published under the LGPL license, in addition to the commercial and GPL licenses. So, if you can comply with the LGPL, you can now avoid the commercial license.



### TOpenDialog/TSaveDialog filter limitations

The display filter for [TOpenDialog](<TOpenDialog.md> "TOpenDialog")/[TSaveDialog](<TSaveDialog.md> "TSaveDialog") will always include the filtered extensions in parentheses. Qt always adds parentheses with the filtered extensions, so if this is already present, the interface will try to remove them. For more information see TQtWSFileDialog.GetQtFilterString on the Qt interface. 

## Win CE issues

---

_Source: [https://wiki.freepascal.org/Lazarus_known_issues_(things_that_will_never_be_fixed)](https://web.archive.org/web/20250301000000/https://wiki.freepascal.org/Lazarus_known_issues_(things_that_will_never_be_fixed))_
