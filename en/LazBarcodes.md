# LazBarcodes

## Contents

  * 1 General
    * 1.1 About
    * 1.2 Author
    * 1.3 License
    * 1.4 Dependencies / System Requirements
  * 2 Getting LazBarcodes
    * 2.1 Development version
    * 2.2 Release version
    * 2.3 Change Log
  * 3 Documentation
    * 3.1 General
      * 3.1.1 Properties
      * 3.1.2 Methods
    * 3.2 Components
      * 3.2.1 1D codes
      * 3.2.2 Stacked barcodes
      * 3.2.3 2D codes
      * 3.2.4 Special properties of some components
    * 3.3 Demo application
  * 4 Final notes



## General

[![lazbarcodes demo.png](https://wiki.freepascal.org/images/8/87/lazbarcodes_demo.png)](</File:lazbarcodes_demo.png>)

### About

_LazBarcodes_ is a set of controls to create 1D and 2D barcodes. The backend engine is a port of [Zint](<http://sourceforge.net/projects/zint/>) at sourceforge. Some units were heavily refactored to obtain a more Pascal-like code. 

The library supports the generation of 1D and 2D barcodes: 

  * **1D barcodes** : Code-11, Code-128, Channel code, EAN-128, several 2-of-5 variants, several 3-of-9 variants, EAN, UPC, Plessey, MSI/Plessey, Telepen, medical/pharmaceutical codes (CodaBar, Code-32, Pharma one-track, Pharma two-track, PZN), postal codes (Australian post, Japanese post, Korean Post, Deutsche Post IdentCode and LeitCode, Dutch KIX, Planet, PostNet, Royal Mail RM4SCC)
  * **Stacked barcodes** : PDF417, compact PDF417, MicroPDF417
  * **2D barcodes** : QR, MicroQR, Aztec, Aztec Rune, Data Matrix, Maxicode



### Author

2011 - [Jose Mejuto](</index.php?title=User:Joshy&action=edit&redlink=1> "User:Joshy \(page does not exist\)")

### License

BSD 3 as it is being inherited from the zint source code. 

### Dependencies / System Requirements

  * None



  


## Getting LazBarcodes

The download contains the packages _lazbarcodes_ and _lazbarcodes_runtimeonly_. The first one gives you visual components of the above barcodes and the second can be used just to access the ported zint code for advanced barcode generation. 

### Development version

The latest source code can be found on [Lazarus-CCR](<https://sourceforge.net/p/lazarus-ccr/svn/HEAD/tree/components/lazbarcodes/>). Use SVN to load the source files, or download the "Snapshot" by clicking on the corresponding button of the sourceforge site and unzip it to some directory. For installation go to "Package" > "Load package file (*.lpk)", navigate to the folder with the downloaded sources and load the fole _lazbarcodes_runtimeonly.lpk_. Then load the package _lazbarcodes.lpk_. Click "Use" > "Install", and confirm the question to rebuild the IDE. After some time the IDE restarts, and you find the barcodes in the "LazBarcodes" tab of the component palette. 

### Release version

Occasionally a release version is prepared and made available for easy installation by means of the [Online-Package-Manager](<Online_Package_Manager.md> "Online Package Manager") in the Lazarus IDE. Open "Package" > "Online package manager", check the item "LazBarcodes" in the list and click "Install". Again, confirm the question to rebuild the IDE. When the IDE restarts after some time you find the barcodes in the "LazBarcodes" tab of the component palette. 

### Change Log

  * v2.0.0: Added a lot of 1D barcodes
  * v1.0.3: Internally used units renamed to avoid naming conflict with CodeTyphon package PL_APE.
  * v1.0.2: Change of license from GPL to BSD
  * Initial development.



## Documentation

### General

The components installed by the LazBarcodes package descent from TGraphicControl and thus inherit fundamental control properties such as `Width`, `Height`, `AutoSize`, etc. They share a variety of other properties and methods; some properties, however, are not published fwhen they are not appropriate for the current barcode type. 

#### Properties

  * `Text: TCaption` \- Text to be encoded as a barcode. Note that, depending on the barcode type, there exist limitations on allowed text length and allowed characters.
  * `ShowHumanReadableText: Boolean` \- Displays the encoded text underneath the barcode symbol. Note that the encoding algorithm may add check sum or additional characters to the input text.
  * `BarcodeType` \- Some barcode components can be modified to create various symbologies by selecting the corresponding value in this (type-dependent) enumeration.
  * `Scale: Integer` \- Horizontal scaling factor of the barcode determining the width of the smallest feature (bar or space) in pixels. `Scale=0` tries to select an internal scale value such that the barcode fills the control's width as much as possible. Note that this scaling factor applies to all bars and spaces and therefore, increasing `Scale` does not increase the barcode with gradually but by more or less large steps.
  * `SymbolHeight: Integer` \- Similar to `Scale`, but refers now to the vertical direction. Again, `SymbolHeight=0` fills adjusts the internal symbol height to fill the height
  * `MinSymbolHeight: Integer` \- Prevents the symbol to become too small.
  * `RecommendedSymbolSize: Boolean` \- If `true`, overrides the `Scale` and `SymbolHeight` settings so that the size of the symbol is as close as possible to its specification.
  * `AutoSize: Boolean` \- inherited standard property. When `true`, `Width` and `Height` are selected such that the current size of the barcode is enclosed. Note that `AutoSize=true` is ignored when `Scale=0` or `SymbolHeight=0`.
  * `Margin: Integer` \- Amount of empty pixels added around the symbol
  * `WhitespaceWidth: Integer` \- Amount of empty pixels added to the left and right of some symbologies to enhance readability of barcode scanners.
  * `BearerBarMode: TBearerBarMode = (bbmNone, bbmBearerBars, bbmBox)` \- adds horizontal bars above and below the barcode (`bbmBearerBars`) or a box around the barcode (`bbmBox`). The thickness of the bars and the box border is given by the `Margin` multiplied by the currently used internal `Scale` parameter.
  * `ForegroundColor: TColor` \- Color used to draw the bars
  * `BackgroundColor: TColor` \- Color used to draw the background of the barcode (spaces, margin and text area).



#### Methods

  * `CopyToClipboard` \- Copies the barcode to the clipboard as a bitmap
  * `SaveToFile(const AFileName: String; AImageClass: TFPImageBitmapClass)` \- Saves the barcode as a graphic image of the specified class to file.
  * `SaveToStream(const AStream: TStream; AImageClass: TFPImageBitmapClass)` \- Saves the barcode as a graphic image of the specified class to a stream.
  * `SaveToEPSFile(const AFileName: String)` \- Saves the barcode as EPS file
  * `SaveToEPSStream(const AStream: TStream)` \- Saves the barcode in EPS format as stream.
  * `SaveToSVGFile(const AFileName: String)` \- Saves the barcode in SVG format to file
  * `SaveToSVGStream(const AStream: TStream)` \- Saves the barcode in SVG format to a stream
  * `ErrorString: String` \- In case of an error, this read-only property contains a message why the `Text` could not be encoded correctly.



### Components

#### 1D codes

Component | BarcodeType | Name | Application | Allowed chars | Text length | Reference   
---|---|---|---|---|---|---  
`TBarcode2of5`  
[![tbarcode2of5.png](https://wiki.freepascal.org/images/1/10/tbarcode2of5.png)](</File:tbarcode2of5.png>) | `bctCode25Standard` | Standard 2-of-5, or   
Matrix 2-of-5 | Warehouse sorting  
photo finishing  
airline ticket marking | `'0'..'9'` | up to 80 | <https://en.wikipedia.org/wiki/Matrix_2_of_5>  
`bctCode25DataLogic` | DataLogic 2-of-5 | Processing and sorting of mail for the Chinese Postal Service | `'0'..'9'` | up to 80 | <https://en.wikipedia.org/wiki/Matrix_2_of_5>  
`bctCode25IATA` | IATA 2-of-5 | Airline cargo | `'0'..'9'` | up to 45 | <https://en.wikipedia.org/wiki/Industrial_2_of_5#IATA_2_of_5>  
`bctCode25Industrial` | Industrial 2-of-5 | Cardboard printing  
photo developing envelopes  
warehouse sorting systems | `'0'..'9'` | up to 45 | <https://en.wikipedia.org/wiki/Industrial_2_of_5>  
`bctCode25Interleaved` | Interleaved 2-of-5 | 135 film  
cartons of some products | `'0'..'9'` | up to 89 | <https://en.wikipedia.org/wiki/Interleaved_2_of_5>  
`bctITF14` | ITF-14 | Packaging levels of a product | `'0'..'9'` | up to 13 | <https://en.wikipedia.org/wiki/ITF-14>  
`TBarcode3of9`  
[![tbarcode3of9.png](https://wiki.freepascal.org/images/3/3f/tbarcode3of9.png)](</File:tbarcode3of9.png>) | `bctCode39  
bctLOGMARS` | Code 3-of-9 (C39)  
LOGMARS | Name badges, inventory and  
industrial applications   
LOGMARS: different bar widths | `'0'..'9', 'A'..'Z', ' ',   
'–', '.', '$', '/', '+', '%'` | up to 74 | <https://en.wikipedia.org/wiki/Code_39>  
`bctCode39Ext` | Extended Code 3-of-9  
(C39ext) | Name badges, inventory and  
industrial applications | `#0..#127` | up to 74 | <https://en.wikipedia.org/wiki/Code_39#Full_ASCII_Code_39>  
`bctCode93` | Code-93 | Canada post | like C39 | up to 107 | <https://en.wikipedia.org/wiki/Code_93>  
`TBarcodeC11`  
[![tbarcodec11.png](https://wiki.freepascal.org/images/e/ef/tbarcodec11.png)](</File:tbarcodec11.png>) |  | Code-11 | Telecommunication | `'0'..'9', '-'` | up to 121 | [https://en.wikipedia.org/wiki/Code_11 ](<https://en.wikipedia.org/wiki/Code_11>)  
`TBarcodeC128`  
[![tbarcodec128.png](https://wiki.freepascal.org/images/2/21/tbarcodec128.png)](</File:tbarcodec128.png>) | `bctC128` | Code-128 | Industrial applications | `#0..#127` | up to 160 | <https://en.wikipedia.org/wiki/Code_128>  
`bctEAN128` | EAN-128, or   
GS1-128 | Industrial applications | `#0..#127` | Depending on "Application identifier"   
to be given in square parenthesis | <https://en.wikipedia.org/wiki/GS1-128>]   
`TBarcodeChannelCode`  
[![tbarcodechannelcode.png](https://wiki.freepascal.org/images/a/ae/tbarcodechannelcode.png)](</File:tbarcodechannelcode.png>) |  | Channel Code | high densitiy codes | `'0'..'9'` | up to 7 | <https://barcodeguide.seagullscientific.com/Content/Symbologies/Channel_Code.htm>  
`TBarcodeEAN`  
[![tbarcodeean.png](https://wiki.freepascal.org/images/3/3d/tbarcodeean.png)](</File:tbarcodeean.png>) | `bctEAN` | EAN-8, EAN-13 | Article numbers | `'0'..'9'`  
`'+'` for addon code | up to 19 | <https://en.wikipedia.org/wiki/International_Article_Number>]   
`bctEAN14` | EAN-14 | Trading goods | `'0'..'9'` | up to 13 | <https://www.barcodestalk.com/ean14>  
`bctISBN` | ISBN | Books | `'0'..'9', 'X'`  
`'+'` for addon code | 9, 10 or 13 | <https://en.wikipedia.org/wiki/International_Standard_Book_Number>  
`bctNVE18` | NVE-18 | Shipping and storage | `'0'..'9'` | up to 25 |   
`bctUPCA` | UPC/A | Trade items | `'0'..'9'`  
`'+'` for addon code | up to 10  
plus 2 check digits | <https://en.wikipedia.org/wiki/Universal_Product_Code>  
`bctUPCE` | UPC/E | Trade items  
(more compact than UPC/A) | `'0'..'9'`  
`'+'` for addon code | up to 7 | <https://en.wikipedia.org/wiki/Universal_Product_Code#UPC-E>  
`TBarcodeMedical`  
[![tbarcodemedical.png](https://wiki.freepascal.org/images/2/24/tbarcodemedical.png)](</File:tbarcodemedical.png>) | `bctCodabar` | Codabar | Blood identification | `'0'..'9', '-', '$', ':', '/', '.', '+', 'A', 'B', 'C', 'D'` | up to 60 | <https://en.wikipedia.org/wiki/Codabar>  
`bctCode32` | Code 32 (Italian PHARMACODE) | Pharmaceutical products in Italy | `'0'..'9'` | up to 8 | <https://barcodeguide.seagullscientific.com/Content/Symbologies/Code_32.htm>  
`bctPharmaOne` | Pharmacode One-Track | Packaging of pharmaceutical products | `'0'..'9'` | up to 6 | <https://barcode-test.com/301/what-is-pharmacode/>  
`bctPharmaTwo` | Pharmacode Two-Track | Packaging of pharmaceutical products | `'0'..'9'` | up to 8 | <https://barcodeguide.seagullscientific.com/Content/Symbologies/TwoTrackPharmacode.htm>  
`bctPZN7` | Pharmazentralnummer (PZN) | Identification of pharmaceutical products (Germany, Austria) | `'0'..'9'` | 7 (6 by user + 1 auto-generated check digit) | <https://barcodeguide.seagullscientific.com/Content/Symbologies/PZN.htm>  
`bctPZN8` | 8 (7 by user + 1 auto-generated check digit)   
`TBarcodePlessey`  
[![tbarcodeplessey.png](https://wiki.freepascal.org/images/f/f2/tbarcodeplessey.png)](</File:tbarcodeplessey.png>) | `bctPlessey` | Plessey | Shelf tags in retail stores | `'0'..'9', 'A'..'F'` | up to 65 | <https://en.wikipedia.org/wiki/Plessey_Code>  
`bctMSIPlessey` | MSI/Plessey | `'0'..'9'` | depending on checkdigits  
(see `CheckChar` property below) | <https://en.wikipedia.org/wiki/MSI_Barcode>  
`TBarcodePostal`  
[![tbarcodepostal.png](https://wiki.freepascal.org/images/a/a3/tbarcodepostal.png)](</File:tbarcodepostal.png>) | `bctAustraliaPostCustomer` | Australia Post Standard Customer Barcode | Postal code and automatic mail sorting in Australia | `'0'..'9', 'A'..'Z', 'a'..'z'`, space and hash (`'#'`) | 8, 13, 16, 18, or 23 | <http://www.neodynamic.com/Products/Help/BarcodeWPF3.0/barcodes/AustraliaPost.htm>   
<http://fileformats.archiveteam.org/wiki/Australia_Post_standard_customer_barcode>  
`bctAustraliaPostReplyPaid` | Australia Post Reply Paid | `'0'..'9'` | 8 |   
`bctAustraliaPostRoute` | Australia Post Routing | `'0'..'9'` | 8 |   
`bctAustraliaPostRedirect` | Australia Post Redirection | `'0'..'9'` | 8 |   
`bctDAFT` | DAFT Code | Generic system of transcribing 4-state barcodes where the data encoding is provided by an external program. | `'D'`: Descender  
`'A'`: Ascender  
`'F'`: Full height  
`'T'`: Tracker (short bar) | up to 50 | <https://www.neodynamic.com/barcodes/DAFT-4-state-Barcode.aspx>  
`bctDeutschePostIdentCode` | Deutsche Post Identcode | Mail routing in Germany | `'0'..'9'` | (up to) 11 digits | <https://barcodeguide.seagullscientific.com/Content/Symbologies/Identcode.htm>  
`bctDeutschePostLeitCode` | Deutsche Post Leitcode | Specifies destination for mail in Germany | `'0'..'9'` | (up to) 14 digits | <https://barcodeguide.seagullscientific.com/Content/Symbologies/Leitcode.htm>  
`bctFIM` | FIM (Facing Identification Mark) | Automatic facing, or orientation, of the mail piece for cancellation | `'A'..'D', 'a'..'d'` | 1 | <https://en.wikipedia.org/wiki/Facing_Identification_Mark>  
`bctJapanPost` | Japanese Post 4-State Customer Code | Encodes addresses on mail for faster automatic processing in Japan | `'0'..'9', 'A'..'Z', '-'` |  | <https://barcodeguide.seagullscientific.com/Content/Symbologies/Japanese_Postal.htm>  
`bctKix` | Dutch KIX 4-State Customer Code | Automatic processing of bulk mail in the Netherlands | `'0'..'9', 'A'..'Z'` | up to 18 chars | <https://barcodeguide.seagullscientific.com/Content/Symbologies/KIX_4_State_Customer_Code.htm>  
`bctKoreaPost` | Korean Postal Authority Code | Automatic sorting of mail in Korea | `'0'..'9'` | up to 6 digits | <https://barcodeguide.seagullscientific.com/Content/Symbologies/Korea_Post.htm>  
`bctPlanet` | USPS PLANET (Postal Alpha Numeric Encoding Technique) | Automatic identification, sorting, and tracking of pieces of mail in the U.S. | `'0'..'9'` | up to 38 | <https://barcodeguide.seagullscientific.com/Content/Symbologies/USPS_PLANET.htm>  
`bctPostNet` | USPS POSTNET (Postal Numeric Encoding Technique) | Sorting of business reply and courtesy reply mail in the U.S. | `'0'..'9'` | up to 38 | <https://en.wikipedia.org/wiki/POSTNET>  
`bctRM4SCC` | RM4SCC (Royal Mail 4-State Customer Code) | Mail service in United Kingdom | `'0'..'9','A'..'Z'` | up to 120 | <https://en.wikipedia.org/wiki/RM4SCC>  
`TBarcodeTelepen`  
[![tbarcodetelepen.png](https://wiki.freepascal.org/images/5/50/tbarcodetelepen.png)](</File:tbarcodetelepen.png>) | `bctTelepen` | Telepen Alpha | University and other academic libraries in the UK | `#0..#127` | up to 30 | <https://en.wikipedia.org/wiki/Telepen>  
`bctTelepenNum` | Telepen Numeric | `'0'..'9', 'X'`  
'X' must be at even character position. | up to 60   
  
#### Stacked barcodes

Component | BarcodeType | Name | Application | Allowed chars | Text length | Reference   
---|---|---|---|---|---|---  
`TBarcodePDF417`  
[![tbarcodepdf417.png](https://wiki.freepascal.org/images/d/d2/tbarcodepdf417.png)](</File:tbarcodepdf417.png>) | `bctPDF417` | PDF417 (Portable Data File) | Government-issued identification cards (such as driver's licenses), airline boarding passes, postage stamps, package labels, etc. | `#0..#255` (ISO-8859-1) | up to 1850 alphanumeric, or 2710 numeric characters, or 1108 bytes | <https://en.wikipedia.org/wiki/PDF417>  
`bctPDF417trunc` | PDF417 truncated  
Compact PDF417 | <https://www.neodynamic.com/Products/Help/BarcodeCF2.0/barcodes/CompactPdf417.htm>  
`bctMicroPDF417` | MicroPDF417 | Designed to encode a moderate amount of data in a very small space | up to 250 alphanumeric or 366 numeric characters | <https://barcodeguide.seagullscientific.com/Content/Symbologies/MicroPDF417.htm>  
  
#### 2D codes

Component | Name | Application | Allowed chars | Text length | Reference   
---|---|---|---|---|---  
`TBarcodeAztec`  
[![tbarcodeaztec.png](https://wiki.freepascal.org/images/2/29/tbarcodeaztec.png)](</File:tbarcodeaztec.png>) | Aztec Code | Transport ticketing (Railroad, Airline). Governmental. | `#0..#255` interpreted as ISO-8859-1 | up to 3823 numeric or 3067 alphabetic characters or 1914 bytes of data | <http://en.wikipedia.org/wiki/Aztec_Code>  
`TBarcodeAztecRune`  
[![tbarcodeaztecrune.png](https://wiki.freepascal.org/images/4/49/tbarcodeaztecrune.png)](</File:tbarcodeaztecrune.png>) | Aztec Rune | special | `'0'..'9'` | up to 3 (8-bit value `0..255`) | <http://en.wikipedia.org/wiki/Aztec_Code#Encoding>  
`TBarcodeDatamatrix`  
[![tbarcodedatamatrix.png](https://wiki.freepascal.org/images/9/94/tbarcodedatamatrix.png)](</File:tbarcodedatamatrix.png>) | Data Matrix | Direct part marking | UTF-8 | up to 2335 alphanumeric characters, 3116 numbers, or 1556 bytes of binary data. | <https://en.wikipedia.org/wiki/Data_Matrix>  
`TBarcodeQR`  
[![tbarcodeqr.png](https://wiki.freepascal.org/images/8/8a/tbarcodeqr.png)](</File:tbarcodeqr.png>) | QR Code | Advertisement, web links, and many more | UTF-8 | up to 7089 numeric digits, 4296 alphanumeric characters or 2953 bytes of data | <https://en.wikipedia.org/wiki/QR_code>  
`TBarcodeMicroQR`  
[![tbarcodemicroqr.png](https://wiki.freepascal.org/images/6/69/tbarcodemicroqr.png)](</File:tbarcodemicroqr.png>) | Micro QR Code | Like QR Code, preferred when less space is available | UTF-8 | up to 35 | <http://en.wikipedia.org/wiki/Qr_code#Micro_QR_code>  
`TBarcodeMaxicode`  
[![tbarcodemaxicode.png](https://wiki.freepascal.org/images/8/8b/tbarcodemaxicode.png)](</File:tbarcodemaxicode.png>) | Maxicode | Tracking and managing the shipment of packages | `#0..#255` interpreted as ISO-8859-1 | about 90 characters (plus "primary message" needed in property `Primary` for mode 2 and 3) | <https://en.wikipedia.org/wiki/MaxiCode>  
  
#### Special properties of some components

  * `TBarcodePlessey`
    * `CheckChar: TPlesseyCheckChar` \- determines the checksum digit(s) added to the MSI/Plessey symbol (used only when `BarcodeType = bctMSIPlessey`): 
      * `pcOneMod10`: adds one check digit calculated by a _mod 10_ algorithm
      * `pcTwoMod10`: adds two check digits, both calculated by a _mod 10_ algorithm
      * `pcOneMod11`: adds one check digit calculated by a _mod 11_ algorithm
      * `pcOneMod10Mod11`: add two check digits, the first one calculated by a _mod 10_ , the second one calculated by a _mod 11_ algorithm.
  * `TBarcodeQR`
    * `ECCLevel: TBarcodeQR_ECCLevel` \- determines the level of error correction (used also by `TBarcodeMicroQR`): 
      * `eBarcodeQR_ECCLevel_Auto`: selects one of the other options depending on data length
      * `eBarcodeQR_ECCLevel_L`: Error correction capacity: approx 20% of symbol, recovery capacity: approx 7%
      * `eBarcodeQR_ECCLevel_M`: Error correction capacity: approx 37% of symbol, recovery capacity: approx 15%
      * `eBarcodeQR_ECCLevel_Q`: Error correction capacity: approx 55% of symbol, recovery capacity: approx 25%
      * `eBarcodeQR_ECCLevel_H`: Error correction capacity: approx 65% of symbol, recovery capacity: approx 30%
  * `TBarcodeMaxicode`
    * `Mode: TBarcodeMaxicodeMode` \- message mode 
      * `mcmAuto`: tries to determine the mode from the type of the data
      * `mcmMode2`: In addition to the `Text` ("secondary message") also a 15-digit "primary message" must be provided in the property `Primary`. It consists of 9 numerical digits for the US-domestic post-code (property `PostCode: String`), 3 numeric digits for the country code (ISO 3166, property `CountryCode: Integer`) and 3 numeric digits for the service code (property `ServiceCode: Integer`).
      * `mcmMode3`: In addition to the `Text` ("secondary message") also a 15-digit "primary message" must be provided in the property `Primary`. It consists of 6 alphanumeric digits for the international post-code (property `PostCode: String`), 3 numeric digits for the country code (ISO 3166, property `CountryCode: Integer`) and 3 numeric digits for the service code (property `ServiceCode: Integer`).
      * `mcmMode4`: No `Primary` needed.
      * `mcmMode5`: No `Primary` needed.
      * `mcmMode6`: No `Primary` needed.
    * `Primary: String` \- see `TBarcodeMaxicode.Mode`
    * `PostCode: String` \- see `TBarcodeMaxicode.Mode`
    * `CountryCode: String` \- see `TBarcodeMaxicode.Mode`
    * `ServiceCode: String` \- see `TBarcodeMaxicode.Mode`



### Demo application

A demo project is contained in the LazBarcodes installation. It allows to generate barcode from all barcode types supported. Please note that - since the requirements on length and characters changes from type to type - sometimes error messages are displayed; in this case, click on the "Sample text" button to use a predefined sample text for the selected barcode type. A screenshot is displayed at the top of this page. 

## Final notes

If you need the generation of another barcode and it is present in Zint, please ask for it in the [Lazarus forum](<http://forum.lazarus.freepascal.org/>) or mailing lists.

---

_Source: [https://wiki.freepascal.org/LazBarcodes](https://web.archive.org/web/20250219110816/https://wiki.freepascal.org/LazBarcodes)_
