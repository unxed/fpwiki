# lookup

│ **English (en)** │  **[français (fr)](</lookup/fr> "lookup/fr")** │    
****

## Definition

Unit: FreePascal, unit **db**
    
    
        function Lookup(const KeyFields: string; const KeyValues: Variant; const ResultFields: string): Variant;
    

Official documentation: none 

## Description

**lookup** is similar to [locate](<locate.md> "locate") in that it looks for a record in the dataset where the specified search values for the specified fields match. 

If found, the function returns a variant array with the values of the ResultFields (a semicolon-delimited list of desired fields). If not found, it returns **Null**. In neither case does it change the current physical record/cursor. 

KeyFields can be a single field name or a semicolon-separated list of fields. 

KeyValue can be a variant or a variant array and the number of items must match the number of fields specified in KeyFields. 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** Lookup is only implemented in non-unidirectional datasets, i.e. you must be able to move back and forwards through the dataset.

## See also

  * [locate](<locate.md> "locate")
  * [Description of how to use lookup with Delphi](<http://delphi.about.com/od/database/l/aa052901a.htm>)

---

_Source: [https://wiki.freepascal.org/lookup](https://web.archive.org/web/20250516132846/https://wiki.freepascal.org/lookup)_
