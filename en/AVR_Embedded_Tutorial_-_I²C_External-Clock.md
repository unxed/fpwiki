# AVR Embedded Tutorial - I²C External-Clock

│ [**Deutsch (de)**](</AVR_Embedded_Tutorial_-_I%C2%B2C_External-Clock/de> "AVR Embedded Tutorial - I²C External-Clock/de") │  **English (en)** │    
****

## Contents

  * 1 I²C external clock DS3231
    * 1.1 Functions for the DS3231 control
      * 1.1.1 Constants
      * 1.1.2 Record for time storage
      * 1.1.3 BCD to BIN, BIN to BCD
      * 1.1.4 Write the time
      * 1.1.5 Read the time
    * 1.2 Example program
      * 1.2.1 Variables
      * 1.2.2 Main program
  * 2 See also



# I²C external clock DS3231

This code is for an Atmega328 / Arduino with 16MHz clock and a [DS3231 clock module](<https://www.maximintegrated.com/en/products/analog/real-time-clocks/DS3231.html>). 

The following functions are described in other tutorials: 

  * [Delays](<AVR_Embedded_Tutorial_-_Delays.md> "AVR Embedded Tutorial - Delays")
  * [UART - serial interface](<AVR_Embedded_Tutorial_-_UART.md> "AVR Embedded Tutorial - UART")
  * [Activate I²C bus](<AVR_Embedded_Tutorial_-_I²C,_TWI.md> "AVR Embedded Tutorial - I²C, TWI") or [AVR Embedded Tutorial - Software I2C, TWI](<AVR_Embedded_Tutorial_-_Software_I2C,_TWI.md> "AVR Embedded Tutorial - Software I2C, TWI")



This example shows how to address the DS3231 clock module, which is connected to the I²C bus. 

**PullUp resistors are necessary** , the DS3231 does not contain any itself. 

## Functions for the DS3231 control

### Constants
    
    
     const
       I2CAddr   = $68;  // Default address of the DS3231.
       TWI_Write = 0;
       TWI_Read  = 1;
    

### Record for time storage
    
    
     type
       TDate = record
         year : uint16;
         month, day, hour, minute, second, dayOfTheWeek : uint8;
       end;
     var
       Date : TDate;
    

### BCD to BIN, BIN to BCD

These conversion functions are required because the time is in BCD format. 
    
    
      function bcd2bin(val : uint8) : uint8;
       begin
         Result := val - 6 * (val shr 4);
       end;
    
       function bin2bcd(val : uint8) : uint8;
       begin
         Result := val + 6 * (val div 10);
       end;
    

### Write the time

Write the block with a time. You can see that you have to convert the format. 
    
    
      procedure WriteDS3231(addr : UInt16);
       begin
         TWIStart((addr shl 1) or TWI_Write);
         TWIWrite(0);
         TWIWrite(bin2bcd(Date.Second));
         TWIWrite(bin2bcd(Date.Minute));
         TWIWrite(bin2bcd(Date.Hour));
         TWIWrite(bin2bcd(0));
         TWIWrite(bin2bcd(Date.Day));
         TWIWrite(bin2bcd(Date.Month));
         TWIWrite(bin2bcd(Date.Year - 2000));
         TWIStop;
       end;
    

### Read the time

Read the time from the block. You can see that you have to convert the format. 
    
    
      procedure ReadDS3231(addr : UInt16);
       begin
         TWIStart((addr shl 1) or TWI_Write);
         TWIWrite(0);
         TWIStop;
    
         TWIStart((addr shl 1) or TWI_Read);
         Date.second := bcd2bin(TWIReadACK_Error and $7F);
         Date.minute := bcd2bin(TWIReadACK_Error);
         Date.hour   := bcd2bin(TWIReadACK_Error);
    
         TWIReadACK_Error;
    
         Date.day    := bcd2bin(TWIReadACK_Error);
         Date.month  := bcd2bin(TWIReadACK_Error);
         Date.year   := bcd2bin(TWIReadNACK_Error) + 2000;
    
         TWIStop;
       end;
    

## Example program

### Variables
    
    
      var
       s : ShortString;
    

### Main program

You only have to write the time to the DS3231 to if you want to set the clock. 
    
    
      begin
       UARTInit;     // initialize UART
       TWIInit;      // initialize I²C
       asm Sei end;  // enable interrupts
    
       // Write a time from the DS3231.
       Date.second := 33;
       Date.minute := 22;
       Date.hour   := 11;
    
       WriteDS3231(I2CAddr);
    
       // Read time and output via UART
       repeat
         ReadDS3231(I2CAddr);
    
         str(Date.second : 6, s);
         UARTSendString('Sec:' + s);
         str(Date.minute : 6, s);
         UARTSendString('Minute:' + s);
         str(Date.hour : 6, s);
         UARTSendString('hours:' + s);
    
         UARTSendString(#13#10);
       until 1 = 2;
     end.
    

# See also

  * Overview page - [AVR Embedded Tutorials](<AVR_Embedded_Tutorials.md> "AVR Embedded Tutorials").

---

_Source: [https://wiki.freepascal.org/AVR_Embedded_Tutorial_-_I%C2%B2C_External-Clock](https://web.archive.org/web/20210117124417/https://wiki.freepascal.org/AVR_Embedded_Tutorial_-_I%C2%B2C_External-Clock)_
