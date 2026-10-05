# ARM Embedded Tutorial - Simple Timer

│ **English (en)** │

## Contents

  * 1 Simple timer function
    * 1.1 Program head
    * 1.2 Constants and identifiers
    * 1.3 Interrupt function
    * 1.4 Main program and initialization
    * 1.5 Multiple timers
  * 2 See also



# Simple timer function

In this example, a simple timer for an STM32F103C is used. This microcontroller has 4 timers and you can use timers 2-4 very easily. Timer 1 has several options, so it is more complicated to use. 

## Program head
    
    
    program Timer_Blink;
    
    uses
      cortexm3;
    

## Constants and identifiers
    
    
    const
      TIM_CR1_CEN : uInt16 = $0001;            // Counter enable
      RCC_APB1ENR_TIM2EN : uInt32 = $00000001; // TIM2 clock enabled
    
      TIM_DIER_CC1IE : uInt16 = $0002;         // Capture / Compare 1 interrupt enable
      TIM_SR_CC1IF : uInt16 = $0002;           // Capture / Compare 1 interrupt flag
      TIM2_IRQn = 28;                          // TIM2 global interrupt
    
      // For easy access to the LED PC13.
      Type
        TLed = bitpacked array[0..15] of boolean;
    
    var
      LedC : TLed absolute GPIOC.ODR;
    

## Interrupt function
    
    
    procedure Timer2_Interrupt; public name 'TIM2_global_interrupt';  interrupt;
      begin
        TIM2.SR  := TIM2.SR and not TIM_SR_CC1IF; // Clear CC1IF
        LedC[13] := not LedC[13];                 // Toggle LED PC13
      end;
    

## Main program and initialization

With **TIM2.PSC** and **TIM2.ARR** you can set the speed of the timer, the smaller the values, the faster it runs. 
    
    
      begin
        // Turn on ports
        RCC.APB2ENR := RCC.APB2ENR or (%111 shl 2);
    
        // Switch ports to output
        GPIOC.CRL := $33333333;
        GPIOC.CRH := $33333333;
    
        // --- timer ---
        // RCC enable for Timer2
        RCC.APB1ENR := RCC.APB1ENR or RCC_APB1ENR_TIM2EN;
    
        // Various default settings
        TIM2.CR1   := $0000;  // Control register 1;  Wait with timer enable until settings complete
        TIM2.CR2   := $0000;  // Control register 2
        TIM2.CCMR1 := $0000;  // Capture / compare mode register
        TIM2.CCER  := $0000;  // Capture / compare enable register
    
        // Prescaler
        TIM2.PSC := 1000;
    
        // AutoReload value
        TIM2.ARR := 1000;
    
        // DMA and interrupt control register
        TIM2.DIER := TIM_DIER_CC1IE;  // Compare 1 interrupt enable
    
        // Enable counter
        TIM2.CR1 := TIM2.CR1 or TIM_CR1_CEN;
    
        // NVIC configuration
        NVIC.IP[TIM2_IRQn] := $20;    // IRQ priority, right 4 bits always to be zero
        NVIC.ISER[TIM2_IRQn shr 5] := 1 shl (TIM2_IRQn and $1F);  // Enable interrupt
    
        // --- main loop ---
        while True do begin
          // Do something
        end;
     end.
    

## Multiple timers

You can use 3 simple timers in parallel. These are Timer2 to Timer4. Timer1 is used differently. If you want to use timers 3 and 4, you simply have to replace TIM2 with TIM3 and/or TIM4. 

# See also

  * [ARM Embedded Tutorials](<ARM_Embedded_Tutorials.md> "ARM Embedded Tutorials") \- Overview

---

_Source: [https://wiki.freepascal.org/ARM_Embedded_Tutorial_-_Simple_Timer](https://web.archive.org/web/20240701000000/https://wiki.freepascal.org/ARM_Embedded_Tutorial_-_Simple_Timer)_
