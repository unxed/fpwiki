# lazdatadesktop

**English (en)** | [**español (es)**](</lazdatadesktop/es> "lazdatadesktop/es")   
****

## Contents

  * 1 Purpose
  * 2 Documentation
  * 3 Getting it
  * 4 Multi-language support
  * 5 Extending LazDataDesktop
    * 5.1 Basic support
    * 5.2 Full support



## Purpose

Lazdatadesktop is a database tool that can be used with Lazarus or standalone. It lets you manage data dictionaries, browse them etc. 

[![lazdatadesktop.png](https://wiki.freepascal.org/images/6/63/lazdatadesktop.png)](</File:lazdatadesktop.png>)

## Documentation

No official documentation; see [here](<http://www.freepascal.org/~michael/articles/lazdbdesktop/lazdbdesktop.pdf>) for an article describing its use. 

## Getting it

Lazdatadesktop is a Lazarus project in the <lazarusdir>\tools\lazdatadesktop directory. Simply compile it like you would any other Lazarus project. 

You can add it to the Lazarus tools menu if you want to. 

## Multi-language support

The LazDataDesktop application has been translated into several languages. By default, it starts in the user's local language. If you want to run it in a different language use the command-line argument `-language lc` or `--language=lc` where "lc" is the language code abbreviation of the requested language, e. g. "ru" for Russian: `-language ru`. 

## Extending LazDataDesktop

You can add additional database drivers to Lazdatadesktop. We're assuming you're adding support for sqldb database drivers. You will need to perform additional work if adding e.g. ZEOS drivers. 

### Basic support

Basic support can be added by first adding support to FreePascal data dictionary code in <fpcsource>\packages\fcl-db\src\datadict: 

  * creating a basic data dictionary unit. You could try copying and modifying another basic unit such as fpddmysql40.pp
  * edit buildd.lpr to add the unit you created above.
  * edit Makefile.fpc to add the unit to the packages_<OS>=* lines in the [required] section, and the [target] section.



Recompile FPC to get support into Lazarus. 

Then add support to the data dictionary: 

  * adding the data dictionary unit (e.g. fpddmysql51) to the implementation uses clause of frmmain.pp



Recompile lazdatadesktop 

### Full support

For full support, you will need to edit the fpdd<database>.pp unit. 

An alternative approach would be to rewrite the fpddd* units to use database schema data from the underlying drivers (e.g. the sqldb drivers).

---

_Source: [https://wiki.freepascal.org/lazdatadesktop](https://web.archive.org/web/20241201000000/https://wiki.freepascal.org/lazdatadesktop)_
