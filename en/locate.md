# locate

│ **English (en)** │  **[français (fr)](</locate/fr> "locate/fr")** │    
****

## Contents

  * 1 Definition
  * 2 Description
  * 3 Examples
  * 4 See also



## Definition

Unit: FreePascal, unit **db**
    
    
        function  Locate(const KeyFields: string; const KeyValues: Variant; Options: TLocateOptions): Boolean; override;
    

Official documentation: none 

## Description

**locate** looks for a record in the dataset where the specified search values for the specified fields match. If found, the function returns true and the cursor position/current record moves to that record. 

KeyFields can be a single field name or a semicolon-separated list of fields. 

KeyValue can be a variant or a variant array and the number of items must match the number of fields specified in KeyFields. 

Search options can be: 

  * loCaseInsensitive: ignore upper case/lower case differences when searching
  * loPartialKey: match partial finds (instead of the complete key). Note: this only works for string-type fields, not for number/date/currency etc fields where this option is ignored.



![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** Locate is only implemented in non-unidirectional datasets, i.e. you must be able to move back and forwards through the dataset.

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** Locate and lookup act at low level inside a dataset to search for records. Often it is more efficient to filter/limit what gets into the dataset in the first place, e.g. using SQL WHERE clauses.

## Examples

Using a TDBF dataset and looking for 'di Angelo', 'Di Angelo' etc in a field called LASTNAME: 
    
    
    uses dbf, db
    ...
    if MyDBF.Locate('LASTNAME','di Angelo',[loCaseInsensitive])then
      writeln('Found record.');
    

Using multiple fields to find a record. 

Note: you have to add Variants to the uses clause. 
    
    
    uses Variants;
    ..
    procedure LocateMyRecord;
    var
      aCityID, aCountryID: integer;
    begin
      if Locate('city_id;country_id', VarArrayOf([aCityID, aCountryID]), []) then
      begin
        DoSomething;
      end;
    end;
    

## See also

  * [lookup](<lookup.md> "lookup")
  * [Description of how to use locate with Delphi](<http://delphi.about.com/od/database/l/aa052901a.htm>)

---

_Source: [https://wiki.freepascal.org/locate](https://web.archive.org/web/20250301000000/https://wiki.freepascal.org/locate)_
