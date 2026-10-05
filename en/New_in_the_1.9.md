# New in the 1.9.x rtl (not yet in the docs)

The following stuff is new in 1.9.x and is not yet in the docs: 

  * A lot of integer types are changed to SizeInt/SizeUInt to have common declarations for 32 and 64 Bit targets.



The following routines are new in the 1.9.x rtl and aren't yet documented: 
    
    
     function UnicodeToUtf8(Dest: PChar; Source: PWideChar; MaxBytes: SizeInt): SizeInt;
     function UnicodeToUtf8(Dest: PChar; MaxDestBytes: SizeUInt; Source: PWideChar; SourceChars: SizeUInt): SizeUInt;
     function Utf8ToUnicode(Dest: PWideChar; Source: PChar; MaxChars: SizeInt): SizeInt;
     function Utf8ToUnicode(Dest: PWideChar; MaxDestChars: SizeUInt; Source: PChar; SourceBytes: SizeUInt): SizeUInt;
    

See also [Language related articles](<Language_related_articles.md> "Language related articles") for compiler stuff not yet documented. 

UPD: Now it documented in [UTF-8](<UTF-8.md> "UTF-8") page.

---

_Source: [https://wiki.freepascal.org/New_in_the_1.9.x_rtl_(not_yet_in_the_docs)](https://web.archive.org/web/20250122120934/https://wiki.freepascal.org/New_in_the_1.9.x_rtl_(not_yet_in_the_docs))_
