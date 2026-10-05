# openurl

## Definition

Unit: Lazarus **lclintf**
    
    
    function OpenURL(AURL: String): Boolean;

Official documentation: [[1]](<http://lazarus-ccr.sourceforge.net/docs/lcl/lclintf/openurl.html>)

## Description

**openurl** opens an URL with the default/registered web browser as specified by the operating system. 

Apparently spaces need to be encoded (as %20); see [[2]](<http://www.ietf.org/rfc/rfc1738.txt>)

[![Warning-icon.png](https://wiki.freepascal.org/images/b/b2/Warning-icon.png)](</File:Warning-icon.png>)

**Warning:** On Windows with Lazarus versions older than development version r40490 (2013-03-06), opening an URL using the file:// URI scheme (to open a local file) may fail if there are spaces in the filename.  
With those older Lazarus versions, you should enclose the entire string in double quotes, if you are on an NT platform.  
This is unfortunately system and browser dependant. Note: opening a file:// URL with spaces on Win9x will probably fail if you enclose the URL in quotes (See bug 21659 in the bugtracker).  
On Windows it is preferable to not use OpenUrl with file:// URI scheme, but use [OpenDocument](<opendocument.md> "opendocument") with the filename instead because of the above mentioned problems.  
When using the file:// URI scheme, you should not escape spaces with %20.

## Example
    
    
    uses 
    ...
    lclintf
    ...
    OpenURL('http://wiki.lazarus.freepascal.org/');

---

_Source: [https://wiki.freepascal.org/openurl](https://web.archive.org/web/20180829221747/https://wiki.freepascal.org/openurl)_
