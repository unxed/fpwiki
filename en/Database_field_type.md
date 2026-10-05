# Database field type

│ **English (en)** │

## Contents

  * 1 Overview
  * 2 Types
  * 3 Size, DataSize and Unicode
  * 4 Defining types in your dataset
  * 5 Assigning and retrieving values
  * 6 See also



## Overview

FCL-DB database fields can have different data types. In datasets, a subset of these types is available. What types are supported differ per dataset type. 

## Types

Currently, the following field types are defined. See [FCL field type documentation](<http://lazarus-ccr.sourceforge.net/docs/fcl/db/tfieldtype.html>). To do: add assignment information (e.g. can you use .AsString) and additional information below. 

Field type | Type | Size | Description   
---|---|---|---  
ftADT |  |  | Abstract data type (user defined types created on the server); not supported yet (?).   
ftArray |  |  | Represents an Interbase 6 / Firebird array datatype (array of simple data-types like varchar or integer). However, SQLDB does not support the array datatype currently.   
ftAutoInc |  |  | An auto-incrementing integer field.   
ftBCD | packed record |  | A binary coded decimal floating point value.   
ftBlob |  |  | A binary large object (BLOB), meant to store arbitrary binary data.   
ftBoolean | Boolean | 1 byte | A boolean value (yes/no).   
ftBytes |  |  | Presumably a fixed number of bytes stored as-is. Needs to have its Size property set to work.   
ftCurrency |  |  | A format to precisely store currency values.   
ftCursor |  |  |   
ftDataSet |  |  | Presumably meant to store an entire dataset (possibly to implement master/detail table).   
ftDate |  |  | A date without time information.   
ftDateTime |  |  | A date and time information.   
ftDBaseOle |  |  | Presumably meant to store OLE objects in a DBase database. Needs to have its Size property set to work.   
ftFMTBcd |  |  | A form of Binary Coded Decimal (BCD) number field. Needs to have its Size property set to work.   
ftFixedChar |  |  | A fixed width character field, similar to a Pascal shortstring. Needs to have its Size property set to work.   
ftFixedWideChar |  |  | A fixed width multibyte character field. Needs to have its Size property set to work.   
ftFloat | Double | 8 bytes | A floating point numeric type.   
ftFmtMemo |  |  | Needs to have its Size property set to work.   
ftGraphic |  |  | Needs to have its Size property set to work.   
ftGuid |  |  | A field used to store a GUID (Globally Unique Identifier). With the current code, this field needs to have its Size property set to 38.   
ftIDispatch |  |  |   
ftInteger | Longint | 4 bytes | An integer field.   
ftInterface |  |  |   
ftLargeint |  | 8 bytes | A field that contains an integer, stores more bytes than an integer and therefore has a larger range.   
ftMemo |  |  | Stores a variable amount of string/text data; needs no size set.   
ftOraBlob |  |  | Presumably stores Oracle BLOB.   
ftOraClob |  |  | Presumably stores Oracle CLOB: an Oracle data type that can hold up to 4 GB of data [[1]](<http://www.orafaq.com/wiki/CLOB>).   
ftParadoxOle |  |  | Presumably meant to store OLE objects in a Paradox database.   
ftReference |  |  |   
ftSmallint |  | 2 bytes, signed | An integer type field with less bytes than ftInteger.   
ftString |  |  | Stores string data; needs to have its Size property set to the maximum number of characters possible in that field.   
ftTime |  |  | Stores time-only data.   
ftTimeStamp |  |  | Stores date/time data. Probably equivalent to ftDateTime.   
ftTypedBinary |  |  | Some kind of BLOB-like field?   
ftUnknown |  |  |   
ftVarBytes |  |  | Presumably a variant with byte/binary data?   
ftVariant |  |  | Presumably meant to store variant data.   
ftWideMemo |  |  | An ftMemo with widestring (UTF-16) characters.   
ftWideString |  |  | An ftString with widestring (UTF-16) characters.   
ftWord |  | 2 bytes, unsigned | Presumably stores an integer value.   
  
## Size, DataSize and Unicode

Note that for string type fields, Size indicates the number of characters that can be stored. As indicated in [FPC Unicode support#Introduction](<FPC_Unicode_support.md> "FPC Unicode support"), FPC up to and including 2.6 only deals with **ANSI/ASCII** single byte characters; it does not support Unicode/UTF8/UTF16/Unicodestring characters. 

The read-only property DataSize indicates the field size in bytes. 

If you use multibyte characters (e.g. UTF8 or UTF16/Unicodestring encoded), DataSize and Size do not mean the same thing. If you use only ANSI/ASCII characters, DataSize and Size are effectively the same thing. 

## Defining types in your dataset

_Todo: write me. Explain various ways of doing things (TFieldDef, TFields) and which dataset supports which methods._

## Assigning and retrieving values

Once you have the fields in your dataset defined, you can assign and retrieve data like this - suppose you have a dataset called FTestDataset: 
    
    
      FTestDataset.Open; //Open for viewing/editing/inserting
      FTestDataset.Append;
      FTestDataset.FieldByName('YourFieldName').AsString := 'This is my field data'; //Suppose field YourFieldName is a memo field
      FTestDataset.Post; //"Commit"/save changes in the record to the dataset.
      Writeln('YourFieldName has data:' + FTestDataset.FieldByName('YourFieldName').AsString);
      { Retrieve the field value of the current record. Because we haven't moved the record, we should get what we just entered }
    

For text/memo fields, use the AsString method. For date/time fields, use the AsDateTime method. For binary fields, I suppose you can use the AsString method - _but there must be another way, too_

## See also

  * [Databases](<Databases.md> "Databases")

---

_Source: [https://wiki.freepascal.org/Database_field_type](https://web.archive.org/web/20250323145400/https://wiki.freepascal.org/Database_field_type)_
