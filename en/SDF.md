# SDF

│ **English (en)** │  [**français (fr)**](</SDF/fr> "SDF/fr") │    
****

**SDF** is a delimited text file format used in Delphi (and later FreePascal) that is very much like, but not the same, as [CSV](<CSV.md> "CSV"). (`.sdf` is also the extension of Microsoft SQL Server data files, therefore it may not be a good idea to use the `.sdf` extension for your files). 

## SDF format

A summary of the SDF format as used in [TStringList](<TStringList.md> "TStringList"), as paraphrased/synthesized from various versions of Delphi help (e.g. [DelimitedText](<http://docwiki.embarcadero.com/Libraries/en/System.Classes.TStrings.DelimitedText>), [StrictDelimiter](<http://docwiki.embarcadero.com/Libraries/en/System.Classes.TStrings.StrictDelimiter>)) 

  * If you get DelimitedText, strings that include spaces, Delimiter (e.g. ,) or QuoteChar **must** be enclosed in QuoteChars (" by default) aka quoted. (In practice - read Delphi does this -, however, with StrictDelimiter on, e.g. spaced text is not quoted)
  * In addition, a QuoteChar in a string **must** be repeated.
  * When setting DelimitedText, strings **must** be separated by Delimiter characters **or** spaces.
  * Strings **may** be enclosed in QuoteChars.
  * QuoteChars in the string **must** be repeated to distinguish them from the QuoteChars enclosing the string.
  * Delimiters that are not contained within QuoteChar marks **must** be handled as delimiters
  * If StrictDelimiter is false, spaces that are not contained within QuoteChar marks '_must_ be handled as delimiters.
  * Two Delimiters in a row indicate an empty string.



## Example output

Example SDF output (tested in Delphi and FPC after applying the patch for issue 19610): 

[![Note-icon.png](https://wiki.freepascal.org/images/b/be/Note-icon.png)](</File:Note-icon.png>)

**Note:** FPC versions below 2.6.0 do not generate SDF according to the specification

This series of strings in a stringlist (note: numbers in front indicate the field for clarity, they are not part of the data): 
    
    
    0normal_string
    1includes;delimiter
    2includes space
    3includes"quote
    4includes "quote and space
    5tab	character
    6"single_starting_quote
    7multi
    line.
    8   SpacesInFront
    9SpacesAtTheEnd
    

gets output as this with _StrictDelimiter_ true (and ; as delimiter): 
    
    
    normal_string;"includes;delimiter";includes space;"includes""quote";"includes ""quote and space";tab	character;"""single_starting_quote";multi
    line.;   SpacesInFront;SpacesAtTheEnd  
    

... and output as this with _StrictDelimiter_ false: 
    
    
    normal_string;"includes;delimiter";"includes space";"includes""quote";"includes ""quote and space";"tab	character";"""single_starting_quote";"multi
    line.";"   SpacesInFront";"SpacesAtTheEnd  "
    

## See also

  * [CSV](<CSV.md> "CSV") A related format
  * [a patch that improved SDF input/output](<http://bugs.freepascal.org/view.php?id=19610>)
  * [TSdfDataSet](<TSdfDataSet.md> "TSdfDataSet")

---

_Source: [https://wiki.freepascal.org/SDF](https://web.archive.org/web/20191020164740/https://wiki.freepascal.org/SDF)_
