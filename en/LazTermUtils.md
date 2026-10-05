# LazTermUtils

## Contents

  * 1 About
  * 2 Functionality
  * 3 Requirements
  * 4 Usage
  * 5 Notes
  * 6 Download



## About

This small library contains a few classes for managing terminal I/O using ANSI escape sequences. The goal of this unit is provide the functionality provided by the CRT unit (and some more), but optimized for only the most prevalent systems, by only using escape sequences. It was tested under Windows 10 and Linux and allows for writing portable, pretty console applications on both systems. 

Author: Frederic Kehrein (Warfley) 

License: BSD-2-Clause License 

## Functionality

  * Colorizing output (background, foreground)
  * Modifying output (bold, italic, underlined, etc.)
  * Reading input keys and modifiers
  * Managing terminal window (getting size, jumping around, clearing)
  * Non blocking reading of chars and keys



Features: 

  * Allows to create a Terminal that either uses STDIN, STDOUT and STDERR, or can read and write from files (or tty pseudo files in Linux). This allows even accessing other terminals, e.g. when you want to send a message to all open sessions on a server.
  * Supports most widespread Textmodifications like bold, italic, underlined, striked, etc.
  * Allows to specify colors as 24 bit values, compatibly with non system LCL colors (like $FF0000 for red)
  * Can read keys including their modifications directly, e.g. it can detect ctrl + key or alt + key, as well as support for most special keys
  * Allows for both line buffered echoed input and direct invisible input (i.e. where the key is recognized on press and not printed to the terminal)
  * Functionality for moving the cursor, clearing the screen and clearing the current line
  * Can buffer output to a given size to allow for a better performance (rather than writing each char at a time)
  * Provides a cross plattform IsATTY function to check if the handle used is connected to a terminal or a file
  * Non blocking read to allow to check if a keypress was made without waiting for it
  * UTF-8 support for keystroke reading



## Requirements

This library uses xTerm compatible ANSI escape sequences. 

This works with any Terminal emulator supporting this including, but not limited to xTerm, Konsole, GNOME Terminal and the Windows Terminal in Windows 10 (not compatible with older versions of Windows) If the terminal does not support 24 bit (true) color (e.g. the macOS system terminal emulator), you might want to set the define Col8 (only required in TerminalColor.pas), which limits the output to 8bit colors. Lower than that is not supported. 

## Usage

See the "example" directory for some examples. 

  * example/ReadkeyExample shows how to read Keystrokes and process them
  * example/Colortest colors the whole display (each cell individually) while measuring the FPS to brenchmark the performance of redrawing every cell
  * example/Textmods prints text with different modifications. Which of them get displayed correctly depends on your Terminal emulator.
  * example/NonBlockingReadTest shows how to use non blocking read to update your view and check for keystrokes single threaded



## Notes

This is limited due to the limitations of the escape sequences. This means some things are simply not possible Ctrl + I for example is treated as a Tab, there is no way the library can distinquish between these two and therefore always says it recieved a tab when Ctrl + I is used. 

When being in direct read mode (i.e. reading without linebuffering and echoing by the terminal) all special key combinations like ctrl + c are disabled and you will recieve the key strokes for those rather than the event (like SIGINT) 

Some terminals like the macOS terminal do not support 24 bit (true) color. In that case simply define Col8 (either in the package options or in the TerminalColor.pas) to use 8-bit colors. Don't worry the API does not change so no code has do be adopted, the color resolution will simply be downscaled from 24 to 8 bit during runtime. 

I tried to keep the normal FPC functions like WriteLn and ReadLn still working, but of course modifications (like changing the color) can also effect these functions. Also when direct read mode is enabled, a lot of the behaviour changes. Newlines will be #13 (enter key code) and newlines under Unix will not reset the cursor (so #13 needs to be printed to return the cursor). One is best advised to only use the provided Stream classes for this interface. 

Examples can be found in the example folder. See Github readme for more information. 

The Colortest example measures the fps it can archive when printing every single cell of the terminal in an infinite loop. On my 1080p display with Konsole I get 45 fps when running in fullscreen, I would like to know what performance you archive with this 

This is still a work in progress, I just thought it is now in such a state that I might share it with others (so the basic functionality seems to work quite well). If you find bugs report them, the best way to do it is by opening a Github issue. 

## Download

GitHub repo: <https://github.com/Warfley/LazTermUtils>

---

_Source: [https://wiki.freepascal.org/LazTermUtils](https://web.archive.org/web/20250219051314/https://wiki.freepascal.org/LazTermUtils)_
