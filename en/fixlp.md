# fixlp

## About

In Lazarus 2.2.0 a new structure of the XML files (*.lpi, *.lps, *.lpk) was introduced. It replaces the numbered XML nodes used in older versions by unnumbered nodes. 

While Lazarus 2.2.0 can read both types of file formats and can also convert between them, older versions cannot read the new structure correctly leading to a lot of difficult-to-understand errors when trying to compile. 

**fixlp** is a small utility program, it converts the new Lazarus XML file format to the format used by older versions. 

## Usage
    
    
    fixlp <filename1> [, <filename2> [, ...]]
     
    

<filenames> are the names of the Lazarus .lpi, .lps or .lpk files (with absolute or relative path) to be converted. Wildcards are allowed. 

## Download

Source code (in Pascal): [SourceForge page](<https://sourceforge.net/p/lazarus-ccr/svn/HEAD/tree/applications/fixlp/>).

---

_Source: [https://wiki.freepascal.org/fixlp](https://web.archive.org/web/20240121085851/https://wiki.freepascal.org/fixlp)_
