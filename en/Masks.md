# Masks

This page describes the Masks unit as per Lazarus version 2.3 

UNDER CONSTRUCTION 

## Contents

  * 1 Overview
  * 2 Controlling how the mask is interpreted
    * 2.1 TMaskOpcodes
      * 2.1.1 Predefined MaskOpcodes
    * 2.2 Specific Windows Quirks
      * 2.2.1 Predefined TWindowsQuirks
    * 2.3 TMask classes
      * 2.3.1 TMask
      * 2.3.2 TWindowsMask
      * 2.3.3 TMaskList and TWindowsMaskList
  * 3 Code examples



## Overview

The masks unit provides classes and functions for pattern matching using wildcards, sets/ranges and/or literal characters.  


## Controlling how the mask is interpreted

### TMaskOpcodes

TMaskOpcodes is defined as set of TMaskOpcode.  
The TMaskOpcode enumeration type consist of: 

Name | Interpretation   
---|---  
mocAnyChar | Treat ? as a wildcard to match exactly one char   
mocAnyCharOrNone | Treat [?] to match any char or the absence of a char   
mocAnyText | Treat * as a wildcard to mach zero or any mumber of chars   
mocRange | Treat [a-z] to match any character in the range from 'a' up to and including 'z'.  
A '-' is treated as a range indicator and it's presence is required (unless mocSet is also enabled).  
To have a literal '-' in a range definition, you must escape it with EscapeChar.   
mocSet | Treat [a-z] to match either 'a', '-' or 'z'  
Notice that when mocRange is also enabled, an unescaped '-' will always be treated as a range indicator.   
mocNegateGroup | Treat [!a-c] to not match 'a', 'b', or 'c', but match any other char. Requires mocRange and/or mocSet   
mocEscapeChar | Treat EscapeChar (defaults to '\') to take the next char as a literal, so '\\*' is treated as a literal '*'.   
  
  


#### Predefined MaskOpcodes

The masks unit has some predefined MaskOpcodes constants.  


Name | Interpretation   
---|---  
AllMaskOpCodes | Self explanatory.   
MaskOpCodesDisableRange | Do not interpret [ as the start of a range or set.   
MaskOpCodesNoEscape | Interpret [?] as a set with a literal question mark instead of 0..1 chars wildcard. Disable escaping.   
DefaultMaskOpCodes | Equals to MaskOpCodesNoEscape.   
  
If no TMaskOpcodes parameter is supplied to one of the Matches methods or functions, DefaultMaskOpCodes is assumed. 

### Specific Windows Quirks

Windows mask works in a different mode than regular masks, it has many quirks and corner cases inherited from CP/M, then adapted to DOS (8.3) file names and adapted again for long file names. 

TWindowsQuirks is defined as set of TWindowsQuirks.  
The TWindowsQuirk enumeration type consists of: 

Name | Example mask | Interpretation   
---|---|---  
wqAnyExtension | Anything*.* | The filename is not required to have an extension   
wqFilenameEnd | Anything??.abc | The '?' matches 1 or 0 chars (except '.')   
wqExtension3More | Anything.abc | Matches Anything.abc but also Anything.abc* (so '*.pas' also matches with 'file.pas.bak')   
wqEmptyIsAny |  | Empty string matches anything, so acts like '*'   
wqAllByExtension | .abc | Is treated as *.abc   
wqNoExtension | Anything*. | Matches Anything* without extension   
  
TWindowsQuirks can only be used in the Windows specfic classses and functions. 

#### Predefined TWindowsQuirks

The masks unit has some predefined WindowsQuirks constants.  


Name | Interpretation   
---|---  
AllWindowsQuirks | Self explanatory.   
DefaultWindowsQuirks | Equals to [wqAnyExtension,wqEmptyIsAny,wqNoExtension]   
  
If no TWindowsQuirks parameter is supplied to one of the Windows specific Matches methods or functions, DefaultWindowsQuirks is assumed. 

### TMask classes

All pattern matching is done by the TMask classes.  
The convenience functions like MatchesMask() internally use an instance of a TMask class.  
Not all of the properties of the TMask classes are exposed in these convenience functions, so when you need more control over how the mask is interpreted, you should use a TMask class directly.  
This also makes sense if you do multiple calls to a matching routine, this avoids repetetive creating of a TMask class instance. 

#### TMask

Does pattern matching unix style (or should one say "non-Windows" style).  
The mask '*.*' requires that there must be at least one period ('.') in the filename to match.  


The following properties of TMask (and all other TMask classes) are not exposed in the aforementioned convenience functions: 

Name | Type | Default value | Meaning   
---|---|---|---  
AutoReverseRange | Boolean | True | When True, it interprets the range [z-a] as [a-z], otherwise as an empty range.   
Requires mocRange enabled.   
EscapeChar | Char | '\' | The char used to escape characters that would otherwise have special meaning. E.g. '\\*' will be interpreted as a literal '*'.   
The value of EscapeChar must be in the range #0..#127 (so 7-bit ASCII), otherwise an exception is raised.  
The use of EscapeChar requires mocEscapeChar to be enabled.   
  
  
To maintain backwards compatibility the TMask class provides the method MatchesWindowsMask(), but this hase been deprecated in favour of the direct use of the Matches method of TWindowsMask. This method will be removed in a future release. 

#### TWindowsMask

Does pattern matching in Windows style.  
So (by default) the mask '*.*' wil match any string of any (including zero) length, with or withoud a period it.  
Notice that #0 is not allowed as a character in the Mask in TWindowsMask (and therefore #0 can also not be used as EscapeChar).  
If you set it's property Quirks to [] (empty set), it will behave just like TMask. 

#### TMaskList and TWindowsMaskList

These classes support pattarn matching against a list of patterns separated by a Separator (which defaults to ';').  
Typically it will be used to match e.g. a list of filename extensions: '*.pas;*.pp;*.inc'.  
  


Like TMask, the TMaskList class provides the method MatchesWindowsMask() for backwards compatibility. It's fate will be the same... 

## Code examples

ToDo

---

_Source: [https://wiki.freepascal.org/Masks](https://web.archive.org/web/20231206110725/https://wiki.freepascal.org/Masks)_
