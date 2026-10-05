# FPC Unicode support

│ **English (en)** │  **[русский (ru)](<../ru/FPC_Unicode_support.md>)** │

## Contents

  * 1 Introduction
  * 2 Backward compatibility
  * 3 Code pages
    * 3.1 Code page identifiers
    * 3.2 Code page settings
      * 3.2.1 DefaultSystemCodePage
      * 3.2.2 DefaultFileSystemCodePage
      * 3.2.3 DefaultRTLFileSystemCodePage
    * 3.3 Source file codepage
  * 4 String/character types
    * 4.1 Shortstring
    * 4.2 PAnsiChar/AnsiChar
    * 4.3 PWideChar/PUnicodeChar and WideChar/UnicodeChar
    * 4.4 UnicodeString/WideString
    * 4.5 Ansistring
      * 4.5.1 Declared code page
      * 4.5.2 Dynamic code page
      * 4.5.3 RawByteString
    * 4.6 String concatenations
    * 4.7 String constants
    * 4.8 String indexing
  * 5 RTL changes
  * 6 RTL todos
    * 6.1 TFormatSettings and DefaultFormatSettings
  * 7 Old/obsolete sections
    * 7.1 User visible changes
  * 8 See Also



## Introduction

Up to and including FPC 2.6.x, the RTL was based on the ones of Turbo Pascal and Delphi 7. This means it was primarily based around the _shortstring_ , _ansistring_ and _pchar_ types. None of these types had any encoding information associated with them, but were implicitly assumed to be encoded in the "default system encoding" and were passed on to OS API calls without any conversion. 

In Delphi 2009, Embarcadero switched the entire RTL over to the _UnicodeString_ type, which represents strings using UTF-16. Additionally, they also made the AnsiString type "code page-aware". This means that AnsiStrings from then on contain the code page according to which their data should be interpreted. 

FPC's language-level support for these string types is already available in current stable versions of the compiler (FPC 3.0.0 and up). The RTL level support is not yet complete. This page gives an overview of the code page-related behaviour of these string types, the current level of support in the RTL, and possible future ways of how this support may be improved. 

## Backward compatibility

If you have existing code that works in a defined way (*) with a previous version of FPC and make no changes to it, it should continue to work unmodified with the new FPC version. Guaranteeing this is the main purpose of the multitude of Default*CodePage variables and their default values as described below. 

(*) this primarily means: you do not store data in an ansistring that has been encoded using something else than the system's default code page, and subsequently pass this string as-is to an FPC RTL routine. E.g., current Lazarus code is generally fine, as you are supposed to call UTF8ToAnsi() before passing its strings to FPC RTL routines. 

If your existing code did use ansistrings in an unsupported way, namely by storing data in it that is not encoded in the system's default code page and not taking care when interfacing with other code (such as RTL routines), you still may be able to work around most of the issues if this data always uses the same encoding. In that case, you can call SetMultiByteConversionCodePage() when starting your program, with as argument the code page of the data that your ansistrings contain. Note that this will also affect the interpretation of all ShortString, AnsiChar and PAnsiChar data. 

## Code pages

A code page defines how the individual bytes of a string should be interpreted, i.e., which letter, symbol or other graphic character corresponds to every byte or sequence of bytes. 

### Code page identifiers

