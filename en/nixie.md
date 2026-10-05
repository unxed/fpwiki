# nixie

## Contents

  * 1 About
  * 2 Author
  * 3 License
  * 4 Download
  * 5 Dependencies / System Requirements
  * 6 Installation



## About

_nixie_ (TNixieDisplay) is a a component to display numeric values using images of nixie tubes: 

[![nixie.png](https://wiki.freepascal.org/images/a/ae/nixie.png)](</File:nixie.png>)

play with the properties: 

  * **Digits** how many digits to show
  * **Value** the number to show (if there aren't enough digits ---- will be displayed instead)
  * **LeadingZero** if you want to show the leading zeroes or omit them (i.e. the tube will be shown in an off state)
  * **Color** using _clNone_ will paint transparently over the background, any other color will be solid
  * **Style** _NsTube_ will use images of ZM1082 nixie tubes while _NsRound_ will show round nixies



  


## Author

Luca Olivetti 

## License

Modified LGPL as per the FCL license 

ZM1082 images by Cestmir Hybl in the public domain, see <https://commons.wikimedia.org/wiki/File:ZM1082_operating_animation_front_250px.gif>

Round nixie images by Hellbus under a Creative Commons Attributions-Share Alike 3.0 Unported license, see <https://commons.wikimedia.org/wiki/File:Nixie2.gif>

## Download

You can get this component from the source repository [here](<https://github.com/olivluca/nixie>). 

## Dependencies / System Requirements

  * None that I know of



Status: _Stable_ (I hope) 

  


## Installation

  * Get the source from [here](<https://bitbucket.org/olivluca/nixie>).
  * Open the package nixiedisplay.lpk in the lazarus ide
  * Click install
  * After the installation the _TNixieDisplay_ component will be in the _Misc_ tab of the components palette

---

_Source: [https://wiki.freepascal.org/nixie](https://web.archive.org/web/20240920204139/https://wiki.freepascal.org/nixie)_
