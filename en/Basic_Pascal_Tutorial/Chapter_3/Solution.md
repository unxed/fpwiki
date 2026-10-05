# Basic Pascal Tutorial/Chapter 3/Solution

│ **[български (bg)](</Basic_Pascal_Tutorial/Chapter_3/Solution/bg> "Basic Pascal Tutorial/Chapter 3/Solution/bg")** │  **English (en)** │  **[français (fr)](</Basic_Pascal_Tutorial/Chapter_3/Solution/fr> "Basic Pascal Tutorial/Chapter 3/Solution/fr")** │  **[日本語 (ja)](</Basic_Pascal_Tutorial/Chapter_3/Solution/ja> "Basic Pascal Tutorial/Chapter 3/Solution/ja")** │  **[中文（中国大陆） (zh_CN)](</Basic_Pascal_Tutorial/Chapter_3/Solution/zh_CN> "Basic Pascal Tutorial/Chapter 3/Solution/zh CN")** │    
****

[ ◄ ](<Programming_Assignment.md> "Basic Pascal Tutorial/Chapter 3/Programming Assignment") | [ ▲ ](<../Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<../Chapter_4/Procedures.md> "Basic Pascal Tutorial/Chapter 4/Procedures")  
---|---|---  
  
3Ea - Solutions (author: Tao Yue, state: unchanged) 

**Solution to Fibonacci Sequence Problem**
    
    
    (* Author:    Tao Yue
       Date:      19 July 1997
       Description:
          Find the first 10 Fibonacci numbers
       Version:
          1.0 - original version
    *)
    
    program Fibonacci;
    
    var
       Fibonacci1, Fibonacci2 : integer;
       temp : integer;
       count : integer;
    
    begin    (* Main *)
       writeln ('First ten Fibonacci numbers are:');
       count := 0;
       Fibonacci1 := 0;
       Fibonacci2 := 1;
       repeat
          write (Fibonacci2:7);
          temp := Fibonacci2;
          Fibonacci2 := Fibonacci1 + Fibonacci2;
          Fibonacci1 := Temp;
          count := count + 1
       until count = 10;
       writeln;
    
       (* Of course, you could use a FOR loop or a WHILE loop
          to solve this problem. *)
    
    end.     (* Main *)
    

**Solution to Powers of Two Problem**
    
    
    (* Author:    Tao Yue
       Date:      13 July 2000
       Description:
          Display all powers of two up to 20000, five per line
       Version:
          1.0 - original version
    *)
    
    program PowersofTwo;
    
    const
       numperline = 5;
       maxnum = 20000;
       base = 2;
    
    var
       number : longint;
       linecount : integer;
    
    begin    (* Main *)
       writeln ('Powers of ', base, ', 1 <= x <= ', maxnum, ':');
       (* Set up for loop *)
       number := 1;
       linecount := 0;
       (* Loop *)
       while number <= maxnum do
          begin
             linecount := linecount + 1;
             (* Print a comma and space unless this is the first
                number on the line *)
             if linecount > 1 then
                write (', ');
             (* Display the number *)
             write (number);
             (* Print a comma and go to the next line if this is
                the last number on the line UNLESS it is the
                last number of the series *)
             if (linecount = numperline) and not (number * 2 > maxnum) then
                begin
                   writeln (',');
                   linecount := 0
                end;
             (* Increment number *)
             number := number * base;
          end;  (* while *)
       writeln;
    
       (* This program can also be written using a
          REPEAT..UNTIL loop. *)
    
    end.     (* Main *)
    

Note that I used three constants: the base, the number of powers to display on each line, and the maximum number. This ensures that the program can be easily adaptable in the future. 

Using constants rather than literals is a good programming habit to form. When you write really long programs, you may refer to certain numbers thousands of times. If you hardcoded them into your code, you'd have to search them out. Also, you might use the same value in a different context, so you can't simply do a global Search-and-Replace. Using a constant makes it simpler to expand the program. 

Also note that I used the `longint` type for the number variable. This is because to fail the test `number <= 20000`, `number` would have to reach `32768`, the next power of two after `16384`. This exceeds the range of the integer type: `-32768` to `32767`. (try it without `longint` and see what happens) 

[ ◄ ](<Programming_Assignment.md> "Basic Pascal Tutorial/Chapter 3/Programming Assignment") | [ ▲ ](<../Contents.md> "Basic Pascal Tutorial/Contents") | [ ► ](<../Chapter_4/Procedures.md> "Basic Pascal Tutorial/Chapter 4/Procedures")  
---|---|---

---

_Source: [https://wiki.freepascal.org/Basic_Pascal_Tutorial/Chapter_3/Solution](https://web.archive.org/web/20250403223925/https://wiki.freepascal.org/Basic_Pascal_Tutorial/Chapter_3/Solution)_
