# AVR Embedded Tutorial - Int to digits

│ **[Deutsch (de)](</AVR_Embedded_Tutorial_-_Int_to_digits/de> "AVR Embedded Tutorial - Int to digits/de")** │  **English (en)** │ 

## Contents

  * 1 Output digits (7 segment display)
    * 1.1 4-digit output on a 7-segment display
      * 1.1.1 Declaration of the variables
      * 1.1.2 On the PC
      * 1.1.3 On the AVR
      * 1.1.4 Hexadecimal output
      * 1.1.5 Call
      * 1.1.6 Output
  * 2 See also



# Output digits (7 segment display)

If you want to output an integer on a 7-segment display, the best way to do this is with the procedure described below. This looks more complex than with div and mode, but is much faster because the AVR cannot do hardware division. The AVR has to do division with great effort and this takes a lot of time. 

## 4-digit output on a 7-segment display

### Declaration of the variables
    
    
    var
      Digit   : array[0..3] of byte; // Values ​​of the individual digits
      MyValue : Integer;             // An integer to be output
    

### On the PC

The individual digits would be calculated on the PC in this way. With an ATtiny this cannot even be compiled because it cannot do division in hardware. You could do the division in software, but this would take a lot of time. 
    
    
    Digit[0] := MyValue div 1000;
    Digit[1] := MyValue div 100 mod 10;
    Digit[2] := MyValue div 10 mod 10;
    Digit[3] := MyValue mod 10;
    

### On the AVR

But since the ATmega cannot divide and the ATtiny does not even know multiplication, you have to calculate this as follows: 
    
    
    procedure IntToDigit(val : UInt16);
      var
        aChr  : byte;
        empty : boolean;
    begin
        // Thousands
        aChr  := 0;
        empty := True;
    
        while(val >= 1000) do 
          begin
            Dec(val, 1000);
            Inc(aChr);
            empty := false;
          end;
    
        if empty then 
          begin
            achr := 16;
          end;
    
        Digit[0] := aChr;
    
        // Hundreds
        aChr := 0;
    
        while(val > = 100) do 
          begin
            Dec(val, 100);
            Inc(aChr);
            empty := false;
          end;
    
        if empty then 
          begin
            achr := 16;
          end;
    
        Digit[1] := aChr;
    
        // Tens
        aChr : = 0;
    
        while(val > = 10) do 
          begin
            Dec(val, 10);
            Inc(aChr);
            empty := false;
          end;
    
        if empty then 
          begin
            aChr := 16;
          end;
    
        Digit[2] := aChr;
    
        // Ones
        aChr := 0;
    
        while(val >= 1) do 
          begin
            Dec(val);
            Inc(aChr);
          end;
    
        Digit[3] := aChr;
      end;
    

### Hexadecimal output

If you want hexadecimal output, you can do it this way, since only **shift _and_** _and'_ operations are needed, which the AVR can do very quickly. 
    
    
     Digit[0] := MyValue shr 12;
     Digit[1] := MyValue shr 8 and %1111;
     Digit[2] := MyValue shr 4 and %1111;
     Digit[3] := MyValue and %1111;
    

### Call

With the call to **IntToDigit** values ​​are assigned to Digit[], which can then be output on a 7-segment display. 
    
    
     MyValue := 1234;      // Assign a value
     IntToDigit(MyValue);  // The digits array gets values
    

### Output

The following is suitable for output to a 7-segment display, since you do not want to sacrifice 32 AVR pins, if any. 

  * Via 4 shift registers
  * Multiplex, 12 pins are used for this, if you also connect the decimal point.
  * Shift register and multiplex.



See also: 

  * [Multiplex](<AVR_Embedded_Tutorial_-_Multiplex.md> "AVR Embedded Tutorial - Multiplex") \- Multiplex based on a 4-fold seven-segment display.
  * [Shift registers](<AVR_Embedded_Tutorial_-_Shiftregister.md> "AVR Embedded Tutorial - Shiftregister") \- How do I control shift registers?



# See also

  * [AVR Embedded Tutorials](<AVR_Embedded_Tutorial.md> "AVR Embedded Tutorial") \- Overview

---

_Source: [https://wiki.freepascal.org/AVR_Embedded_Tutorial_-_Int_to_digits](https://web.archive.org/web/20240121085313/https://wiki.freepascal.org/AVR_Embedded_Tutorial_-_Int_to_digits)_
