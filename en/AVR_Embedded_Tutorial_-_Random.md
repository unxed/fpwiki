# AVR Embedded Tutorial - Random

│ **[Deutsch (de)](</AVR_Embedded_Tutorial_-_Random/de> "AVR Embedded Tutorial - Random/de")** │  **English (en)** │ 

## Contents

  * 1 Random number generator
    * 1.1 A single dice
    * 1.2 Several dice
      * 1.2.1 Bad solution
      * 1.2.2 Ideal solution
  * 2 See also



# Random number generator

A random number generator is not that easy on an AVR. A random(...) function from the System unit is unfortunately not possible. This tutorial shows how to program at least one cube on an AVR. 

The range of the random number must be known from the beginning, for a classic cube this is 6. 

For communication via UART see: [UART ](<AVR_Embedded_Tutorial_-_UART.md> "AVR Embedded Tutorial - UART"). 

The following methods are suitable for simple random numbers. The prerequisite is that the random numbers are queried at irregular intervals. That is, for example. the case is also the case when a button is pressed which is queried via GPIO. 

This requirement can be seen well in the first example. If you hold down a key in the terminal program, you can easily see a regular pattern. 

If you want a **float** as a random number, it will be very complicated. I haven't found a solution (yet). 

## A single dice
    
    
     var
       Random : byte;    // The random number
    
     procedure Timer0_Interrupt; public name 'TIMER0_OVF_ISR'; interrupt;
     const
       RandomCount = 6;  // Number of dice eyes
     begin
       Inc(Random);
    
       if Random >= RandomCount then 
         begin
           Random := 0;
         end;
     end;
    
     var
       s : ShortString;
     begin
       // Timer 0
       TCCR0A := %00;            // Normal timer
       TCCR0B := %001;           // Clock = CPU
       TIMSK0 := (1 shl TOIE0);  // Enable Timer0 interrupt
      
       // UART
       UARTInit;                 // Initialize UART
    
       // Enable interrupts
       asm 
         sei
       end;                 
    
       // Main loop
       repeat
         UARTReadChar;            // Get a keystroke
         Str(random + 1 : 4 , s); // Get the random number
         UARTSendString(s);       // Output random number
       until 1 = 2;
     end.
    

## Several dice

### Bad solution

One could get the idea and do the following with several dice: 
    
    
     Str(random + 1 : 4 , s);  // Get the random number
     UARTSendString(s);        // Output random number
     Str(random + 1 : 4 , s);  // Depends on the first letter
     ...
    

You get the problem that the order of the throws of the dice is predictable. Therefore this idea is unusable! 

### Ideal solution

For this reason I have the following code, because it happens unpredictably. 

The only drawback, it is a bit more complicated, but each random number is independent of the others. In this example there are 15 dice. The timer had to be set a little slower than in the example with only one dice. 

The trick is, not all dice are incremented with every timer call. The second dice only increments with every second call, the third only with the third call, etc. 
    
    
     // Global variables.
     const
       numDice = 15;
     var
       Random : array[0..numDice - 1] of byte;
    
     procedure Timer0_Interrupt; public name 'TIMER0_OVF_ISR'; interrupt;
     // Local variables
     const
       RandomCount = 6;    // number of dice eyes
     var
       i   : integer;
       rnd : array[0..numDice - 1] of byte;
    
     begin
       for i := 0 to numDice - 1 do 
         begin
           Inc(rnd[i]);
    
             if rnd[i] > i then 
               begin
                 rnd[i] := 0;
                 Inc(Random[i]);
               
                 if Random[i] >= RandomCount then 
                   begin
                     Random[i] := 0;
                   end;   
               end;
           end;
     end;
    
     var
       s : ShortString;
       i : integer;
    
     begin
       // timer 0
       TCCR0A := %00;            // Normal timer
       TCCR0B := %011;           // Clock = CPU / 16
       TIMSK0 := (1 shl TOIE0);  // Enable Timer0 interrupt
    
       // UART
       UARTInit;                 // Initialize UART
    
       // Enable interrupts
       asm 
         sei
       end;              
    
       // Main loop
       repeat
         UARTReadChar;
    
         for i := 0 to numDice - 1 do
           begin
             Str(Random[i] + 1 : 4 , s);  // Each dice has its own random number
             UARTSendString(s);
           end;
        
         UARTSendString(#13#10);
       until 1 = 2;
     end.
    

# See also

  * [AVR Embedded Tutorials](<AVR_Embedded_Tutorial.md> "AVR Embedded Tutorial") \- Overview

---

_Source: [https://wiki.freepascal.org/AVR_Embedded_Tutorial_-_Random](https://web.archive.org/web/20241209225018/https://wiki.freepascal.org/AVR_Embedded_Tutorial_-_Random)_
