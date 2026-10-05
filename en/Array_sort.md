# Array sort

A typical task is to sort an array. One of the problems is that an array could be of any type. Not a simple type, but more complicated types. 

## Contents

  * 1 Basic Version
    * 1.1 Source Code
    * 1.2 Use Sample
  * 2 Generics Version
    * 2.1 Source Code
    * 2.2 Use Sample



## Basic Version

Prior to introducing [Generics](<Generics.md> "Generics") into Freepascal, sorting an array of arbitrary type could be achieve via untyped variables (implicit [Pointers](<Pointers.md> "Pointers")). 

  * No generics involved
  * works on any version of FPC (or Delphi)
  * any array data type



### Source Code
    
    
    unit anysort;
     
    {$ifdef fpc}{$mode delphi}{$H+}{$endif}
     
    interface
     
    type
      TCompareFunc = function (const elem1, elem2): Integer;
     
    procedure AnySort(var Arr; Count: Integer; Stride: Integer; CompareFunc: TCompareFunc);
     
    implementation
     
    type
      TByteArray = array [Word] of byte;
      PByteArray = ^TByteArray;
     
    procedure AnyQuickSort(var Arr; idxL, idxH: Integer;
      Stride: Integer; CompareFunc: TCompareFunc; var SwapBuf, MedBuf);
    var
      ls,hs : Integer;
      li,hi : Integer;
      mi    : Integer;
      ms    : Integer;
      pb    : PByteArray;
    begin
      pb:=@Arr;
      li:=idxL;
      hi:=idxH;
      mi:=(li+hi) div 2;
      ls:=li*Stride;
      hs:=hi*Stride;
      ms:=mi*Stride;
      Move(pb[ms], medBuf, Stride);
      repeat
        while CompareFunc( pb[ls], medBuf) < 0 do begin
          inc(ls, Stride);
          inc(li);
        end;
        while CompareFunc( medBuf, pb[hs] ) < 0 do begin
          dec(hs, Stride);
          dec(hi);
        end;
        if ls <= hs then begin
          Move(pb[ls], SwapBuf, Stride);
          Move(pb[hs], pb[ls], Stride);
          Move(SwapBuf, pb[hs], Stride);
          // begin fix 11/11/2021: update ms if the reference point is moved
          if li=mi then ms:=hs;
          if hi=mi then ms:=ls;
          // end fix
          inc(ls, Stride); inc(li);
          dec(hs, Stride); dec(hi);
        end;
      until ls>hs;
      if hi>idxL then AnyQuickSort(Arr, idxL, hi, Stride, CompareFunc, SwapBuf, MedBuf);
      if li<idxH then AnyQuickSort(Arr, li, idxH, Stride, CompareFunc, SwapBuf, MedBuf);
    end;
     
    procedure AnySort(var Arr; Count: Integer; Stride: Integer; CompareFunc: TCompareFunc);
    var
      buf: array of byte;
    begin
      if Count <= 1 then Exit; // should be more than 1 to be sortable
      SetLength(buf, Stride*2);
      AnyQuickSort(Arr, 0, Count-1, Stride, compareFunc, buf[0], buf[Stride]);
    end;
     
    end.
    

### Use Sample

Here's an example on how to use AnySort() function to sort an array of Integer 
    
    
    interface
    
    uses AnySort;
    
    procedure SortArrayInteger(var arr: array of Integer; count: Integer);
    
    implementation
     
    function CompareInt(const d1,d2): integer;
    var
      i1 : integer absolute d1;
      i2 : integer absolute d2;
    begin
      if i1=i2 then Result:=0
      else if i1<i2 then Result:=-1
      else Result:=1;
    end;
    
    procedure SortArrayInteger(var arr: array of Integer; count: Integer);
    begin
      anysort.AnySort(arr, Count, sizeof(Integer), @CompareInt);
    end;
    

## Generics Version

  * FPC-only.
  * Uses **generic** with static comparer.
  * Comparer is a simple _less_ predicate.
  * Essentially an Introsort, a QuickSort that falls back to a simple algorithm for small enough subarrays.
  * Bypasses managed types handling to greatly speed up sorting arrays of reference-counted types; won't work with custom Copy/AddRef operators.
  * Protected against 
    * O(N²) time (falls back to heap sort);
    * O(N) recursion depth; maximum recursion depth is ⌈bitsizeof(SizeUint) - log₂ SelectionThreshold⌉.



