# HashLib4Pascal

**HashLib4Pascal** is a Delphi/FreePascal compatible library that provides an easy to use interface for computing hashes and checksums of strings (with a specified encoding), files, streams, byte arrays and untyped data to mention but a few. It also supports Incremental Hashing. 

## Contents

  * 1 Supported Algorithms
  * 2 Supported Compilers
  * 3 Installing the Library
  * 4 Example
  * 5 Unit Tests
    * 5.1 For FPC
    * 5.2 For Delphi
  * 6 License
  * 7 Download



### Supported Algorithms

non-cryptographic 32-bits hash algorithms
    AP, BKDR, Bernstein, Bernstein1, DEK, DJB, ELF, FNV, FNV1a, JS, Jenkins3, Murmur2, MurmurHash3_x86_32, OneAtTime, PJW, RS, Rotating, SDBM, ShiftAndXor, SuperFast, XXHash32.

non-cryptographic 64-bits algorithms
    FNV, FNV1a, Murmur2_64, SipHash2_4, XXHash64.

non-cryptographic 128-bits algorithms
    MurmurHash3_x86_128, MurmurHash3_x64_128.

checksum algorithms
    Adler32, All CRC Variants from CRC3 to CRC64.

cryptographic algorithms
    GOST, Grindahl, HAS160, Haval, MD2, MD4, MD5, Panama, RadioGatun, RIPEMD, RIPEMD128, RIPEMD160, RIPEMD256, RIPEMD320, SHA0, SHA1, SHA2-224, SHA2-256, SHA2-384, SHA2-512, SHA2-512-224, SHA2-512-256, SHA3-224, SHA3-256, SHA3-384, SHA3-512, Keccak-224, Keccak-256, Keccak-288, Keccak-384, Keccak-512, Snefru128, Snefru256, Tiger, Tiger2, WhirlPool, Blake2B, Blake2S, Streebog (GOST3411_2012_256, GOST3411_2012_512), Shake_128, Shake_256.

HMAC for any of the above. 

PBKDF2_HMAC for any of the above. 

PBKDF_Argon2 (2i, 2d and 2id variants). 

PBKDF_Scrypt. 

### Supported Compilers

  * FreePascal 3.0.0 and above.
  * Delphi 2010 and above.



### Installing the Library

  * Method one: use the provided packages in the "Packages" folder.
  * Method two: add the Library path and Sub path to your project Search Path.



### Example

HashLib4Pascal is not a visual component, so you won't click it off the component list. To add an installed non-visual component to your project, click on menu "Project > Project inspector ...". In the newly opened window, add a new dependency: "Add > New requirement" and you select "HashLib4PascalPackage" and click "OK" - now you can close the "Project inspector" window. Then in the "uses" section you need to add the modules needed to generate HMAC: 
    
    
    uses
      //...
      HlpIHashInfo, HlpConverters, HlpHashFactory;
    

And the code that generates the HMAC looks something like this: 
    
    
    procedure TForm1.Button1Click(Sender: TObject);
    var
      LHMAC: IHMAC;
      tekst, secret_key, hmac512_value: String;
    begin
      tekst := 'test';
      secret_key := '123';
      LHMAC := THashFactory.THMAC.CreateHMAC(THashFactory.TCrypto.CreateSHA2_512);
      LHMAC.Key := TConverters.ConvertStringToBytes(secret_key, TEncoding.UTF8);
      hmac512_value := LHMAC.ComputeString(tekst, TEncoding.UTF8).ToString();
      ShowMessage(hmac512_value);
    end;
    

### Unit Tests

#### For FPC

Simply compile and run "HashLib.Tests" project in "FreePascal.Tests" Folder. 

#### For Delphi

Method one, using DUnit Test Runner: to build and run the unit tests for Delphi 10 Seattle (should be similar for other versions) 

  * Open Project Options of Unit Test (HashLib.Tests) in "Delphi.Tests" Folder.
  * Change Target to All Configurations (Or "Base" In Older Delphi Versions.)
  * In Output directory add ".\$(Platform)\$(Config)" without the quotes.
  * In Search path add "$(BDS)\Source\DUnit\src" without the quotes.
  * In Unit output directory add "." without the quotes.
  * In Unit scope names (If Available), Delete "DUnitX" from the List.



Press Ok and save, then build and run. 

Method two (using TestInsight, preferred). 

  * Download and Install TestInsight.
  * Open Project Options of Unit Test (HashLib.Tests.TestInsight) in **Delphi.Tests** Folder.
  * Change Target to All Configurations (Or "Base" In Older Delphi Versions.)
  * In Unit scope names (If Available), Delete "DUnitX" from the List.
  * To Use TestInsight, right-click on the project, then select **Enable for TestInsight** or **TestInsight Project**. Save Project then Build and Run Test Project through TestInsight.



### License

Licensed Under MIT License. 

### Download

  * <https://github.com/Xor-el/HashLib4Pascal>

---

_Source: [https://wiki.freepascal.org/HashLib4Pascal](https://web.archive.org/web/20250122100422/https://wiki.freepascal.org/HashLib4Pascal)_
