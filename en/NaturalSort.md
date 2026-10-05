# NaturalSort

│ **English (en)** │  [**français (fr)**](</NaturalSort/fr> "NaturalSort/fr") │    
****

## Contents

  * 1 About
  * 2 Authors
  * 3 Platforms
  * 4 Features
    * 4.1 Two types of sorting
    * 4.2 Collated alpha-numerical sorting (stNatural or stFloatThousand):
    * 4.3 Topic enumerators sorting (stNatural):
    * 4.4 Integers sorting (stNatural):
    * 4.5 IP addresses sorting (stNatural):
    * 4.6 Floating point numbers sorting (stFloatThousand):
    * 4.7 Thousand separated numbers sorting (stFloatThousand):
  * 5 Download
    * 5.1 Functions and Procedures
      * 5.1.1 procedure NaturalSort(aList: TStrings; SortType: TSortType);
      * 5.1.2 function UTF8NaturalCompareList(aList: TStringList; Index1, Index2: Integer): Integer;
      * 5.1.3 function UTF8FloatThousandCompareList(aList: TStringList; Index1, Index2: Integer): Integer;
      * 5.1.4 function UTF8FloatThousandCompareText(const S1, S2: string): Integer;
      * 5.1.5 function UTF8LogicalCompareText(const S1, S2: string): Integer;
      * 5.1.6 function UTF8NaturalCompareText(const S1, S2: string): Integer;
  * 6 Natural sort controls



# About

Natural sort order is an ordering of strings in alphabetical order, except that multi-digit numbers are ordered as a single character. Natural sort order has been promoted as being more human-friendly ("natural") than the machine-oriented pure alphabetical order. 

For example, in alphabethical sorting "z11" would be sorted before "z2" because "2" is sorted as smaller than "1", while in natural sorting "z2" is sorted as smaller than "z11" because "2" is sorted as smaller than "11". 

Functionality to sort by natural sort order is built into many progamming languages and libraries. 

# Authors

Antônio Galvão and Rik van Kekem 

# Platforms

**Linux** and **Windows**. 

# Features

##### Two types of sorting
    
    
     type
       TSortType = (stNatural, stFloatThousand);
    

Topic numerators, integers and IP addresses are sorted by **stNatural** sort type. Thousand separated and floating point numbers are sorted by **stFloatThousand** sort type. Both make collated alphabetical order and sort internal numbers in alphabetical strings. 

##### Collated alpha-numerical sorting (stNatural or stFloatThousand):

Alphabetical parts sorted by OS (Windows or Linux). 
    
    
        0o3ö6ãrõxögùmî1ó5egõ
        1I8iuã0u4ô8EeU7öuOkü
        3â0á0e6ùsunueUtö8írò
        3áiokúdùsétöbïqãkôvI
        4õ4îcë4ò0à1ólOaUpáxã
        5î2ãgâoí8î4ü1ò3ïwowë
        5úrüqUfò5u0íyïfü5âlí
    

##### Topic enumerators sorting (stNatural):
    
    
        1
        1.1.1
        1.1.2
        1.2.1
        1.99.99
        2
    

##### Integers sorting (stNatural):
    
    
        0
        00
        000
        1
        2
        10
    

##### IP addresses sorting (stNatural):
    
    
        10.145.254.9
        10.145.255.9
        10.145.255.10
        10.146.254.9
        121.243.100.0
        255.255.255.254
    

##### Floating point numbers sorting (stFloatThousand):
    
    
        0,99
        1
        1,01
        1,99
        2
    

##### Thousand separated numbers sorting (stFloatThousand):
    
    
        1.198
        1.199
        1.199,50
        1.200
        1.201
        1.101.300
    

Thousand and decimal separators are the system default ones. 

**It is fast. See the time against other functions:**
    
    
         StrCmpLogicalW: 1
        WideCompareText: 1,238
            NaturalSort: 0,746
    

# Download

The latest version is available here: <http://sourceforge.net/projects/lazarusfiles/files/naturalsort.zip/download>

A demo project is included. 

## Functions and Procedures

#### procedure NaturalSort(aList: TStrings; SortType: TSortType);

This procedure provides a way to sort a list of TStrings directly, according to the chosen SortType, stFloatThousand (for floating point and thousand separators sort) or stNatural (for all other supported sorts). 

Example of usage: 
    
    
     procedure TForm1.Button1Click(Sender :TObject);
     begin
       NaturalSort(Memo1.Lines, stNatural); 
       NaturalSort(Memo2.Lines, stFloatThousand); 
     end;
    

#### function UTF8NaturalCompareList(aList: TStringList; Index1, Index2: Integer): Integer;

Example of usage: 
    
    
     procedure NaturalSort(aList: TStrings);
     var
       L: TStringList;
     begin
       L := TStringList.Create;
       try
         L.Assign(aList);
         L.CustomSort(@UTF8NaturalCompareList);
         aList.Assign(L);
       finally
         L.Free;
       end;
     end;
    

#### function UTF8FloatThousandCompareList(aList: TStringList; Index1, Index2: Integer): Integer;

Example of usage: 
    
    
     procedure FloatThousandSort(aList: TStrings);
     var
       L: TStringList;
     begin
       L := TStringList.Create;
       try
         L.Assign(aList);
         L.CustomSort(@UTF8FloatThousandCompareList);
         aList.Assign(L);
       finally
         L.Free;
       end;
     end;
    

#### function UTF8FloatThousandCompareText(const S1, S2: string): Integer;

#### function UTF8LogicalCompareText(const S1, S2: string): Integer;

#### function UTF8NaturalCompareText(const S1, S2: string): Integer;

# Natural sort controls

A **TNaturalListBox** and a **TNaturalComboBox** are provided along with the demo project.

---

_Source: [https://wiki.freepascal.org/NaturalSort](https://web.archive.org/web/20210124004419/https://wiki.freepascal.org/NaturalSort)_
