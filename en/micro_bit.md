# micro:bit

The micro:bit is a small embedded system equipped with a Nordic Semiconductor nRF51822 controller based on the Cortex-M0 architecture. With FPC trunk, it is possible to build programs for it. Follow the instructions at [Embedded ARM](<TARGET_Embedded.md>) to setup FPC for the target arm-embedded. However, do not use the provided command line to build and install FPC but use 
    
    
     make clean buildbase installbase CROSSINSTALL=1 OS_TARGET=embedded CPU_TARGET=arm SUBARCH=armv6m
    

instead to build FPC for the controller used by the micro:bit. 

Now enter the following program: 
    
    
      const
        GPIO_PIN_CNF_INPUT_Disconnect = 1;
        GPIO_PIN_CNF_DIR_Output = 1;
        microbit_led_col1 = 4;
        microbit_led_row1 = 13;
      var
        i : longint;
      begin
        GPIO.PIN_CNF[microbit_led_col1]:=(GPIO_PIN_CNF_INPUT_Disconnect shl 1) or GPIO_PIN_CNF_DIR_Output;
        GPIO.PIN_CNF[microbit_led_row1]:=(GPIO_PIN_CNF_INPUT_Disconnect shl 1) or GPIO_PIN_CNF_DIR_Output;
        while true do
          begin
            GPIO.OUTSET:=1 shl microbit_led_row1;
            GPIO.OUTCLR:=1 shl microbit_led_col1;
            for i:=1 to 500000 do
              asm
                nop
              end;
            GPIO.OUTCLR:=1 shl microbit_led_row1;
            for i:=1 to 500000 do
              asm
                nop
              end;
          end;
      end.
    

save it as example.pp and compile it with: 
    
    
     fpc -Parm -Tembedded -Wpnrf51822_xxaa example -Cparmv6m
    

When connecting a micro:bit to PC, it identifies itself as an USB drive. Find out where this drive is located and copy the resulting example.hex to this drive. 

If everything worked fine, the upper left LED will blink.

---

_Source: [https://wiki.freepascal.org/micro%3Abit](https://web.archive.org/web/20250222115333/https://wiki.freepascal.org/micro%3Abit)_
