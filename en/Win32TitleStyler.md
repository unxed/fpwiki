# Win32TitleStyler

## About

This is unit, it gives the procedure to make the form's title - dark, using Win32 API. It works on Windows 10 and 11+ (it doesn't do anything on older Windows, and it doesn't work on some older builds of Windows 10). 

[Download link on GitHub](<https://github.com/Alexey-T/CudaText/blob/master/comp/win32titlestyler.pas>)

Screenshot of CudaText with applied dark color. The TMainMenu color was changed by [Win32MenuStyler](<Win32MenuStyler.md> "Win32MenuStyler"). 

[![cudatext-dark-title.png](https://wiki.freepascal.org/images/d/d9/cudatext-dark-title.png)](</File:cudatext-dark-title.png>)

## Usage
    
    
     ApplyFormDarkTitle(AForm: TForm; ADarkMode: bool; AForceApply: bool);
    

  * Parameter ADarkMode: set to True for dark title, set to False for default title (white on Windows 10).
  * Parameter AForceApply: value True: component will apply the change harder, it will change the form size by 1 pixel up, then by 1 pixel down, to force the applying. With value False, CudaText cannot make the title dark - after restoring from the full-screen mode.

---

_Source: [https://wiki.freepascal.org/Win32TitleStyler](https://web.archive.org/web/20250126124245/https://wiki.freepascal.org/Win32TitleStyler)_
