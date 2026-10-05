# Example: Multidimensional dynamic array

│ **English (en)** │

### Code
    
    
    program project1;
    
    type
      TArr2D = array of array of Integer;
    
    procedure  FillArr2D(Arr: TArr2D);
    var
      i, j: Integer;
    begin
      for i:=Low(Arr) to High(Arr) do
        for j:=Low(Arr[i]) to High(Arr[i]) do
          Arr[i][j] := i*10 + j;
    end;
    
    procedure  ShowArr2D(Arr: TArr2D);
    var
      i, j: Integer;
    begin
      for i:=Low(Arr) to High(Arr) do
      begin
        Write('Arr[', i, ']:  ');
        for j:=Low(Arr[i]) to High(Arr[i]) do
          Write(Arr[i][j]:2, '  ');
        WriteLn;
      end;
    end;
    
    procedure Pause;
    begin
      WriteLn;
      Write('...');
      ReadLn;
      WriteLn;
    end;
    
    
    var
      Arr2D: TArr2D;
    
    begin
      SetLength(Arr2D, 5, 3);  // resize on two dimensions
        WriteLn('Rectangular dynamic array after resizing:');
        WriteLn('-----------------------------------------');
        ShowArr2D(Arr2D);
        Pause;
    
      FillArr2D(Arr2D);
        WriteLn('Rectangular dynamic array after filling:');
        WriteLn('-----------------------------------------');
        ShowArr2D(Arr2D);
        Pause;
    
      SetLength(Arr2D, 10);  // resize on one dimension
        WriteLn('Non-rectangular dynamic array after resizing on first dimension:');
        WriteLn('----------------------------------------------------------------');
        ShowArr2D(Arr2D);
        Pause;
    
      SetLength(Arr2D[0], 9);
      SetLength(Arr2D[1], 7);
      SetLength(Arr2D[2], 6);
      SetLength(Arr2D[3], 9);
      SetLength(Arr2D[4], 5);
      SetLength(Arr2D[5], 8);
      SetLength(Arr2D[6], 5);
      SetLength(Arr2D[7], 3);
      SetLength(Arr2D[8], 1);
      SetLength(Arr2D[9], 7);
        WriteLn('Non-rectangular dynamic array after individual resizing on second dimension:');
        WriteLn('----------------------------------------------------------------------------');
        ShowArr2D(Arr2D);
        Pause;
    
      FillArr2D(Arr2D);
        WriteLn('Non-rectangular dynamic array after filling:');
        WriteLn('--------------------------------------------');
        ShowArr2D(Arr2D);
        Pause;
    end.
    

### Output
    
    
    Rectangular dynamic array after resizing:
    -----------------------------------------
    Arr[0]:   0   0   0
    Arr[1]:   0   0   0
    Arr[2]:   0   0   0
    Arr[3]:   0   0   0
    Arr[4]:   0   0   0
    
    ...
    
    Rectangular dynamic array after filling:
    -----------------------------------------
    Arr[0]:   0   1   2
    Arr[1]:  10  11  12
    Arr[2]:  20  21  22
    Arr[3]:  30  31  32
    Arr[4]:  40  41  42
    
    ...
    
    Non-rectangular dynamic array after resizing on first dimension:
    ----------------------------------------------------------------
    Arr[0]:   0   1   2
    Arr[1]:  10  11  12
    Arr[2]:  20  21  22
    Arr[3]:  30  31  32
    Arr[4]:  40  41  42
    Arr[5]:
    Arr[6]:
    Arr[7]:
    Arr[8]:
    Arr[9]:
    
    ...
    
    Non-rectangular dynamic array after individual resizing on second dimension:
    ----------------------------------------------------------------------------
    Arr[0]:   0   1   2   0   0   0   0   0   0
    Arr[1]:  10  11  12   0   0   0   0
    Arr[2]:  20  21  22   0   0   0
    Arr[3]:  30  31  32   0   0   0   0   0   0
    Arr[4]:  40  41  42   0   0
    Arr[5]:   0   0   0   0   0   0   0   0
    Arr[6]:   0   0   0   0   0
    Arr[7]:   0   0   0
    Arr[8]:   0
    Arr[9]:   0   0   0   0   0   0   0
    
    ...
    
    Non-rectangular dynamic array after filling:
    --------------------------------------------
    Arr[0]:   0   1   2   3   4   5   6   7   8
    Arr[1]:  10  11  12  13  14  15  16
    Arr[2]:  20  21  22  23  24  25
    Arr[3]:  30  31  32  33  34  35  36  37  38
    Arr[4]:  40  41  42  43  44
    Arr[5]:  50  51  52  53  54  55  56  57
    Arr[6]:  60  61  62  63  64
    Arr[7]:  70  71  72
    Arr[8]:  80
    Arr[9]:  90  91  92  93  94  95  96
    
    ...
    

  


## See also

  * [Dynamic array](<Dynamic_array.md> "Dynamic array")
  * [Free Pascal Reference guide: Dynamic arrays](<http://www.freepascal.org/docs-html/current/ref/refsu15.html#x39-520003.3.1>)

---

_Source: [https://wiki.freepascal.org/Example%3A_Multidimensional_dynamic_array](https://web.archive.org/web/20241127045139/https://wiki.freepascal.org/Example%3A_Multidimensional_dynamic_array)_
