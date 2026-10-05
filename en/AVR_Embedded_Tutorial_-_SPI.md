# AVR Embedded Tutorial - SPI

│ **[Deutsch (de)](</AVR_Embedded_Tutorial_-_SPI/de> "AVR Embedded Tutorial - SPI/de")** │  **English (en)** │    
****

## Contents

  * 1 Use of the hardware SPI interface with an ATmega328 / Arduino
    * 1.1 SPI master
      * 1.1.1 Unit header
      * 1.1.2 Initialization
      * 1.1.3 Data Transfer
      * 1.1.4 Write data only
      * 1.1.5 Send and receive a single byte
      * 1.1.6 The unit is finished
    * 1.2 Use as a master
      * 1.2.1 Write config
      * 1.2.2 Read out config
      * 1.2.3 Write register
      * 1.2.4 Read register
  * 2 See also



# Use of the hardware SPI interface with an ATmega328 / Arduino

A prerequisite is an established crosscompiler for AVR microcontrollers as described here: [Getting started with Lazarus and Arduino](<AVR_Embedded_Tutorial_-_Entry_Lazarus_and_Arduino.md> "AVR Embedded Tutorial - Entry Lazarus and Arduino"). 

For the basics of SPI, master-slave, wiring see Wikipedia: <https://de.wikipedia.org/wiki/Serial_Peripheral_Interface>

This is a very simple SPI unit in which the settings for: 

  * Data format
  * Mode
  * ChipSelect
  * Speed



must be done by hand. For the function of the individual registers and flags, see the data sheet for the ATmega328. 

## SPI master

### Unit header

First comes the Unit header which contains the pin and port configuration: 
    
    
     unit gh_spi;
    
     {$mode objfpc} {$H+}
     {$goto on}
    
     interface
    
     uses
       ; 
    
     procedure spi_init();
     procedure spi_transfer_sync(var dout : array of uint8; var din : array of uint8; len : uint8);
     procedure spi_transmit_sync(var dout : array of uint8; len : uint8);
     function spi_fast_shift(data : uint8) : uint8;
    
     const
       Psck  = 1 shl 5;  // PB5
       Pmiso = 1 shl 4;  // PB4
       Pmosi = 1 shl 3;  // PB3
       Pss   = 1 shl 2;  // PB2
       Pcs   = 1 shl 1;  // PB1
    
     var
       DDRspi  : byte absolute DDRB;  // SPI on port B
       PORTspi : byte absolute PORTB;
       PINspi  : byte absolute PINB;
    
     implementation
    

If you want to address multiple SPI devices, you can define multiple CS (ChipSelect) pins. The SS pin must be an output. If it is an input, the SPI interface can be switched to slave mode - and nothing works. And once it is an output, you can also use it for CE (ChipEnable) or CS (ChipSelect). 

### Initialization
    
    
     // initialize SPI
    
     procedure spi_init();
     begin
       DDRspi := DDRspi and not (Pmiso);           // MISO entrance
       DDRspi := DDRspi or (Pmosi or Psck or Pss); // MOSI, SCK, SS output
       // SS must be output, otherwise switching to slave from outside is possible
    
       SPCR   := ((1 shl SPE) or (0 shl SPIE) or (0 shl SPORD) or (1 shl MSTR) or (0 shl CPOL) or (0 shl CPHA) or (%01 shl SPR));
       // SPI enable, MSB first, Master select, SCK 1MHz = XTAL / 16 x 2
       SPSR   := (1 shl SPI2X);  // SCK x 2 on 1 MHz
     end ;
    

Register settings: 

  * SPE = 1: SPI enable
  * SPIE = 0: no interrupt
  * DORD = 0: MSB first, 1: LSB first
  * MSTR = 1: as master, but can be overwritten by Pin SS from outside, 0: as slave
  * CPOL = 0: SCK polarity normal, 1: SCK inverted
  * CPHA = 0: SCK phase normal, 1: SCK shifted
  * SPR = 01: fcpu / 16
  * SPI2X = 1: fcpu * 2, gives 1MHz clock at 8Mhz controller clock, see data sheet for other clock rates.



The registers corresponding to the ATmega328P unit from the Embedded AVR Sources. For other controllers, check the registers using the data sheet. 

### Data Transfer

To read data from a device, data must also be written at the same time. But that can also be dummy data. The number of bytes written and read is the same. 
    
    
     // Send, receive SPI data
    
     procedure spi_transfer_sync(var dout : array of uint8; var din : array of uint8; len : uint8);
     var
       cnt : uint8;
     begin
       for cnt := 0 to len - 1 do 
         begin 
           SPDR := dout[cnt];                     // output byte to be sent
           while (SPSR and (1 shl SPIF)) = 0 do ; // wait until done
           din[cnt] := SPDR;                      // read the received byte
         end;
     end;
    

