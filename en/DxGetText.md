# DxGetText

## Contents

  * 1 About
  * 2 Conversion by
  * 3 License
  * 4 Download
  * 5 Change Log
  * 6 Dependencies / System Requirements
  * 7 Installation



## About

Lazarus dxGetText is a conversion of the dxGetText project (<http://dybdahl.dk/dxgettext/>). From the dxGetTest website: "Initially, this project used a Windows port of the GNU gettext library, but has made it much further and today it is a complete reimplementation of the GNU gettext library with many enhancements". 

With this lib you can create a localizable application with a minimum of work. You do not need to extract all your strings and to transform them into ResourceString. 

You use syntax _('message') and the tool for extract... a few moments later, you have a .po file containing all the strings including those of the properties of the objects. 

For more informations, see the help: <http://dybdahl.dk/dxgettext/docs/online/>

## Conversion by

[Olivier Guilbaud](<http://sourceforge.net/users/golivier/>)

## License

<http://dybdahl.dk/dxgettext/license/> (Seems like some older BSD license with advertising clause... as the page says, it's very liberal, but probably not compatible with GPL programs) 

## Download

The latest stable release can be found on the [Lazarus CCR Files page.](<http://sourceforge.net/project/showfiles.php?group_id=92177&package_id=98986&release_id=219366>)

## Change Log

Getting the latest source from CVS 
    
    
       cvs -d:pserver:anonymous@cvs.sourceforge.net:/cvsroot/lazarus-ccr login
       cvs -z3 -d:pserver:anonymous@cvs.sourceforge.net:/cvsroot/lazarus-ccr co dxgettext
    

## Dependencies / System Requirements

  * FPC 1.9.x minimum



## Installation

---

_Source: [https://wiki.freepascal.org/DxGetText](https://web.archive.org/web/20250325132742/https://wiki.freepascal.org/DxGetText)_
