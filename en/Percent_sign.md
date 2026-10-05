# Percent sign

│ **English (en)** │  **[suomi (fi)](</Percent_sign/fi> "Percent sign/fi")** │  **[français (fr)](</Percent_sign/fr> "Percent sign/fr")** │  **[português (pt)](</Percent_sign/pt> "Percent sign/pt")** │  **[русский (ru)](<../ru/Percent_sign.md> "Percent sign/ru")** │    
****

%

In [ASCII](<ASCII.md> "ASCII"), the character code decimal `37` (or [hexadecimal](<Hexadecimal.md> "Hexadecimal") `25`) is defined to be `%` . 

The symbol `%` (pronounced "percent sign") is used in [Pascal](<Pascal.md> "Pascal"): 

  * it indicates a [binary numeral expression/number](<Binary_numeral_system.md> "Binary numeral system").



The percent sign also appears in Lazarus [IDE directives](<IDE_directives.md> "IDE directives") of the form `{%directive}`

  


## Example
    
    
    program simple_binary_digit;
    
    var b:byte;
    begin
      b := %1010011;
      writeln (b);
      writeln (binStr(b,8));
      writeln ;
      writeln ('Press [Enter] to finish');
      readln;
    end.
    

The output prints as follows: 
    
    
    83
    01010011
     
    Press [enter] to finish
    

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** Binary number literals are not supported in [`{$mode Delphi}`](<Mode_Delphi.md> "Mode Delphi") and [`{$mode TP}`](<Mode_TP.md> "Mode TP").

## See also

  * `function` [binStr](<http://lazarus-ccr.sourceforge.net/docs/rtl/system/binstr.html> "doc:rtl/system/binstr.html") \- Convert [integer](<Integer.md> "Integer") to [string](<String.md> "String") with binary representation.

---

_Source: [https://wiki.freepascal.org/Percent_sign](https://web.archive.org/web/20250219111148/https://wiki.freepascal.org/Percent_sign)_
