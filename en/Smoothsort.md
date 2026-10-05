# Smoothsort

│ **English (en)** │

Smoothsort is a comparison-based [sorting algorithm](<sorting_algorithm.md> "sorting algorithm"). 

## unit USmoothsortExtra.pas
    
    
    unit USmoothsortExtra;
    
    {$mode objfpc}{$H+}
    
    interface
    
    
    type
      // data type
     TItemSmoothsort = integer;
    
    // comparison function
    function IsAscendingSmoothsort(v1, v2: TItemSmoothsort): boolean;
    
    implementation
    
    // customizable comparison function
    function IsAscendingSmoothsort(v1, v2: TItemSmoothsort): boolean; inline;
    begin
       result := v1 <= v2;
    end;
    
    end.
    

## unit USmoothsort.pas
    
    
    unit USmoothsort;
    
    interface
    uses USmoothsortExtra;
    
    // sorting function
    procedure Smoothsort(var A: array of TItemSmoothsort);
    
    implementation
    
    // implementation of Djikstra's algorithm
    procedure Smoothsort(var A: array of TItemSmoothsort);
    var
      q, r,
      p, b, c,
      r1, b1, c1,
      N: integer;
    
      procedure up(var vb, vc: integer);
      var
        temp: integer;
      begin
        temp := vb;
        vb := vb + vc + 1;
        vc := temp;
      end;
    
      procedure down(var vb, vc: integer);
      var
        temp: integer;
      begin
        temp := vc;
        vc := vb - vc - 1;
        vb := temp;
      end;
    
      procedure sift;
      var
        r0, r2: integer;
        T: TItemSmoothsort;
      begin
        r0 := r1;
        T := A[r0];
        while b1 >= 3 do
          begin
            r2 := r1 - b1 + c1;
            if not IsAscendingSmoothsort(A[r1 - 1], A[r2]) then
              begin
                r2 := r1 - 1;
                down(b1, c1);
              end;
            if IsAscendingSmoothsort(A[r2], T) then b1 := 1
              else
                begin
                  A[r1] := A[r2];
                  r1 := r2;
                  down(b1, c1);
                end;
          end; // of while b1 >= 3
        if r1 <> r0 then A[r1] := T;
      end;
    
      procedure trinkle;
      var
        p1, r2, r3, r0 : integer;
        T: TItemSmoothsort;
      begin
        p1 := p;
        b1 := b;
        c1 := c;
        r0 := r1;
        T := A[r0];
        while p1 > 0 do
          begin
            while (p1 and 1) = 0 do
              begin
                p1 := p1 shr 1;
                up(b1, c1);
              end; // of while (p1 and 1) = 0
            r3 := r1 - b1;
            if (p1 = 1) or IsAscendingSmoothsort(A[r3], T) then p1 := 0
              else // p1>1
                begin
                  p1 := p1 - 1;
                  if b1 = 1 then
                    begin
                      A[r1] := A[r3];
                      r1 := r3;
                    end
                  else
                    if b1 >= 3 then
                      begin
                        r2 := r1 - b1 + c1;
                        if not IsAscendingSmoothsort(A[r1 - 1], A[r2]) then
                          begin
                            r2 := r1 - 1;
                            down(b1, c1);
                            p1 := p1 shl 1;
                          end;
                        if IsAscendingSmoothsort(A[r2], A[r3]) then
                          begin
                            A[r1] := A[r3];
                            r1 := r3;
                          end
                        else
                          begin
                            A[r1] := A[r2];
                            r1 := r2;
                            down(b1, c1);
                            p1 := 0;
                          end;
                      end; // of if b1 >= 3
                end;  // of else  p1 > 1
          end;  // of while p1 > 0
        if r0 <> r1 then A[r1] := T;
        sift;
      end;
    
      procedure semitrinkle;
      var
        T: TItemSmoothsort;
      begin
        r1 := r - c;
        if not IsAscendingSmoothsort(A[r1], A[r]) then
          begin
            T := A[r];
            A[r] := A[r1];
            A[r1] := T;
            trinkle;
          end;
      end;
    
    begin
      N := length(A);
      q := 1;
      r := 0;
      p := 1;
      b := 1;
      c := 1;
    
      //building tree
      while q < N do
        begin
          r1 := r;
          if (p and 7) = 3 then
            begin
              b1 := b;
              c1 := c;
              sift;
              p := ( p + 1 ) shr 2;
              up(b, c);
              up(b, c);
            end
          else
            if (p and 3) = 1 then
              begin
               if q + c < N then
                 begin
                    b1 := b;
                    c1 := c;
                    sift;
                 end
                 else trinkle;
               down(b, c);
               p := p shl 1;
               while b > 1 do
               begin
                 down(b, c);
                 p := p shl 1;
               end;  // of while b > 1
               p := p + 1;
              end;  // of if (p and 3) = 1
         q := q + 1;
         r := r + 1;
       end;
       r1 := r;
       trinkle;
    
       //bulding sorted array
       while q > 1 do
         begin
           q := q - 1;
           if b = 1 then
           begin
             r := r - 1;
             p := p - 1;
             while (p and 1) = 0 do
               begin
                 p := p shr 1;
                 up(b, c);
               end; // of while (p and 1) = 0
           end
           else
             if b >= 3 then
               begin
                 p := p - 1;
                 r := r - b + c;
                 if p > 0 then semitrinkle;
                 down(b, c);
                 p := p shl 1 + 1;
                 r := r + c;
                 semitrinkle;
                 down(b, c);
                 p := p shl 1 + 1;
               end; // of if b >= 3
           //element q is done
         end;
       //element 0 is done
    end;
    
    end.
    

## Example of the use
    
    
    uses
      USmoothsort
    
      ...
    
    var
    
      a: array[0..100] of integer; 
    
    
    begin
    
      ...
    
      Smoothsort( a );

---

_Source: [https://wiki.freepascal.org/Smoothsort](https://web.archive.org/web/20240906180543/https://wiki.freepascal.org/Smoothsort)_
