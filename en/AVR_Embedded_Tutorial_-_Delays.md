# AVR Embedded Tutorial - Delays

│ **[Deutsch (de)](</AVR_Embedded_Tutorial_-_Delays/de> "AVR Embedded Tutorial - Delays/de")** │  **English (en)** │ 

## Contents

  * 1 Delays for ATmega328 / Arduino
  * 2 Short delays in the microsecond range
  * 3 Longer delays in the millisecond range
  * 4 Even longer delays
  * 5 See also



## Delays for ATmega328 / Arduino

... and of course other AVR controllers. 

Accurate timing on the AVR controllers is actually very simple - if you do it in assembler. Simple loops in Pascal, on the other hand, cannot be precisely timed, because you never know how the compiler will build the loop next time. 

For example, with compiler option -o1, a for loop with a 16bit counter variable is created, the same loop with -o2 and -o3 with 8 bit. This makes the loop almost twice as fast. 

## Short delays in the microsecond range
    
    
     {$ goto on} // must be specified
    
     procedure delay_short();
     label
       loop;
     begin
       asm
         ldi r16, 60
         loop:
           nop
           dec r16
           brne loop
       end ['r16'];
     end;
    

You have to try out the size of the counter variable. The loop lasts 4 clock cycles, so at 8MHz 2 loops would be one microsecond. In addition, there is a jump into the routine and the compiler tinkers with some stack pointer and register backup. 

## Longer delays in the millisecond range
    
    
     {$ goto on} // must be specified
    
     // pause msec, 1 to 255 msec
    
     procedure delay_ms(time: uint8);
     const
       fmul = 1 * fcpu div 1000000;
     label
       loop1, loop2, loop3;
     begin
       asm
         ldd r20, time
         loop1:
           ldi r21, fmul
           loop2:      // 1000 * fmul = 1000 * 1 * 8 = 8000 cycles / 8MHz
             idi r22, 250
             loop3:    // 4 * 250 = 1000 cycles
               nop
               dec r22
               brne loop3
             dec r21
             brne loop2
           dec r20
           brne loop1
       end ['r20', 'r21', 'r22'];
     end;
    

With this loop you get pretty accurate timing in the range 1 to 255msec. The loop may be interrupted by interrupts and then, of course, it will take longer. 

The inner loop lasts 4 clock cycles per run, ergo 1000 cycles. The middle loop makes it 1msec depending on the clock frequency (1 pass at 1Mhz, 8 passes at 8MHz, 16 passes at 16MHz). The outer loop then corresponds to the number of milliseconds. 

**Caution!** fmul, fcpu must be constants. Then the compiler calculates the value when compiling and uses it permanently. The compiler would insert an integer division as variables and you don't really want that - in addition, the assignment ldi to a variable goes wrong. 

## Even longer delays

... in seconds? Really, it doesn't have to be that way. Already 50msec can be taken at startup when the controller still has to wait for the peripherals. But long delays in the program flow are bad style and there are always better options. 

## See also

  * [AVR Embedded Tutorials](<AVR_Embedded_Tutorial.md> "AVR Embedded Tutorial") \- Overview page

---

_Source: [https://wiki.freepascal.org/AVR_Embedded_Tutorial_-_Delays](https://web.archive.org/web/20250419010821/https://wiki.freepascal.org/AVR_Embedded_Tutorial_-_Delays)_
