# Source Editor Maximizer

[![](https://wiki.freepascal.org/images/8/8d/se_maximized_osx.png)](</File:se_maximized_osx.png>)

[](</File:se_maximized_osx.png> "Enlarge")

Maximized Source Editor under OSX

[![](https://wiki.freepascal.org/images/7/7e/se_maximize_win.png)](</File:se_maximize_win.png>)

[](</File:se_maximize_win.png> "Enlarge")

Maximized Source Editor under Windows 10

**S** ource **E** ditor **MAX** imizer is Lazarus IDE plugin that forces Source Editor to be maximized under the IDE main bar. 

The plugin is designed to mimic Delphi 7 UI and prevent to key IDE windows from overlapping each other, without using any kind of docking. Docking is available these days for Lazarus IDE, thus you might not want to use the package at all. 

## Contents

  * 1 Source Code
  * 2 Author and License
  * 3 Supported Widgets
  * 4 To be done
  * 5 How to Use
  * 6 See Also



## Source Code

  * [semax Github](<https://github.com/skalogryz/semax>)
  * [Download as ZIP from Github](<https://github.com/skalogryz/semax/archive/master.zip>)



## Author and License

Dmitry 'skalogryz' Boyarintsev 

You're free to use this IDE Plugin and its sources in any way you find useful. 

## Supported Widgets

The code is written in cross-platform manner. But due to inconsistency in windows placement functions, across different platforms the results might not be as good as expected. Possible issues are not good enough placement of the maximized source editor. Editor might "jump" on maximize. 

The following operating systems have been verified and tested: 

  * Windows (tested on XP and Win10) - the best results.
  * Mac OS X (Carbon) - "jumps", but is placed correctly.



## To be done

  * check for multi-monitor system



## How to Use

  * select the package for installation and rebuild Lazarus.
  * any source editor window maximized should go under the main bar



## See Also

  * [Manual Docker](<Manual_Docker.md> "Manual Docker")

---

_Source: [https://wiki.freepascal.org/Source_Editor_Maximizer](https://web.archive.org/web/20201021103551/https://wiki.freepascal.org/Source_Editor_Maximizer)_
