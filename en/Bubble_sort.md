# Bubble sort

│ **English (en)** │

Bubble sort is a simple [sorting algorithm](<sorting_algorithm.md> "sorting algorithm"). 

## Contents

  * 1 Characteristics
  * 2 Unit UBubbleSort
  * 3 Example of usage
  * 4 See also



## Characteristics

  * Very slow
  * Suitable only for small quantities sorting
  * Fast only when the array is nearly sorted



## Unit UBubbleSort
    
    
    unit UBubbleSort;
    
    interface
    
    type
      // data type
      TItemBubbleSort=integer;
    
    procedure BubbleSort( var a: array of TItemBubbleSort );
    
    
    implementation
    
    procedure swap( var a, b:TItemBubbleSort );
    var
      temp : TItemBubbleSort;
    begin
      temp := a;
      a := b;
      b := temp;
    end;
    
    
    procedure BubbleSort( var a: array of TItemBubbleSort );
    var
      n, newn, i:integer;
    begin
      n := high( a );
      repeat
        newn := 0;
        for i := 1 to n   do
          begin
            if a[ i - 1 ] > a[ i ] then
              begin
                swap( a[ i - 1 ], a[ i ]);
                newn := i ;
              end;
          end ;
        n := newn;
      until n = 0;
    end;
    
    end.
    

## Example of usage
    
    
    uses
      UBubbleSort
      ...
    
    var
      a: array[0..100] of integer; 
    begin
      ...
      BubbleSort(a);
      ...
    

## See also

  * [COMP1 Revise & Learn : Bubble Sort](<https://www.youtube.com/watch?v=DXO-nqATduU%7C>)

---

_Source: [https://wiki.freepascal.org/Bubble_sort](https://web.archive.org/web/20210616235345/https://wiki.freepascal.org/Bubble_sort)_
