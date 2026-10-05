# AVR Embedded Tutorial - SPI MCP4922

│ **English (en)** │

## Contents

  * 1 Control an MCP4922 12 bit DAC via SPI
    * 1.1 Functions for the 74HC595 shift register
      * 1.1.1 Constants
      * 1.1.2 Initialize SPI
      * 1.1.3 Write SPI
    * 1.2 MCP4922 functions
      * 1.2.1 Send to DAC
    * 1.3 Example program
      * 1.3.1 Constants and variables
      * 1.3.2 Main program
  * 2 See also



# Control an MCP4922 12 bit DAC via SPI

This code is for an Atmega328 / Arduino with a 16MHz clock and an SPI compatible [MCP4922](<https://www.microchip.com/wwwproducts/en/MCP4922>) Digital/Analog Converter (DAC). 

The following functions are described in other tutorials: 

  * [GPIO](<AVR_Embedded_Tutorial_-_Simple_GPIO_on_and_off_output.md> "AVR Embedded Tutorial - Simple GPIO on and off output") \- Output/Input - How do I access the GPIO on the AVR.
  * [SPI](<AVR_Embedded_Tutorial_-_SPI.md> "AVR Embedded Tutorial - SPI") \- use of the hardware SPI interface.



This example shows how to control the MCP4922 DAC via SPI. 

## Functions for the 74HC595 shift register

### Constants

Pins which are required at PORTB for SPI. 
    
    
     type
       TSPIGPIO = bitpacked record
         p0, p1, SlaveSelect, MOSI, MISO, Clock, p6, p7 : boolean;
       end;
    
     var
       SPI_PORT : TSPIGPIO absolute PORTB;
       SPI_DDR  : TSPIGPIO absolute DDRB;
    

### Initialize SPI
    
    
     procedure SPIInit;
     begin
       // Setup SPI port for output
       SPI_DDR.MOSI        := True;
       SPI_DDR.Clock       := True;
       SPI_DDR.SlaveSelect := True;
    
       SPCR := (1 shl SPE) or (0 shl SPIE) or (0 shl DORD) or (1 shl MSTR) or (0 shl CPOL) or (0 shl CPHA) or (%01 shl SPR);
       SPSR := 1 shl SPI2X;  // SCK x 2 on 1 MHz
     end;
    

### Write SPI
    
    
     procedure SPIWrite(p : PByte; len : byte);
     var
       i : byte;
     begin
       SPI_Port.SlaveSelect := False;
    
       for i := len - 1 downto 0 do
         begin
           SPDR := p[i];
           
           while(SPSR and (1 shl SPIF)) = 0 do
             begin
             end;
       end;
    
       SPI_Port.SlaveSelect := True;
     end;
    

## MCP4922 functions

Bit assignments for the MCP4922: 

Bit | Name | Description   
---|---|---  
15 | **A/B:** DACA or DACB Selection bit | **0** = Write to DACA  
**1** = Write to DACB   
14 | **BUF:** Vref Input Buffer Controlbit | **0** = Unbuffered  
**1** = Buffered   
13 | **GA:** Output Gain Selection bit | **0** = 2x (VOUT = 2 * VREF * D / 4096)  
**1** = 1x (VOUT = VREF * D / 4096)   
12 | **SHDN:** Output Shutdown Control bit | **0** = Shutdown the selected DAC channel. Analog output is not available at the channel that was shut down. VOUT pin is connected to 500 kOhm(typical)  
**1** = Active mode operation. VOUT is available.   
11:0 | **D11:D0:** DAC Input Data bits. Bit x is ignored. |   
  
### Send to DAC
    
    
     procedure MCP4922sendValue(Value : UInt16; Channel : Byte);
     type
       TADC = bitpacked record
         Value : 0..4095;  // bits 0-11
         ShutDown,         // bit 12
         Gain,             // bit 13
         VRef,             // bit 14
         DAC : 0..1;       // bit 15
       end;
    
     begin
       with TADC(Value) do 
         begin
           ShutDown := 1;
           Gain     := 1;
           VRef     := 0;
           Dac      := Channel;
       end;
    
       SPIWrite(@Value, 2);
     end;
    

## Example program

This example shows the output of two analog values ​​from the DAC. Two LEDs connected to the DAC flash alternately with a very smooth transition. 

### Constants and variables

A counting variable that counts up to 4095. 
    
    
     var
       z : Int16 ;
    

### Main program

Alternately write a value in DAC 0 & 1\. 
    
    
     begin
       // initialize SPI.
       SPIInit; 
    
       repeat
         for z := 0 to 4095 do 
           begin
             MCP4922sendValue(4095 - z, 0);
             MCP4922sendValue(z, 1);
           end;
       until 1 = 2;
     end.
    

# See also

  * Overview page - [AVR Embedded Tutorials](<AVR_Embedded_Tutorials.md> "AVR Embedded Tutorials").
  * Use of the hardware SPI interface with an ATmega328 / Arduino - [SPI](<AVR_Embedded_Tutorial_-_SPI.md> "AVR Embedded Tutorial - SPI").

---

_Source: [https://wiki.freepascal.org/AVR_Embedded_Tutorial_-_SPI_MCP4922](https://web.archive.org/web/20240121085317/https://wiki.freepascal.org/AVR_Embedded_Tutorial_-_SPI_MCP4922)_
