# macOS NSString Regular Expressions

│ **English (en)** │

[![macOSlogo.png](https://wiki.freepascal.org/images/1/15/macOSlogo.png)](</File:macOSlogo.png>)

This article applies to [macOS](</Category:macOS> "Category:macOS") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

  
****

## Contents

  * 1 Regular expressions
  * 2 NSString Regular Expression Overview
    * 2.1 Metacharacters
    * 2.2 Operators
  * 3 Example 1 - match a pattern
    * 3.1 Code
    * 3.2 Output
  * 4 Example 2 - replace matched pattern
    * 4.1 Code
    * 4.2 Output
  * 5 See also
  * 6 External links



## Regular expressions

Regular expressions are patterns used to match specified alpha-numeric character combinations in the string data being searched. 

Each character in a regular expression (that is, each character in the string describing its pattern) is either a metacharacter or operator, having a special meaning, or a regular character that has a literal meaning. 

Regular expressions can be incredibly complex. Indeed, whole books have been written about them! For a gentle introduction to regular expressions, see this [O'Reilly article](<https://www.oreilly.com/content/an-introduction-to-regular-expressions/>). 

## NSString Regular Expression Overview

It is a little known fact that you can use regular expressions to match and replace patterns without having to resort to the more sophisticated, but complicated, [NSRegularExpression](<macOS_NSRegularExpression.md> "macOS NSRegularExpression"): NSString has the _rangeOfString:options:range:_ method for matching patterns and the _stringByReplacingOccurrencesOfString:withString:_ method for replacing patterns. These are demonstrated in the examples below. 

### Metacharacters

For a comprehensive list of characters used that have a special meaning in regular expression patterns, see the [ICU listing](<https://unicode-org.github.io/icu/userguide/strings/regexp.html#regular-expression-metacharacters>). 

### Operators

For a comprehensive list of regular expression operators, see the [ICU listing](<https://unicode-org.github.io/icu/userguide/strings/regexp.html#regular-expression-operators>). 

## Example 1 - match a pattern

### Code
    
    
    Program regex_ex1;
    
    {$mode objfpc}{$H+}
    {$modeswitch objectivec2}
    
    Uses
      MacOSAll, CocoaAll, SysUtils;
    
    Var
      srchStr : String;
      patnStr : String;
      i: NSRange;
      j: NSRange;
    
    Begin
      srchStr := 'I have 43 bags of 60 marbles.';
      patnStr := '\d+';
    
      // Find first match
      i := NSStr(srchStr).rangeOfString_options_range(NSStr(patnStr), (NSRegularExpressionSearch or NSCaseInsensitiveSearch), NSMakeRange(0, srchStr.Length));
    
      // Find second match
      j := NSStr(srchStr).rangeOfString_options_range(NSStr(patnStr), (NSRegularExpressionSearch or NSCaseInsensitiveSearch), NSMakeRange(i.location + i.length, srchStr.Length - (i.location + i.length)));
    
      // Output
      NSLog(NSStr('Search string: %@'), NSStr(srchStr));
      NSLog(NSStr('Pattern string: %@'), NSStr(patnStr));
    
      NSLog(NSStr('Range 1: %@'), NSStringFromRange(i));
      NSLog(NSStr('Location: %d'), i.location);
      NSLog(NSStr('Length: %d'), i.length);
      NSLog(NSStr('Match: %@'), NSStr(srchStr).substringWithRange(i));
    
      NSLog(NSStr('Range 2: %@'), NSStringFromRange(j));
      NSLog(NSStr('Location: %d'), j.location);
      NSLog(NSStr('Length: %d'), j.length);
      NSLog(NSStr('Match: %@'), NSStr(srchStr).substringWithRange(j));
    End.
    

### Output
    
    
    2021-06-15 21:43:49.049 regex_ex1[23182:177970] Search string: I have 43 bags of 60 marbles.
    2021-06-15 21:43:49.049 regex_ex1[23182:177970] Pattern string: \d+
    2021-06-15 21:43:49.049 regex_ex1[23182:177970] Range 1: {7, 2}
    2021-06-15 21:43:49.049 regex_ex1[23182:177970] Location: 7
    2021-06-15 21:43:49.049 regex_ex1[23182:177970] Length: 2
    2021-06-15 21:43:49.049 regex_ex1[23182:177970] Match: 43
    2021-06-15 21:43:49.049 regex_ex1[23182:177970] Range 2: {18, 2}
    2021-06-15 21:43:49.049 regex_ex1[23182:177970] Location: 18
    2021-06-15 21:43:49.049 regex_ex1[23182:177970] Length: 2
    2021-06-15 21:43:49.049 regex_ex1[23182:177970] Match: 60
    

## Example 2 - replace matched pattern

### Code
    
    
    Program regex_ex2;
      
    {$mode objfpc}{$H+}
    {$modeswitch objectivec2}
    
    Uses
      MacOSAll, CocoaAll, SysUtils;
    
    Var
      srchStr : NSString;
      patnStr : NSString;
      resStr  : NSString;
      i: NSRange;
      j: NSRange;
    
    Begin
      srchStr := NSStr('I have 43 bags of 60 marbles.');
      patnStr := NSStr('\d+');
    
      // Find first match
      i := srchStr.rangeOfString_options_range(patnStr, NSRegularExpressionSearch, NSMakeRange(0, srchStr.Length));
    
      // Do first replacement
      resStr := srchStr.stringByReplacingOccurrencesOfString_withString(NSStr('43'), NSStr('forty-three'));
    
      // Find second match
      j := srchStr.rangeOfString_options_range(patnStr, NSRegularExpressionSearch, NSMakeRange(i.location + i.length, srchStr.Length - (i.location + i.length)));
    
      // Do second replacement
      resStr := resStr.stringByReplacingOccurrencesOfString_withString(NSStr('60'), NSStr('sixty'));
    
      // Output
      NSLog(NSStr('Search string: %@'), srchStr);
      NSLog(NSStr('Pattern string: %@'), patnStr);
    
      NSLog(NSStr('Range 1: %@'), NSStringFromRange(i));
      NSLog(NSStr('Location: %d'), i.location);
      NSLog(NSStr('Length: %d'), i.length);
      NSLog(NSStr('Match: %@'), srchStr.substringWithRange(i));
    
      NSLog(NSStr('Range 2: %@'), NSStringFromRange(j));
      NSLog(NSStr('Location: %d'), j.location);
      NSLog(NSStr('Length: %d'), j.length);
      NSLog(NSStr('Match: %@'), srchStr.substringWithRange(j));
    
      NSLog(NSStr('Result: %@'), resStr);
    End.
    

### Output
    
    
    2021-06-16 11:41:24.025 regex_ex2[5761:21687] Search string: I have 43 bags of 60 marbles.
    2021-06-16 11:41:24.026 regex_ex2[5761:21687] Pattern string: \d+
    2021-06-16 11:41:24.026 regex_ex2[5761:21687] Range 1: {7, 2}
    2021-06-16 11:41:24.026 regex_ex2[5761:21687] Location: 7
    2021-06-16 11:41:24.026 regex_ex2[5761:21687] Length: 2
    2021-06-16 11:41:24.026 regex_ex2[5761:21687] Match: 43
    2021-06-16 11:41:24.026 regex_ex2[5761:21687] Range 2: {18, 2}
    2021-06-16 11:41:24.026 regex_ex2[5761:21687] Location: 18
    2021-06-16 11:41:24.026 regex_ex2[5761:21687] Length: 2
    2021-06-16 11:41:24.026 regex_ex2[5761:21687] Match: 60
    2021-06-16 11:41:24.026 regex_ex2[5761:21687] Result: I have forty-three bags of sixty marbles.
    

## See also

  * [macOS NSRegularExpression](<macOS_NSRegularExpression.md> "macOS NSRegularExpression").
  * [RegEx packages](<RegEx_packages.md> "RegEx packages") \- cross platform.



## External links

  * [Apple: NSString](<https://developer.apple.com/documentation/foundation/nsstring>).
  * [Apple: rangeOfString:options:range:](<https://developer.apple.com/documentation/foundation/nsstring/1415073-rangeofstring>).
  * [Apple: NSStringCompareOptions](<https://developer.apple.com/documentation/foundation/nsstringcompareoptions>).
  * [Apple: NSRegularExpressionSearch](<https://developer.apple.com/documentation/foundation/nsstringcompareoptions/nsregularexpressionsearch>).
  * [Apple: stringByReplacingOccurrencesOfString:withString:](<https://developer.apple.com/documentation/foundation/nsstring/1412937-stringbyreplacingoccurrencesofst>).

---

_Source: [https://wiki.freepascal.org/macOS_NSString_Regular_Expressions](https://web.archive.org/web/20241004014731/https://wiki.freepascal.org/macOS_NSString_Regular_Expressions)_
