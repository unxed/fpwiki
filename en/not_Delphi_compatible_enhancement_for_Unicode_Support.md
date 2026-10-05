# not Delphi compatible enhancement for Unicode Support

## Contents

  * 1 Introduction
  * 2 Scope
  * 3 Paradigm
  * 4 Analysis
  * 5 The problem
  * 6 A possible solution
  * 7 The type "String"
  * 8 More additional string types
  * 9 Suggestion for Syntax and implementation details
    * 9.1 Defining String variables and String types
    * 9.2 Implementation details
    * 9.3 Undecided syntax details
  * 10 See Also



## Introduction

This is an idea for an alternative implementation of Unicode string types by Michael Schnell. If you are looking for documentation about Unicode in FPC or Lazarus see here: 

  * [FPC Unicode support](<FPC_Unicode_support.md> "FPC Unicode support")
  * [LCL Unicode Support](<LCL_Unicode_Support.md> "LCL Unicode Support")



I (Michael Schnell) feel that there are several shortcomings with the Delphi style Code Aware Strings that with fpc can be overcome quite easily and in a rather compatible way. 

I myself don't have access to Delphi XE, so my impressions of same are derived from talks with my colleagues who use it. 

## Scope

The scope of this paper is just the handling of the stored content of "Strings" including the handling of the "encoding brand" and "bytes per element" for such variables. The interpretation of such content (e.g. the composition of printable characters from same) not in the scope but assumed to be handled as in the current (as of 2016) fpc/fpc-rtl implementation. There are lots of common problems arising from this issue, but this is a completely different topic. 

## Paradigm

In this article, a "String" is thought of as a reference counted ordered array of a number of "Things" (aka elements). (I feel that this is what the name String suggests.) 

Other than an array, String provides syntax candy for lazy copy, pass by value, concatenation, comparing, searching for and accessing Elements and sub-Strings. 

If the elements of a single string need all to have the same size, I can live with that. Any different implementation would degrade performance greatly. 

If we allow for multiple strings with differently element-size and add a number that for each string denotes a "kind" (aka encoding-brand) to a String, and add a library that provides means to translate strings of different kinds into each other, plus syntax candy "auto conversion", voila, we have the Delphi paradigm and versatile syntax for "Code Aware Strings". (Including static and dynamic encoding-brand selection.) 

If the elements of the strings are printable characters or partial codes of UTF. OK, this is nice (provided the conversion functions are in place) and makes doing programs handling conventional problems very easy, and it's fully compatible with Delphi and legacy fpc user code. But this is not the only usage of this paradigm, and it's versatility helps with doing portable and high performance code. 

