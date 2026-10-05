# AVR Embedded Tutorial - Timer, Counter

│ **English (en)** │

## Contents

  * 1 Timer / counter
    * 1.1 Example, control LEDs via timer
      * 1.1.1 Example code
        * 1.1.1.1 Timer 0 interrupt
        * 1.1.1.2 Timer 1 interrupt
        * 1.1.1.3 Initialize the timer
    * 1.2 Timer clocked externally
      * 1.2.1 Clock example
        * 1.2.1.1 Timer interrupt
        * 1.2.1.2 Initialize the timer
        * 1.2.1.3 Pitfalls
  * 2 See also



# Timer / counter

This is not about understanding all the AVR registers. This is only a practical example of how to implement timers/counters in FPC. You should understand that timers are simply counters. 

More details are available [here](<https://www.mikrocontroller.net/articles/AVR-GCC-Tutorial/Die_Timer_und_Z%C3%A4hler_des_AVR>) (in German) or use your favourite search engine. 

## Example, control LEDs via timer

This example shows how you can use the two timers of an ATtiny2313. Each timer controls an LED, which are at bits 0 and 1 of Port D. The LED on PD0 should flash a little faster. 

### Example code
    
    
    program Project1;
    

#### Timer 0 interrupt
    
    
    procedure Timer0_Interrupt; public name 'TIMER0_COMPA_ISR'; interrupt;
      const
        t = 10;           // LED should only switch every 10th pass
        z : integer = 0;  // Counter for idle runs
      begin
        TCNT0 := 128;     // Half speed 0 = slow (default)
        Inc(z);
    
        if(z = t) then 
          begin
            PORTD := PORTD or (1 shl 0);      // Pin 0 LED on
          end;
    
        if(z = t shl 1) then 
          begin
            PORTD := PORTD and not (1 shl 0); // Pin 0 LED off
            z := 0;
          end;
      end;
    

#### Timer 1 interrupt
    
    
    procedure Timer1_Interrupt; public name 'TIMER1_COMPA_ISR'; interrupt;
      const
        t = 500;          // LED should only switch every 500th pass
        z : integer = 0;  // Counter for idle runs
      begin
        Inc (z);
    
        if(z = t) then 
          begin
            PORTD := PORTD or (1 shl 1);      // Pin 1 LED on
          end;
      
        if(z = t shl 1) then 
          begin
            PORTD := PORTD and not (1 shl 1); // Pin 1 LED off
            z : = 0;
          end;
      end;
    

#### Initialize the timer
    
    
      begin
       // Disable interrupts
       asm 
         cli 
       end;
    
       // Set PD0 and PD1 to output
       DDRD := %00000011;
    
       // Initialize timer0
       TCCR0A := 0;                        // Normal mode
       TCCR0B := %101;                     // CPU clock / 1024
       TIMSK  := TIMSK or (1 shl OCIE0A);  // Timer0 should trigger an interrupt
    
       // Initialize timer1
       TCCR1A := 1;                        // CTC (Clear Timer on Compare match) mode
       TCCR1B := %010;                     // CPU clock / 8
       TIMSK  := TIMSK or (1 shl OCIE1A);  // Timer1 should trigger an interrupt
    
       // Enable interrupts
       asm 
         sei
       end;
    
       // Main loop
       repeat
         // Do something
       until 1 = 2;
     end.
    

## Timer clocked externally

A timer can also be clocked externally. The following example demonstrates this with an Atmega328. The following pins are available on the AVR for external clocking: 

  * **T0** clocks timer 0
  * **T1** clocks timer 1



Timer 2 cannot be clocked externally. 

### Clock example

A quartz crystal/oscillator with 4.194304MHz is suitable for an exact clock. This can easily be divided into a power of two, so that you get a cycle of one second. Here timer 0 is clocked externally, for this you have to connect the oscillator to T0 (PD4). 

#### Timer interrupt

Every 16,384th call of the interrupt is one second. 
    
    
    procedure Timer0_Interrupt; public name 'TIMER0_OVF_ISR'; interrupt;
      const
        cl = 16384 shr 1;     // 4194304/256/2
        counter : UInt16 = 0; // Counts until cl is reached
    
      begin
        Inc(counter);
    
        // Turn off the LED after half a second
        if count = cl then 
          begin
            PORTB := PORTB and not (1 shl 5);
          end;
    
        // 1 second is reached.
        if count > = cl shl 1 then 
          begin
            PORTB := PORTB or (1 shl 5);
            count := 0;       // Reset counter.
          end;
      end;
    

#### Initialize the timer

The important point is the value at TCCR0B which determines that the timer is clocked externally. 
    
    
      begin
        DDRB := 1 shl 5;         // Set pin PD5 as an output
    
        TCCR0A := %00;           // Normal timer
        TCCR0B := %111;          // Clock / external pin TO, rising edge.
        TIMSK0 := (1 shl TOIE0); // Activate timer 0 interrupt.
    
        asm 
          sei  // enable interrupts
        end;  
    
       repeat
         // Here the time can be shown on a display
       until 1 = 2; 
     end.
    

#### Pitfalls

The names of the procedures can differ, for example: 
    
    
     // ATtiny2313
     procedure Timer0_Interrupt; public name 'TIMER0_COMPA_ISR'; interrupt;
     // ATtiny44
     procedure Timer0_Interrupt; public name 'TIM0_COMPA_ISR'; interrupt;
    

# See also

  * [AVR Embedded Tutorials](<AVR_Embedded_Tutorial.md> "AVR Embedded Tutorial") \- Overview

---

_Source: [https://wiki.freepascal.org/AVR_Embedded_Tutorial_-_Timer,_Counter](https://web.archive.org/web/20220819230545/https://wiki.freepascal.org/AVR_Embedded_Tutorial_-_Timer,_Counter)_
