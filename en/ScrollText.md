# ScrollingText

## Contents

  * 1 About
  * 2 Properties screenshot
  * 3 Installation
  * 4 Usage
  * 5 Download
  * 6 License
  * 7 Requirements
  * 8 Support
  * 9 See also



# About

TScrollingText is a visual component for Lazarus/fpc by minesadorada@charcodelvalle.com 

This is a graphic panel that will display text that scrolls upwards. The effect is like the Lazarus Help/'About Lazarus' dialog Contributors tab. It is an encapsulation and addition to the IDE code in AboutFrm.pas to make a visual drop-in component. 

# Properties screenshot

[![scrolltext100 properties.png](https://wiki.freepascal.org/images/1/15/scrolltext100_properties.png)](</File:scrolltext100_properties.png>)

# Installation

  * Make a new folder 'scrolltext' in lazarus/components
  * [Download the latest version from the ccr](<https://sourceforge.net/p/lazarus-ccr/svn/HEAD/tree/components/>)
  * Or install from Online Package Manager
  * In Lazarus open the file 'scrolltext.lpk' as a package project
  * Click 'Compile' then 'Use/Install'
  * If asked 'do you want to recompile the IDE?' then click 'Yes'



After the compilation, the ScrollingText component will be on the 'Additional' component palette. 

# Usage

Drop a ScrollingText onto a form and set the properties as required If you want the text to come from an external text file (UseTextFile=TRUE) then name the file 'scrolling.txt' and deploy it in the same folder as the executable. 

You can set Active=True when designing to see the scrolling text but remember than any http:// links are only clickable in runtime mode 

# Download

Download from the lazarus CCR [here](<https://sourceforge.net/p/lazarus-ccr/svn/HEAD/tree/components/>)

Current version is 1.1.2.0. 

# License

Modified GPL License (see source code). 

# Requirements

Tested with Lazarus 1.x fpc 2.6x Windows 64-bit 

  * If when you click Lazarus Help/'About Lazaus' Contributors tab, you can see a scrolling screen, then this component is compatible with your IDE



# Support

minesadorada@charcodelvalle.com 

Note the GPL license conditions 

# See also

  * [SplashAbout component (also by this author)](<SplashAbout.md> "SplashAbout")
  * [Components and code examples](<Components_and_Code_examples.md> "Components and Code examples")

---

_Source: [https://wiki.freepascal.org/ScrollText](https://web.archive.org/web/20250114055819/https://wiki.freepascal.org/ScrollText)_
