# AVR Embedded Tutorial - Library

│ **[Deutsch (de)](</AVR_Embedded_Tutorial_-_Library/de> "AVR Embedded Tutorial - Library/de")** │  **English (en)** │ 

# AVR Libraries

## Intrinsics unit

The intrinsics unit contains the following procedures, which you can also do with: 
    
    
      asm 
        ... 
      end;
    

You can use: 

**avr_cli;** | Block interrupts   
---|---  
**avr_sei;** | Unlock interrupts   
**avr_wdr;** | Watchdog reset   
**avr_sleep;** | Pause until a predetermined event   
**avr_nop;** | An empty command   
  
Note: an **avr_cli;** is the same as: 
    
    
      asm 
        cli 
      end;
    

# See also

  * [AVR Embedded Tutorials](<AVR_Embedded_Tutorial.md> "AVR Embedded Tutorial") \- Overview

---

_Source: [https://wiki.freepascal.org/AVR_Embedded_Tutorial_-_Library](https://web.archive.org/web/20250417153615/https://wiki.freepascal.org/AVR_Embedded_Tutorial_-_Library)_
