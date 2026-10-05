# SimpleBaseLib4Pascal

**SimpleBaseLib4Pascal** as the name implies is a simple to use Base Encoding Package for Delphi/FreePascal Compilers that provides at the moment support for encoding and decoding various bases such as Base16, Base32 (various variants), Base58 (various variants) and Base64 (various variants) and Base85 (various variants). 

## Contents

  * 1 Supported Encodings
  * 2 Supported Compilers
  * 3 Installing the Library
  * 4 Unit Tests
    * 4.1 For FPC
    * 4.2 For Delphi
  * 5 License
  * 6 Download



### Supported Encodings

Base32
    RFC 4648, Crockford and Extended Hex (BASE32-HEX) alphabets with Crockford character substitution (or any other custom alphabets you might want to use)

Base58
    Bitcoin, Ripple and Flickr alphabets (and any custom alphabet you might have)

Base64
    Default, DefaultNoPadding, UrlEncoding, XmlEncoding, RegExEncoding and FileEncoding alphabets (and any custom alphabet you might have)

Base85
    Ascii85 (Original), Z85 and custom flavors.

Base16
    An experimental hexadecimal encoder/decoder.

### Supported Compilers

  * FreePascal 3.0.0 and above.
  * Delphi 2010 and above.



### Installing the Library

  * Method one: use the provided packages in the "Packages" folder.
  * Method two: add the Library path and Sub path to your project Search Path.



### Unit Tests

#### For FPC

Simply compile and run "SimpleBaseLib.Tests" project in "FreePascal.Tests" Folder. 

#### For Delphi

Method one, using DUnit Test Runner: to build and run the unit tests for Delphi 10 Tokyo (should be similar for other versions) 

  * Open Project Options of Unit Test (SimpleBaseLib.Tests) in "Delphi.Tests" Folder.
  * Change Target to All Configurations (Or "Base" In Older Delphi Versions.)
  * In Output directory add ".\$(Platform)\$(Config)" without the quotes.
  * In Search path add "$(BDS)\Source\DUnit\src" without the quotes.
  * In Unit output directory add "." without the quotes.
  * In Unit scope names (If Available), Delete "DUnitX" from the List.



Press Ok and save, then build and run. 

Method two (using TestInsight, preferred). 

  * Download and Install TestInsight.
  * Open Project Options of Unit Test (SimpleBaseLib.Tests.TestInsight) in **Delphi.Tests** Folder.
  * Change Target to All Configurations (Or "Base" In Older Delphi Versions.)
  * In Unit scope names (If Available), Delete "DUnitX" from the List.
  * To Use TestInsight, right-click on the project, then select **Enable for TestInsight** or **TestInsight Project**. Save Project then Build and Run Test Project through TestInsight.



### License

Licensed Under MIT License. 

### Download

  * <https://github.com/Xor-el/SimpleBaseLib4Pascal>

---

_Source: [https://wiki.freepascal.org/SimpleBaseLib4Pascal](https://web.archive.org/web/20240121085858/https://wiki.freepascal.org/SimpleBaseLib4Pascal)_
