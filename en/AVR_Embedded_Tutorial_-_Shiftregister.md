# AVR Embedded Tutorial - Shiftregister

│ **English (en)** │

## Contents

  * 1 Shift Registers
  * 2 Port access
  * 3 Port constants
  * 4 Shift register write 74HC595
  * 5 Shift register read 74HC165
  * 6 Arduino shift register
  * 7 Shift register examples
    * 7.1 Read from a shift register
      * 7.1.1 Example for a 74HC165
      * 7.1.2 Example for cascaded 74HC165
    * 7.2 Write to a shift register
      * 7.2.1 Example for a 74HC595
      * 7.2.2 Example for cascaded 74HC595
  * 8 See also



# Shift Registers

The sample code is for an [ATtiny44](<https://upload.chipdatas.com/root/pdf/Atmel-7701_Automotive-Microcontrollers-ATtiny24-44-84_Datasheet_eXTunT.pdf>) and the shift registers used are the 74HC165 for reading and the 74HC595 for writing. 

[![Warning-icon.png](https://wiki.freepascal.org/images/b/b2/Warning-icon.png)](</File:Warning-icon.png>)

**Warning:** The read routine is **not** compatible with Arduino **shiftIn (...**! The Arduino read function is for other shift registers. The clock must be controlled differently.

If you want to swap **MSB** to **LSB** , you simply have to swap **downto** to **to**. 

# Port access

For how the shift register commands access the ports, see here: [GPIO output](<AVR_Embedded_Tutorial_-_Simple_GPIO_on_and_off_output.md> "AVR Embedded Tutorial - Simple GPIO on and off output"). 

Do not forget to set the outputs as outputs (**DDRx**). 

# Port constants

The pins used for the ATtiny44 and the shift register. With the AVR you can use any pins. I have declared the ports, except the data port, globally, so access from the shift registers is a little faster. If you declared them locally, everything would be more complicated because we use direct port access. Arduino does this differently, but it is about 10 times slower. 

IC | Attiny44 | 74HC165 | 74HC595   
---|---|---|---  
| Pin No. | Description | Pin No. | Description | Pin No. | Description   
dataOutPin | 13 | PA0 |  |  | 14 | DS   
dataInPin | 12 | PA1 | 10 | DS |  |   
latchPin | 11 | PA2 | 1 | PL | 12 | ST_CP   
clockPin | 10 | PA3 | 2 | CP | 11 | SH_CP   
  
A variable is declared for the port, this has the advantage that you do not have to change all ports in the shift register functions. 
    
    
      var
       shiftPORT : byte absolute PORTA;
       shiftDDR  : byte absolute DDRA;
       shiftPIN  : byte absolute PINA;
     const
       dataOutPin = 0;
       dataInPin = 1;
       latchPin = 2;
       clockPin = 3;
    

# Shift register write 74HC595
    
    
    procedure shiftOut595(val : byte);
      var
        i : byte;
    begin
      for i : = 7 downto 0 do // MSB FIRST
        begin 
          if(val and (1 shl i)) <> 0 then 
            begin
              shiftPORT := shiftPORT or (1 shl dataOutPin);
            end 
          else 
            begin
              shiftPORT := shiftPORT and not (1 shl dataOutPin);
            end;
    
          shiftPORT := shiftPORT or (1 shl clockPin);
          shiftPORT := shiftPORT and not (1 shl clockPin);
        end;
    end;
    

# Shift register read 74HC165
    
    
    function ShiftIn165 : byte;
     var
       i : byte;
     begin
       Result := 0;
    
       for i := 0 to 7 do  // LSB FIRST
         begin 
           Result := (Result shl 1) or ((shiftPIN and (1 shl dataInPin)) shr dataInPin);
           shiftPORT := shiftPORT or (1 shl clockPin);
           shiftPORT := shiftPORT and not (1 shl clockPin);
         end;
     end ;
    

# Arduino shift register

This version corresponds to Arduino's **shiftIn**. 
    
    
    function ShiftIn : byte;
      var
        i : byte;
    begin
      Result := 0;
    
      for i := 0 to 7 do  // LSB FIRST 
        begin 
          shiftPORT := shiftPORT or (1 shl clockPin);
          Result := (Result shl 1) or ((shiftPIN and (1 shl dataInPin)) shr dataInPin);
          shiftPORT := shiftPORT and not (1 shl clockPin);
        end;
    end;
    

# Shift register examples

## Read from a shift register

Before the shift register can be read out, the port status must be written to the latch register, this is done via the LatchPin. 

### Example for a 74HC165
    
    
     var
       b : byte;
     begin
    
       // Latch data
       shiftPORT := shiftPORT and not (1 shl latchPin);
       shiftPORT := shiftPORT or (1 shl latchPin);
    
       // Read shift register
       b := ShiftIn74HC165;
    

### Example for cascaded 74HC165
    
    
     var
       b0, bn : byte;
     begin
    
       // Latch data
       shiftPORT := shiftPORT and not (1 shl latchPin);
       shiftPORT := shiftPORT or (1 shl latchPin);
    
       // Read shift register
       b0 := ShiftIn74HC165;
    
       // ...
    
       bn := ShiftIn74HC165;
    

## Write to a shift register

If the data is written into the shift register, you have to add the latch clock that the current outputs become active. 

### Example for a 74HC595
    
    
     var
       b : byte;
     begin
       b := 123;
    
       // Write shift register
       shiftOut595(b);
    
       // Latch data
       shiftPORT := shiftPORT and not (1 shl latchPin);
       shiftPORT := shiftPORT or (1 shl latchPin);
    

### Example for cascaded 74HC595
    
    
     var
       b0, bn : byte;
     begin
       b0 := 123;
    
       // ...
    
       bn := 789;
    
       // Write shift register
       shiftOut595(b0);
       
       // ...
       
       shiftOut595(bn);
    
       // Latch data
       shiftPORT := shiftPORT and not (1 shl latchPin);
       shiftPORT := shiftPORT or (1 shl latchPin);
    

# See also

  * [AVR Embedded Tutorials](<AVR_Embedded_Tutorial.md> "AVR Embedded Tutorial") \- Overview
  * [SPI shift register](<AVR_Embedded_Tutorial_-_SPI_Shiftregister.md> "AVR Embedded Tutorial - SPI Shiftregister") \- control shift register 74HC595 via SPI (very fast).

---

_Source: [https://wiki.freepascal.org/AVR_Embedded_Tutorial_-_Shiftregister](https://web.archive.org/web/20250401020343/https://wiki.freepascal.org/AVR_Embedded_Tutorial_-_Shiftregister)_
