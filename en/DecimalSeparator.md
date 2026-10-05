# DecimalSeparator

│ **English (en)** │  **[français (fr)](</DecimalSeparator/fr> "DecimalSeparator/fr")** │    
****

[![Warning-icon.png](https://wiki.freepascal.org/images/b/b2/Warning-icon.png)](</File:Warning-icon.png>)

**Warning:** This variable is deprecated. Please see hints for information on replacement.

**DecimalSeparator** is a [global char variable](<Global_variables.md> "Global variables") whose contents is determined by the used [locale](</index.php?title=locale&action=edit&redlink=1> "locale \(page does not exist\)"). It is used for representation of floating point numbers on screen and within functions like StrToFloat() and FloatToStr(). 

As long as every instance of a program uses the same decimal separator everything will behave as expected. However, if one instance of a program has its locale set to Dutch (nl_NL), writes a file containing some floating point numbers which is read by another instance of the same program with a locale set to US-English (en_US) you run into trouble because a formatted number like 123,456 will generate a conversion exception... 

A workaround can be to use a function like: 
    
    
    function DecFloat2Str( const d: double ): string;
    var
       myseparator: char;
    begin
      myseparator := DecimalSeparator;
      DecimalSeparator := '.';
      Result := FloatToStr( d );
      DecimalSeparator := myseparator;
    end;
    

DecimalSeparator is defined in sysinth.inc as: 
    
    
     DecimalSeparator : Char absolute DefaultFormatSettings.DecimalSeparator deprecated;
    

## Important hint

DecimalSeparator is deprecated. In all new projects, it should be replaced by the [DefaultFormatSettings](<DefaultFormatSettings.md> "DefaultFormatSettings") variable. 

## See also

  * [ThousandSeparator](</index.php?title=ThousandSeparator&action=edit&redlink=1> "ThousandSeparator \(page does not exist\)")
  * [ListSeparator](</index.php?title=ListSeparator&action=edit&redlink=1> "ListSeparator \(page does not exist\)")
  * [Documentation on DecimalSeparator](<http://www.freepascal.org/docs-html/rtl/sysutils/decimalseparator.html>)

---

_Source: [https://wiki.freepascal.org/DecimalSeparator](https://web.archive.org/web/20250219021551/https://wiki.freepascal.org/DecimalSeparator)_
