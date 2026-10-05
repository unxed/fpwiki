# AVR Embedded Tutorial - Simple GPIO on and off output

│ **English (en)** │

## Contents

  * 1 Easy GPIO input and output
    * 1.1 Direct port manipulation
    * 1.2 Blink example
      * 1.2.1 Alternating indicator ATtiny2313
      * 1.2.2 Toggle pin ATtiny2313
    * 1.3 Simplify port access
      * 1.3.1 pinMode
      * 1.3.2 Digital write
      * 1.3.3 Digital switch
      * 1.3.4 Digital read
    * 1.4 Address pin directly
      * 1.4.1 Bitpacked array
      * 1.4.2 Bitpacked record
      * 1.4.3 Set
      * 1.4.4 DDR and PORT
    * 1.5 Arduino pins PD0 and PD1
    * 1.6 See also



# Easy GPIO input and output

## Direct port manipulation

Before you can switch a GPIO pin x to HIGH, you have to configure it as an output. This is done using **DDR** x. 

  * Configure pin as an output:


    
    
      DDRx := DDRx or (1 shl Pinx);
    

  * Set pin to be HIGH:


    
    
      PORTx := PORTx or (1 shl Pinx);
    

  * Set pin to be LOW:


    
    
      PORTx := PORTx and not (1 shl Pinx);
    

  * Switch pin (high to low or low to high):


    
    
      PORTx := PORTx xor (1 shl Pinx);
    

If you leave **DDR** x LOW, a pull-up resistor is enabled using **PORT** x. This is needed to connect a push button to GND. So you can do without an external pull-up resistor. 

The status of the GPIO pin x can be queried using **PIN** x. 
    
    
      if PINx and (1 shl Pinx) > 0 then 
        Pin_is_HIGH;
    

## Blink example

This example shows how to address a GPIO with an ATtiy2313. For this, two LEDs are connected to pins 0 and 1 of Port D. 

### Alternating indicator ATtiny2313
    
    
     program Project1;
     const
       DP0 = 0;      // Pin 0 PortD
       DP1 = 1;      // Pin 1 PortD
       sl = 150000;  // Delay
    
     procedure mysleep(t: int32);  // A simple delay
     var
       i: Int32;
     begin
       for i := 0 to t do
         asm 
           nop
         end;
     end;
    
     begin
       // Configure pin 0 and 1 of Port D as output
       DDRD := DDRD or (1 shl DP0) or (1 shl DP1);
    
       repeat
         // change LED
         PORTD := PORTD or (1 shl DP0);       // Pin 0 on (high)
         PORTD := PORTD and not (1 shl DP1);  // Pin 1 off (low)
         mysleep(sl);                         // Delay
    
         // change LED
         PORTD := PORTD or (1 shl DP1);       // Pin 1 on (high)
         PORTD := PORTD and not (1 shl DP0);  // Pin 0 off (low)
         mysleep(sl);                         // Delay
       until 1 = 2;
     end.
    

### Toggle pin ATtiny2313

If you want to switch the pin, no matter what its original state, you can use an **xor** operator. This also works for an alternating high/low signal. **DP0** is first set to HIGH. 
    
    
      begin
       // Configure Pin 0 and 1 of Port D as output
       DDRD  := DDRD or (1 shl DP0) or (1 shl DP1);
       PORTD := PORTD or (1 shl DP0);     // Pin 0 on (high)
    
       repeat
         // Blink LED
         PORTD := PORTD xor (1 shl DP0);  // Switch pin 0
         PORTD := PORTD xor (1 shl DP1);  // Switch pin 1
         mysleep(sl);                     // Delay
       until 1 = 2;
     end.
    

## Simplify port access

If you access ports and do not always want to write the _and_ , or '_and xor_ operators, you can also use the following procedures/functions. The examples below show this for PORT D. You can easily adjust these for other ports. 

### pinMode

