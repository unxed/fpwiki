# QRCodeGenLib4Pascal

## About

**QRCodeGenLib4Pascal** is a Delphi/FPC compatible library that provides an easy to use interface for generating QR Codes, from text strings and byte arrays. 

Core features: 

  * Supports encoding all 40 versions (sizes) and all 4 error correction levels, as per the QR Code Model 2 standard.
  * Output formats: Raw modules/pixels of the QR symbol, SVG XML string/file, Image object (bmp, jpg, png).
  * Encodes numeric and special-alphanumeric text in less space than general text.
  * Ability to change the backgound and foreground colors of the generated QRCode.



Manual parameters: 

  * User can specify minimum and maximum version numbers allowed, then library will automatically choose smallest version in the range that fits the data.
  * User can specify mask pattern manually, otherwise library will automatically evaluate all 8 masks and select the optimal one.
  * User can specify absolute error correction level, or allow the library to boost it if it doesn't increase the version number.
  * User can create a list of data segments manually and add ECI segments.



For demos, check out the QRCodeGenLib.Demo folder. 

## Download

Homepage is <https://github.com/Xor-el/QRCodeGenLib4Pascal>

---

_Source: [https://wiki.freepascal.org/QRCodeGenLib4Pascal](https://web.archive.org/web/20250219052103/https://wiki.freepascal.org/QRCodeGenLib4Pascal)_
