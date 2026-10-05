# Format function

│ **English (en)** │  **[suomi (fi)](</Format_function/fi> "Format function/fi")** │    
****

The **format** [function](<Function.md> "Function") formats a series of values into a [string](<String.md> "String"). The format function is found in the unit _[SysUtils](</index.php?title=SysUtils_unit&action=edit&redlink=1> "SysUtils unit \(page does not exist\)")_. For an in-depth explanation see the [`sysutils.format`](<https://www.freepascal.org/docs-html/rtl/sysutils/format.html>) on line documentation. 

## Contents

  * 1 Syntax
    * 1.1 Integer formatting
    * 1.2 String formatting
  * 2 Example of usage
  * 3 See also



## Syntax

Format is overloaded with two ways to call it: 
    
    
    function Format( const Fmt: String; const Args: array of Const) : String;
    function Format( const Fmt: String; const Args: array of Const; const FormatSettings: TFormatSettings) : String;
    

The Fmt String argument contains _format specifiers_ or _placeholders_ (e.g. %d) which correspond to and are replaced in the result string by values from the Args array. An [array of const](<Const.md> "Const") can hold a [variable](<Variable.md> "Variable") amount of values of different types, with the applicable types for the Format function being the variations of the [Real](<Real.md> "Real"), [Integer](<Integer.md> "Integer"), [Pointer](<Pointer.md> "Pointer") and [String](<String.md> "String") types. [TFormatSettings](</index.php?title=TFormatSettings&action=edit&redlink=1> "TFormatSettings \(page does not exist\)") is a [record](<Record.md> "Record") which holds information related to desired formatting of number, time, date and currency values. 

  
The formatting rule for each given _format specifier_ is: 
    
    
    % [ArgumentIndex:] ['-'] [width] [. precision] ArgumentType
    

d | Decimal (integer)   
---|---  
e | Scientific   
f | Fixed   
g | General   
m | Money   
n | Number (floating)   
p | Pointer   
s | String   
u | Unsigned decimal   
x | Hexadecimal   
  
### Integer formatting

%d  | Will print the integer as it is.   
---|---  
%8d  | Will print the integer as it is. If the number of digits is less than 8, the output will be padded on the left.   
%-8d  | Will print the integer as it is. If the number of digits is less than 8, the output will be padded on the right.   
%.8d  | Will print the integer as it is. If the number of digits is less than 8, the output will be padded on the left with zeroes.   
  
### String formatting

%s  | Will print the string as it is.   
---|---  
%8s  | Will print the string as it is. If the string has less than 8 characters, the output will be space-padded on the left (right-justified).   
%-8s  | Will print the string as it is. If the string has less than 8 characters, the output will be space-padded on the right (left-justified).   
  
## Example of usage
    
    
    procedure TForm1.Button1Click(Sender: TObject);
    var
      title,  title2,
      underline,
      line, row1, row2, row3,
      fmt : string;
      i : integer;
    begin
      fmt := '%-12s';
      title := format(fmt,['Column 1']) + format(fmt,['Column 2']) ;
      for i := 1 to 12 do underline := underline + '-';
      underline := underline + underline ;
      fmt := '%-12d';
      line := format(fmt,[15]) + format(fmt,[8]) ;
      fmt := '%12s';
      title2 := format(fmt,['Column 1']) + format(fmt,['Column 2']) ;
      fmt := '%12d';
      row1 := format(fmt,[15]) + format(fmt,[8]) ;
      row2 := format(fmt,[1005]) + format(fmt,[809]) ;
      fmt := '%12.5d';
      row3 := format(fmt,[1005]) + format(fmt,[809]) ;
      Memo1.Lines.Add( title );
      Memo1.Lines.Add( underline );
      Memo1.Lines.Add( line );
      Memo1.Lines.Add( '' );
      Memo1.Lines.Add( title2 );
      Memo1.Lines.Add( underline );
      Memo1.Lines.Add( row1 );
      Memo1.Lines.Add( row2 );
      Memo1.Lines.Add( row3 );
    end;
    

The above statements will create a set of strings that look like: 
    
    
    Column 1    Column 2    
    ------------------------
    15          8           
    
    
    
        Column 1    Column 2
    ------------------------
              15           8
            1005         809
           01005       00809
    

## See also

  * [ Format ](<http://lazarus-ccr.sourceforge.net/docs/rtl/sysutils/format.html> "doc:rtl/sysutils/format.html")
  * [ FloatToStrF ](<http://lazarus-ccr.sourceforge.net/docs/rtl/sysutils/floattostrf.html> "doc:rtl/sysutils/floattostrf.html")
  * [ FormatFloat ](<http://lazarus-ccr.sourceforge.net/docs/rtl/sysutils/formatfloat.html> "doc:rtl/sysutils/formatfloat.html")
  * [ IntToHex ](<http://lazarus-ccr.sourceforge.net/docs/rtl/sysutils/inttohex.html> "doc:rtl/sysutils/inttohex.html") \- Convert an integer into a [hexadecimal](<Hexadecimal.md> "Hexadecimal") string.

---

_Source: [https://wiki.freepascal.org/Format_function](https://web.archive.org/web/20250516142941/https://wiki.freepascal.org/Format_function)_
