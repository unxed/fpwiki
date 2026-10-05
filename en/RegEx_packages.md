# RegEx packages

│ **[Deutsch (de)](</RegEx_packages/de> "RegEx packages/de")** │  **English (en)** │  **[español (es)](</RegEx_packages/es> "RegEx packages/es")** │  **[Bahasa Indonesia (id)](</RegEx_packages/id> "RegEx packages/id")** │  **[polski (pl)](</RegEx_packages/pl> "RegEx packages/pl")** │  **[中文（中国大陆） (zh_CN)](</RegEx_packages/zh_CN> "RegEx packages/zh CN")** │    
****

Free Pascal includes the RegExpr package, which includes several "engines" for regular expressions. The main of these engines is TRegExpr, others are deprecated and not developed anymore. 

Read the Wikipedia page about regular expressions to get introduced to the subject: [POSIX Basic Regular Expressions](<http://en.wikipedia.org/wiki/Regular%20expression#POSIX%20Basic%20Regular%20Expressions>). 

## Contents

  * 1 TRegExpr by Sorokin
    * 1.1 Improvements in 2019/2020
    * 1.2 Improvements in 2023
    * 1.3 Change case on replaces
    * 1.4 Example
  * 2 FLRE - Fast Light Regular Expressions
    * 2.1 Example
  * 3 Regexpr by Joost
  * 4 Regexpr by Florian
  * 5 See also



## TRegExpr by Sorokin

This is the most complete implementation, it is available in FPC in the file "packages/regexpr/src/regexpr.pas". This package implements a subset of Perl's regular expressions and the syntax provided by the package is [documented here](<https://regex.sorokin.engineer/regular_expressions/>). 

Official repository: [GitHub](<https://github.com/andgineer/TRegExpr>). The development is going on there, FPC package is following this repo with some delay. Version from this repo works OK in FPC. 

### Improvements in 2019/2020

In 2019 year, after about 15 years of missed patches from anybody, work on TRegExpr was continued by [Alexey Torgashin](<https://github.com/Alexey-T/>), [User:Alextp](</User:Alextp> "User:Alextp"). These changes were merged to FPC Git 'main' branch. 

Changes are: 

  * Optimizations. One major optimization made the test_benchmark project (in official GitHub repo) faster by 5-10 times in 1-2 tests.
  * Support for meta-classes \W \D \S inside char-classes.
  * Added meta-classes \h \H \v \V (also inside char-classes).
  * Support named groups: (?P<name>regex), and back-references to named groups: (?P=name).
  * Support non-capturing groups: (?:regex).
  * Support lookaround, positive+negative: foo(?=bar), foo(?!bar), (?<=foo)bar, (?<!foo)bar.
  * Support atomic groups: (?>foo|bar).
  * Support possessive quantifier: a++, a*+, a?+, a{n,m}+.
  * Support Unicode categories: \pL \p{L} \p{Lu}, with negative \P.
  * Support control escape sequences: \cA ... \cZ.
  * Support recursion: (?R) and (?0).
  * Support subroutine calls: (?1) ... (?80), and to named groups: (?P>name).
  * Added define FastUnicodeData, which uses additional arrays to speed-up Unicode functions in Unicode mode. Added unit regexpr_unicodedata.
  * Added backward search possibility (ABackward parameter).
  * Added possibility to test regex at single offset without advancing to next positions (ATryOnce parameter).
  * Support NULL chars, both in regex and in the input string.
  * Support Unicode bigger than U+FFFF, ie emoji symbols. Dot, \W, \S, \D etc now should correctly find 2 WideChars for 1 emoji.
  * Added \z, changed behaviour of \Z to be like in major engines.



### Improvements in 2023

In 2023, Martin Friebe (SynEdit maintainer) made lot of good changes. These changes were also merged to FPC TRegExpr fork. 

  * Added \K (truncate match from left).
  * Added \G for ExecNext to match the position where the previous match ended.
  * Added full look-ahead support. Added full fixed length look-behind support. Added limited variable length look-behind support.
  * Added (?modifier:pattern) support.
  * Added "property AllowBraceWithoutMin" to allow {,2} instead of {0,2}.
  * Added "property AllowLiteralBraceWithoutRange" to allow "{" to be matched as literal, if no range follows.
  * Optimization, detect more anchors: ^ $ \G .*
  * Fixed 'atomic' groups. Allow backtracking the entire group.
  * Removed limit "LoopStackMax = 10" for nested loops.



And from Alexey Torgashin: 

  * Added \R support (any line break).



### Change case on replaces

You can change case of found fragments, use modifiers in replace-with field (case change works after position of modifier): 

  * \l - First char to lower
  * \L - All chars to lower
  * \u - First char to upper
  * \U - All chars to upper



E.g. if found a word, use replace-with field "\L$0" to change word to lowercase (here $0 is group 0, found text). 

In addition \n will be replaced with the line-break character(s) (LF or CR LF, this depends on internal constant). 

### Example

Using TRegExpr to check if an expression is present in a string is very easy, just create an instance of TRegExpr, then place your regular expression in the property TRegExpr.Expression and use the method Exec to verify if there are any matches for this regular expression. Exec will return true if the expression matches the string passed to it. 
    
    
    var
      RegexObj: TRegExpr;
    begin
      RegexObj := TRegExpr.Create;
      RegexObj.Expression := '.*login.*';
      if RegexObj.Exec('Please try to login here') then WriteLn('The login was found!');
      RegexObj.Free;
    end;
    

Subexpression match: 
    
    
    program Project1;
    
    uses
      RegExpr;
    
    var
      re: TRegExpr;
    begin
      re := TRegExpr.Create('hello (.*?)!');
      if re.Exec('hello world! hello pascal!') then
      begin
        WriteLn(re.Match[1]);
        while re.ExecNext do
        begin
          WriteLn(re.Match[1]);
        end;
      end;
      re.Free;
    end.
    

Output: 
    
    
    world
    pascal
    

## FLRE - Fast Light Regular Expressions

FLRE (**F** ast **L** ight **R** egular **E** xpressions) is a fast, safe and efficient regular expression library, which is implemented in Object Pascal (Delphi and Free Pascal) but which is even usable from other languages like C/C++ and so on. It can handle Unicode and UTF-8 strings. 

It implements the many of the most common Perl and POSIX features, except irregular expression features like [forward references and nested back references](<https://www.regular-expressions.info/backref2.html>) and so on, which aren't supported at FLRE, only real ["back" references](<https://www.regular-expressions.info/backref.html>) are supported, hence also the word "Light" at the FLRE name. It also finds the leftmost-first match, the same match that Perl and PCRE would, and can return submatch information. But it also features a flag for a yet experimental POSIX-style leftmost-longest match behaviour mode. 

FLRE is licensed under the LGPL v2.1 with static-linking-exception. 

[GitHub repository](<https://github.com/BeRo1985/flre>)

### Example

Implementing PHP parse_url function (based on [php.js parse_url](<http://locutus.io/php/url/parse_url/>) implementation) 
    
    
    uses Classes, SysUtils, flre;
    
    type
    
      RUrlParser = record
      private
        Fcomponents: array[1..13] of string;
      private
        function getComponent( aIndex : integer ) : string;
        procedure setComponent( aIndex : integer; const aValue : string );
      public
        function parse( const aUrl : UTF8String ) : boolean;
      public
        property scheme : string index 1 read getComponent write setComponent; // e.g. http
        property authority : string index 2 read getComponent;
        property userInfo : string index 3 read getComponent;
        property user : string index 4 read getComponent;
        property pass : string index 5 read getComponent;
        property host : string index 6 read getComponent;
        property port : string index 7 read getComponent;
        property path : string index 9 read getComponent;
        property directory : string index 10 read getComponent;
        property fileName : string index 11 read getComponent;
        property query : string  index 12 read getComponent; // after the question mark ?
        property fragment : string  index 13 read getComponent; // after the hashmark #
      end;
    
    implementation
    
    function RUrlParser.getComponent( aIndex : integer ) : string;
    begin
        Result := Fcomponents[ aIndex ];
    end;
    
    procedure RUrlParser.setComponent( aIndex : integer; const aValue : string );
    begin
        Fcomponents[ aIndex ] := aValue;
    end;
    
    function RUrlParser.parse( const aUrl : UTF8String ) : boolean; overload;
    var
        i : integer;
        re : TFLRE;
        parts : TFLREMultiStrings;
    begin
        re := TFLRE.Create( '(?:([^:\\/\?#]+):)?'
            + '(?:\/\/()(?:(?:()(?:([^:@\/]*):?([^:@\/]*))?@)?([^:\/?#]*)(?::(\d*))?))?'
            + '()'
            + '(?:(()(?:(?:[^?#\/]*\/)*)()(?:[^?#]*))(?:\?([^#]*))?(?:#(.*))?)'
            , [ rfUTF8 ]
        ); 
    
        parts := nil;
        Result := re.UTF8ExtractAll( aUrl, parts );
        if ( Result ) then
        begin
            for i := 1 to Length( parts[0] ) - 1 do
            begin
                setComponent( i, string( parts[0][i] ) );
            end;
        end;
    
        // Free regexp memory
        for i := 0 to Length( parts ) - 1 do
        begin
            SetLength( parts[i], 0 );
        end;
        parts := nil;
    
        re.Free();
    end;
    

## Regexpr by Joost

Regexpr by Joost (units oldregexpr.pp and regex.pp) is a very basic regex (Regular Expression) unit, it handles most regular expressions as GNU regexpr. 

The current unit is far from complete, and still misses very simple syntax support of [POSIX](<http://www.opengroup.org/onlinepubs/007908799/xbd/re.html>) or more complex syntax such as [Perl regex](<http://www.pcre.org/>), [Java Regex](<http://java.sun.com/j2se/1.4.2/docs/api/java/util/regex/Pattern.html>), [Ruby Regex](<http://www.jpsoft.com/help/index.htm?regularexpressionsyntax.htm>) etc... 

The unit contains 4 functions for now: 

  * GenerateRegExprEngine – This function compiles the regex pattern.
  * RegExprPos – Finds the pattern inside a given string.
  * DestroyRegExprEngine – Free the compilation of the pattern
  * RegExprEscapeStr – Escape reserve syntax of regular expression language so it will be understood as string instead of regex syntax.



There is also one test: 

  * The **testreg1** test program demonstrates the supported regular expressions.



## Regexpr by Florian

This is the oldest implementation. It is present in packages/regexpr/src/old and is not currently compiled by the makefiles, so it is not available precompiled by default in released FPC/Lazarus versions. 

## See also

  * [macOS NSRegularExpression](<macOS_NSRegularExpression.md> "macOS NSRegularExpression")
  * [Comparison of regular expression engines](<http://en.wikipedia.org/wiki/Comparison_of_regular_expression_engines>)



Go to back [Packages List](<Package_List.md> "Package List")

---

_Source: [https://wiki.freepascal.org/RegEx_packages](https://web.archive.org/web/20250317225515/https://wiki.freepascal.org/RegEx_packages)_
