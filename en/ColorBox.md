# ColorBox

│ **[Deutsch (de)](</ColorBox/de> "ColorBox/de")** │  **English (en)** │  **[français (fr)](</ColorBox/fr> "ColorBox/fr")** │    
****

## Contents

  * 1 About
  * 2 Author
  * 3 License
  * 4 Download
  * 5 Change Log
  * 6 Dependencies / System Requirements
  * 7 Notes
  * 8 Installation
  * 9 Creation at runtime
  * 10 See also



### About

ColorBox is a component that lets you select any predefined color with preview. There are two predefined palettes available: 

  * cpDefault (some frequently used named colors)
  * cpFull (all named colors)



This component was designed for cross-platform applications. 

### Author

[Darius Blaszijk](</index.php?title=User:Dblaszijk&action=edit&redlink=1> "User:Dblaszijk \(page does not exist\)")

### License

[LGPL](<http://www.opensource.org/licenses/lgpl-license.php>)

### Download

[![Warning-icon.png](https://wiki.freepascal.org/images/b/b2/Warning-icon.png)](</File:Warning-icon.png>)

**Warning:** This component is now part of Lazarus/the LCL; the download is only of historical interest.

The download contains the component, and a patch to make use of the cpFull option. 

The download can be found on the [Lazarus CCR Files page](<http://sourceforge.net/project/showfiles.php?group_id=92177>). 

### Change Log

  * Version 1.0 2005/05/16



### Dependencies / System Requirements

  * None



### Notes

Status: Beta 

Issues: Tested on Windows. Needs testing on Linux. 

### Installation

[![Warning-icon.png](https://wiki.freepascal.org/images/b/b2/Warning-icon.png)](</File:Warning-icon.png>)

**Warning:** ColorBox is currently included in Lazarus; the instructions below only apply for manual installs

  * Apply the patch colorbox.diff
  * Install the component or create it at runtime



### Creation at runtime

To create the component at runtime use the following code : 
    
    
    Uses ...ColorBox...
    
    procedure TForm1.Form1Create(Sender: TObject);
    begin
      cbColorBox := TColorBox.Create(Self);
      cbColorBox.Parent := Self;
      cbColorBox.Left := 100;
      cbColorBox.Top := 100;
      cbColorBox.Palette := cpFull;
    end;
    

Make sure you don't forget to declare a global variable cbColorBox. 

### See also

[TColorBox documentation](<https://lazarus-ccr.sourceforge.io/docs/lcl/colorbox/tcolorbox.html>)

---

_Source: [https://wiki.freepascal.org/ColorBox](https://web.archive.org/web/20240308084919/https://wiki.freepascal.org/ColorBox)_
