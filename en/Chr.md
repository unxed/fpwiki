# Chr

│ **[Deutsch (de)](</Chr/de> "Chr/de")** │  **English (en)** │  **[français (fr)](</Chr/fr> "Chr/fr")** │  **[русский (ru)](<../ru/Chr.md> "Chr/ru")** │    
****

The function `chr` returns the [`char`](<Char.md> "Char") which has [ASCII](<ASCII.md> "ASCII") value `b`. The signature reads: 
    
    
    function chr(b: byte): char;
    

The function is used and necessary because of Pascal's strong type safety. With the advent of [typecasts](<Typecast.md> "Typecast") a general method became available, though using `chr` is better style. 

A trivial example shall demonstrate the usage of `chr`: 
    
    
    type
    	// modern Latin alphabet has 26 letters
    	latinAlphabetCharacterIndex = 0..25;
    
    {$push}
    // let compiler generate code ensuring n >= 0 and n < 26
    {$rangeChecks on}
    function getChrInLatinAlphabet(const n: latinAlphabetCharacterIndex): char;
    begin
    	getChrInLatinAlphabet := chr(ord('A') + n);
    end;
    {$pop}
    

[FPC](<FPC.md> "FPC") has implemented `chr` as a compiler intrinsic [`in_chr_byte`](<https://svn.freepascal.org/cgi-bin/viewvc.cgi/tags/release_3_0_4/compiler/ninl.pas?revision=37113&view=markup#l2221>). 

## see also

  * [`chr`](<https://www.freepascal.org/docs-html/rtl/system/chr.html>) in the system unit reference
  * [`ord`](<Ord.md> "Ord") does the reverse operation.

---

_Source: [https://wiki.freepascal.org/Chr](https://web.archive.org/web/20241212112347/https://wiki.freepascal.org/Chr)_