Regarding the implementation, in fact portability, performance, and compatibility issues require allowing for statically (at compile time) defining the encoding brand (and element size) of a string variable, and providing a singe "DynamicString" type that can hold content with any encoding brand (and element size). (Unfortunately Embarcadero failed to do a decent implementation of this with "their RawByteString, which behaves in a more or less useless way.) 

## Analysis

With Delphi XE, Embarcadero decided that the one-byte-per-character String type that uses an (ANSI-compatible) encoding restricted to a certain set of languages (usually defined by the system locale) is not versatile enough. So they introduced a “code-page aware” string type that allows for each string to be held in a different encoding. A broad set of encoding variants is offered, including one Byte ANSI code pages, and Unicode in 1 and 2 Byte variants. In principle each string variable in a project can be defined to use a different encoding. Hence the type “String” now comes in several “brands” (that in a variable or typename definition can be denoted by a number in brackets). When leaving out the bracket notation, the default is 2 byte Unicode, which makes a lot of sense with Windows OS, that internally uses this encoding style. 

Delphi provides automatic code-conversion of the string content whenever it seems necessary. 

Unfortunately, Embarcadero forces a peculiar mix of static and dynamic “branding” of the string variables. Each variable gets a certain encoding brand by defining it's name, on the other hand each string variable content additionally contains a (potentially) dynamically definable setting of as well the count of bytes per element as the encoding style to be used to interpret the content whenever necessary. IMHO this is the cause of major misconception. Supposedly they feared to be bashed due to some run-time overhead necessary with fully dynamic string code handling. Obviously with the partly-dynamic string code handling, the decision whether or not to do an automatic conversion (and which one) when accessing a string, in most cases can be done at compile time rather than at run time. 

I found that in fpc, the current implementation of the "New" String type is done in a "dual issue" way, that provides completely separate support for one-byte-per-element Strings and for two-bytes-per-element strings (and no support whatsoever for more than two bytes per elements). This perhaps makes the extensions suggested here harder to do, as they are based on a more "universal" paradigm, that "intuitively" uses (statically and dynamically) as well the "bytes-per-element" as the "code-ID-number" setting of the string (type or content). 

## The problem

While simple assignment and compiler built-in functions such as “pos()” and “copy()” with each occurrence can decide what to do – i. e. do or not do auto-conversion of the content based on the encoding brand of the arguments (by implicitly faking fully dynamically encoded arguments), not-built-in functions and properties need to be defined in a fixed string brand. Hence when using them with differently encoded Strings, the compiler will introduce very time consuming and loss-prone auto conversion (unless the function itself is provided in multiple brands). 

This problem is much more urgent with fpc than with Delphi, as fpc is supposed to run with multiple OSes that might use different system-wide default encoding styles (e. g. UTF-8 with Linux and UTF-16 with Windows). While Delphi XE just creates Windows software, and projects that explicitly handle other but system-default-endcoded strings (e. g. read from a file or retrieved from a network resource) are rather seldom done, fpc needs to be able to handle user software that is compilable for multiple OSes with different encoding defaults. 

The most obvious candidate for pain on that behalf is “TStrings”. This basic class is used to derive lots of string handling classes from. Storing the string and retrieving the content could easily – and with close to no overhead – be done in a fully dynamic way: each string comes with it's branding notes, anyway. But the fixed branding of the string type used in Delphi compatible TString's interface forces auto-conversion in all cases but one. Another example is the interface of a GUI connection libraries – such as Lazarus or mse: portable programs would benefit from a common Interface for all OSes, but forcing any fixed encoding independent from the OS does not seem appropriate either, as permanent code conversion on the way between the user code and the OS could not be avoided at all. Of course the fpc RTL could be mentioned as well. 

## A possible solution

Considering this, I think, it should be possible to find a solution that of course is not the “Delphi Way” but rather compatible both to Delphi XE as to earlier fpc and Delphi versions, so that normal user software would not have to be aware of this modification. IMHO it should be rather easy to do (on top of the Delphi compatible Code Aware String library) and not eat up any noticeable amount of CPU cycles at run time. 

Delphi just provides the coding brand “RawByteString” as a string type that potentially could be used in a dynamically encoded way, but the docs look rather discouraging, and the implementation in fpc seems to greatly differ from the implementation in Delphi. 

Hence the suggestion is to introduce yet another brand of string encoding styles, that is fully dynamic and prone to auto-conversion. This might be called the “DynamicString” encoding brand and might be assigned by the encoding brand number "CP_ANY". As with $0000 and $FFFF the obvious choices for this are already taken by Delphi "CP_ACP" and "CP_NONE", valid suggestions might be $F000 or $FF00 (to leave some room for encoding brands more similar to RawByteString which is CP_NONE=$FFFF). 

CP_ANY is just a setting for the **static** Type of a variable. Setting the dynamic code page of a not-empty String to CP_ANY should be forbidden/avoided. 

By this, all normal String handling introduced by an operator and compiler-built-in function stays completely unchanged, unless we explicitly use the DynamicString brand. Also “old-style” user or library functions using the traditional Delphi String brands, the calling also stays as it is defined by Delphi. 

If in an assignment **one or more of the partners is a DynamicString** , the compiler needs to adhere to some simple rules and needs to generate code to check the dynamic encoding brand of the appropriate String(s). 

If the target is Dynamic: Just assign the source (be it dynamic or not or even a RawByteString). _Question: (how) can this be done using lazy copy ?_

If the target is not dynamic (but no Raw String), generate code to check the encoding brand of the source (this is just a single Word, so no noticeable overhead). If this is the same as the static (compile-time) brand of the target, just assign (_lazy copy !_). If the encoding is different, have the appropriate conversion library function be called (which would be necessary with a pure static encoding paradigm in a similar case as well). 

If the target is a Raw String (static compile-Time brand), generate code to check the encoding brand of the source (this is just a single Word, so no noticeable overhead). If this is Raw String as well, just assign, if the encoding is different, behave like Delphi would do (I don't know what it does when assigning something to a RawByteString). 

One more case: if the source is a DynamicString and happens to have been assigned a “RAW” content, but the target is static and not RAW, behave like Delphi would do when assigning Raw to any other encoding. 

Now it's obvious how a “dynamic” version of TStrings would work. 

As the type of the parameter of “Add” is dynamic, here we would never need a conversion. The string is just forwarded to the overridden add procedure and there (e.g. with TStringList) be stored as it is, including it's header that defines its encoding brand. With TStringList, there is a very slight overhead, as the size of the storage area is calculated with regard to the “Bytes per Element” setting: a dynamic multiplication instead of just a fixed factor “2”. 

When retrieving a string from a TStrings based storage, the compiler would automatically check the dynamically defined encoding brand of the source and do an auto-conversion if necessary (see above). 

Reworking the code for storing and retrieving with TStringList would be easy to do. Of course more complex classes and functions (such as sorting) would need some more effort. 

Hence, if setting the same (static) encoding brand for all the variables you use with a TStrings based store, there will provide exactly the same functionality and no noticeable overhead regarding the Delphi way of always using UTF-16. 

Another advantage of the dynamic TStringList besides the obvious enhanced versatility for (portable) code that is done with another String encoding brand as UTF-16, is, that you can force it to use any encoding you like by just assigning your strings to an appropriate variable before storing (no need for any considering while retrieving). So you can e. g. do a compact UTF-8 store while using Delphi-style UTF-16 (more easily handled regarding pos() and friends) in your user code. The conversion between UTF-8 and UTF-16 is fast and not prone to information loss. 

## The type "String"

While this is not really related to the suggestion of an additional DynamicString encooding brand, I suppose same would work nicely together with the rather often heard suggestion to define the type "String" (denoted by "CP_DEFAUT") differently according to the OS fpc compiles for (and supposedly according to an appropriate $MODE setting). So CP_DEFAULT results in CP_UTF8 in Linux and CP_UTF16 in Windows, and hence, system-wide, "String" without an encoding brand is defined appropriately. 

This way, user programmers that don't bother to think about the encoding of their strings (supposedly the vast majority), will get programs that perform rather good (no conversions, but not always using the optimum encoding style for the task in question) independent of the underlying OS and are very compatible to Delphi XE when compiled for Windows and rather (if not explicitly using String element positions) compatible to older (UTF-8-based) fpc/Lazarus programs and even rather (if strictly using ASCII) compatible to older (ANSI-based) Delphi and fpc/lazarus versions. 

With TStrings and friends using DynamicString, portability would not be degraded by using different String encoding brands for different OSes. 

## More additional string types

While we are defining a not Delphi compatible String type brand, of course we should add the obviously missing types RawWordString, RawDWordString, and RawQWordString. (CP-numbers e.g. $FFFE, $FFFD; and $FFFC). The functionality of these types is really simple and rather obvious. 

But if the RawByteString type for Delphi compatibility needs to be defined to provide any conversion capability at all, we need to invent a different name scheme and CP-numbers for these "non-printable" types. 

Of course UTF-32 would be a decent standard encoding type that should be supported out of the box. 

A really nice feature would be to provide user settable encoding types, associated to an additional "CP" number (and byte per element count) and a user supplied set of conversion functions. 

## Suggestion for Syntax and implementation details

### Defining String variables and String types

The basic syntax to define a String type or variable is: 

Type TMyString = **String**(EncodingBrandNumber); 

and 

Var MyString: **String**(EncodingBrandNumber); 

For compatibility, **ANSIString** instead of "String" also is allowed. 

  
**EncodingBrandNumber** is a constant between 0 and $FFFF. 

With each EncodingBrandNumber, an appropriate setting of the ElementSize is defined. 

Several such numbers are predefined: 

**CP_DEFAULT** : when compiling for Windows = CP_UTF16, when compiling for Linux = CP_UTF8 

**CP_NONE** = $FFFF; // ElementSize undefined // Delphi workalike RawByteString encoding 

**CP_ANY** = $FF00 // ElementSize dynamically assigned // fully dynamical String for intermediate storing string content // just assigned to the Type or variable, never used in the "Encoding" field in the string header. 

**CP_ACP** = 0; // ElementSize = 1 // default ANSI code page 

**CP_OEMCP** = 1; // ElementSize = 1 // default OEM ANSI code page 

**CP_UTF16** = 1200; // ElementSize = 2 // utf-16 

**CP_UTF16BE** = 1201; // ElementSize = 2 // unicodeFFFE 

**CP_UTF32** = ????; // ElementSize = 4 // utf-32 

**CP_UTF32BE** = ????; // ElementSize = 4 // unicode ???? 

**CP_UTF7** = 65000; // ElementSize = 1 // utf-7 

**CP_UTF8** = 65001; // ElementSize = 1 // utf-8 

**CP_ASCII** = 20127; // ElementSize = 1 // us-ascii 

**CP_BYTE** = $FF01; // ElementSize = 1 // Byte String 

**CP_Word** = $FF02; // ElementSize = 2 // Word String 

**CP_DWord** = $FF04; // ElementSize = 4 //Double Word String 

**CP_QWord** = $FF08; // ElementSize = 8 // Quad Word String 

Appropriate constants for ANSI code pages supported by the RTL are defined, as well. 

Several String Types are predefined: 

**String** = String(CP_Default); // similar Syntax as **Class** is a synonym for CLASS(TObject) 

**AnsiString** Syntax candy synonym for "String" 

**UTFString** = String(CP_UTF16); 

**UTF8String** = String(CP_UTF8); 

**RawByteString** = String(CP_NONE); 

**DynamicString** = String(CP_ANY); 

**ByteString** = String(CP_BYTE); 

**WordString** = String(CP_WORD); 

**DWordString** = String(CP_DWORD); 

**QWordString** = String(CP_QWORD); 

With that, "String", "AnsiString", "UTFString", and "RawByteString" are defined compatible with Dephi XE and legacy fpc versions. 

### Implementation details

### Undecided syntax details

(How) can a sibling override DynamicString to a static encoding and/or vice versa ? 

For Strings holding printable text, syntax candy for handling "printable entities" would be nice (in the syntax, avoiding the term "printable" to allow for other stuff but text handled in that way). Here some encoding brand aware enumerator syntax and "pos...", "copy...", "delete..." "length..." and similar functions should be in place, even though they might be very inefficient to use. 

## See Also

  * [FPC Unicode support](<FPC_Unicode_support.md> "FPC Unicode support")

---

_Source: [https://wiki.freepascal.org/not_Delphi_compatible_enhancement_for_Unicode_Support](https://web.archive.org/web/20250516232026/https://wiki.freepascal.org/not_Delphi_compatible_enhancement_for_Unicode_Support)_
