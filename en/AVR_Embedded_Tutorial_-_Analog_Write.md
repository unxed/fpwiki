# AVR Embedded Tutorial - Analog Write

│ **English (en)** │

## Contents

  * 1 PWM Analog Write
    * 1.1 Delay
    * 1.2 Example
    * 1.3 Pitfalls
  * 2 See also



# PWM Analog Write

An Atmega328 is used in this example. 

## Delay

A simple delay function used to slowly increase and decrease the brightness of the LEDs. 
    
    
    procedure sleep;
      var
        i : Int16;
      begin
        for i := 0 to 1999 do 
          asm 
            Nop  // No operation instruction to waste clock cycles
          end;
      end;
    

## Example

In this example, the PWM frequency is set to be very slow, so that you can see the brightening and darkening of the LEDs. You can see that the analog output is actually modulated. 

The following must be set: 

  * With **DDR** x, the PWM output must be set as an output.
  * **COM** xx specifies the pins to run in PWM mode. The ATmega328 has 6 pins that can run in this mode. Each timer can operate two outputs.
  * **WGM** activates the PWM output of the timer.
  * **TCCR0B** is set to a clock of 1024, if you choose a smaller value, you will no longer see the LEDs flashing.
  * **OCR0** specifies the **HIGL** length of the modulation. 0 is off, 128 is the mean and 255 is the maximum.


    
    
      var
       i : integer;  // Loop counter
     begin
       // Configure PD5 and PD6 as outputs
       DDRD := %01100000;
    
       // Configure PD5 and PD6 in PWM mode
       TCCR0A := (%10 shl COM0A) or (%10 shl COM0B) or (1 shl WGM0);
    
       // Enable timer 0, clock / 1024
       TCCR0B := TCCR0B or %101;
    
       // Main loop that continuously makes the LEDs brighter and darker
       repeat
         for i := 0 to 255 do 
           begin
             OCR0A := i;
             OCR0B := 255 - i;
             sleep;
         end;
    
         for i := 0 to 255 do 
           begin
             OCR0A := 255 - i;
             OCR0B := i;
             sleep;
           end;
       until False;
     end.
    

## Pitfalls

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** Since PWM is controlled by a timer, there may be overlaps with the timer functions.

For information on the timer functions, see the [Timer, Counter](<AVR_Embedded_Tutorial_-_Timer,_Counter.md> "AVR Embedded Tutorial - Timer, Counter") tutorial. 

# See also

  * [AVR Embedded Tutorials](<AVR_Embedded_Tutorial.md> "AVR Embedded Tutorial") \- Overview

---

_Source: [https://wiki.freepascal.org/AVR_Embedded_Tutorial_-_Analog_Write](https://web.archive.org/web/20240907142613/https://wiki.freepascal.org/AVR_Embedded_Tutorial_-_Analog_Write)_
