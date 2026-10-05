# AVR Embedded Tutorial - Analog Read

│ **English (en)** │

## Contents

  * 1 Analog Read
    * 1.1 Polling method
      * 1.1.1 Initialize ADC
      * 1.1.2 Measuring
      * 1.1.3 Output via UART
    * 1.2 Interrupt method
      * 1.2.1 Initialisation
      * 1.2.2 Output measured value in the interrupt
      * 1.2.3 Main program
  * 2 See also



# Analog Read

This code is for an Atmega328/Arduino runnning at 16MHz. How to control the UART can be found here: 
    
    
       [UART UARTInit and UARTSendString (...](<AVR_Embedded_Tutorial_-_UART.md> "AVR Embedded Tutorial - UART")
    

The ATmega328 can measure with 10-bit accuracy, so a value between 0..1023 is output. 

## Polling method

### Initialize ADC

Configure analog reading. AVcc is used as the reference voltage, this is the easiest way. This must be configured with **ADMUX**. 

The following reference voltages can be configured with ADMUX: 

Bit 7 | Bit 6 | Description   
---|---|---  
0 | 0 | Pin AREF   
0 | 1 | Use AVcc   
1 | 0 | \-   
1 | 1 | 2.56V   
  
The converter is started with **ADCSRA**. 
    
    
    procedure ADC_Init;
      begin
        ADMUX  := (1 shl REFS);         // Use AVcc for reference
        ADCSRA := (1 shl ADEN) or %111; // Switch on converter, divider 128
      end;
    

### Measuring

The procedure for measuring is as follows. 

  * Specify the port.
  * Wait until the measurement is complete.
  * Read out the measured value.



With ADMUX you have to enter the reference voltage again because it shares the register with the reference. You could also do an OR operation with ADMUX . 
    
    
    function ADC_Read(Port : byte) : integer;
      begin
        ADMUX  := (1 shl REFS) or (Port and $0F);  // Specify port
        ADCSRA := ADCSRA or ( 1 shl ADSC );        // Start measuring
    
        while (ADCSRA and (1 shl ADSC)) <> 0 do    // Wait until measured
          begin 
          end;
    
        Result := ADC;  // Read out the measured value
      end;
    

If you always read in from the same port, you only have to call up the ADMUX line once. You can also start measuring directly after the while loop which saves a few clock cycles. 

### Output via UART

Buffer variables for the measurement data. 
    
    
    var
      Data : integer;
      s : string[10];
    

The measurement data is then output in an endless loop. 
    
    
      begin
       // Initialize UART
       UARTInit;
    
       // Initialize ADC
       ADC_Init;
    
       // Allow interrupt
       asm Sei end;
    
       // output measured values.
       repeat
         Data := ADC_Read(0);
         str(Data : 6, s);
         UARTSendString(s);
       until 1 = 2;
     end.
    

## Interrupt method

The advantage of the interrupt-controlled method is that you do not have to wait until the measurement is finished, so you have time for other things. 

### Initialisation

The difference: 

  * With ADMUX you enter the input port at the beginning.
  * At ADCSRA you have to start the first measurement with ADSC.
  * With ADIE you communicate that the conversion interrupt is controlled.


    
    
    procedure ADC_Init;
      const
        Port = 0;
    
      begin
        ADMUX  := (1 shl REFS) or (Port and $0F);
        ADCSRA := %111 or (1 shl ADEN) or (1 shl ADSC) or (1 shl ADIE);
      end;
    

### Output measured value in the interrupt
    
    
    procedure ADC_Ready public name 'ADC_ISR';  interrupt;
      var
        Data : integer;
        s : string[10];
    
      begin
        Data   := ADC;
        ADCSRA := ADCSRA or (1 shl ADSC);
        str(Data : 6, s);
        UARTSendString(s);
      end;
    

[![Note-icon.png](https://wiki.freepascal.org/images/b/be/Note-icon.png)](</File:Note-icon.png>)

**Tip:** Interrupt routines should be kept short. This does not mean that you cannot do any processing in an interrupt routine, but you do need to ensure that when the next interrupt occurs, the last one is not still running.

### Main program

Since the measurement is interrupt-controlled, the main loop remains empty. It is important to switch on the interrupts. 
    
    
    begin
      // Initialize UART
      UARTInit;
    
      // ADC
      ADC_Init;
    
      // Allow interrupt
      asm Sei end;
    
      // main loop
      repeat
        // Do something
      until 1 = 2;
    end.
    

# See also

  * [AVR Embedded Tutorials](<AVR_Embedded_Tutorial.md> "AVR Embedded Tutorial") \- Overview

---

_Source: [https://wiki.freepascal.org/AVR_Embedded_Tutorial_-_Analog_Read](https://web.archive.org/web/20221208051609/https://wiki.freepascal.org/AVR_Embedded_Tutorial_-_Analog_Read)_
