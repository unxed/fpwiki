# AVR Embedded Tutorial - SPI-Slave

│ **English (en)** │

## Contents

  * 1 SPI as a slave
  * 2 Atmega328 / Arduino Uno / Nano Example
    * 2.1 Units
    * 2.2 Constants and variables
    * 2.3 Received characters from SPI
    * 2.4 Main program
  * 3 SPI send code for the ATtiny2313
  * 4 See also



# SPI as a slave

SPI can also be used as a slave. The best way to handle SPI responding to an input is to use interrupt control. 

The following functions are described in these tutorials: 

  * [GPIO - Out / Input](<AVR_Embedded_Tutorial_-_Simple_GPIO_on_and_off_output.md> "AVR Embedded Tutorial - Simple GPIO on and off output") \- How do I make a GPIO access on the AVR.
  * [UART - serial input and output](<AVR_Embedded_Tutorial_-_UART.md> "AVR Embedded Tutorial - UART") \- (COM port).
  * [SPI - use of the hardware SPI interface](<AVR_Embedded_Tutorial_-_SPI.md> "AVR Embedded Tutorial - SPI").



# Atmega328 / Arduino Uno / Nano Example

In this example, an interrupt is triggered as soon as a character is present at the input and then this is written to the ring buffer. 

The main loop checks whether something is in the ring buffer, and if so, this is output via the UART. 

What you have to watch out for is that you don't get too much in via SPI, since this interface is a few times faster than UART. 

As a master I used an ATtiny2313, which outputs a string via SPI at the push of a button. 

At the end there is the code for the ATTiny that sends the strings. 

## Units

Required for SEI. 
    
    
     uses
       intrinsics;
    

## Constants and variables

Constant and variables for ring buffers. 
    
    
     const
       // Buffer size
       SPI_Buf_Len = 255;
    
     var
       // buffer pointer and buffer
       SPI_Buf_FirstOut : byte = 0 ;
       SPI_Buf_LastIn   : byte = 0 ;
       SPI_Buf_Data     : array[0..SPI_Buf_Len - 1] of byte;
    

PORTB pins which are required for SPI - only needed if you send something. 
    
    
      type
       TSPI_GPIO = bitpacked record
         p0, p1, SlaveSelect, MOSI, MISO, Clock, p6, p7 : boolean;
       end;
    
     var
       SPI_Port : TSPI_GPIO absolute PORTB;
       SPI_DDR  : TSPI_GPIO absolute DDRB;
    

## Received characters from SPI

As soon as the master activates SS, the interrupt is triggered and the incoming character is written to the ring buffer. 
    
    
      procedure SPI_Int_Send;  public name 'SPI__STC_ISR'; interrupt;
       var
         tchr : byte;
      begin
        tchr := SPDR;
    
        if tchr <> 0 then 
          begin
            SPI_Buf_Data[SPI_Buf_LastIn] := tchr;
            Inc(SPI_Buf_LastIn);
            if SPI_Buf_LastIn >= SPI_Buf_Len then 
              begin
                SPI_Buf_LastIn := 0;
              end;
          end;
      end;
    

## Main program

The main program simply checks whether something is in the ring buffer and as soon as something is there it is output via UART. 
    
    
     var
       ch : byte;
    
     begin
       // initialize UART
       UARTInit;
    
       // SPI as slave, interrupt controlled.
       SPCR := (1 shl SPE) or (1 shl SPIE);
    
       // activate interrupt
       avr_sei ;
    
       // Send Pin
       SPI_DDR.MISO := True;  // Only necessary if the slave also sends.
    
       repeat
         while SPI_Buf_LastIn <> SPI_Buf_FirstOut do // Is there something in the buffer?
           begin 
             ch := SPI_Buf_Data[SPI_Buf_FirstOut];
             Inc (SPI_Buf_FirstOut);
    
           if SPI_Buf_FirstOut >= SPI_Buf_Len then 
             begin
               SPI_Buf_FirstOut := 0;
             end;
    
           UARTSendChar(char(ch));
         end;
       until 1 = 2;
     end.
    

# SPI send code for the ATtiny2313

This code sends a string as soon as a key is pressed. 

Since the ATTiny2313 does not have a real SPI, the output is made via the universal interface (USI). 

There are 3 buttons. 
    
    
      program Project1 ;
    
     {$O-}
     type
       TSPI_GPIO = bitpacked record
         p0, p1, p2, p3, SlaveSelect, DataInt, DataOut, Clock : boolean;
       end ;
    
       TButton_GPIO = bitpacked array[0..2] of boolean;
    
     var
       SPI_PORT : TSPI_GPIO absolute PORTB;
       SPI_DDR : TSPI_GPIO absolute DDRB;
    
       Button_PIN : TButton_GPIO absolute PIND;
    
       procedure delay;
       var
         i : UInt32;
       begin
         for i := 0 to 65000 do ;
       end;
    
       procedure SPIWriteData(p : PByte; len : byte);
       var
         i : byte;
       begin
         SPI_PORT.SlaveSelect := False;
    
         for i := 0 to len - 1 do 
           begin
             USIDR := p[i];
             USISR := 1 shl USIOIF;
    
             repeat
               USICR := (%01 shl USIWM) or (%10 shl USICS) or (1 shl USICLK) or (1 shl USITC);
             until (USISR and (1 shl USIOIF)) <> 0;
    
           end;
    
         SPI_PORT.SlaveSelect := True;
       end;
    
     const
       Text0 = 'Lazarus is great!' + #13#10;
       Text1 = 'Hello World !, Hello AVR' + #13#10;
       Text2 = 'SPI as slave' + #13#10;
    
     begin
       SPI_DDR.DataOut     := True;
       SPI_DDR.Clock       := True;
       SPI_DDR.SlaveSelect := True;
    
       repeat
         if not Button_PIN[0] then 
           begin
             SPIWriteData(@Text0[1], Length(Text0));
             delay;
           end;
    
         if not Button_PIN[1] then 
           begin
             SPIWriteData(@Text1[1], Length(Text1));
             delay;
           end;
    
         if not Button_PIN[2] then 
           begin
             SPIWriteData(@Text2[1], Length(Text2));
             delay;
           end;
       until 1 = 2;
     end.
    

# See also

  * Overview page - [AVR Embedded Tutorial](<AVR_Embedded_Tutorial.md> "AVR Embedded Tutorial")
  * [Shift registers](<AVR_Embedded_Tutorial_-_Shiftregister.md> "AVR Embedded Tutorial - Shiftregister") \- How do I control shift registers?
  * [Use of the hardware SPI interface with an ATmega328 / Arduino](<AVR_Embedded_Tutorial_-_SPI.md> "AVR Embedded Tutorial - SPI")

---

_Source: [https://wiki.freepascal.org/AVR_Embedded_Tutorial_-_SPI-Slave](https://web.archive.org/web/20240121085947/https://wiki.freepascal.org/AVR_Embedded_Tutorial_-_SPI-Slave)_
