# Win32MenuStyler

[![Windows logo - 2012.svg](https://upload.wikimedia.org/wikipedia/commons/thumb/5/5f/Windows_logo_-_2012.svg/50px-Windows_logo_-_2012.svg.png)](</File:Windows_logo_-_2012.svg>)

This article applies to [Windows](</Category:Windows> "Category:Windows") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

## Contents

  * 1 About
  * 2 Detailed info
  * 3 Usage
  * 4 Download



# About

This is unit win32menustyler, which helps to theme TMainMenu/TPopupMenu for Lazarus Windows apps. Sometimes app has dark theme, so it's needed to make TMainMenu also dark. 

[![win32menustyler .png](https://wiki.freepascal.org/images/8/82/win32menustyler_.png)](</File:win32menustyler_.png>)

Author: Alexey Torgashin 

License: MPL 2.0 or LGPL 

# Detailed info

What it paints for menu items: 

  * background, caption, also for disabled state
  * checked state, also for radio-items
  * separator line
  * shortcut text right-aligned
  * sub-menu arrow
  * icons from ImageList
  * icons from menuitem.Bitmap
  * underlines for accelerators, and only when OS requires it (not painted until menu is activated)



What is not supported: 

  * menuitem.SubMenuImages
  * menuitem.RightJustify
  * white frame is painted for PopupMenus (cannot find a way to fill it, even if I call MenuStyler.ApplyBackColor in OnPopup)
  * horizontal white line is painted under menu bar (non-client area, must [handle WM_NCPAINT somehow](<https://stackoverflow.com/questions/57177310/how-to-paint-over-white-line-between-menu-bar-and-client-area-of-window>))



About other OS: GitHub repo has cross-platform "lazmenustyler" unit, but it don't give any effect on gtk2 demo app. And it should not work on macOS, macOS has very special theming and menu is located on the screen top. 

# Usage

  * add win32menustyler to "uses" section
  * in form's OnShow, call: 
    * MenuStyler.ApplyToForm(Self) to theme MainMenu
    * MenuStyler.ApplyToMenu() for all needed PopupMenus
  * when needed to apply different colors later: 
    * change them in global theme var
    * call MenuStyler.ApplyToForm(Self)
    * not needed to call again MenuStyler.ApplyToMenu() for PopupMenus
  * to cancel theming, call MenuStyler.ResetForm() and MenuStyler.ResetMenu()



Unit gives global var to change all theming details: 
    
    
    type
      TWin32MenuStylerTheme = record
        ColorBk: TColor;
        ColorBkSelected: TColor;
        ColorBkSelectedDisabled: TColor; //used only if <>clNone
        ColorSelBorder: TColor; //used only if <>clNone
        ColorFont: TColor;
        ColorFontSelected: TColor; //used only if <>clNone
        ColorFontDisabled: TColor;
        ColorFontShortcut: TColor;
        CharCheckmark: WideChar;
        CharRadiomark: WideChar;
        CharSubmenu: WideChar;
        FontName: string;
        FontSize: integer;
        //indents in percents of average char width
        IndentMinPercents: integer; //indent from edges to separator line
        IndentBigPercents: integer; //indent from left edge to caption
        IndentIconPercents: integer; //indents around the icon
        IndentRightPercents: integer; //indent from right edge to end of shortcut text
        IndentSubmenuArrowPercents: integer; //indent from right edge to submenu '>' char
      end;
    
    var
      MenuStylerTheme: TWin32MenuStylerTheme;
    

# Download

GitHub: <https://github.com/Alexey-T/Win32MenuStyler>

---

_Source: [https://wiki.freepascal.org/Win32MenuStyler](https://web.archive.org/web/20250126131355/https://wiki.freepascal.org/Win32MenuStyler)_
