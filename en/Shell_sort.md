# Shell sort

│ **English (en)** │    
****

The Shell sort algorithm (aka shellsort or Shell's sort) is an [integer](<Integer.md> "Integer") [sorting algorithm](<sorting_algorithm.md> "sorting algorithm"). It is a fast sorting algorithm (although slower than quicksort) that has the advantage that it is non-recursive, so it doesn't use the call stack. Therefore the method is advantageous on small and embedded systems and for sorting very large arrays. The algorithm was published in assembler code by Donald L. Shell in 1959. 

## Contents

  * 1 Features
  * 2 unit UShellSort.pas
  * 3 Example of the use
  * 4 References



## Features

  * Very fast
  * Doesn't need the call stack



## unit UShellSort.pas
    
    
    unit UShellSort;
    
    {$mode objfpc}{$H+}
    
    interface
    
    uses
      Classes, SysUtils;
    
    type
      TShellSortItem = integer;
    
    procedure ShellSort(var a: array of TShellSortItem);
    
    implementation
    
    procedure ShellSort(var a: array of TShellSortItem);
    var i, j, h, n, v : integer;
    begin
      n := length(a);
      h := 1;
      repeat
       h := 3*h + 1
      until h > n;
      repeat
       h := h div 3;
       for i := h to n-1 do
        begin
         v := a[i];
         j := i;
         while (j >= h) AND (a[j-h] > v) do
          begin
            a[j] := a[j-h];
            j := j - h;
          end;
         a[j] := v;
        end
       until h = 1;
    end;
    
    end.
    

## Example of the use
    
    
    uses
      UShellSort
    
      ...
    
    var
    
      a: array[0..100] of integer; 
    
    
    begin
    
      ...
    
      ShellSort(a);
    

## References

  * Shell, D. L. (1959). [A High-Speed Sorting Procedure](<http://penguin.ewu.edu/cscd300/Topic/AdvSorting/p30-shell.pdf>) (PDF). Communications of the ACM. 2 (7): 30–32. [doi: 10.1145/368370.368387](<https://doi.org/10.1145%2F368370.368387>).
  * Karen Van Houten, Shell Sort, University of Idaho. Archived at [Wayback Machine](<https://web.archive.org/web/20060914153030/http://www.cs.uidaho.edu/~karenv/cs213/cs213.useful.pages/shell.sort.html>).

---

_Source: [https://wiki.freepascal.org/Shell_sort](https://web.archive.org/web/20240907084009/https://wiki.freepascal.org/Shell_sort)_
