# In

│ **[Deutsch (de)](</In/de> "In/de")** │  **English (en)** │  **[suomi (fi)](</In/fi> "In/fi")** │    
****

The [reserved word](<Reserved_word.md> "Reserved word") `in`: 

  * It tests whether a value is in a [set](<Set.md> "Set"). It returns the [boolean](<Boolean.md> "Boolean") value [ `true`](<True.md> "True") if the value belongs to the set and [ `false`](<False.md> "False") if the value does not belong to the set.
  * It is also used with the reserved word [`for`](<For.md> "For") in [for-in loop](<for-in_loop.md> "for-in loop").
  * It is also usable in [`uses`‑clauses](<Uses.md> "Uses").



## Example #1
    
    
    program projectin;
    uses SysUtils, TypInfo;
    type
      Berry = (Blueberry, FlyHoneysuckle, Lingonberry, Raspberry, Snowberry, Strawberry);
      Berries = set of berry;
    var
       basket: Berries;
       someberry: berry;
       str: string;
       i: integer;
    begin
      basket := [];
      writeLn('Choose a berry from the following berries into your basket');
      repeat
        i := 1;
        for someberry in berry do
          begin
            Str := GetEnumName(TypeInfo(Berry),ord(someberry));
            writeln(i,' : ',Str);
            inc(i);
          end;
        writeln('0 : exit');
        writeln;
        readln(i);
        if i>0 then
          begin
            someberry :=Berry(i-1);
            Include(basket, someberry);
          end;
      until  i=0;
      if (FlyHoneysuckle in basket) or (Snowberry in basket) then
       begin
         writeln('You have poisonous berries in your basket');
         if FlyHoneysuckle in basket then
           writeln('Your basket has a poisonous Fly honeysuckle!') ;
         if Snowberry in basket then
           writeln('Your basket has a poisonous Snowberry!');
       end;
      writeln('So you had these berries in your basket:');
      for someberry in basket do
      begin
        Str := GetEnumName(TypeInfo(Berry), Ord(someberry));
        writeln(Str);
      end;
      readln;
    end.
    

## Example #2
    
    
    const Test: char = 'q';
    
    begin
      if (Test in ['0'..'9']) then
        Writeln('Digit')
      else if (Test in ['A'..'z']) then
        Writeln('Letter')
      else
        Writeln('Other');
    end.

---

_Source: [https://wiki.freepascal.org/In](https://web.archive.org/web/20240304025818/https://wiki.freepascal.org/In)_
