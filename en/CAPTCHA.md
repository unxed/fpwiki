# CAPTCHA

│ **English (en)** │

## Contents

  * 1 About
    * 1.1 Author
    * 1.2 License
  * 2 Description
    * 2.1 Methods
    * 2.2 Properties
  * 3 Installation
    * 3.1 Development version
    * 3.2 Release version



## About

[![CAPTCHA simple demo](https://wiki.freepascal.org/images/f/f6/CAPTCHA_simple_demo.png)](</File:CAPTCHA_simple_demo.png> "CAPTCHA simple demo")

According to [wikipedia](<https://en.wikipedia.org/wiki/CAPTCHA>), _a**CAPTCHA** (a contrived acronym for "**C** ompletely **A** utomated **P** ublic **T** uring test to tell **C** omputers and **H** umans **A** part") is a type of challenge–response test used in computing to determine whether or not the user is human_. 

The component `TCaptchaLabel` allows to create a CAPTCHA to be used on any LCL form. It displays random characters which are colored and rotated and overlayed by colored lines to impede machine detection of the CAPTCHA text. 

### Author

Werner Pamler 

### License

Modified LGPL (with linking exception), like Lazarus LCL. See the file COPYING.modifiedLGPL.txt, included in the Lazarus distribution, for details about the license. 

## Description

`TCaptchaLabel` descends from `TGraphicControl` and contains its own drawing routine. Just place it on the form and use it. It displays a random string with rotated characters and overlaid lines. 

### Methods

  * `**function** Verify(**const** AText: String): Boolean` \- Compares whether the user-provided string `AText` matches the CAPTCHA string and returns `true` in case of an agreement, or `false` otherwise.
  * `**procedure** NewCaptcha` \- Generates and displays a new CAPTCHA string



### Properties

  * `Text: String` \- CAPTCHA string. The characters of the string are selected randomly from the `UpperCaseChars`, `LowerCaseChars`, `NumericChars` and/or `CustomChars`.
  * `NumChars: Integer` \- Number of characters in the CAPTCHA, i.e. length of `Text`
  * `NumLines: Integer` \- Number of lines drawn over the CAPTCHA
  * `Font1, Font2: TFont` \- Two fonts to be mixed within the CAPTCHA characters randomly
  * `MaxAngle: Integer` \- Maximum rotation angle (in degrees) for the characters which are rotated by a random angle between `-MaxAngle` and `+MaxAngle`
  * `Color: TColor` \- Background color of the area of the control covered by the CAPTCHA. Character and line colors are selected such that a minimum brightness difference to the background color is achieved.
  * `UpperCaseChars: String` \- Contains the characters among which upper-case characters in the CAPTCHA are selected randomly. UTF8 encoding is supported as usual (and like for the following strings)
  * `LowerCaseChars: String` \- Contains the characters among which lower-case characters in the CAPTCHA are selected randomly
  * `NumericChars: String` \- Contains the numeric characters characters for the CAPTCHA
  * `CustomChars: String` \- Contains non-alpha and non-numeric characters for the CAPTCHA
  * `Options: TCaptchaOptions` \- This is a set of the following enumerated options which can be combined: 
    * `coAlphaUpper` \- Use upper-case characters as defined by the the `UppercaseChars` string  
Note: The characters which are hard to distinguish are skipped, O (upper-case o) vs 0 (zero), l (lower-case L) vs I (upper-case I)
    * `coAlphaLower` \- Use lower-case characters as defined by the `LowercaseChars` string
    * `coNumeric` \- Use numeric characters as defined by the `NumericChars` string
    * `coCustom` \- Use special characters as defined by the `CustomChars` string
    * `coRotated` \- Characters are rotated
    * `coFont1` \- Font1 is used
    * `coFont2` \- Font2 is used
    * `coLines` \- Lines are drawn over the CAPTCHA
  * `NewCaptchaEvent: TNewCaptchaEvent` \- Enumerated property which determines a short-cut how to create a new CAPTCHA string at runtime: 
    * `nceNone` \- Feature is deactivated; CAPTCHA can only be changed by code.
    * `nceClick` \- A new CAPTCHA is created when the user clicks on the control
    * `nceDblClick` \- A new CAPTCHA is created when the user double-clicks on the control.



## Installation

### Development version

The development version (aka trunk) is available at <https://sourceforge.net/p/lazarus-ccr/svn/HEAD/tree/components/captcha/>. Download the package using svn or get the zipped _snapshot_ version. 

Load the package file **captcha_pkg.lpk** into the Lazarus Package Editor and click _"Use"_ > _"Install"_ to rebuild the Lazarus IDE. When Lazarus restarts you find the new component in the palette _"Misc"_. 

### Release version

The stable release version is distributed via the [**Online-Package-Manager (OPM)**](<Online_Package_Manager.md> "Online Package Manager"): Check the item "Captcha" in the OPM list, click "Install" and confirm the prompt to rebuild the IDE.

---

_Source: [https://wiki.freepascal.org/CAPTCHA](https://web.archive.org/web/20250323022720/https://wiki.freepascal.org/CAPTCHA)_