The nice thing about Pascal is that you can work with the right arrays and not have to mess around with pointers like C does. It is important here that the arrays are passed via var. Then Pascal works with the original array, otherwise a copy is made, which takes up unnecessary time and memory. 

The routine waits until all bytes have been written out of dout and the received data has been read into din at the same time, which takes about 10µsec per byte at 1Mhz. Since these are really only a few system clocks per byte, an interrupt is not worthwhile here. The routine can be interrupted by other interrupts (timer, UART), which delays the output of the next byte, but is not a problem. 

The same array may also be used for dout and din in which case the original data will be replaced by the received data. 

**Caution!** The following applies here and for the following routines: If the routine is called without SPE being set in the SPCR (see Init), the controller remains in the queue because the data byte is not sent and consequently the SPIF flag is not set. 

### Write data only

If only data is to be written but no response is expected from the device, the routine can be simplified as follows: 
    
    
     // Send SPI data without receiving
    
     procedure spi_transmit_sync(var dout : array of uint8; len : uint8);
     var
       cnt : uint8;
     begin
      for cnt := 0 to len - 1 do 
        begin
          SPDR := dout[cnt];                    // output byte to be sent
          while(SPSR and (1 shl SPIF)) = 0 do ; // wait until done
        end;
     end;
    

The same applies here as for the transfer routine. 

### Send and receive a single byte

Sometimes only a single byte has to be transmitted, for example to query a status. 
    
    
     // Send, receive SPI single byte
    
     function spi_fast_shift(data : uint8) : uint8;
     begin
       SPDR := data;  // output byte to be sent
       while(SPSR and (1 shl SPIF)) = 0 do ; // wait until done
       spi_fast_shift := SPDR;               // read the received byte
     end;
    

Out, wait, in. Quite simple, actually. 

### The unit is finished
    
    
      end.
    

## Use as a master

An example application of the unit as a master for an NRF module. 

Of course, **spi_init** must be called at the beginning of the program. In addition, the registers of the NRF module are declared as constants and a send and a receive buffer are set up as an array of uint8. 

We have to take care of CS ourselves. In the NRF module, the CS is low-active, so the idle level is high and is pulled to low for activation. 

### Write config
    
    
     procedure nrf_set_config(reg, data : uint8);
     begin
       PORTspi := PORTspi and not (Pspi_cs);                    // set CS low active for the module
       spi_fast_shift(cW_REGISTER or (cREGISTER_MASK and reg)); // send control byte
       spi_fast_shift(data);                                    // send data byte
       PORTspi := PORTspi or Pspi_cs;                           // unset CS
     end;
    

One byte is sent to address the register and one byte with the value to which the register is to be set. 

### Read out config
    
    
     function nrf_get_config(reg : uint8) : uint8;
     begin
       PORTspi := PORTspi and not (Pspi_cs);                    // set CS low active for the module
       spi_fast_shift(cR_REGISTER or (cREGISTER_MASK and reg)); // send control byte
       nrf_get_config := spi_fast_shift(0);                     // send dummy and read data byte
       PORTspi := PORTspi or Pspi_cs;                           // unset CS
     end;
    

A byte is sent to address the register, then a dummy byte is sent. The received register content is transferred. 

### Write register
    
    
     procedure nrf_write_register(reg : uint8; len : uint8);
     begin
       PORTspi := PORTspi and not (Pspi_cs);                    // set CS low active for the module
       spi_fast_shift(cW_REGISTER or (cREGISTER_MASK and reg)); // send control byte
       spi_transmit_sync(nrftxbuf, len);                        // send data from the Tx buffer
       PORTspi := PORTspi or Pspi_cs;                           // unset CS
     end;
    

A control byte is sent, followed by the data to be written. CS remains low throughout the process. 

### Read register
    
    
     procedure nrf_read_register(reg : uint8; len : uint8);
     begin
       PORTspi := PORTspi and not (Pspi_cs);                    // set CS is low active for the module
       spi_fast_shift(cR_REGISTER or (cREGISTER_MASK and reg)); // send control byte
       spi_transfer_sync(nrftxbuf, nrfrxbuf, len);              // read data in Rx buffer, Tx buffer is dummy
       PORTspi := PORTspi or Pspi_cs;                           // unset CS
     end;
    

A control byte is sent, followed by dummy data. The received data is in the Rx buffer. 

Extensive instructions for the nRF24L01 + radio modules will follow shortly. 

# See also

  * Overview page - [AVR Embedded Tutorials](<AVR_Embedded_Tutorials.md> "AVR Embedded Tutorials")

---

_Source: [https://wiki.freepascal.org/AVR_Embedded_Tutorial_-_SPI](https://web.archive.org/web/20250319094453/https://wiki.freepascal.org/AVR_Embedded_Tutorial_-_SPI)_
