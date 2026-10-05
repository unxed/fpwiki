# AVR Embedded Tutorial - EEPROM

│ **English (en)** │

## Contents

  * 1 Read and write EEPROM data
    * 1.1 Accessing the EEPROM
      * 1.1.1 Writing data
      * 1.1.2 Reading data
    * 1.2 Example
  * 2 See also



# Read and write EEPROM data

This code in this tutorial is for an Atmega328/Arduino with 16MHz. For how to control the UART with **UARTInit** and **UARTSendString(...** , see the [UART Tutorial](<AVR_Embedded_Tutorial_-_UART.md> "AVR Embedded Tutorial - UART"). 

## Accessing the EEPROM

Every single byte in the EEPROM can be accessed via an address. This gives you the opportunity to read and write each character individually. 

### Writing data

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** The two lines with **EECR** must be separate, otherwise nothing is written!
    
    
    procedure EEPROM_write(Address : int16; Data : byte);
      begin
        // Check if ready to write
        while (EECR and (1 shl EEPE)) <> 0 do 
          begin
            ;
          end;
    
        EEAR := Address;
        EEDR := Data;                   // Byte to write
        EECR := EECR or (1 shl EEMPE);  // There must be 2 lines!
        EECR := EECR or (1 shl EEPE);
      end;
    

### Reading data
    
    
    function EEPROM_read(Address : int16) : byte;
      begin
        // Check if ready to read
        while (EECR and (1 shl EEPE )) <> 0 do 
          begin
            ;
          end;
    
        EEAR   := Address;
        EECR   := EECR or (1 shl EERE);
        Result := EEDR;                  // Read byte
      end;
    

  


## Example

Sample code that shows how to read and write data. 
    
    
     const
       sc = 'Hello World!';
    
     var
       i : integer;
       s : shortString; 
    
     begin
       UARTInit ;
    
       // Write string, including length byte
       for i := 0 to Length (sc) do 
         begin
           EEPROM_write(i, byte(sc[i]));
         end;
    
       // Read string, including length byte
       SetLength(s, EEPROM_read(0));         // Read length byte
    
       for i := 1 to Length(s) do 
         begin
           s[i] := char(EEPROM_read(i));     // Read string data bytes
         end;
    
       repeat
         UARTSendString(s);
         UARTSendString(#13#10);
       until 1 = 2;
     end.
    

Once the first routine (above) has been executed, you can easily see with the second routine (below) that "Hello World!" was stored in the EEPROM, even if you have disconnected the ATmega from the power in the meantime. 
    
    
     var
       i : integer;
       s : shortString;
    
     begin
       UARTInit;
    
       // Read string, including length byte
       SetLength(s, EEPROM_read(0));          // Read length byte
    
       for i := 1 to Length(s) do 
         begin
           s[i] := char(EEPROM_read(i));
         end;
    
       repeat
         UARTSendString(s);
         UARTSendString(#13#10);
       until 1 = 2;
     end.
    

# See also

  * [AVR Embedded Tutorials](<AVR_Embedded_Tutorial.md> "AVR Embedded Tutorial") \- Overview

---

_Source: [https://wiki.freepascal.org/AVR_Embedded_Tutorial_-_EEPROM](https://web.archive.org/web/20241209221847/https://wiki.freepascal.org/AVR_Embedded_Tutorial_-_EEPROM)_
