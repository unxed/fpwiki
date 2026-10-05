# ARM Embedded Tutorial - Raspberry Pi Pico Scanning for I2C Devices

## Introduction

The I2C Protocol allows us in theory to connect and control more than 100 devices with a single pair of wires. 

To be able to do so every device has an on-board 7-bit ID that often can be changed between several numbers to allow more than one instance of a device to be connected to the bus. 

This small program allows us to scan the bus for all connected devices and it outputs the result in a little table. 

## Load the project into Lazarus

Please open the project i2c/i2c_scan-raspi_pico.lpi in Lazarus 
    
    
    program i2c_scan;
    {$MODE OBJFPC}
    {$H+}
    {$MEMORY 10000,10000}
    
    uses
      pico_gpio_c,
      pico_uart_c,
      pico_i2c_c,
      pico_timer_c,
      pico_c;
    
    const
      BAUD_RATE=115200;
    var
      addr : byte;
      ret : longInt;
      rxData : array[0..0] of byte;
      tmpStr : String;
    begin
      gpio_init(TPicoPin.LED);
      gpio_set_dir(TPicoPin.LED,TGPIODirection.GPIO_OUT);
    
      uart_init(uart0, BAUD_RATE);
      gpio_set_function(TPicoPin.GP0_UART0_TX, GPIO_FUNC_UART);
      gpio_set_function(TPicoPin.GP1_UART0_RX, GPIO_FUNC_UART);
    
      i2c_init(i2c0Inst, 100000);
      gpio_set_function(TPicoPin.GP4_I2C0_SDA, GPIO_FUNC_I2C);
      gpio_set_function(TPicoPin.GP5_I2C0_SCL, GPIO_FUNC_I2C);
      gpio_pull_up(TPicoPin.GP4_I2C0_SDA);
      gpio_pull_up(TPicoPin.GP5_I2C0_SCL);
    
      repeat
        uart_puts(uart0,'I2C Bus Scan'+#13#10);
        uart_puts(uart0,'     0  1  2  3  4  5  6  7  8  9  A  B  C  D  E  F'+#13#10);
    
        for addr := 0 to 127 do
        begin
          if (addr mod 16) = 0 then
          begin
            str(addr div 16,tmpStr);
            uart_puts(uart0,'$'+tmpStr+'0 ');
          end;
          ret := i2c_read_blocking(i2c0Inst, addr, rxdata, 1, false);
          if ret < 0 then
            uart_puts(uart0,' . ')
          else
            uart_puts(uart0,' X ');
          if addr mod 16 = 15 then
            uart_puts(uart0,#13#10);
        end;
        uart_puts(uart0,#13#10);
        busy_wait_us_32(2000000);
      until 1=0;
    end.
    

What is important for the I2C bus is that it is always terminated to +3.3V. For this reason we enable pull-up's for both of the I2C Pins we use: 
    
    
    gpio_pull_up(TPicoPin.GP4_I2C0_SDA);
    gpio_pull_up(TPicoPin.GP5_I2C0_SCL);
    

Without the pull up's the code does not work! 

## See also

  * [ARM Embedded Raspberry Pi Pico Tutorials](<ARM_Embedded_Tutorial_-_FPC_and_the_Raspberry_Pi_Pico.md> "ARM Embedded Tutorial - FPC and the Raspberry Pi Pico")

---

_Source: [https://wiki.freepascal.org/ARM_Embedded_Tutorial_-_Raspberry_Pi_Pico_Scanning_for_I2C_Devices](https://web.archive.org/web/20250403220134/https://wiki.freepascal.org/ARM_Embedded_Tutorial_-_Raspberry_Pi_Pico_Scanning_for_I2C_Devices)_
