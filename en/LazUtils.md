# LazUtils

## Overview

LazUtils is collection of units that provides non-visual utility functions/classes; it is part of the units supplied by Lazarus but not the LCL (Lazarus Component Library). LazUtils do not have a dependancy on LCL, so they are also suitable for use in command-line, non-GUI applications. 

## Contents

  * 1 Overview
    * 1.1 TLookupStringList
    * 1.2 UTF8WrapText function
  * 2 See also



### TLookupStringList

The standard TStringList has a serious limitation about duplicated items, Duplicates property only works for sorted lists. It is not possible to avoid duplication in a list which is not sorted or has a custom sort. 

TLookupStringList is an unsorted _TStringList_ with a fast lookup feature. Internally it uses a string map container to store the string again. It is then used for _InsertItem_ , _Contains_ , _IndexOf_ and _Find_ methods. The extra container does not reserve excessive memory because the strings are reference-counted and not actually copied. 

_TLookupStringList_ fully supports all Duplicates property values including _dupIgnore_ and _dupError_. A normal unsorted _TStringList_ lacks this support for some values of Duplicates. 

This class is particularly useful when you need to preserve the order of strings at the same time doing fast lookups (to check if a string is already there), or when you need to prevent addition of duplicate strings. 

For a simple dedupe task, just load the strings you want to dedupe and it is done. 

It is very memory efficient, because: 

  * Internally it uses a balanced tree container (TStringMap) to store the strings again. A balanced tree is memory efficient compared to a hash map.
  * Strings have reference count and lazy copy semantics. It means only a reference to a string is copied to the other container.



You can find an example for TLookupStringList in your Lazarus folder: {LazarusDir}\components\lazutils\examples. 

### UTF8WrapText function

It wraps UTF8 text provided as a string into a MaxCol length. It inserts LineEndings at every last space/tab/hyphen character before MaxCol value or just on it and returns the resulting string. 

## See also

  * [LazUtils Documentation Roadmap](<LazUtils_Documentation_Roadmap.md> "LazUtils Documentation Roadmap")
  * [LazFileUtils](<LazFileUtils.md> "LazFileUtils")

---

_Source: [https://wiki.freepascal.org/LazUtils](https://web.archive.org/web/20250209011110/https://wiki.freepascal.org/LazUtils)_