A code page identifier is always stored as a _TSystemCodePage_ , which is an alias for [Word](<Word.md> "Word"). The value represents the corresponding code page as defined by [Microsoft Windows](<http://msdn.microsoft.com/en-us/library/windows/desktop/dd317756\(v=vs.85\).aspx>). Additionally, there are 3 special code page values: 

  * CP_ACP: this value represents the currently set "default system code page". See #Code page settings for more information.
  * CP_OEM: this value represents the OEM code page. On Windows platforms this corresponds to the code page used by the console (e.g. cmd.exe windows). On other platforms this value is interpreted the same as CP_ACP.
  * CP_NONE: this value indicates that no code page information has been associated with the string data. The result of any operation on a string that has this dynamic code page is undefined. The same holds for any other code page that is not in the above list, but unlike the other invalid code page values, CP_NONE has a special meaning in case it is used as declared code page.



Note: code page identifiers are different from codepage names as used in the _[{$codepage xxx}](<http://www.freepascal.org/docs-html/prog/progsu87.html>)_ directives (which is available in current stable FPC already). Codepage names are the names of individual codepage units exposed by the charset unit, which have names such as _cp866_ and _cp1251_ and _utf8_. 

### Code page settings

The system unit contains several global variables that indicate the default code page used for certain operations. 

#### DefaultSystemCodePage

  * **Purpose** : determines how CP_ACP is interpreted
  * **Initial value** : 
    * Windows: The result of the _GetACP_ OS call, which returns the Windows ANSI code page.
    * iOS: CP_ACP if no [widestring manager](<http://www.freepascal.org/docs-html/rtl/system/setwidestringmanager.html>) is installed, otherwise UTF-8
    * Unix (excluding iOS): CP_ACP if no [widestring manager](<http://www.freepascal.org/docs-html/rtl/system/setwidestringmanager.html>) is installed, otherwise it is based on the currently set _LANG_ or _LC_CTYPE_ environment variables. This is usually UTF-8, but that is not guaranteed to be the case.
    * OS/2: Current code page as provided in the first value returned by DosQueryCP and then translated to the code page number used for the same character set under MS Windows (because that is what has been used under Delphi originally and the FPC implementation tries to be compatible to Delphi); it's possible to enforce always using native OS/2 code page numbers in FPC RTL by changing RTLUsesWinCP boolean variable to false (default is true). Note that the code page numbers are largely identical for OS/2 and MS Windows with code pages allowed for current process code page under OS/2 (the code page numbers are different for the so-called ANSI code pages, ISO-8859-x code pages and Mac OS code pages, but neither of these may be used as current process code page under OS/2).
    * Other platforms: CP_ACP (these platforms currently do not support multiple code pages, and are hardcoded to use their OS-specific code page in all cases)
  * **Modifications** : you can modify this value by calling _SetMultiByteConversionCodePage(CodePage: TSystemCodePage)_
  * **Notes** : Since the value of this variable can be changed, it is not a good idea to use its value to determine the real OS "default system code page" (unless you do it at program startup and are certain no other unit has changed it in its initialisation code).



#### DefaultFileSystemCodePage

  * **Purpose** : defines the code page to which file/path names are translated before they are passed to OS API calls, _**if**_ the RTL uses a single byte OS API for this purpose on the current platform. This code page is also used for intermediate operations on file paths inside the RTL before making OS API calls. This variable does not exist in Delphi, and has been introduced in FPC to make it possible to change the value of _DefaultSystemCodePage_ without breaking RTL interfaces with the OS file system API calls.
  * **Initial value** : 
    * Windows: UTF-8, because the RTL uses UTF-16 OS API calls (so no data is lost in intermediate operations).
    * macOS and iOS: DefaultSystemCodePage if no [widestring manager](<http://www.freepascal.org/docs-html/rtl/system/setwidestringmanager.html>) is installed, otherwise UTF-8 (as defined by Apple)
    * Unix (excluding macOS and iOS): DefaultSystemCodePage, because the encoding of file names is undefined on Unix platforms (it's an untyped array of bytes that can be interpreted in any way; it is not guaranteed to be valid UTF-8)
    * OS/2: DefaultSystemCodePage, because OS/2 provides no possibility for specifying a different code page for file I/O operations other than the current process-wide code page.
    * Other platforms: same as DefaultSystemCodePage
  * **Modifications** : you can modify this value by calling _SetMultiByteFileSystemCodePage(CodePage: TSystemCodePage)_ ; note that under OS/2 this variable is being synchronized with the current process code page (set e.g. by DosSetProcessCP) during all file I/O operations in order to avoid invalid transformations
  * **Notes** : the Unix/macOS/iOS settings only apply in case the _cwstring_ widestring manager is installed, otherwise DefaultFileSystemCodePage will have the same value as DefaultSystemCodePage after program startup.



#### DefaultRTLFileSystemCodePage

  * **Purpose** : defines the code page to which file/path names are translated before they are returned from RawByteString file/path RTL routines. Examples include the file/path names returned by the RawbyteString versions of _SysUtils.FindFirst_ and _System.GetDir_. The main reason for its existence is to enable the RTL to provide backward compatibility with earlier versions of FPC, as these always returned strings encoded in whatever the OS' single byte API used (which was usually what is now known as _DefaultSystemCodePage_).
  * **Initial value**
    * Windows: DefaultSystemCodePage, for backward compatibility.
    * macOS and iOS: DefaultSystemCodePage if no [widestring manager](<http://www.freepascal.org/docs-html/rtl/system/setwidestringmanager.html>) is installed, otherwise UTF-8 for backward compatibility (it was already always UTF-8 in the past, since that's what the OS file APIs return and we did not convert this data).
    * Unix (excluding macOS and iOS): DefaultSystemCodePage, for the same reason as with DefaultFileSystemCodePage. Setting this to a different value than DefaultFileSystemCodePage is a bad idea on these platforms, since any code page conversion can corrupt these strings as their initial encoding is unknown.
    * OS/2: same as DefaultSystemCodePage (for backward compatibility and also because it's the most natural choice unless you need to play with different code pages)
    * Other platforms: same as DefaultSystemCodePage
  * **Modifications** : you can modify this value by calling _SetMultiByteRTLFileSystemCodePage(CodePage: TSystemCodePage)_ ; you may use this possibility for reading and/or writing files with an arbitrary code page
  * **Notes** : same as for DefaultFileSystemCodePage.



### Source file codepage

The _source file codepage_ determines how string constants are interpreted, and where the compiler will insert codepage conversion operations when assigning one string type to another. 

The source file codepage is determined as follows: 

  * if a file contains a _{$codepage xxx}_ directive (e.g. `{$codepage UTF8}`), then the source file codepage is this codepage, otherwise
  * if the file starts with an UTF-8 BOM, then the source file codepage is UTF-8, otherwise
  * if _{$modeswitch systemcodepage}_ is active, the source file codepage is the _DefaultSystemCodePage_ _**of the computer on which the compiler itself is currently running**_ (i.e., compiling the source code on a different system may result in a program that behaves differently; this switch is available for Delphi compatibility and is enabled by default in _{$mode delphiunicode}_), otherwise
  * the source file codepage is set to CP_ACP (for backward compatibility with previous FPC versions)



## String/character types

### Shortstring

The code page of a shortstring is implicitly CP_ACP and hence will always be equal to the current value of DefaultSystemCodePage. 

### PAnsiChar/AnsiChar

These types are the same as the old PChar/Char types. In all compiler modes except for _{$mode delphiunicode}_ , PChar/Char are also still aliases for PAnsiChar/AnsiChar. Their code page is implicitly CP_ACP and hence will always be equal to the current value of DefaultSystemCodePage. 

### PWideChar/PUnicodeChar and WideChar/UnicodeChar

These types remain unchanged. WideChar/UnicodeChar can contain a single UTF-16 code unit, while PWideChar/PUnicodeChar point to a single or an array of UTF-16 code units. 

In _{$mode delphiunicode}_ , PChar becomes an alias for PWideChar/PUnicodeChar and Char becomes an alias for WideChar/UnicodeChar. 

### UnicodeString/WideString

These types behave the same as in previous versions: 

  * _Widestring_ is the same as a "COM BSTR" on Windows, and an alias for UnicodeString on all other platforms. Its string data is encoded using UTF-16.
  * _UnicodeString_ is a reference-counted string with a maximum length of high(SizeInt) UTF-16 code units.



### Ansistring

AnsiStrings are reference-counted types with a maximum length of high(SizeInt) bytes. Additionally, they now also have code page information associated with them. 

The most important thing to understand about the new AnsiString type is that it both has a declared/static/preferred/default code page (called _declared code page_ from now on), and a dynamic code page. The declared code page tells the compiler that when assigning something to that AnsiString, it should first convert the data to that declared code page (except if it is CP_NONE, see RawByteString below). The dynamic code page is a property of the AnsiString which, similar to the length and the reference count, defines the actual code page of the data currently held by that AnsiString. 

#### Declared code page

The declared code page of an AnsiString can only be defined by declaring a new type as follows: 
    
    
    type
      CP866String = type AnsiString(866); // note the extra "type"
    

The declared code page of a variable declared as plain _AnsiString_ is CP_ACP. In effect, the AnsiString type is now semantically defined in the System unit as: 
    
    
    type
      AnsiString = type AnsiString(CP_ACP);
    

Another predefined AnsiString(X) type in the System unit is UTF8String: 
    
    
    type
      UTF8String = type AnsiString(CP_UTF8);
    

Once you have defined such a custom AnsiString(X) type, you can use it to declare variables, parameters, fields etc as usual. 

Note that CP_UTF16 and CP_UTF16BE are not valid as code pages for AnsiStrings. The result of defining an AnsiString with such a code page is undefined. 

#### Dynamic code page

If a string with a declared code page SOURCE_CP is assigned to a string with declared code page DEST_CP , then 

  * if (SOURCE_CP = CP_NONE) or (DEST_CP = CP_NONE), see RawByteString, otherwise
  * if (source file codepage <> CP_ACP), then if (DEST_CP = CP_ACP) and (SOURCE_CP = source file codepage) or vice versa, no conversion will occur (even if at run time _DefaultSystemCodePage_ has a different value from the source file code page). The reason for the "(source file codepage <> CP_ACP)" condition is backward compatibility with previous FPC versions (while they did not support AnsiStrings with arbitrary code pages, they did always reinterpret AnsiStrings according to the current value of the system code page). Otherwise,
  * if (SOURCE_CP <> DEST_CP), the string data will be converted from codepage SOURCE_CP to codepage DEST_CP before assignment, whereby CP_ACP will be interpreted as the current value of _DefaultSystemCodePage_. Otherwise,
  * if (SOURCE_CP = DEST_CP), no codepage conversion will be performed.



These rules mean that it is perfectly possible for an AnsiString variable to get a dynamic code page that differs from its declared code page. E.g. in the third case SOURCE_CP could be CP_ACP, while after the assignment it may have a dynamic code page equal to _DefaultSystemCodePage_. 

Note: as mentioned above, whether or not a potential code page conversion happens only depends on the _declared code pages_ of the involved strings. This means that if you assign one AnsiString(X) to another AnsiString(X) and the former's dynamic code was different from X, the string data will _not_ be converted to code page X by the assignment. 

#### RawByteString

The RawByteString type is defined as: 
    
    
    type
      RawByteString = type AnsiString(CP_NONE);
    

As mentioned earlier, the results of operations on strings with the CP_NONE code page are undefined. As it does not make sense to define a type in the RTL whose behaviour is undefined, the behaviour of RawByteString is somewhat different than that of other AnsiString(X) types. 

As a first approximation, _RawByteString_ can be thought of as an "untyped AnsiString": assigning an AnsiString(X) to a RawByteString has exactly the same behaviour as assigning that AnsiString(X) to another AnsiString(X) variable with the same value of X: no code page conversion or copying occurs, just the reference count is increased. 

Less intuitive is probably that when a RawByteString is assigned to an AnsiString(X), the same happens: no code page conversion or copying, just the reference count is increased. Note that this means that results from functions returning a RawByteString will never be converted to the destination's declared code page. This is another way in which the dynamic code page of an AnsiString(X) can become different from its declared code page. 

This type is mainly used to declare _const_ , _constref_ and value parameters that accept any AnsiString(X) value without converting it to a predefined declared code page. Note that if you do this, the routine accepting those parameters should be able to handle strings with any possible dynamic code page. 

_var_ and _out_ parameters can also be declared as _RawByteString_ , but in this case the compiler will give an error if an AnsiString(X) whose declared code page is different from CP_NONE is passed in. This is consistent with _var_ and _out_ parameters in general: they require an exactly matching type to be passed in. You can add an explicit RawByteString() typecast around an argument to remove this error, but then you must be prepared to deal with the fact that the returned string can have any dynamic code page. 

### String concatenations

Normally, in Pascal the result type of an expression is independent of how its result is used afterwards. E.g. multiplying two longints on a 32 bit platform and assigning the result to an int64 will still perform the multiplication using 32 bit arithmetic, and only afterwards the result is converted to 64 bit. 

Code page-aware strings are the only exception to this rule: concatenating two or more strings always occurs without data loss, although afterwards the resulting string will of course still be converted to the declared code page of the destination (which may result in data loss). 

Assigning the result of a concatenation to a RawByteString is again special: 

  * if all concatenated strings have the same dynamic code page, the result will have this code page too
  * in other cases the result will be converted to CP_ACP (we may add an option in the future to change this RawByteString behaviour, as it is not very practical).



### String constants

The compiler has to know the code page according to which it should interpret string constants, as it may have to convert them at compile time. Normally, a string constant is interpreted according to the source file codepage. If the source file codepage is CP_ACP, a default is used instead: in that case, during conversions the constant strings are assumed to have code page 28591 (_ISO 8859-1 Latin 1; Western European_). 

When a string constant is assigned to an AnsiString(X) either in code or as part of a typed constant or variable initialisation, then 

  * if X = CP_NONE (i.e., the target is a RawByteString), the result is the same as if the constant string were assigned to an AnsiString(CP_ACP)
  * if X = CP_ACP and the code page of the string constant is different from CP_ACP, then the string constant is converted, at compile time, to the source file code page. If the source file code page is also CP_ACP, it will be stored in the program unaltered with a code page of CP_ACP and hence its meaning/interpretation will depend on the actual value of _DefaultSystemCodePage_ at run time. This ensures compatibility with older versions of FPC when assigning string constants to AnsiString variables without using a _{$codepage xxx}_ directive or UTF-8 BOM.
  * for other values of X, the string constant is converted, at compile time, to code page X



Similarly, if a string constant is assigned to a UnicodeString, the string constant is converted, at compile time, from the source file code page to UTF-16. 

For ShortString and PChar, the same rule as for AnsiString(CP_ACP) is followed. 

Note that symbolic string constants will be converted at compile time to the appropriate string type and code page whenever they are used. This means that there is no speed overhead when using a single string constant in multiple code page and string type contexts, only some data size overhead. 

From the above it follows that to ensure predictable interpretation of string constants in your source code, it is best to either include an explicit _{$codepage xxx}_ directive (or use the equivalent _-Fc_ command line option), or to save the source code in UTF-8 with a BOM. 

### String indexing

Nothing changes to string indexing. Every string element of a UnicodeString/WideString is two bytes and every string element of all other strings is one byte. The string indexing mechanism completely ignores code pages and composite code points. 

## RTL changes

In order to fully guarantee data integrity in the presence of codepage-aware strings, all routines in the RTL and packages that accept _AnsiString_ parameters must be adapted. The reason is that if their parameters remain plain _AnsiString_ , then any string with a different declared code page will be converted to _DefaultSystemCodePage_ when it is passed in. This can result in data loss. 

Until now, primarily routines dealing with file system access have been updated to preserve all character data. Below is an exhaustive list of all routines that preserve the string encoding in FPC 3.0. Unless where explicitly noted otherwise, these routines also all have overloads that accept _UnicodeString_ parameters. 

  * **System** : FExpand, LowerCase, UpperCase, GetDir, MKDir, ChDir, RMDir, Assign, Erase, Rename, standard I/O (Read/Write/Readln/Writeln/Readstr/Writestr), Insert, Copy, Delete, SetString
  * **ObjPas** (used automatically in Delphi and ObjFPC modes): AssignFile
  * **SysUtils** : FileCreate, FileOpen, FileExists, DirectoryExists, FileSetDate, FileGetAttr, FileSetAttr, DeleteFile, RenameFile, FileSearch, ExeSearch, FindFirst, FindNext, FindClose, FileIsReadOnly, GetCurrentDir, SetCurrentDir, ChangeFileExt, ExtractFilePath, ExtractFileDrive, ExtractFileName, ExtractFileExt, ExtractFileDir, ExtractShortPathName, ExpandFileName, ExpandFileNameCase, ExpandUNCFileName, ExtractRelativepath, IncludeTrailingPathDelimiter, IncludeTrailingBackslash, ExcludeTrailingBackslash, ExcludeTrailingPathDelimiter, IncludeLeadingPathDelimiter, ExcludeLeadingPathDelimiter, IsPathDelimiter, DoDirSeparators, SetDirSeparators, GetDirs, ConcatPaths, GetEnvironmentVariable
  * **Unix** : fp*() routines related to file system operations (no _UnicodeString_ overloads), POpen
  * **DynLibs** : all routines



## RTL todos

As the above list is exhaustive, no other RTL routines support arbitrary code pages yet. This section contains a list of gotchas that some people have identified and, if possible, workarounds. Note that routines not mentioned here nor above are equally unsafe as the ones that are explicitly mentioned. 

### TFormatSettings and DefaultFormatSettings

The type of _ThousandSeparator_ and _DecimalSeparator_ is AnsiChar type. This means that if _DefaultSystemCodePage_ is UTF-8 and the locale's separator is more than one byte long in that encoding, these fields are not large enough. Examples are the French and Russian non-breaking white space character used to represent the _ThousandSeparator_. 

## Old/obsolete sections

[![Warning-icon.png](https://wiki.freepascal.org/images/b/b2/Warning-icon.png)](</File:Warning-icon.png>)

**Warning:** These sections are kept for historical reference - please update the sections above with this information if it is still applicable. Since FPC 2.7 (development version before the release of 3.0.0), extensive Unicode support has been implemented.

### User visible changes

Full support of code page aware strings is not possible without breaking some existing code. The following list tries to summarize the most important user visible changes. 

  * The string header has two new fields: encoding and element size. On 32 Bit platforms this increases the header size by 4 and on 64 bit platforms by 8 bytes.
  * WideCharLenToString, UnicodeCharLenToString, WideCharToString, UnicodeCharToString and OleStrToString return an UnicodeString instead of an Ansistring before.
  * the type of the dest parameter of WideCharLenToString and UnicodeCharLenToString has been changed from Ansistring to Unicodestring
  * UTF8ToAnsi and AnsiToUTF8 take a RawByteString now



## See Also

  * [Character and string types](<Character_and_string_types.md> "Character and string types")
  * [unicode use cases](<unicode_use_cases.md> "unicode use cases")
  * [LCL Unicode Support](<LCL_Unicode_Support.md> "LCL Unicode Support")
  * [FPC Unicode RTL](<FPC_Unicode_RTL.md> "FPC Unicode RTL")
  * Suggestion: [not Delphi compatible enhancement for Unicode Support](<not_Delphi_compatible_enhancement_for_Unicode_Support.md> "not Delphi compatible enhancement for Unicode Support")

---

_Source: [https://wiki.freepascal.org/FPC_Unicode_support](https://web.archive.org/web/20250422074428/https://wiki.freepascal.org/FPC_Unicode_support)_
