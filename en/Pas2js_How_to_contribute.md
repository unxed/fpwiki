# Pas2js How to contribute

You want to help the pas2js project and don't know how? 

Here are some ideas. If you want to help give us a note on the mailing list, so we can help you. 

## Contents

  * 1 Spread the word
  * 2 Answer newbie question on forums and mailing lists
  * 3 Write an overview, what units/packages already exist and what they provide
  * 4 Easier quick start
  * 5 Lazarus help
  * 6 Simple HOWTOs
  * 7 Write external classes for importing JS libraries
  * 8 Help in compiler
  * 9 WidgetSet
  * 10 Navigation



# Spread the word

Post on social media, organize meetings 

# Answer newbie question on forums and mailing lists

  * Official mailing list (English): <http://lists.freepascal.org/cgi-bin/mailman/listinfo/pas2js>
  * Pas2js board on Lazarus forum: <http://forum.lazarus-ide.org/index.php/board,76.0.html>



# Write an overview, what units/packages already exist and what they provide

For example a wiki page with a list of all units and their classes with one or two sentences for each unit/class. 

# Easier quick start

  * Write a better installer
  * or a better wiki page how to quick start from nil to the first webapp.
  * or extend pas2jsdsgn with a button to download and setup pas2js
  * or a script to create rpm/deb/dmg packages



# Lazarus help

Help for pas2js units on F1: 

  * fpdoc files
  * or external help entries



# Simple HOWTOs

  * Wiki: Register a user in the wiki and write a page how to use pas2js with library X, or how to port Delphi code, or how to use pas2js in editor Y, etc
  * or create a Youtube tutorial
  * Donate examples. Every example that demonstrates what can be done is welcome !



# Write external classes for importing JS libraries

  * If a JS library has an IDL, use the webidl tool to auto create a basic Pascal unit for it, and if needed improve it manually
  * Use class2pas unit to create a rough Pascal representation, then use the documentation to fill the gaps (create some types).



# Help in compiler

The whole compiler is open source, so everyone can improve it and send patches. 

  * Here are the needed [optimizations](<Pas2js_optimizations.md> "Pas2js optimizations").



# WidgetSet

As pas2js is maturing, the next step is to write a widgetset. Some efforts are underway: [pas2js_widgetsets](<pas2js_widgetsets.md> "pas2js widgetsets")

The end goal is a a widgetset that can be used to underpin the LCL. But a widgetset should be usable without having to use the LCL: When developing for web, the mechanisms of a desktop development environment are not always suitable. 

# Navigation

  * [pas2js](<pas2js.md> "pas2js")
  * [Lazarus integration](<lazarus_pas2js_integration.md> "lazarus pas2js integration")
  * [WidgetSets](<pas2js_widgetsets.md> "pas2js widgetsets")

---

_Source: [https://wiki.freepascal.org/Pas2js_How_to_contribute](https://web.archive.org/web/20240304092743/https://wiki.freepascal.org/Pas2js_How_to_contribute)_
