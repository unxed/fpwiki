# hash

│ **English (en)** │  **[français (fr)](</hash/fr> "hash/fr")** │  **[русский (ru)](<../ru/hash.md> "hash/ru")** │    
****

The package **hash** contains implementations for CRC, MD5, NTLM (1?) and, under Linux, crypt. Crypt might be better used from pkg "user". 

## Unit md5

This unit implements the MD5 digest algorithm according to the specifications in RFC 1321. There are routines to calculate hashes from some buffer, or a hash from an entire file. 

The test program md5test calculates the hash value for some reference string. The reference output is provided for comparison. 

A simple way to calculate the MD5 of a given string is to use MD5String followed by MD5Print, like this: 
    
    
    uses md5;
    
    var
      Password, PasswordHash: string;
    begin
      PasswordHash := MD5Print(MD5String(Password));
    

Similarly, to generate the MD5 hash of a file one can use: 
    
    
    uses md5;
    
    var
      PathToFile, FileHash: string;
    begin
      FileHash := MD5Print(MD5File(PathToFile));
    

## Hash units

The following hash units are available in FPC: 

  * Implements a NTLM version 1.0, password hash algorithm-unit : ntlm.pas
  * Implements a MD2 digest algorithm (RFC 1319)-unit: md5.pp
  * Implements a MD4 digest algorithm (RFC 1320)-unit: md5.pp
  * Implements a MD5 digest algorithm (RFC 1321)-unit: md5.pp
  * Implements CRC algorithm-unit: crc.pas
  * Implements SHA1 digst algorithm-unit: sha1.pp
  * Hash-based supporting HMAC-MD5 and HMAC-SHA-1 - unit: hmac.pp (FPC trunk)



## See also

  * [HMAC-MD5](<HMAC-MD5.md> "HMAC-MD5")
  * [HMAC-SHA1](<HMAC-SHA1.md> "HMAC-SHA1")



Go back to [Packages List](<Package_List.md> "Package List")

---

_Source: [https://wiki.freepascal.org/hash](https://web.archive.org/web/20240914020449/https://wiki.freepascal.org/hash)_
