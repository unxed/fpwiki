# Counting sort

│ **English (en)** │

The counting sort is an [integer](<Integer.md> "Integer") [sorting algorithm](<sorting_algorithm.md> "sorting algorithm"). 

## Contents

  * 1 Features
  * 2 unit UCountingSortExtra.pas
  * 3 unit UCountingSort.pas
  * 4 Example of the use



## Features

  * Very fast
  * Only small integers



## unit UCountingSortExtra.pas
    
    
    unit UCountingSortExtra;
    
    interface
    
    type
      // data type
      TItemCountingSort=integer;
    
    // sort order
    function IsAscendingCountingSort: boolean; inline;
    
    implementation
    
    // sort order
    function IsAscendingCountingSort: boolean; inline;
    begin
       result := true;
    end;
    
    end.
    

  


## unit UCountingSort.pas
    
    
    unit UCountingSort;
    
    interface
    
    uses
      UCountingSortExtra;
    
    // sorting function
    procedure CountingSort( var a: array of TItemCountingSort );
    
    implementation
    
    
    procedure CountingSort( var a: array of TItemCountingSort );
    var
      min, max : TItemCountingSort;
      count_a  : array of integer;
      i, j, z  : integer;
    begin
      min := high( a );
      max := min;
      for i := low( a ) to high( a ) do
        begin
          if a[ i ] < min then min := a[ i ];
          if a[ i ] > max then max := a[ i ];
        end;
      SetLength( count_a, max - min + 1);
      for i := 0 to ( max - min ) do count_a[ i ] := 0;
      for i := low( a ) to high( a ) do
        count_a[ a[ i ] - min ] := count_a[ a[ i ] - min ] + 1;
      if IsAscendingCountingSort then z:= low( a ) else z := high( a );
      for i := min to max do
         for j := 0 to ( count_a[ i - min ] - 1 ) do
           begin
             a[ z ] := i;
             if IsAscendingCountingSort then inc( z ) else dec( z );
           end;
    end;
    
    end.
    

## Example of the use
    
    
    uses
      UCountingSort
    
      ...
    
    var
    
      a: array[0..100] of integer;
    
    
    begin
    
      ...
    
      CountingSort( a );

---

_Source: [https://wiki.freepascal.org/Counting_sort](https://web.archive.org/web/20250516143331/https://wiki.freepascal.org/Counting_sort)_
