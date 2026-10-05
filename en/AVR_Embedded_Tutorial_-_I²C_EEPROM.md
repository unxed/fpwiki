# AVR Embedded Tutorial - I²C EEPROM

│ **English (en)** │

## Contents

  * 1 I²C EEPROM (24LC256)
    * 1.1 Functions for EEPROM (24LC256) control
      * 1.1.1 Constants
      * 1.1.2 Functions
        * 1.1.2.1 Functions for writing and reading a byte
        * 1.1.2.2 Functions for writing and reading a string
    * 1.2 Example program
      * 1.2.1 Constants and variables
      * 1.2.2 Main program
  * 2 See also



# I²C EEPROM (24LC256)

This code is for an Atmega328 / Arduino with a 16MHz clock and an [24LC256](<https://www.microchip.com/wwwproducts/en/24LC256>) I2C compatible 2-wire Serial EEPROM. Other 24LCxx EEPROMs can also be controlled. 

The following functions are described in other tutorials: 

  * [Delays](<AVR_Embedded_Tutorial_-_Delays.md> "AVR Embedded Tutorial - Delays")
  * [UART - serial interface](<AVR_Embedded_Tutorial_-_UART.md> "AVR Embedded Tutorial - UART")
  * [Activate I²C bus](<AVR_Embedded_Tutorial_-_I²C,_TWI.md> "AVR Embedded Tutorial - I²C, TWI") or [AVR Embedded Tutorial - Software I2C, TWI](<AVR_Embedded_Tutorial_-_Software_I2C,_TWI.md> "AVR Embedded Tutorial - Software I2C, TWI")



This tutorial shows you how to control an external EEPROM (24LC256) which is connected to the I²C bus. 

**PullUp resistors are necessary** , the EEPROM does not contain any itself. 

## Functions for EEPROM (24LC256) control

### Constants
    
    
      const
       TWI_Write = 0;
       TWI_Read  = 1;
    

### Functions

Note: For a [ShortString](<Shortstring.md> "Shortstring"), the length byte is stored at position [0] in the string. 

Call parameters: 

  * **addr** device address on the I²C bus
  * **pos** storage location in the EEPROM
  * **value** byte to be written



#### Functions for writing and reading a byte
    
    
     // Write a character
     procedure I2C_EEPROM_Write(addr , pos : Int16; value : Byte);
     begin
       TWIStart((addr shl 1) or TWI_Write);
       TWIWrite(pos shr 8);
       TWIWrite(pos and $FF);
       TWIWrite(value);
       TWIStop;
       delay_ms(10);
     end;
    
     // Read a character
     function I2C_EEPROM_Read(addr, pos : Int16) : byte;
     begin
       TWIStart((addr shl 1) or TWI_Write);
       TWIWrite(pos shr 8);
       TWIWrite(pos and $FF);
       TWIStop;
    
       TWIStart((addr shl 1) or TWI_Read);
       Result := TWIReadNACK;
       TWIStop;
     end;
    

#### Functions for writing and reading a string

A [ShortString](<Shortstring.md> "Shortstring"), or other buffer, is written / read character by character. 
    
    
     // Write a string, including length byte
     procedure I2C_EEPROM_WriteString(addr, pos : Int16; s : ShortString);
     var
       i : byte;
     begin
       for i := 0 to Length(s) do 
         begin
           I2C_EEPROM_Write(addr, pos + i, byte(s[i]));
         end;
     end;
    
     // Read a string, including length byte
     function I2C_EEPROM_ReadString(addr, pos : Int16) : ShortString;
     var
       i, l : byte;
     begin
       l := I2C_EEPROM_Read(addr, pos);  // read length of string stored at [0]
       SetLength(Result, l);
       for i := 1 to l do 
         begin
           Result[i] := Char(I2C_EEPROM_Read(addr, pos + i));
         end;
     end;
    

## Example program

### Constants and variables
    
    
     const
       I2Caddr = $54;  // Address of the 24LC256 on the I²C bus
       s1 = 'Hello World!';
       s2 = 'AVR with Lazarus is so beautiful';
    
     var
       s : ShortString;
    

### Main program

Since a [ShortString](<Shortstring.md> "Shortstring") is 256 bytes long, the second string at address $100 (256) is saved in the EEPROM. 
    
    
     begin
       UARTInit;    // initialize UART
       TWIInit;     // initialize I²C
       asm Sei end; // enable interrupts
    
       // save the two strings
       I2C_EEPROM_WriteString(I2Caddr, 0, s1);    // first string
       I2C_EEPROM_WriteString(I2Caddr, $100, s2); // second string
    
       // eeload the strings for verification
       repeat
         s := I2C_EEPROM_ReadString(I2Caddr, 0);
         UARTSendString('s1:');
         UARTSendString(s);
         UARTSendString(#13#10);
    
         s := I2C_EEPROM_ReadString(I2Caddr, $100);
         UARTSendString('s2:');
         UARTSendString(s);
         UARTSendString(#13#10);
       until 1 = 2;
     end.
    

If you do not believe that the string is in the EEPROM, you can exclude the two lines with WriteString (... and test again. 

# See also

  * Overview page - [AVR Embedded Tutorials](<AVR_Embedded_Tutorial.md> "AVR Embedded Tutorial")

---

_Source: [https://wiki.freepascal.org/AVR_Embedded_Tutorial_-_I%C2%B2C_EEPROM](https://web.archive.org/web/20210123223446/https://wiki.freepascal.org/AVR_Embedded_Tutorial_-_I%C2%B2C_EEPROM)_
