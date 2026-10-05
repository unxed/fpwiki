# WebView4Delphi

[![Windows logo - 2012.svg](https://upload.wikimedia.org/wikipedia/commons/thumb/5/5f/Windows_logo_-_2012.svg/60px-Windows_logo_-_2012.svg.png)](</File:Windows_logo_-_2012.svg>)

This article applies to [Windows](</Category:Windows> "Category:Windows") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

## Contents

  * 1 About
  * 2 Author
  * 3 Licence
  * 4 Requirements
  * 5 Demos



## About

[WebView4Delphi](<https://github.com/salvadordf/WebView4Delphi>) allows to embed Chromium-based web browsers in your Delphi or Lazarus applications using the WebView2 runtime. It uses many of the tricks from CEF4Delphi and you will notice many similarities if you used it. There are a few things pending like the "windowless mode". 

## Author

Author: Salvador Díaz Fau 

## Licence

License: MIT 

## Requirements

If you have Windows 11 then you already have the "evergreen" version of the Microsoft Edge WebView2 Runtime installed in your computer but older Windows versions need to install it. Download it [from here](<https://developer.microsoft.com/en-us/microsoft-edge/webview2/#download-section>). 

The WebView4Delphi demos are configured to use the "evergreen" version. 

Read the license carefully and pay special attention to the 3.a, 3.b, 9.a and 9.b points because your users might not like what Microsoft is doing. 

## Demos

WebView4Delphi loads the WebView2Loader.dll library found inside the Microsoft.Web.WebView2 NuGet package version 1.0.1054.31 but author already extracted that DLL and copied it into the bin32 and bin64 directories, where the demo executables are automatically created.

---

_Source: [https://wiki.freepascal.org/WebView4Delphi](https://web.archive.org/web/20260111103433/https://wiki.freepascal.org/WebView4Delphi)_
