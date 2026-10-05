# About

**AEncoding** is a small unit, which partly replaces LazUtils.LConvEncoding. It provides a few functions for converting strings from/to some encoding schemes: 
    
    
    function AEncodingConvertFromUTF8(const AInput: RawByteString; out AOutput: RawByteString;
      AEnc: TSystemCodePage): boolean; inline;
    
    function AEncodingConvertToUTF8(const AInput: RawByteString; out AOutput: RawByteString;
      AEnc: TSystemCodePage): boolean; inline;
    
    function AEncodingConvertFromTo(const AInput: RawByteString; out AOutput: RawByteString;
      AEncInput, AEncOutput: TSystemCodePage): boolean;
    

To use functions with "string" var, do this: 
    
    
     AEncoding....(RawByteString(S), ....);
    

Why "partly replaces"? 

  * It uses FPC 3.x SetCodePage, which requires OS support for many encodings, whereas LConvEncoding doesn't require OS support. For common encodings (CP1250...CP1258, CP437, CP866 etc) OS support should work, on Windows and Unix'es.
  * FPC SetCodePage doesn't raise exceptions, while LConvEncoding can do it (when global variable is set in LConvEncoding unit).



It also gives few integer constants aeid_NNNN for common encodings. 

  * Author: Alexey Torgashin.
  * License: MPL 2.0 or LGPL.



# Download

GitHub: <https://github.com/Alexey-T/AEncoding>

---

_Source: [https://wiki.freepascal.org/AEncoding](https://web.archive.org/web/20250126101330/https://wiki.freepascal.org/AEncoding)_
