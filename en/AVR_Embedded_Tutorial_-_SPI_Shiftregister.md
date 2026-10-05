# AVR Embedded Tutorial - SPI Shiftregister

│ [**Deutsch (de)**](</AVR_Embedded_Tutorial_-_SPI_Shiftregister/de> "AVR Embedded Tutorial - SPI Shiftregister/de") │  **English (en)** │    
****

## Contents

  * 1 Controlling a 74HC595 shift register via SPI
    * 1.1 Atmega328 / Arduino Uno / Nano example
      * 1.1.1 Constants
      * 1.1.2 Write to a pin
      * 1.1.3 Write character string to the SPI port
      * 1.1.4 Main program
    * 1.2 ATTiny2313 example
      * 1.2.1 Declarations
      * 1.2.2 Write a character string to the SPI port
      * 1.2.3 Main program - Hardware version
      * 1.2.4 Main program - Software version
  * 2 See also



# Controlling a 74HC595 shift register via SPI

While other shift registers can also be controlled, the SPI compatible [74HC595](<https://www.ti.com/product/SN74HC595>) has a big advantage over a normal shift register in that it is about 10 times faster (ATmega328). This is noticeable with a larger multiplex. 

The following functions are described in other tutorials: 

  * [GPIO - Output / Input](<AVR_Embedded_Tutorial_-_Simple_GPIO_on_and_off_output.md> "AVR Embedded Tutorial - Simple GPIO on and off output") \- How do I make a GPIO access on the AVR.
  * [SPI](<AVR_Embedded_Tutorial_-_SPI.md> "AVR Embedded Tutorial - SPI") \- use of the hardware SPI interface.



## Atmega328 / Arduino Uno / Nano example

### Constants

Pins which are required at PORTB for SPI. 
    
    
     const
       SPI_SlaveSelect = 2;
       SPI_MOSI        = 3;
       SPI_Clock       = 5;
    

### Write to a pin

Writing a PORTB pin is used for SlaveSelect and for the software version. 
    
    
      procedure WritePortB(Pin : byte; Value : boolean);
       begin
         if Value then 
           begin
             PORTB := PORTB or (1 shl pin);
           end 
         else 
           begin
             PORTB := PORTB and not (1 shl pin);
           end;
       end;
    

### Write character string to the SPI port

Writing the string 
    
    
      procedure WriteDataHardSPI(p : PByte; len : byte);
      var
        i : byte;
      begin
        WritePortB(SPI_SlaveSelect, False);
    
        for i := len - 1 downto 0 do 
          begin
            SPDR := p[i];
    
            while(SPSR and (1 shl SPIF)) = 0 do 
              begin
              end;
          end;
        
        WritePortB(SPI_SlaveSelect, True);
      end;
    

### Main program

The SPI register is set to the highest possible speed. 
    
    
     var
       z : uint16;
     begin
       // All pins set to output
       DDRB := (1 shl SPI_MOSI) or (1 shl SPI_Clock) or (1 shl SPI_SlaveSelect);
    
       // Set the SPI register
       SPCR := (1 shl SPE) or (1 shl MSTR) or (%00 shl SPR);
       SPSR := (1 shl SPI2X);  
    
       repeat
         // Increment the counter, return to 0 if overflow occurs
         Inc(z);
    
         // SPI port described
         WriteDataHardSPI(@z, 2);  // write 2 bytes.
       until 1 = 2;
     end.
    

## ATTiny2313 example

The ATTiny2313 has no direct SPI interface, but there is the universal USI interface, which is used here for SPI . This is a little more complicated when writing than with the direct SPI interface of the Atmega. 

Note: The ATTiny2313 also has MOSI and MISO connections, but these are only used for the programmer. There are DO and DI for this, but they work exactly in the reverse order. 

### Declarations

Pins which are required at PORTB for SPI. 

A bitpacked record is used for this, so you can access the pins directly. 
    
    
     type
       TSPIGPIO = bitpacked record
         p0, p1, p2, p3, SlaveSelect, DataInt, DataOut, Clock : boolean;
       end;
    
     var
       SPI_PORT : TSPIGPIO absolute PORTB;
       SPI_DDR  : TSPIGPIO absolute DDRB;
    

### Write a character string to the SPI port

Writing the string 
    
    
      procedure SPIWriteData(p : PByte; len : byte);
       var
         i : byte;
       begin
         SPI_PORT.SlaveSelect := False;
    
         for i : = len - 1 downto 0 do
           begin
             USIDR := p[i];
             USISR := 1 shl USIOIF;
    
             repeat
               USICR := (%01 shl USIWM) or (%10 shl USICS) or (1 shl USICLK) or (1 shl USITC);
             until (USISR and (1 shl USIOIF)) <> 0;
           end;
    
         SPI_PORT.SlaveSelect := True;
       end;
    

### Main program - Hardware version

This example shows how to write 2 cascaded shift registers. 

An integer value is simply output. 

You can also see that no configuration of USI is necessary at the beginning, except that you have to set the ports to output. 
    
    
      var
       z : uint16 = 0;
     begin
       SPI_DDR.DataOut     := True;
       SPI_DDR.Clock       := True;
       SPI_DDR.SlaveSelect := True;
    
       repeat
         Inc(z);
         SPIWriteData(@z, 2);  // output integer
       until 1 = 2;
     end.
    

### Main program - Software version

I can show that you can also do software emulation very well here. 

However, this is about a factor of 10 slower than the hardware version of the Atmega328, you can use any ports for this. 

The software version has the advantage that it runs on every AVR and can also be used if you need multiple SPI interfaces. 
    
    
      procedure WriteDataSoftSPI(p : PByte; len : byte);
      var
        i , j : byte;
      begin
        WritePortB(SPI_SlaveSelect, False);
    
        for j := 0 to len - 1 do 
          begin
            for i := 7 downto 0 do 
              begin
                if(p[j] and (1 shl i)) <> 0 then
                  begin
                    WritePortB(SPI_MOSI, True);
                  end 
                else 
                  begin
                    WritePortB(SPI_MOSI, False);
                  end;
    
                WritePortB(SPI_Clock, True);
                WritePortB(SPI_Clock, False);
              end;
            
            WritePortB(SPI_SlaveSelect, True);
          end;
      end;
    

# See also

  * Overview page - [AVR Embedded Tutorials](<AVR_Embedded_Tutorials.md> "AVR Embedded Tutorials").
  * [Shift registers](<AVR_Embedded_Tutorial_-_Shiftregister.md> "AVR Embedded Tutorial - Shiftregister") \- How do I control shift registers?
  * [SPI](<AVR_Embedded_Tutorial_-_SPI.md> "AVR Embedded Tutorial - SPI") \- Use of the hardware SPI interface with an ATmega328 / Arduino.

---

_Source: [https://wiki.freepascal.org/AVR_Embedded_Tutorial_-_SPI_Shiftregister](https://web.archive.org/web/20221002134759/https://wiki.freepascal.org/AVR_Embedded_Tutorial_-_SPI_Shiftregister)_
