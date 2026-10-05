# ARM Embedded Tutorial - Simple GPIO on and off output

│ **English (en)** │  **[русский (ru)](<../ru/ARM_Embedded_Tutorial_-_Simple_GPIO_on_and_off_output.md>)** │

## Contents

  * 1 Easy GPIO input and output
    * 1.1 Port access registers
    * 1.2 Switch on GPIO
    * 1.3 Direct port manipulation
      * 1.3.1 **CRx** \- Set pin function
      * 1.3.2 **IDR** \- Read out entire port
      * 1.3.3 **ODR** \- Write entire port
      * 1.3.4 **BSRR** \- Single pin to LOW
      * 1.3.5 **BSRR** \- Change the entire port using the bit mask
      * 1.3.6 **BRR** \- Single pin to HIGH
    * 1.4 See also



# Easy GPIO input and output

A port consists of **16 pins** , which is why the lower **16 bits** are used in most registers. 

Exception: **CRx** and **BSRR** , with **BSRR** you can use the upper 16 bits to set pins. 

## Port access registers

The following registers are available for GPIO access: 

  * **CRL** \- Configuration Register Low
  * **CRH** \- Configuration Register High
  * **IDR** \- Input Data Register
  * **ODR** \- Output Data Register
  * **BSRR** \- Bit Set Reset Register
  * **BRR** \- Bit Reset Register
  * **LCKR** \- Port Configuration Lock Register



With all registers except **CRL** and **CRH** , each bit corresponds to a microcontroller pin. 

## Switch on GPIO

Before you can access the GPIO, you have to switch it on. Without doing this, no access is possible! This is done as follows for the PortA - PortC. 
    
    
      RCC.APB2ENR := RCC.APB2ENR or (% 111 shl 2);
    

## Direct port manipulation

Before you can switch a GPIO pin to HIGH, you have to configure it as an output. This is done via the **CRL** and **CRH** registers. 

I usually choose **Pin 13 of Port C** for these mini-examples, because the LED of the STM32F103C is connected to this pin. 

Attention: The LED lights up as the anode is connected to the power supply. 

### **CRx** \- Set pin function

One nibble (4 bit) is available for the function setting. Pin 13 of Port C on output. Since **13** is higher than **7** , the HIGH register CRH is used for this. **13 - 8 = 5;** It is the fifth nibble in **CHR**. Pin0 - Pin7 would be configured with **CHL**. 
    
    
      PortC.CRH := $00300000;
    

### **IDR** \- Read out entire port
    
    
      // Is Pin 13 HIGH?
      Result := PortC.ODR and (1 shl 13) > 0;
    

### **ODR** \- Write entire port

With this function, the whole port is written at once. 
    
    
      // Only pin13 is set to HIGH.
      PortC.ODR := 1 shl 13; 
    
      // All pin on LOW.
      PortC.ODR := 0;
    

### **BSRR** \- Single pin to LOW

With this function, you can set individual pins to **LOW**. 
    
    
      // Only set pin13 to LOW, the others remain unchanged.
      PortC.BSRR := 1 shl 13;
    

### **BSRR** \- Change the entire port using the bit mask

With this function, you can set individual pins to **LOW**. 
    
    
      // Only set Pin13 to LOW, PIN12 to HIGH, the others remain unchanged.
      PortC.BSRR := (1 shl 13) + ((1 shl 12) shl 16);
    

### **BRR** \- Single pin to HIGH

With this function, individual pins can be set to **HIGH**. 
    
    
      // Only set pin13 to HIGH, the others remain unchanged.
      PortC.BSRR := 1 shl 13;
    

The upper 16 bits of BSRR correspond to the 16 bits of **BRR**. 

## See also

  * [ARM Embedded Tutorials](<ARM_Embedded_Tutorials.md> "ARM Embedded Tutorials") Overview

---

_Source: [https://wiki.freepascal.org/ARM_Embedded_Tutorial_-_Simple_GPIO_on_and_off_output](https://web.archive.org/web/20250403023636/https://wiki.freepascal.org/ARM_Embedded_Tutorial_-_Simple_GPIO_on_and_off_output)_
