# AVR Embedded Tutorial - Multiplex

│ **English (en)** │

## Contents

  * 1 Charlieplexing (Multiplexing) LEDs
    * 1.1 Port access
    * 1.2 Example
    * 1.3 Digits constant for 7-segment display
    * 1.4 Values ​​of the individual segments
    * 1.5 Multiplex
    * 1.6 Delay
    * 1.7 Initialization and main loop
  * 2 See also



# Charlieplexing (Multiplexing) LEDs

Charlieplexing is a technique for driving a multiplexed display in which relatively few I/O pins of a microcontroller are used to drive an array of LEDs or LED segments. 

## Port access

For how the shift register commands access the ports, see: 

  * [GPIO](<AVR_Embedded_Tutorial_-_Simple_GPIO_on_and_off_output.md> "AVR Embedded Tutorial - Simple GPIO on and off output")
  * [Shift registers](<AVR_Embedded_Tutorial_-_Shiftregister.md> "AVR Embedded Tutorial - Shiftregister")



## Example

Each time the timer is run through, a different LED segment is activated. So the timer has to run 4 times until a frame is updated. For a constant repetition frequency, the timer is best suited because it is called up regularly. A multiplex could also be made in the main loop, but if there is a short pause there for other functions, the display will start to flicker. 

The output takes place via two shift registers (74HC595). If the AVR has enough GPIO pins, you could also do this without the shift registers. 

## Digits constant for 7-segment display

The LED combinations of the digits are stored in the Digits[] constant. HEX characters are also stored in this constant. 
    
    
     const
       Digits : packed array[0..15] of byte = (
       %00111111, // = 0
       %00000110, // = 1
       %01011011, // = 2
       %01001111, // = 3
       %01100110, // = 4
       %01101101, // = 5
       %01111101, // = 6
       %00000111, // = 7
       %01111111, // = 8
       %01100111, // = 9
       %01110111, // = a - for HEX
       %01111100, // = b
       %00111001, // = c
       %01011110, // = d
       %01111001, // = e
       %01110001  // = f
       );
    

## Values ​​of the individual segments

As an example, a 4-digit 7-segment display is used. If you want a larger multiplex, you have to increase the number of digits in the display. 
    
    
     const
       digitCount = 4;
     var
       Number : array[0..digitCount - 1] of byte;
    

## Multiplex

That is the heart of the multiplex. Each time the timer is run through, the segment is changed. If you want to see this, you have to reduce the timer frequency so that you can see it. For this example, **Timer0** is used. 
    
    
    procedure Timer0_Interrupt; public name 'TIM0_COMPA_ISR'; interrupt;
    const
      segPos : byte = 0;
    begin
      segPos := segPos + 1;
    
      if(segPos > digitCount - 1) then
        begin
          segPos := 0;
        end;
    
      WritePortA(latchPin, False);
      WritePortA(latchPin, True);
    
      shiftOut595(Digits[Number[segPos]]); // Go to the current segment in the number
      shiftOut595(1 shl segPos);           // Activate current segment
    end;
    

## Delay

A simple delay. This is required when changing the number in the main loop. 
    
    
    procedure delay;
    var
      i : integer;
    begin
      for i := 0 to 200 do
        asm 
          nop  // waste cycles 
        end;
    end;
    

## Initialization and main loop

Timer0 for the mutiplex is initialized here and the required ports are switched to output. This example alternates between 1234 and 5678. 

For more about timers, see: [Timers](<AVR_Embedded_Tutorial_-_Timer,_Counter.md> "AVR Embedded Tutorial - Timer, Counter"). 
    
    
     begin
       // Disable interrupts
       asm 
         cli
       end;
    
       // Switch ports to output
       DDRA := (1 shl dataOutPin) or (1 shl latchPin) or (1 shl clockPin);
    
       // Initialize the Timer0
       TCCR0A := (1 shl 1);
       TCCR0B := TCCR0B or %101;
       TIMSK0 := TIMSK0 or (1 shl OCIE0A);
    
       // Enable interrupts
       asm 
         sei
       end;
    
       // Main loop
       repeat
         Number[0] := 1;
         Number[1] := 2;
         Number[2] := 3;
         Number[3] := 4;
         delay;
    
         Number[0] := 5;
         Number[1] := 6;
         Number[2] := 7;
         Number[3] := 8;
         delay ;
       until 1 = 2;
     end;
    

# See also

  * [AVR Embedded Tutorials](<AVR_Embedded_Tutorial.md> "AVR Embedded Tutorial") \- Overview

---

_Source: [https://wiki.freepascal.org/AVR_Embedded_Tutorial_-_Multiplex](https://web.archive.org/web/20250401000253/https://wiki.freepascal.org/AVR_Embedded_Tutorial_-_Multiplex)_
