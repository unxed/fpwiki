# Radix sort

│ **English (en)** │  **[suomi (fi)](</Radix_sort/fi> "Radix sort/fi")** │  **[français (fr)](</Radix_sort/fr> "Radix sort/fr")** │    
****

The Radix sort is an [integer](<Integer.md> "Integer") [sorting algorithm](<sorting_algorithm.md> "sorting algorithm"). 

## Features

  * Very fast



## unit URadixSort.pas
    
    
    unit URadixSort;
    
    {$mode objfpc}{$H+}
    
    interface
    
    uses
      Classes, SysUtils, 
      GQueue ; //  packages/fcl-stl/src/gqueue.pp
    
    type
      // data type
      TItemRadixSort=integer;
    
    // sorting function
    procedure RadixSort( var a: array of TItemRadixSort );
    
    implementation
    
    
    
    procedure RadixSort( var a: array of TItemRadixSort );
    
    const
      BASE = 16;
    
    type TQueueIRS = specialize TQueue< TItemRadixSort >;
    
    var
      jono : array[ 0 .. BASE - 1 ] of TQueueIRS;
      max : TItemRadixSort;
      i ,k : integer;
    
      procedure pick;
      var
        i, j: integer;
      begin
        i := 0;
        j := 0;
        while i < high( a ) do
          begin
             while not jono[ j ].IsEmpty do
               begin
                 a[ i ] := jono[ j ].Front;
                 jono[ j ].Pop;
                 inc( i );
               end;
             inc( j );
          end;
      end;
    
    begin
      max := high( a );
      for i := 0 to BASE - 1 do
        jono[ i ] := TQueueIRS.Create;
      for i := low( a ) to high( a ) do
        begin
          if a[ i ] > max then max := a[ i ];
          jono[ abs( a[ i ] mod BASE ) ].Push( a[ i ] );
        end;
      pick;
      k := BASE;
      while  max > k do
        begin
          for i := low( a ) to high( a ) do
            jono[ abs( a[ i ] div k mod BASE ) ].Push( a[ i ] );
          pick;
          k := k * BASE;
        end;
      for i := 0 to BASE - 1 do
        jono[ i ].Free;
    
    end;
    
    end.
    

  


## Example of the use
    
    
    uses
      URadixSort
    
      ...
    
    var
    
      a: array[0..100] of integer; 
    
    
    begin
    
      ...
    
      RadixSort( a );

---

_Source: [https://wiki.freepascal.org/Radix_sort](https://web.archive.org/web/20231121005012/https://wiki.freepascal.org/Radix_sort)_
