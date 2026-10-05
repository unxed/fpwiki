# Editor Macro Column Align Source

Please see [Editor Macros PascalScript](<Editor_Macros_PascalScript.md> "Editor Macros PascalScript") for info how to use PascalScript Macros in the IDE Source editor. 

The macro shown below aligns selected code to a specific token. It looks for a specific token in each selected line, and aligns each occurrence of it. 

Select the 3 lines. If the selection starts right before the ":" then the ":" will be detected. A prompt will ask you to confirm the ":". Then all ":" will be aligned. (If you use a word for alignment, please note, that it will be matched regardless of word boundaries) 
    
    
      text: string;
      a: Integer;
      foo: boolean
    

The macro: 
    
    
    function IsIdent(c: Char): Boolean;
    begin
      Result := ((c >= 'a') and (c <= 'z')) or
                ((c >= 'A') and (c <= 'Z')) or
                ((c >= '0') and (c <= '9')) or
                (c = '_');
    end;
    
    var
      p1, p2: TPoint;
      s1, s2: string;
      i, j, k: Integer;
    begin
      if not Caller.SelAvail then exit;
      p1 := Caller.BlockBegin;
      p2 := Caller.BlockEnd;
      if (p1.y > p2.y) or ((p1.y = p2.y) and (p1.x > p2.x)) then begin
        p1 := Caller.BlockEnd;
        p2 := Caller.BlockBegin;
      end;
      s1 := Caller.Lines[p1.y - 1];
      s2 := '';
      i := p1.x
      while (i <= length(s1)) and (s1[i] in [#9, ' ']) do inc(i);
      j := i;
      if i <= length(s1) then begin
        if IsIdent(s1[i]) then // pascal identifier
          while (i <= length(s1)) and IsIdent(s1[i]) do inc(i)
        else
          while (i <= length(s1)) and not(IsIdent(s1[i]) or (s1[i] in [#9, ' '])) do inc(i);
      end;
      if i > j then s2 := copy(s1, j, i-j);
    
      if not InputQuery( 'Align', 'Token', s2) then exit;
    
      j := 0;
      for i := p1.y to p2.y do begin
        s1 := Caller.Lines[i - 1];
        k := pos(s2, s1);
        if (k > j) then j := k;
      end;
      if j < 1 then exit;
    
      for i := p1.y to p2.y do begin
        s1 := Caller.Lines[i - 1];
        k := pos(s2, s1);
        if (k > 0) and (k < j) then begin
          Caller.LogicalCaretXY := Point(k, i);
          while k < j do begin
            ecChar(' ');
            inc(k);
          end;
        end;
      end;
    
    end.
    

## See also

  * [Editor Macros PascalScript](<Editor_Macros_PascalScript.md> "Editor Macros PascalScript")
  * [IDE Window: Editor Macros](<IDE_Window__Editor_Macros.md> "IDE Window: Editor Macros")

---

_Source: [https://wiki.freepascal.org/Editor_Macro_Column_Align_Source](https://web.archive.org/web/20230924211618/https://wiki.freepascal.org/Editor_Macro_Column_Align_Source)_
