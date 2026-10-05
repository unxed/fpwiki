# macOS NSRegularExpression

│ **English (en)** │ 

[![macOSlogo.png](https://wiki.freepascal.org/images/1/15/macOSlogo.png)](</File:macOSlogo.png>)

This article applies to [macOS](</Category:macOS> "Category:macOS") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

  
****

## Contents

  * 1 Regular expressions
  * 2 NSRegularExpression Overview
    * 2.1 Metacharacters
    * 2.2 Operators
  * 3 Example 1 - match a pattern
    * 3.1 Code
    * 3.2 Output
    * 3.3 Code explanation
  * 4 Example 2 - match pattern groups
    * 4.1 Code
    * 4.2 Output
    * 4.3 Code explanation
  * 5 Example 3 - replace matched pattern
    * 5.1 Code
    * 5.2 Output
  * 6 Example 4 - rearrange matched pattern groups
    * 6.1 Code
    * 6.2 Output
  * 7 See also
  * 8 External links



## Regular expressions

Regular expressions are patterns used to match specified alpha-numeric character combinations in the string data being searched. 

Each character in a regular expression (that is, each character in the string describing its pattern) is either a metacharacter or operator, having a special meaning, or a regular character that has a literal meaning. 

Regular expressions can be incredibly complex. Indeed, whole books have been written about them! For a gentle introduction to regular expressions, see this [O'Reilly article](<https://www.oreilly.com/content/an-introduction-to-regular-expressions/>). 

## NSRegularExpression Overview

The NSRegularExpression class has convenience methods for returning all the matches as an array, the total number of matches, the first match, and the range of the first match. 

An individual match is represented by an instance of the NSTextCheckingResult class, which carries information about the overall matched range (via its range property), and the range of each individual capture group (via the rangeAtIndex method). 

NSRegularExpression conforms to the International Components for Unicode (ICU) specification for regular expressions. 

### Metacharacters

For a comprehensive list of characters used by the NSRegularExpression class that have a special meaning in regular expression patterns, see the [ICU listing](<https://unicode-org.github.io/icu/userguide/strings/regexp.html#regular-expression-metacharacters>). 

### Operators

For a comprehensive list of operators used by the NSRegularExpression class, see the [ICU listing](<https://unicode-org.github.io/icu/userguide/strings/regexp.html#regular-expression-operators>). 

## Example 1 - match a pattern

In this fairly trivial and contrived code example, we use the **\d** metacharacter which matches a decimal digit and the **+** operator to match the preceding decimal digit one or more times. This pattern **\d+** aims to match all the occurrences of numbers in the search string which we then output using NSLog(). It uses the NSRegularExpression convenience methods for returning all of the matches in the search string as an array and the total number of matches. 

### Code
    
    
    Program regex_ex1;
    
    {$mode objfpc}{$H+}
    {$modeswitch objectivec2}
    
    Uses
      MacOSAll, CocoaAll, SysUtils;
    
    Var
      srchStr : String;
      patnStr : String;
      myRegex : NSregularExpression;
      matches : NSArray;
      match   : NSTextCheckingResult;
      error   : NSErrorPtr;
    
    Begin
      error   := Nil;
      srchStr := 'I have 43 bags of 60 marbles.';
      patnStr := '\d+';
    
      // Create a regular expression with given string and options
      myRegex := NSregularExpression.regularExpressionWithPattern_options_error(NSStr(patnStr), NSRegularExpressionOptions(NSRegularExpressionCaseInsensitive), error);
    
      // Check creation of regular expression with given string and options
      if(error <> Nil) then
        begin
          NSLog(NSStr('Regex creation error: %@'), error);
          Exit;
        end;
    
      // Save any matches in the given string in the matches array
      matches := myRegex.matchesInString_options_range(NSStr(srchStr), 0, NSMakeRange(0, srchStr.Length));
    
      // Output
      NSLog(NSStr('Search string: %@'), NSStr(srchStr));
      NSLog(NSStr('Pattern string: %@'), NSStr(patnStr));
      NSLog(NSStr('Number of matches: %lu'), myRegex.numberOfMatchesInString_options_range(NSStr(srchStr), 0, NSMakeRange(0, srchStr.Length)));
    
      for match in matches do
        NSLog(NSStr('match: %@'), NSStr(srchStr).substringWithRange(match.rangeAtIndex(0)));
    End.
    

### Output

The output from running the above code example is: 
    
    
    2021-06-12 21:05:46.335 regex_ex1[26138:232243] Search string: I have 43 bags of 60 marbles.
    2021-06-12 21:05:46.336 regex_ex1[26138:232243] Pattern string: \d+
    2021-06-12 21:05:46.336 regex_ex1[26138:232243] Number of matches: 2
    2021-06-12 21:05:46.336 regex_ex1[26138:232243] match: 43
    2021-06-12 21:05:46.336 regex_ex1[26138:232243] match: 60
    

### Code explanation

The call to _regularExpressionWithPattern_options_error()_ creates an NSRegularExpression object instance (myRegex) with the specified regular expression pattern and options. 

Options are specified using _NSRegularExpressionOptions()_. Note that by default NSRegularExpression performs case-sensitive searches, so we specified the NSRegularExpressionCaseInsensitive option for case-insenstive searches although, because we are dealing with digits above, this has no effect and we might as well have specified Nil in this example for no options. 

Once we have the NSRegularExpression object, we can then use it for matching text among other operations. 

After checking that the creation of the regular expression did not fail with an error, we call the _matchesInString_options_range()_ method to search for any matches and store them in our NSArray (matches). This method takes our search string, any options (there are none here) and the range to search. The range is specified by giving _NSMakeRange()_ the starting location to search in the string (0 = the beginning of the string) and the length of the search string. 

Next, we output the search string and pattern string, and then call the _numberOfMatchesInString_options_range()_ method to determine the number of matches and output it. 

Finally, we iterate through the matches NSArray and output the matches individually. The call to _rangeAtIndex(0)_ is the full match and is equivalent to simply calling _range_. The code for this looks a little obscure. Let me try to unpack it for you. 

If you just output the content of the matches NSArray you get this: 
    
    
    "<NSSimpleRegularExpressionCheckingResult: 0x14d617570>{7, 2}{<NSRegularExpression: 0x14d614fd0> \\d+ 0x1}",
    "<NSSimpleRegularExpressionCheckingResult: 0x14d617610>{18, 2}{<NSRegularExpression: 0x14d614fd0> \\d+ 0x1}"
    

Notice the {7, 2} and {18, 2} ranges which locate the first number at position 7 (counting from zero) in the search string with a length of 2 and the second number at position 18 with a length of 2. Knowing those ranges, you could use: 
    
    
    NSLog(NSStr('match: ''%@'''), NSStr(srchStr).substringWithRange(NSMakeRange(7, 2)));
    

to output the first number. The _substringWithRange()_ method extracts from our search string the substring that matches the specified range (7, 2). Clearer than mud? I hope so. 

## Example 2 - match pattern groups

This is similar to Example 1 above, except that this time we match groups of characters. Our search string is the same as before, but our pattern string has some added complexity. The pattern matches a decimal digit one or more times as before, but this time as a group which is delineated by using parentheses: **(/d+)**. Next, we use a point **.** to match any character and **?** to match zero or one times following the digit(s). Finally, we match the set of characters from a to z **([a-z]+)** one or more times as a group. 

### Code
    
    
    Program regex_ex2;
    
    {$mode objfpc}{$H+}
    {$modeswitch objectivec2}
    
    Uses
      MacOSAll, CocoaAll, SysUtils;
    
    Var
      srchStr : String;
      patnStr : String;
      myRegex : NSregularExpression;
      matches : NSArray;
      match   : NSTextCheckingResult;
      error   : NSErrorPtr;
    
    Begin
      error   := Nil;
      srchStr := 'I have 43 Bags of 60 Marbles.';
      patnStr := '(\d+).?([a-z]+)';
    
      // Create a regular expression with given string and options
      myRegex := NSregularExpression.regularExpressionWithPattern_options_error(NSStr(patnStr), NSRegularExpressionOptions(NSRegularExpressionCaseInsensitive), error);
    
      // Check creation of regular expression with given string and options
      if(error <> Nil) then
        begin
          NSLog(NSStr('Regex creation error: %@'), error);
          Exit;
        end;
    
      // Save any matches in the given string in the matches array
      matches := myRegex.matchesInString_options_range(NSStr(srchStr), 0, NSMakeRange(0, srchStr.Length));
    
      // Output
      NSLog(NSStr('Search string: %@'), NSStr(srchStr));
      NSLog(NSStr('Pattern string: %@'), NSStr(patnStr));
      NSLog(NSStr('Number of matches: %lu'), myRegex.numberOfMatchesInString_options_range(NSStr(srchStr), 0, NSMakeRange(0, srchStr.Length)));
    
      for match in matches do
        begin
          NSLog(NSStr('match(0): ''%@'''), NSStr(srchStr).substringWithRange(match.rangeAtIndex(0)));
          NSLog(NSStr('match(1): ''%@'''), NSStr(srchStr).substringWithRange(match.rangeAtIndex(1)));
          NSLog(NSStr('match(2): ''%@'''), NSStr(srchStr).substringWithRange(match.rangeAtIndex(2)));
        end;
    End.
    

### Output
    
    
    2021-06-14 17:39:51.255 program2[7376:149163] Search string: I have 43 bags of 60 marbles.
    2021-06-14 17:39:51.255 program2[7376:149163] Pattern string: (\d+).?([a-z]+)
    2021-06-14 17:39:51.255 program2[7376:149163] Number of matches: 2
    2021-06-14 17:39:51.255 program2[7376:149163] match(0): '43 Bags'
    2021-06-14 17:39:51.255 program2[7376:149163] match(1): '43'
    2021-06-14 17:39:51.255 program2[7376:149163] match(2): 'Bags'
    2021-06-14 17:39:51.255 program2[7376:149163] match(0): '60 Marbles'
    2021-06-14 17:39:51.255 program2[7376:149163] match(1): '60'
    2021-06-14 17:39:51.255 program2[7376:149163] match(2): 'Marbles'
    

### Code explanation

The explanation is pretty much the same as for Example 1, except that: 

1) The NSRegularExpressionCaseInsensitive option for case-insenstive searches now has a use. We specified the set of lowercase characters a to z, but because of the option we matched the words Bags and Marbles with initial capital letters. 

2) The call to _rangeAtIndex(0)_ or the equivalent _range_ matches the full pattern; the call to _rangeAtIndex(1)_ matches the first group of digits that we specified; and the call to _rangeAtIndex(2)_ matches the second group of characters that we specified. 

## Example 3 - replace matched pattern

This is similar to Example 1 above, except that this time we replace the decimal numbers matched by the regular expression with words instead of numbers. This is done two ways: one uses the _stringByReplacingMatchesInString_options_range_withTemplate()_ method which returns a new string with the replacements and the other uses the _replaceMatchesInString_options_range_withTemplate()_ method which replaces the matches in the original search string. 

### Code
    
    
    Program regex_ex3;
    
    {$mode objfpc}{$H+}
    {$modeswitch objectivec2}
    
    Uses
      MacOSAll, CocoaAll, SysUtils;
    
    Var
      srchStr : NSString;
      srchStr2: NSMutableString;
      patnStr : NSString;
      templStr: NSString;
      myRegex : NSregularExpression;
      matches : NSArray;
      match   : NSTextCheckingResult;
      numMatch: NSInteger;
      error   : NSErrorPtr;
      count   : ShortInt;
    
    Begin
      error   := Nil;
      srchStr := NSStr('I have 43 Bags of 60 Marbles.');
      srchStr2:= NSMutableString.stringWithString(srchStr);
      patnStr := NSStr('\d+');
    
      // Create a regular expression with given string and options
      myRegex := NSregularExpression.regularExpressionWithPattern_options_error(patnStr, NSRegularExpressionOptions(NSRegularExpressionCaseInsensitive), error);
    
      // Check creation of regular expression with given string and options
      if(error <> Nil) then
        begin
          NSLog(NSStr('Regex creation error: %@'), error);
          Exit;
        end;
    
      // Save any matches in the given string in the matches array
      matches := myRegex.matchesInString_options_range(srchStr, 0, NSMakeRange(0, srchStr.Length));
    
      // Save number of matches
      numMatch := myRegex.numberOfMatchesInString_options_range(srchStr, 0, NSMakeRange(0, srchStr.Length));
    
      // Output
      NSLog(NSStr('Search string: %@'), srchStr);
      NSLog(NSStr('Pattern string: %@'), patnStr);
      NSLog(NSStr('Number of matches: %lu'), numMatch);
    
      // Alternative 1: Using stringByReplacingMatchesInString_options_range_withTemplate()
      for count := numMatch downto 1 do
        begin
          for match in matches do
            begin
              if(srchStr.substringWithRange(match.range) = NSStr('43')) then
                begin
                  templStr := NSStr('forty-three');
                  NSLog(NSStr('Match 1: %@ - Template string: %@'), srchStr.substringWithRange(match.range), templStr);
                end;
    
              if(srchStr.substringWithRange(match.range) = NSStr('60')) then
                begin
                  templStr := NSStr('sixty');
                  NSLog(NSStr('Match 2: %@ - Template string: %@'), srchStr.substringWithRange(match.range), templStr);
                end;
    
              // Do string replacement
              srchStr :=  myRegex.stringByReplacingMatchesInString_options_range_withTemplate(srchStr, NSRegularExpressionOptions(Nil), match.range, templStr);
            end;
    
            // Save any matches in the new search string in the matches array
            matches := myRegex.matchesInString_options_range(srchStr, 0, NSMakeRange(0, srchStr.Length));
         end;
    
      // Output string with replacements
      NSLog(NSStr('Result: %@'), srchStr);
    
      // Save any matches in the given string in the matches array
      matches := myRegex.matchesInString_options_range(srchStr2, 0, NSMakeRange(0, srchStr2.Length));
    
      // Alternative 2: Using replaceMatchesInString_options_range_withTemplate()
      for count := numMatch downto 1 do
        begin
          for match in matches do
            begin
              if(srchStr2.substringWithRange(match.range) = NSStr('43')) then
                begin
                  templStr := NSStr('forty-three');
                  NSLog(NSStr('Match 1: %@ - Template string: %@'), srchStr2.substringWithRange(match.range), templStr);
                end;
              if(srchStr2.substringWithRange(match.range) = NSStr('60')) then
                begin
                  templStr := NSStr('sixty');
                  NSLog(NSStr('Match 2: %@ - Template string: %@'), srchStr2.substringWithRange(match.range), templStr);
                end;
    
              // Do string replacement
              myRegex.replaceMatchesInString_options_range_withTemplate(srchStr2, NSRegularExpressionOptions(Nil), match.range, templStr);
           end;
    
           // Save any matches in the new search string in the matches array
           matches := myRegex.matchesInString_options_range(srchStr2, 0, NSMakeRange(0, srchStr2.Length));
        end;
    
      // Output string with replacements
      NSLog(NSStr('Result: %@'), srchStr2);
    End.
    

### Output
    
    
    2021-06-14 22:42:55.807 program3[9115:227765] Search string: I have 43 Bags of 60 Marbles.
    2021-06-14 22:42:55.808 program3[9115:227765] Pattern string: \d+
    2021-06-14 22:42:55.808 program3[9115:227765] Number of matches: 2
    2021-06-14 22:42:55.808 program3[9115:227765] Match 1: 43 - Template string: forty-three
    2021-06-14 22:42:55.808 program3[9115:227765] Match 2: 60 - Template string: sixty
    2021-06-14 22:42:55.808 program3[9115:227765] Result: I have forty-three Bags of sixty Marbles.
    2021-06-14 22:42:55.808 program3[9115:227765] Match 1: 43 - Template string: forty-three
    2021-06-14 22:42:55.808 program3[9115:227765] Match 2: 60 - Template string: sixty
    2021-06-14 22:42:55.808 program3[9115:227765] Result: I have forty-three Bags of sixty Marbles.
    

## Example 4 - rearrange matched pattern groups

This is similar to Example 2 above, except that this time not only do we match groups of characters, we rearrange them. Our search string is different this time and represents a contact list. We are going to rearrange the format from Firstname, Lastname to the more sensible Lastname, Firstname and then enclose the contact number state area code in parentheses. To do this, our pattern string has some added complexity. 

The pattern matches the first word group as a group of word characters followed by a comma **(\w+),** and does exactly the same again for the second word group, and finally matches the two digits of the area code as a group **(\d{2})**. The curly braces enclosing the number **{2}** after the digit metacharacter specifies that we want to match exactly two digits which comprise the area code. 

The template string rearranges the data groups by switching the order of the first two word groups and adding parentheses around the third group which comprises the area code number. 

### Code
    
    
    Program regex_ex4;
    
    {$mode objfpc}{$H+}
    {$modeswitch objectivec2}
    
    Uses
      MacOSAll, CocoaAll, SysUtils;
    
    Var
      srchStr : NSString;
      srchStr2: NSMutableString;
      patnStr : NSString;
      tmplStr : NSString;
      myRegex : NSregularExpression;
      matches : NSArray;
      match   : NSTextCheckingResult;
      error   : NSErrorPtr;
    
    Begin
      error   := Nil;
      srchStr := NSStr('Firstname, Lastname, 02 9428 4687');
      srchStr2:= NSMutableString.stringWithString(srchStr);
      patnStr := NSStr('(\w+), (\w+), (\d{2})');
      tmplStr := NSStr('$2, $1, ($3)');
    
      // Create a regular expression with given string and options
      myRegex := NSregularExpression.regularExpressionWithPattern_options_error(patnStr, NSRegularExpressionOptions(NSRegularExpressionCaseInsensitive), error);
    
      // Check creation of regular expression with given string and options
      if(error <> Nil) then
        begin
          NSLog(NSStr('Regex creation error: %@'), error);
          Exit;
        end;
    
      // Save any matches in the given string in the matches array
      matches := myRegex.matchesInString_options_range(srchStr, 0, NSMakeRange(0, srchStr.Length));
    
      // Output
      NSLog(NSStr('Search string: %@'), srchStr);
      NSLog(NSStr('Pattern string: %@'), patnStr);
      NSLog(NSStr('Template string: %@'), tmplStr);
      NSLog(NSStr('Number of matches: %lu'), myRegex.numberOfMatchesInString_options_range(srchStr, 0, NSMakeRange(0, srchStr.Length)));
    
      for match in matches do
        begin
          NSLog(NSStr('match(1): %@'), srchStr.substringWithRange(match.rangeAtIndex(1)));
          NSLog(NSStr('match(2): %@'), srchStr.substringWithRange(match.rangeAtIndex(2)));
          NSLog(NSStr('match(3): %@'), srchStr.substringWithRange(match.rangeAtIndex(3)));
          // Do the replacements
          myRegex.replaceMatchesInString_options_range_withTemplate(srchStr2, NSRegularExpressionOptions(Nil), match.range, tmplStr);
        end;
    
       NSLog(NSStr('Result: %@'), srchStr2);
    End.
    

### Output
    
    
    2021-06-15 20:37:54.880 program4[23076:159228] Search string: Firstname, Lastname, 02 9428 4687
    2021-06-15 20:37:54.880 program4[23076:159228] Pattern string: (\w+), (\w+), (\d{2})
    2021-06-15 20:37:54.880 program4[23090:159932] Template string: $2, $1, ($3)
    2021-06-15 20:37:54.881 program4[23076:159228] Number of matches: 1
    2021-06-15 20:37:54.881 program4[23076:159228] match(1): Firstname
    2021-06-15 20:37:54.881 program4[23076:159228] match(2): Lastname
    2021-06-15 20:37:54.881 program4[23076:159228] match(3): 02
    2021-06-15 20:37:54.881 program4[23076:159228] Result: Lastname, Firstname, (02) 9428 4687
    

## See also

  * [macOS NSString Regular Expressions](<macOS_NSString_Regular_Expressions.md> "macOS NSString Regular Expressions").
  * [RegEx packages](<RegEx_packages.md> "RegEx packages") \- cross platform.



## External links

  * [Apple: NSRegularExpression](<https://developer.apple.com/documentation/foundation/nsregularexpression>).


  * [ICU: Documentation](<https://unicode-org.github.io/icu/>).

---

_Source: [https://wiki.freepascal.org/macOS_NSRegularExpression](https://web.archive.org/web/20250313005326/https://wiki.freepascal.org/macOS_NSRegularExpression)_