### Source Code
    
    
    {$mode objfpc} {$coperators on}
    unit AnySort2;
    
    interface
    
    type
    	// Comparer should provide function Less(const a, b: Elem): boolean.
    	generic Sorter<Elem, Comparer> = class
    	type
    		pElem = ^Elem;
    
    		class procedure Sort(p: pElem; count: SizeUint); static;
    		class procedure Sort(var a: array of Elem); static;
    
    	private const
    		SelectionThreshold = 12;
    		HeapArity = 4;
    	type
    		SwapTemp = array[0 .. sizeof(Elem) - 1] of byte;
    
    		class procedure SelectionSort(p: pElem; count: SizeUint); static;
    
    		class function Median(p: pElem; count: SizeUint): pElem; static;
    		class procedure QSort(p: pElem; count, reasonable: SizeUint); static;
    
    		class procedure HeapReplacePessimistic(p: pElem; count, id: SizeUint; const x: SwapTemp); static;
    		class procedure HeapSort(p: pElem; count: SizeUint); static;
    	end;
    
    	generic ComparerLessOp<Elem> = class
    		class function Less(const a, b: Elem): boolean; static; inline;
    	end;
    
    	generic SorterLessOp<Elem> = class(specialize Sorter<Elem, specialize ComparerLessOp<Elem>>) end;
    
    implementation
    
    	class procedure Sorter.Sort(p: pElem; count: SizeUint);
    	begin
    		QSort(p, count, count);
    	end;
    
    	class procedure Sorter.Sort(var a: array of Elem);
    	begin
    		Sort(pElem(a), length(a));
    	end;
    
    	class procedure Sorter.SelectionSort(p: pElem; count: SizeUint);
    	var
    		i, j, imin: SizeInt;
    		t: SwapTemp;
    	begin
    		for i := 0 to SizeInt(count) - 2 do
    		begin
    			imin := i;
    			for j := i + 1 to SizeInt(count) - 1 do
    				if Comparer.Less(p[j], p[imin]) then imin := j;
    			t := SwapTemp(p[i]); SwapTemp(p[i]) := SwapTemp(p[imin]); SwapTemp(p[imin]) := t;
    		end;
    	end;
    
    	class function Sorter.Median(p: pElem; count: SizeUint): pElem;
    	var
    		a, b, c, t: pElem;
    	begin
    		a := p;
    		b := p + count div 2;
    		c := p + SizeUint(count - 1);
    		if Comparer.Less(b^, a^) then begin t := a; a := b; b := t; end;
    		if Comparer.Less(c^, b^) then begin t := c; c := b; b := t; end;
    		if Comparer.Less(b^, a^) then result := a else result := b;
    	end;
    
    	// see MSVC implementation of std::sort
    	class procedure Sorter.QSort(p: pElem; count, reasonable: SizeUint);
    	var
    		L, R: SizeInt;
    		t, pivot: SwapTemp;
    	begin
    		while (count > SelectionThreshold) and (reasonable > 0) do
    		begin
    			reasonable := reasonable div 2 + reasonable div 4;
    
    			pivot := SwapTemp(Median(p, count)^);
    			R := 0;
    			L := count - 1;
    
    			repeat
    				while Comparer.Less(p[R], Elem(pivot)) do inc(R);
    				while Comparer.Less(Elem(pivot), p[L]) do dec(L);
    				if R <= L then
    				begin
    					t := SwapTemp(p[R]); SwapTemp(p[R]) := SwapTemp(p[L]); SwapTemp(p[L]) := t;
    					inc(R);
    					dec(L);
    				end;
    			until R > L;
    
    			// [0 .. L], [R .. count - 1]
    			// possible edge cases are L = -1 or R = count !
    			if SizeInt(count) - R <= L then
    			begin
    				QSort(p + R, SizeInt(count) - R, reasonable); // QSort calls with count = 0 or count = 1 are safe.
    				count := L + 1;
    			end else
    			begin
    				QSort(p, L + 1, reasonable);
    				p += R;
    				count -= R;
    			end;
    		end;
    		if count > SelectionThreshold then
    			HeapSort(p, count)
    		else if count >= 2 then
    			SelectionSort(p, count);
    	end;
    
    	// HeapReplacePessimistic(p, count, id, x)
    	//
    	// is equivalent to
    	//
    	// p[id] := x;
    	// id := HeapDownThoroughly(..., id);
    	// HeapUp(..., id)
    	//
    	// where HeapDownThoroughly doesn't stop at correct position, instead shifting the element all the way down, saving one compare at each level.
    	// HeapUp then bubbles the element up to the correct position.
    	// See Python's 'heapq' for justification over simple HeapDown.
    	//
    	// Careful with 'x' passed by reference and pointing into 'p'!
    
    	class procedure Sorter.HeapReplacePessimistic(p: pElem; count, id: SizeUint; const x: SwapTemp);
    	var
    		child, bestChild, lastChild, parent, start: SizeUint;
    	begin
    		start := id;
    		repeat
    			child := HeapArity * id; // childs of 'id' are p[child + 1] ... p[child + HeapArity].
    			bestChild := child + 1;
    			if bestChild >= count then break;
    
    			lastChild := child + HeapArity;
    			if lastChild >= count then lastChild := count - 1;
    
    			for child := child + 2 to lastChild do
    				if Comparer.Less(p[bestChild], p[child]) then bestChild := child;
    
    			SwapTemp(p[id]) := SwapTemp(p[bestChild]);
    			id := bestChild;
    		until false;
    
    		while id > start do
    		begin
    			parent := (id - 1) div HeapArity;
    			if not Comparer.Less(p[parent], Elem(x)) then break;
    			SwapTemp(p[id]) := SwapTemp(p[parent]);
    			id := parent;
    		end;
    		SwapTemp(p[id]) := x;
    	end;
    
    	class procedure Sorter.HeapSort(p: pElem; count: SizeUint);
    	var
    		i: SizeInt;
    		t: SwapTemp;
    	begin
    		for i := SizeInt((count + (HeapArity - 2)) div HeapArity) - 1 downto 0 do
    		begin
    			t := SwapTemp(p[i]);
    			HeapReplacePessimistic(p, count, i, t);
    		end;
    		for i := SizeInt(count) - 1 downto 1 do
    		begin
    			t := SwapTemp(p[i]);
    			SwapTemp(p[i]) := SwapTemp(p[0]);
    			HeapReplacePessimistic(p, i, 0, t);
    		end;
    	end;
    
    	class function ComparerLessOp.Less(const a, b: Elem): boolean;
    	begin
    		result := a < b;
    	end;
    
    end.
    

### Use Sample

Usage:
    
    
    {$mode objfpc}
    uses AnySort2;
    
    var
    	a: array of integer;
    
    begin
    	a := specialize TArray<integer>.Create(111, 555, 888, 777, 333, 444, 666);
    	specialize SorterLessOp<integer>.Sort(a);
    end.

---

_Source: [https://wiki.freepascal.org/Array_sort](https://web.archive.org/web/20240308084706/https://wiki.freepascal.org/Array_sort)_
