# $codePage

The [global compiler directive](<global_compiler_directives.md> "global compiler directives") `{$codePage}` indicates how to interpret [string](<string.md> "string") literals. 

## available code pages

The [FPC](<FPC.md> "FPC") can understand following code pages: 

code page | description   
---|---  
`{$codePage CP437}` | Original IBM PC or DOS Latin US [code page 437](<https://en.wikipedia.org/wiki/Code_page_437>)  
`{$codePage CP850}` | DOS Latin 1 [code page 850](<https://en.wikipedia.org/wiki/Code_page_850>)  
`{$codePage CP852}` | DOS Latin 2 [code page 852](<https://en.wikipedia.org/wiki/Code_page_852>)  
`{$codePage CP856}` | Hebrew, [code page 856](<https://en.wikipedia.org/wiki/Code_page_856>)  
`{$codePage CP866}` | Cyrillic script, [code page 866](<https://en.wikipedia.org/wiki/Code_page_866>)  
`{$codePage CP874}` | Thai, [ISO/IEC 8859‑11](<https://en.wikipedia.org/wiki/Code_page_874>)  
`{$codePage CP1250}` | [Windows‑1250](<https://en.wikipedia.org/wiki/Windows-1250>)  
`{$codePage CP1251}` | Cyrillic script, [Windows‑1251](<https://en.wikipedia.org/wiki/Windows-1251>)  
`{$codePage CP1252}` | [Windows‑1252](<https://en.wikipedia.org/wiki/Windows-1252>)  
`{$codePage CP1253}` | Greek script, [Windows‑1253](<https://en.wikipedia.org/wiki/Windows-1253>)  
`{$codePage CP1254}` | Turkish, [Windows‑1254](<https://en.wikipedia.org/wiki/Windows-1254>)  
`{$codePage CP1255}` | Hebrew, [Windows‑1255](<https://en.wikipedia.org/wiki/Windows-1255>)  
`{$codePage CP1256}` | Arabic, [Windows‑1256](<https://en.wikipedia.org/wiki/Windows-1256>)  
`{$codePage CP1257}` | [Windows‑1257](<https://en.wikipedia.org/wiki/Windows-1257>)  
`{$codePage CP1258}` | Vietnamese, [Windows-1258](<https://en.wikipedia.org/wiki/Windows-1258>)  
`{$codePage 8859-1}` | ISO Latin-1, [ISO/IEC 8859‑1](<https://en.wikipedia.org/wiki/ISO/IEC_8859-1>)  
`{$codePage 8859-2}` | [ISO/IEC 8859‑2](<https://en.wikipedia.org/wiki/ISO/IEC_8859-2>)  
`{$codePage 8859-5}` | Cyrillic script, [ISO/IEC 8859‑5](<https://en.wikipedia.org/wiki/ISO/IEC_8859-5>)  
`{$codePage UTF-8}`  
`{$codePage UTF8}` | pseudo code page to switch back to UTF-8   
  
The code page can be selected either via a compiler directive or as a command-line parameter: `‑Fc…` (where `…` is one of the code page names listed above). 

## use

Since [FPC 3.0.0](<FPC_New_Features_3.0.md> "FPC New Features 3.0.0") every [`ANSIString`](<Ansistring.md> "Ansistring") is associated with a CP. 

## see also

  * [FPC Unicode support § “source file code page”](<FPC_Unicode_support.md> "FPC Unicode support")
  * [“`$CODEPAGE`: Set the source codepage”](<https://freepascal.org/docs-html/current/prog/progsu88.html>)
  * [Language Codes](<Language_Codes.md> "Language Codes")

---

_Source: [https://wiki.freepascal.org/$codePage](https://web.archive.org/web/20250513171635/https://wiki.freepascal.org/$codePage)_
