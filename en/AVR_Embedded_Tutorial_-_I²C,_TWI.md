# AVR Embedded Tutorial - I²C, TWI

│ **[Deutsch (de)](</AVR_Embedded_Tutorial_-_I%C2%B2C,_TWI/de> "AVR Embedded Tutorial - I²C, TWI/de")** │  **English (en)** │    
****

## Contents

  * 1 I²C / TWI
    * 1.1 Description
    * 1.2 I²C functions
      * 1.2.1 Constants
      * 1.2.2 Initialize I²C
      * 1.2.3 Start I²C send/receive
      * 1.2.4 I²C transmission / reception ended
      * 1.2.5 Send characters
      * 1.2.6 Receive characters
      * 1.2.7 Character received with error query
    * 1.3 ADS1115 specific code
    * 1.4 Example
    * 1.5 ADS1115 parameters
  * 2 See also



# I²C / TWI

This code is for an Atmega328/Arduino with 16MHz clock. 

How to control the UART can be found here: [UARTInit and UARTSendString (...](<AVR_Embedded_Tutorial_-_UART.md> "AVR Embedded Tutorial - UART")

## Description

This example demonstrates the communication with the I²C/TWI interface. 

Two AD1115 A/D converters are used as slave devices. These are very easy to use and are also very easy to obtain. What is very important is that with TWIStart you shift the address one bit to the left and then combine it with _or_ using Write/Read. There are many tutorials on this. 

PulUp resistors are not necessary because they are already installed on the ADS1115 board (**Caution** : check _your_ board for pullup resistors). 

## I²C functions

### Constants
    
    
      const
        CPU_Clock = 16000000; // Arduino clock frequency, default 16MHz.
        ADSaddr0  = $48;      // Address converter 0
        ADSaddr1  = $49;      // Address converter 1
        I2C_clock = 400000;   // I²C clock (400KHz)
        TWI_Write = 0;
        TWI_Read  = 1;
    

### Initialize I²C
    
    
      procedure TWIInit;
        const
          TWBR_val = byte((CPU_Clock div I2C_clock) - 16) div 2;
        begin
          TWSR := 0;
          TWBR := byte(TWBR_val);
        end;
    

### Start I²C send/receive
    
    
      procedure TWIStart(addr : byte);
       begin
         // Initiate sending / receiving
         TWCR := 0;
         TWCR := (1 shl TWINT) or (1 shl TWSTA) or (1 shl TWEN);
    
         while ((TWCR and (1 shl TWINT)) = 0) do 
           begin
           end;
    
         // Send the address of the end device
         TWDR := addr;
         TWCR := (1 shl TWINT) or (1 shl TWEN);
    
         while((TWCR and (1 shl TWINT)) = 0) do 
           begin
           end;
       end;
    

### I²C transmission / reception ended
    
    
      procedure TWIStop ;
        begin
          TWCR := (1 shl TWINT) or (1 shl TWSTO) or (1 shl TWEN);
        end;
    

### Send characters
    
    
      procedure TWIWrite(u8data : byte);
        begin
          TWDR := u8data;
          TWCR := (1 shl TWINT) or (1 shl TWEN);
    
          while((TWCR and (1 shl TWINT)) = 0) do 
            begin
            end;
        end;
    

### Receive characters

Except for the last character, characters must be read with **TWIReadACK (...**. The last character must be read with **TWIReadNACK(...**
    
    
      // Read to the penultimate character.
      function TWIReadACK : byte;
      begin
        TWCR := (1 shl TWINT) or (1 shl TWEN) or (1 shl TWEA);
         
        while (TWCR and (1 shl TWINT)) = 0 do 
          begin
          end;
    
       Result := TWDR;
      end;
    
      // read last character.
      function TWIReadNACK : byte ;
      begin
        TWCR := (1 shl TWINT) or (1 shl TWEN);
    
        while(TWCR and (1 shl TWINT)) = 0 do 
          begin
          end;
       
        Result := TWDR;
      end;
    

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** These functions get stuck in the event of a hardware error or the slave address is wrong!

### Character received with error query

So that these functions do not get stuck, you have to install a counter in the while loop. 

When the counter reaches 255, the loop is interrupted and an error is output. 

255 is reached if no device is found at the I²C address. 
    
    
      // Read to the penultimate character.
     function TWIReadACK_Error : byte;
     var
       err : byte;
     begin
       err := 0;
       TWCR := (1 shl TWINT) or (1 shl TWEN) or (1 shl TWA);
    
       while ((TWCR and (1 shl TWINT)) = 0) and (err < 255) do 
         begin
            Inc(err);
         end;
    
       if err = 255 then 
         begin
           UARTSendString('I²C-Timeout');
           Result := 0;
         end 
       else 
         begin
           Result := TWDR;
         end;
     end;
    
     // read last character.
     function TWIReadNACK_Error : byte;
     var
       err : byte;
     begin
       err  := 0;
       TWCR := (1 shl TWINT) or (1 shl TWEN);
    
       while((TWCR and (1 shl TWINT)) = 0) and (err < 255) do 
         begin
           Inc(err);
         end;
    
       if err = 255 then 
         begin
           UARTSendString('I²C-Timeout');
           Result := 0;
         end 
       else 
         begin
           Result := TWDR;
         end;
     end;
    

## ADS1115 specific code

This code is ADS1115 specific, for other I²C devices you will have to check their data sheets. 
    
    
      // Send parameters to ADS1115 and initiate measurement
       procedure WriteADS1115(addr : UInt16);
       begin
         TWIStart((addr shl 1) or TWI_Write);
         TWIWrite(1);
         TWIWrite(%11000011);  // High
         TWIWrite(%11100011);  // low
         TWIStop;
       end;
    
       // Read out measurement from ADS1115.
       function ReadADS1115(addr : UInt16) : UInt16;
       begin
         TWIStart((addr shl 1) or TWI_Write);
         TWIWrite(0);
         TWIStop;
    
         TWIStart((addr shl 1) or TWI_Read);
         Result := TWIReadACK * $100 + TWIReadNACK;
         TWIStop;
       end;
    

## Example
    
    
      var
       Data : Integer;
       s    : ShortString;
    
     begin
       asm cli end ;  // block interrupt
       UARTInit;
       TWIInit;
       asm be end;    // release interrupt
    
       repeat
         WriteADS1115(ADSaddr0);
         Data := ReadADS1115(ADSaddr0);
    
         str(Data : 6, s);
         UARTSendString('Channel 0:');
         UARTSendString(s);
    
         WriteADS1115(ADSaddr1);
         Data := ReadADS1115(ADSaddr1);
    
         str(Data : 6, s);
         UARTSendString('Channel 1:');
         UARTSendString(s);
         UARTSendString(#13#10);
       until 1 = 2;
     end.
    

## ADS1115 parameters

A list of the parameters for the ADS1115. More information is in the data sheet. 

Bit assignment of the ADS1115: 

**Low:** 11100011 **High:** 11000011 

Bit | Name | Description   
---|---|---  
15 | **OS:** Operational status or single-shot conversion start |  **0** : no effect  
**1** : Start a single conversion (when in power-down state)   
14:12 | **MUX[2:0]:** Input multiplexer configuration (ADS1115 only) |  **000** : AINP = AIN0 and AINN = AIN1 (default)  
**001** : AINP = AIN0 and AINN = AIN3  
**010** : AINP = AIN1 and AINN = AIN3  
**011** : AINP = AIN2 and AINN = AIN3  
**100** : AINP = AIN0 and AINN = GND  
**101** : AINP = AIN1 and AINN = GND  
**110** : AINP = AIN2 and AINN = GND  
**111** : AINP = AIN3 and AINN = GND   
11:9 | **PGA[2:0]:** Programmable gain amplifier configuration |  **000** : FSR = ±6.144 V  
**001** : FSR = ±4.096 V  
**010** : FSR = ±2.048 V (default)  
**011** : FSR = ±1.024 V  
**100** : FSR = ±0.512 V  
**101** : FSR = ±0.256 V  
**110** : FSR = ±0.256 V  
**111** : FSR = ±0.256 V  
  
8 | **MODE:** Device operating mode |  **0** : Continuous-conversion mode  
**1** : Single-shot mode or power-down state (default)   
7:5 | **DR[2:0]:** Data rate |  **000** : 8 SPS  
**001** : 16 SPS  
**010** : 32 SPS  
**011** : 64 SPS  
**100** : 128 SPS (default)  
**101** : 250 SPS  
**110** : 475 SPS  
**111** : 860 SPS  
  
4 | **COMP_MODE:** Comparator mode |  **0** : Traditional comparator (default)  
**1** : Window comparator   
3 | **COMP_POL:** Comparator polarity |  **0** : Active low (default)  
**1** : Active high   
2 | **COMP_LAT:** Latching comparator |  **0** : Nonlatching comparator (default)  
**1** : Latching comparator   
1:0 | **COMP_QUE[1:0]:** Comparator queue and disable |  **00** : Assert after one conversion  
**01** : Assert after two conversions  
**10** : Assert after four conversions  
**11** : Disable comparator and set ALERT/RDY pin to high-impedance (default)   
  
# See also

  * Overview page - [AVR Embedded Tutorial](<AVR_Embedded_Tutorial.md> "AVR Embedded Tutorial")

---

_Source: [https://wiki.freepascal.org/AVR_Embedded_Tutorial_-_I%C2%B2C%2C_TWI](https://web.archive.org/web/20240907040300/https://wiki.freepascal.org/AVR_Embedded_Tutorial_-_I%C2%B2C%2C_TWI)_
