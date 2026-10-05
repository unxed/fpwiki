# multi-tier options with fpc

## Contents

  * 1 Overview
  * 2 tiOPF
  * 3 Laz-n-tier
  * 4 Synopse mORMot
  * 5 More alternatives



## Overview

Free Pascal doesn't support multi-tier database development out of the box. The good news is that there are a few options available. 

## tiOPF

[tiOPF](<tiOPF.md> "tiOPF") supports multi-tier development with Free Pascal. It uses the Indy 10.5's HTTP client and server components. It packs the SQL statements or data in XML, then compresses it with zlib, and then base64 encodes it, before sending the data packet over the wire. That greatly reduces the packet size, and thus leads to faster transfers. 

With tiOPF based database applications, you can switch your application from local storage (XML, CSV or TAB) to Client/Server or to multi-tier, simply with a change of a compiler define. No need to change a single line of code in your application. See Demo 18 (Address Book demo) in the source code repository for a feature complete example of what tiOPF can do. 

## Laz-n-tier

[Laz-n-tier](<https://sourceforge.net/projects/laz-n-tier/>) is a datasnap alternative for Lazarus. Some of its features are discussed in this Lazarus forum [thread](<https://forum.lazarus.freepascal.org/index.php/topic,14147.0.html>). 

## Synopse mORMot

[Synopse _mORMot_](<https://synopse.info/fossil/wiki/Synopse+OpenSource>) is an Open Source Client-Server ORM (Object-Relational Mapping) SOA (Service Oriented Architecture) MVC (Model View Controller) framework for Delphi 6 up to Delphi 10.3 Rio and **Free Pascal** , targeting Windows/Linux for the server, and any platform for clients (including mobile or AJAX). 

## More alternatives

More alternatives may perhaps be found on the [FPC Forums](<https://forum.lazarus.freepascal.org>). If so, please update this page.

---

_Source: [https://wiki.freepascal.org/multi-tier_options_with_fpc](https://web.archive.org/web/20250513163941/https://wiki.freepascal.org/multi-tier_options_with_fpc)_
