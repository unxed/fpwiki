# Terminal & Fonts

This is my initial trial on this topic. Some statements might be incomplete and even wrong. Probably, you may have to do your own research on this topic to resolve your problem. In that case please come back and correct/append, what you have found. 

The [Free Vision](<Free_Vision.md> "Free Vision") (fv) framework uses the [video unit](<video_unit.md> "video unit"). This unit as well as the [crt unit](<crt_unit.md> "crt unit") emulate the DOS console. On this console simple graphics were generated with special graphics characters of the character font named VGA. Their character codes are in the 128-255 range. Other fonts often have special national characters in this range, for example German umlauts (ä, ö, ü, ...) or letters with accents. Two situations are of interest: 

  * Terminal with VT100 character set.
  * Terminal with VGA character set.



VGA character sets allow a visually more appealing user interface VT100 character set. On a terminal with VT100 character set, the video unit will translate VGA characters to VT100 alternate character set characters. 

On a terminal with VGA character set (currently only the Linux console), the video unit will enable the VGA characters and use them without translation. 

An historical description for the xterm terminal of X11 on macOS is here: [How to get the fpc ide running under macOS](<How_to_get_the_fpc_ide_running_under_Mac_OS_X.md> "How to get the fpc ide running under Mac OS X")

Furthermore, the Terminal emulations can use a number of fonts, which in turn are selected from an even larger number of fonts. And this is were another problem arises. When a Linux terminal receives the ESC sequence "ESC (8" it selects the graphics font as the alternate font. However, a VT200 terminal selects the German Font as the alternate font. This ESC sequence is sent to your terminal during initialization of the video and the crt unit. As a result you get a "ß" instead of a "~" in your terminal after the program has finished and your terminal is emulating a VT200 and up. The X11 xterm terminal and the Terminal.app in macOS behave like that. 

In case of problems consider the following actions: 

  * Get and install a VGA font.


  * Try a different terminal program which emulates a Linux terminal and not a VT200.


  * If none of the above helps, as a last resort you can even change the [initialization](<Initialization.md> "Initialization") of the [video](<video_unit.md> "video unit") or [crt units](<crt_unit.md> "crt unit"), although I do not really recommend this, because it most likely will lead to incompatibilities in the future.

---

_Source: [https://wiki.freepascal.org/Terminal_%26_Fonts](https://web.archive.org/web/20250301000000/https://wiki.freepascal.org/Terminal_%26_Fonts)_