This corresponds roughly to **pinMode(...** from the Arduino. 
    
    
    procedure ModePortD(Pin: byte; Value: Boolean);
      begin
        if Value then 
          begin
            DDRD := DDRD or (1 shl pin);
          end
        else 
          begin
            DDRD := DDRD and not (1 shl pin);
          end;
      end;
    

### Digital write

This corresponds approximately to **digitalWrite(...** from the Arduino. 
    
    
    procedure WritePortD(Pin: byte; Value: Boolean);
      begin
        if Value then 
          begin
            PORTD := PORTD or (1 shl pin);
          end
        else 
          begin
            PORTD := PORTD and not (1 shl pin);
        end;
      end;
    

### Digital switch

Switches the pin, there is no equal function from the Arduino. 
    
    
    procedure WritePortD(Pin: byte);
      begin
        PORTD := PORTD xor (1 shl pin);
      end;
    

### Digital read

This corresponds approximately to **digitalRead(...** from the Arduino. 
    
    
     function ReadPortD(bit: byte): Boolean;
     begin
       Result := PIND and (1 shl bit) <> 0;
     end;
    

## Address pin directly

If you connect an absolute bit-packed array, record or set to the port, you can access each pin directly, without annoying **or _and_** _and not'_ operators. 

The compiled code is exactly the same size as the conventional method with **or** and **and not**. It could hardly be more elegant. 

The following three examples address the first 4 pins from PORT D which control 4 LEDs. 

### Bitpacked array
    
    
     var
       LEDPort: bitpacked array[0..7] of boolean absolute PORTD;
    
     begin
       LEDPort [0] := True;
       LEDPort [1] := True;
       LEDPort [2] := False;
       LEDPort [3] := False;
    

### Bitpacked record
    
    
     var
       LEDPort: bitpacked record
         red, green, yellow, blue: boolean;
       end absolute PORTD;
    
     begin
       LEDPort.red    := True;
       LEDPort.green  := True;
       LEDPort.yellow := False;
       LEDPort.blue   := False;
    

### Set
    
    
     var
       LEDPortS: set of (green, yellow, red, blue) absolute PORTD;
    
     begin
       LEDPortS := [yellow, red];                     // yellow, red on
       LEDPortS := LEDPortS + [green, blue] - [red];  // green, blue on; red off
       LEDPortS := LEDPortS - [green, blue] + [red];  // green, blue off; red on
    

### DDR and PORT

**DDR** x can be solved in a similar way to **PORT** x. It is best to declare the LEDs as a type: 
    
    
     Type
       TLed = bitpacked record
         green, yellow, red, blue: boolean;
     end;
    
     var
       LedPORT: TLed absolute PORTD;
       LedDDR: TLed absolute DDRD;
    
     begin
       LedDDR.green := True;
       LedDDR.red   := True;
    
       repeat
         LedPORT.green := True;
         LedPORT.red   := False;
         ...
    

**PIN** x can be solved in a similar way. 

## Arduino pins PD0 and PD1

This description refers to the Arduino Uno/Nano, other AVRs may also be affected. 

The PORTD **PD0** and **PD1** pins are also used for the UART interface by default and are therefore not usable. If you still want to use these pins, you have to disable their UART functions. 
    
    
      UCSR0B := 0;    // Disable UART functions
    

Note: The LEDs **RX** and **TX** will light in the opposite way because the anode of these LEDs is directly connected to Vcc. 

For more information about the UART, see [UART](<AVR_Embedded_Tutorial_-_UART.md> "AVR Embedded Tutorial - UART") \- serial input and output via UART (COM port). 

## See also

  * [ AVR Embedded Tutorials](<AVR_Embedded_Tutorial.md> "AVR Embedded Tutorial") \- Overview

---

_Source: [https://wiki.freepascal.org/AVR_Embedded_Tutorial_-_Simple_GPIO_on_and_off_output](https://web.archive.org/web/20250419002711/https://wiki.freepascal.org/AVR_Embedded_Tutorial_-_Simple_GPIO_on_and_off_output)_
