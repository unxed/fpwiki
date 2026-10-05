# AVR Embedded Tutorial - GPIO-Interrupt

│ **English (en)** │

## Contents

  * 1 GPIO interrupt
    * 1.1 Introduction
    * 1.2 External interrupt
      * 1.2.1 Interrupt routine
      * 1.2.2 Activate interrupt
      * 1.2.3 Register for the Atmega328p
      * 1.2.4 INT1 interrupt
    * 1.3 Pin change interrupt
      * 1.3.1 Terminal issue
      * 1.3.2 Interrupt routine
      * 1.3.3 Declare pin change GPIO
      * 1.3.4 Activate pin change interrupt
      * 1.3.5 Registers for the Atmega328p
      * 1.3.6 Example for PORT B and PORT C
  * 2 See also



# GPIO interrupt

## Introduction

How to control the GPIO and UART can be found here: 

  * GPIO
  * UART _UARTInit_ and _UARTSendString(..._



## External interrupt

Interrupt pin assignment for the ATmega328: 

  * INT0: PD2
  * INT1: PD3



### Interrupt routine

This example shows how this works with INT0. You use **EICRA** to specify which interrupt is triggered. See table below. You can also activate INT0 and INT1 at the same time. 
    
    
    procedure Int0_Interrupt; public name 'INT0_ISR'; interrupt;
    begin
      UARTSendString('INT0');  // Identify interrupt
    end;
    

### Activate interrupt

The parameters are described in the table below. 
    
    
    begin
      PORTD := %00001100;  // PullUp Pin 2 + 3
    
      // Initialize UART
      UARTInit ;
    
      // Activate INT0
      EICRA := %11;  // INT0 is triggered when the pin goes HIGH.
      EIMSK := %01;  // activate INT0.
    
      // Activate interrupt
      asm 
        Sei
      end;
    
      // Main loop
      repeat
      until 1 = 2;
    end.
    

### Register for the Atmega328p

The following registers are responsible for the activation of the ports/pins: 

  * **EICRA** Function of the interrupt.
  * **EIMSK** Activate INT0 and/or INT1.



Table for **EICRA** which shows what kind of action on the pin triggers an interrupt. 

| INT1 |  | INT0 |   
---|---|---|---|---  
Bit | 3 | 2 |  | 1 | 0 |   
| 0 | 0 |  | 0 | 0 | Low Level on the pin   
| 0 | 1 |  | 0 | 1 | Every change   
| 1 | 0 |  | 1 | 0 | Pin goes LOW   
| 1 | 1 |  | 1 | 1 | Pin goes HIGH   
  
### INT1 interrupt

The INT1 interrupt must be set as follows: 
    
    
      EICRA := %1100;  // INT1 is triggered when the pin goes HIGH.
      EIMSK := %10;    // activate INT1.
    

For the INT1 interrupt you have to use the ISR procedure for the INT1 (**INT1_ISR**). 
    
    
    procedure Int1_Interrupt; public name 'INT1_ISR'; interrupt;
    begin
      // do something
    end;
    

## Pin change interrupt

This example is for an Arduino Nano (Atmega328p). 

### Terminal issue

Output the pin status on a terminal. 
    
    
    procedure WritePort(p : byte);
    begin
      UARTSendString ('PB' + char(p + 48) + ':');
    
      if PinB and (1 shl p) = (1 shl p) then 
        begin
          UARTSendString('HIGH');
        end 
      else 
        begin
          UARTSendString('LOW');
        end;
    end;
    

### Interrupt routine

The interrupt fires when a pin changes. 
    
    
    procedure PC_Int0_Interrupt; public name 'PCINT0_ISR'; interrupt;
    var
      i : byte;
    begin
      UARTSendString(#27 '[0; 0H');  // Home the cursor
    
      for i := 0 to 3 do 
        begin
          WritePort(i);
        end;
    end;
    

### Declare pin change GPIO

Activate pins PB0, PB1, PB2 and BP3 for interrupts. 
    
    
    const
      inPortsB = %00001111;  // PCINT0 + PCINT1 + PCINT2 + PCINT3
    

### Activate pin change interrupt

The parameters are described in the table below. 
    
    
    procedure PC_Int0_Interrupt; public name 'PCINT0_ISR'; interrupt;
    begin
      DDRB  := 0;
      PORTB := inPortsB;  // PullUp
    
      // Initialize UART
      UARTInit;
    
      // Activate PCINT0_ISR
      PCICR  := %001;     // Only on PORTB
      PCMSK0 := inPortsB;
    
      // Activate interrupt
      asm 
        Sei
      end;
    
      // Main loop
      repeat
        // do something
      until 1 = 2;
    end.
    

### Registers for the Atmega328p

The following registers are responsible for the activation of the ports/pins: 

  * **PCICR** Ports that trigger an interrupt.
  * **PCMSKx** The individual pins.



Table for PCICR: 

Bit | 2 | 1 | 0   
---|---|---|---  
PCICR | PORTD | PORTC | PORTB   
  
Table for the pins of PCMSKx : 

Bit | 7 | 6 | 5 | 4 | 3 | 2 | 1 | 0   
---|---|---|---|---|---|---|---|---  
PCMSK0 | PCINT7 | PCINT6 | PCINT5 | PCINT4 | PCINT3 | PCINT2 | PCINT1 | PCINT0   
PCMSK1 |  | PCINT14 | PCINT13 | PCINT12 | PCINT11 | PCINT10 | PCINT9 | PCINT8   
PCMSK2 | PCINT23 | PCINT22 | PCINT21 | PCINT20 | PCINT19 | PCINT18 | PCINT17 | PCINT6   
  
### Example for PORT B and PORT C
    
    
    // Activate PCINT0_ISR and PCINT1_ISR
    PCICR  := %011;      // PORT B and PORT C
    PCMSK0 := %11000000; // Pin 6 + 7 of PORT B
    PCMSK1 := %00000011; // Pin 0 + 1 of PORT C
    

A second ISR procedure is added for the PCINT1_ISR. If you would like to use PORT D, then a procedure for the PCINT2_ISR would need to be added. 
    
    
    procedure PC_Int1_Interrupt; public name 'PCINT1_ISR'; interrupt;
    begin
      // do something
    end;
    

# See also

  * [AVR Embedded Tutorials](<AVR_Embedded_Tutorial.md> "AVR Embedded Tutorial") \- Overview

---

_Source: [https://wiki.freepascal.org/AVR_Embedded_Tutorial_-_GPIO-Interrupt](https://web.archive.org/web/20250402182814/https://wiki.freepascal.org/AVR_Embedded_Tutorial_-_GPIO-Interrupt)_
