# AVR Embedded Tutorial - Software I2C, TWI

│ **English (en)** │

## Contents

  * 1 Software I2C / TWI for ATmega328 / Arduino
  * 2 TWI Master software
    * 2.1 TWI header
    * 2.2 Delays
    * 2.3 TWI start
    * 2.4 TWI stop
    * 2.5 Write TWI byte
    * 2.6 Read TWI data byte
    * 2.7 Read TWI data word
    * 2.8 End
  * 3 See also



# Software I2C / TWI for ATmega328 / Arduino

Many AVR controllers offer a hardware I2C interface. Sometimes you need an I2C interface on other pins, or several I2C interfaces. Software I2C helps in these situations. 

For the basics of I2C see this WikiPedia [article](<https://de.wikipedia.org/wiki/I2c>). 

# TWI Master software

## TWI header

The pins for SCL and SDA can be set as desired, but must all be on one port. For pins on different ports, the pin accesses have to be adjusted in the following routines. 
    
    
     unit test_twi;
    
     {$mode objfpc}
     {$goto on}
    
     interface
    
     uses
       ;
    
     procedure twi_start();
     procedure twi_stop();
    
     function twi_write(data : uint8) : boolean;
     function twi_readbyte(ackn : boolean) : uint8;
     function twi_readword(ackn : boolean) : uint16;
    
     const
       Psclin = 1 shl 5;  // PC5 any pins, but on the same port
       Psdain = 1 shl 4;  // PC4
    
     var
       DDRtwi : byte absolute DDRC;  // TWI pins on port C
       PRTtwi : byte absolute PORTC;
       PINtwi : byte absolute PINC;
    
     implementation
    

## Delays

The clock rate is adjusted over the duration of the delays, the numbers after **ldi r16**. Inline assembler does not allow comments here and for timing reasons the values ​​should be entered directly and not passed as variables. 

With the given values ​​9 and 10, a clock rate of 100kHz results at 8MHz clock frequency, with 59 and 60 a clock rate of 10kHz can be used for long data lines. 
    
    
     // pause TWI delays
    
     procedure delay2();
     label
       loop;
     begin
       asm
         ldi r16, 9
         loop:
           dec r16
           brne loop
       end['r16'];
     end;        
    
     procedure delay3();
     label
       loop;
     begin
       asm
         ldi r16, 10
         loop:
           dec r16
           brne loop
       end['r16'];
     end;
    

## TWI start

The start sequence for a transmission. 
    
    
     // Start TWI
    
     procedure twi_start();
     begin
       DDRtwi := DDRtwi or Psdain;  // SDA low
       delay3();
       DDRtwi := DDRtwi or Psclin;  // SCL low
       delay3();
     end;
    

## TWI stop

The stop sequence. For a restart, as required by some data sheets, you simply use a stop sequence followed by a start sequence. 
    
    
     // TWI stop sequence
    
     procedure twi_stop();
     begin
       DDRtwi := DDRtwi or Psdain;        // SDA low
       delay3();
       DDRtwi := DDRtwi and (not Psclin); // SCL high
       delay3();
       DDRtwi := DDRtwi and (not Psdain); // SDA high
       delay3();
     end;
    

## Write TWI byte

Hand over byte and go. Duration is about 10x cycle time, that is 100µsec at 100kHz. The routine may be interrupted by interrupts, the slave then waits. 
    
    
     // Write TWI byte
    
     function twi_write(data : uint8) : boolean;
     var
       i : uint8;
     begin
       for i := 0 to 7 do                      // output 8 bits
         begin
           if(data and (1 shl 7)) <> 0 then
             DDRtwi := DDRtwi and (not Psdain) // SDA high if bit is set
           else
             DDRtwi := DDRtwi or Psdain;       // SDA low if bit is not set
    
           data := data shl 1;                 // shift bit, 7 to 0
           delay2();
           DDRtwi := DDRtwi and (not Psclin);  // SCL high
           delay3();
           DDRtwi := DDRtwi or Psclin;         // SCL low
         end;
    
       DDRtwi := DDRtwi and (not Psdain);      // SDA high
       delay2();
       DDRtwi := DDRtwi and (not Psclin);      // SCL high
       delay2();
    
       if(PINtwi and Psdain) = 0 then          // test SDA on Acknowledge low
         twi_write := true                     // set ack flag
       else
         twi_write := false;                   // otherwise unset ack flag
    
       DDRtwi := DDRtwi or Psclin;             // SCL low
     end;
    

## Read TWI data byte
    
    
     // read TWI byte
    
     function twi_readbyte(ackn : boolean) : uint8;
     var
       data : uint8 = 0;
       i    : uint8;
     begin
       for i := 0 to 7 do                     // output 8 bits
         begin
           data := data shl 1;                // shift bit, 7 to 0
           delay2();
           DDRtwi := DDRtwi and (not Psclin); // SCL high
           delay2();
    
           if(PINtwi and (Psdain)) <> 0 then  // test SDA on high
             data := data or (1 shl 0);       // if high, set bit 0
    	
           DDRtwi := DDRtwi or Psclin;        // SCL low
         end;
    
       if ackn then
         DDRtwi := DDRtwi or Psdain           // SDA low, acknowledge
       else
         DDRtwi := DDRtwi and (not Psdain);   // SDA high, no acknowledge
    
       delay2();
       DDRtwi := DDRtwi and (not Psclin);     // SCL high
       delay3();
       DDRtwi := DDRtwi or Psclin;            // SCL low
       DDRtwi := DDRtwi and (not Psdain);     // SDA high
    
       twi_readbyte := data;                  // return data
     end;
    

## Read TWI data word
    
    
     // Read TWI Word, MSB + LSB
    
     function twi_readword(ackn : boolean ) : uint16;
     var
       data : uint16 = 0;
       i    : uint8;
     begin
       for i := 0 to 7 do                     // output 8 bits
         begin
           data := data shl 1;                // shift bit, 7 to 0
           delay2();
           DDRtwi := DDRtwi and (not Psclin); // SCL high
           delay2();
    
           if(PINtwi and (Psdain)) <> 0 then  // test SDA on high
             data := data or (1 shl 0);       // if high, set bit 0
    
           DDRtwi := DDRtwi or Psclin;        // SCL low
         end;
    
       DDRtwi := DDRtwi or Psdain;            // SDA low, acknowledge
       delay2();
       DDRtwi := DDRtwi and (not Psclin);     // SCL high
       delay3();
       DDRtwi := DDRtwi or Psclin;            // SCL low
       DDRtwi := DDRtwi and (not Psdain);     // SDA high
    
       for i := 0 to 7 do                     // output 8 bits
         begin
           data := data shl 1;                // shift bit, 7 to 0
           delay2();
           DDRtwi := DDRtwi and (not Psclin); // SCL high
           delay2();
    
           if(PINtwi and (Psdain)) <> 0 then  // test SDA on high
             data := data or (1 shl 0);       // if high, set bit 0
    
           DDRtwi := DDRtwi or Psclin;        // SCL low
         end;
    
      if ackn then
        DDRtwi := DDRtwi or Psdain            // SDA low, acknowledge
      else
        DDRtwi := DDRtwi and (not Psdain);    // SDA high, no acknowledge
    
      delay2();
      DDRtwi := DDRtwi and (not Psclin);      // SCL high
      delay3();
      DDRtwi := DDRtwi or Psclin;             // SCL low
      DDRtwi := DDRtwi and (not Psdain);      // SDA high
    
      twi_readword := data;                   // return data
     end;
    

## End

The unit is completed with the following line. 
    
    
      end.
    

# See also

  * Overview page - [AVR Embedded Tutorial](<AVR_Embedded_Tutorial.md> "AVR Embedded Tutorial")

---

_Source: [https://wiki.freepascal.org/AVR_Embedded_Tutorial_-_Software_I2C,_TWI](https://web.archive.org/web/20250404102941/https://wiki.freepascal.org/AVR_Embedded_Tutorial_-_Software_I2C,_TWI)_
