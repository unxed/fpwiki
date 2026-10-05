# ARM Embedded Tutorial - Raspberry Pi Pico using the ADC

### Introduction

There are three ADC Inputs available on the Raspberry Pi which can be accessed on 

  * ADC0 Pin 31 (GP26)
  * ADC1 Pin 32 (GP27)
  * ADC2 Pin 34 (GP28)



the input voltage range of the ADC pins is 0V .. 3.3V 

In addition there is another on-board ADC Input which is connected to a temperature sensor on the chip. 

### Load the demo project into lazarus

This example shows you how to read out the analog pins and the internal sensor. 

Please open adc/adc-raspi_pico.lpi in lazarus. 

The example on this page is a stripped down version of the full source code that you see in Lazarus. 
    
    
    program adc;
    {$MODE OBJFPC}
    {$H+}
    {$MEMORY 10000,10000}
    
    uses
      pico_gpio_c,
      pico_adc_c,
      pico_timer_c,
      pico_c;
    
    var
      milliVolts,milliCelsius : longWord;
    
    begin
      adc_init;
      // Make sure GPIO is high-impedance, no pullups etc
      adc_gpio_init(TPicoPin.ADC0);
    
      // Turn on the Temperature sensor
      adc_set_temp_sensor_enabled(true);
    
      repeat
        // Select ADC input 0 (GPIO26)
        adc_select_input(0);
    
        // For now we avoid floating point, there have been fixes done on FPC trunk which are yet to be verified
        milliVolts := (adc_read * 3300) div 4096;
    
        // Select internal temperature sensor
        adc_select_input(4);
        milliVolts := (adc_read * 3300) div 4096;
    
        //Temperature formula is : T = 27 - (ADC_voltage - 0.706)/0.001721
        milliCelsius := 27000-(milliVolts-706)*581;
    
        busy_wait_us_32(500000);
      until 1=0;
    end.
    

When programming the ADC there is more or less only one thing to be aware of, the Pins are configured with their GPIO Values (26,27,28) but when selecting a ADC Input we need to reference the ADC Inputs by their Name 

  * ADC0 --> 0
  * ADC1 --> 1
  * ADC2 --> 2
  * ADC4 --> 4



no big deal, but easy to get wrong..... 

## See also

  * [ARM Embedded Raspberry Pi Pico Tutorials](<ARM_Embedded_Tutorial_-_FPC_and_the_Raspberry_Pi_Pico.md> "ARM Embedded Tutorial - FPC and the Raspberry Pi Pico")

---

_Source: [https://wiki.freepascal.org/ARM_Embedded_Tutorial_-_Raspberry_Pi_Pico_using_the_ADC](https://web.archive.org/web/20250601000000/https://wiki.freepascal.org/ARM_Embedded_Tutorial_-_Raspberry_Pi_Pico_using_the_ADC)_
