# Megido

The **Megido** project was a historic attempt to develop a visual integrated development environment (IDE) for rapid application development (RAD) with the [Free Pascal](<Free_Pascal.md> "Free Pascal") compiler. 

While the project failed, it unfolded a lasting impact, since it facilitated the transition from [Sibyl](<Sibyl.md> "Sibyl") to Open Sibyl. Even more important is the fact that it laid the foundations for the development of [Lazarus](<Lazarus.md> "Lazarus"). 

## History

Development began in 1998, and it was intended to modify the existing IDE [Sibyl](<Sibyl.md> "Sibyl") to be compatible with Free Pascal. In addition, it was planned to port Megido to [Linux](<Linux.md> "Linux") in order to support cross-platform development. Unlike its successor Lazarus, however, Megido was supposed to function as an open-source [Delphi](<Delphi.md> "Delphi") clone in order to support full compatibility. 

The Megido project failed in 1999 due to restrictions resulting from the Sibyl base, disagreement among the developers and then dramatic changes of the Free Pascal architecture. 

Achievements of the Megido project could be used, however, to support the transition from WDSibyl to Open Sibyl and to lay the foundations of the later successful Lazarus IDE. 

## Architecture

Megido made use of the Free Pascal Compiler, the Free Pascal Runtime Library ([RTL](<RTL.md> "RTL")) and the Free Component Library ([FCL](<FCL.md> "FCL")). Functions that were available in the VCL but not supported by the FCL were implemented by the MCL (Megido Class Library), the library SPCC (Sibyl Portable Component Classes or SpeedSoft's Portable Component Classes, a visual component library for Windows and OS/2) and widget libraries for GTK or Qt. This made it possible to develop platform-sensitive software for Windows, OS/2 and Linux. 

This design anticipated the more complex widgetset mechanism of Lazarus. However, unlike Lazarus, Megido wasn't intended to support development for platforms beyond Windows, OS/2 and Linux, e.g. Mac OS X / macOS, Android, iOS, Solaris, BSD. 

## External links

  * [Comprehensive article on Megido](<https://web.archive.org/web/20170307112255/https://en.wikipedia.org/wiki/Megido_\(Free_Pascal\)>) from Wikipedia (Internet Archive link - Wikipedia article has been deleted).
  * [MEGIDO Project - RAD for Linux](<https://web.archive.org/web/20170307112255/http://max.kleiner.com/megido.html>)
  * [Overview figure showing Megido's architecture at the Wayback Machine (archived March 5, 2014)](<https://web.archive.org/web/20170307112255/https://web.archive.org/web/20140305002753/http://max.kleiner.com/images/megido.jpg>)

---

_Source: [https://wiki.freepascal.org/Megido](https://web.archive.org/web/20220818211413/https://wiki.freepascal.org/Megido)_
