# AVR Embedded Tutorial - UART

│ **English (en)** │

## Contents

  * 1 UART polling
    * 1.1 ATmega328p (Arduino Uno/Nano)
      * 1.1.1 Constants
      * 1.1.2 Initialization
      * 1.1.3 Receive characters
      * 1.1.4 Send characters
      * 1.1.5 Send string
      * 1.1.6 Main loop
    * 1.2 ATtiny2313
  * 2 UART interrupt
    * 2.1 Receive buffer
      * 2.1.1 Declaration
      * 2.1.2 Initialization
      * 2.1.3 Interrupt
      * 2.1.4 Main loop
    * 2.2 Send buffer
      * 2.2.1 Declaration
      * 2.2.2 Initializarion
      * 2.2.3 Interrupt
      * 2.2.4 Send string
      * 2.2.5 Main loop
  * 3 UART emulation in software
    * 3.1 Constants
    * 3.2 Set the receive pin to input
    * 3.3 Send characters
    * 3.4 Receive characters
  * 4 See also



# UART polling

## ATmega328p (Arduino Uno/Nano)

A terminal with the following settings is required for serial output: 

Setting | Value   
---|---  
Baud Rate | 9600   
Bits | 8   
Stopbits | 1   
Parity | none   
  
When pressing the `Space Bar` in the terminal program, a "Hello World!" message is returned. 

### Constants
    
    
     const
       CPU_Clock = 16000000;  // Arduino clock frequency, default 16MHz.
       Baud = 9600;           // baud rate
       Divider = CPU_Clock div(16 * baud) - 1;
    

Note: If the CPU clock is too slow for the baud rate, there are transmission errors. Example: With a clock of 1MHz, a baud rate of 9600 does not work. However, a 1MHz clock and 1200 baud will work. 

### Initialization

Configure the UART interface. 
    
    
     procedure UARTInit;
       begin
         UBRR0  := divider;                         // Baud
         UCSR0A := 0;                               // Normal speed
         UCSR0B := (1 shl TXEN0) or (1 shl RXEN0);  // Receive and send
         UCSR0C := (%011 shl UCSZ0);                // 8-bit word, 1 stop bit
       end;
    

### Receive characters
    
    
      function UARTReadChar : char;
       begin
         while UCSR0A and (1 shl RXC0) = 0 do
           ;                                        // Wait for a character to arrive
         Result := char(UDR0);                      // Read character
       end;
    

### Send characters
    
    
     procedure UARTSendChar(c : char);
       begin
         while UCSR0A and (1 shl UDRE0) = 0 do
           ;                                        // Wait for the last character to be sent
         UDR0 := byte(c);                           // Send character
       end;
    

### Send string
    
    
     procedure UARTSendString(s : ShortString);
       var
         i : integer;
       begin
         for i := 1 to Length(s) do 
           begin
             UARTSendChar(s[i]);  // send characters one by one
           end;
       end;
    

### Main loop

Here you wait until the space character arrives, then transmit "Hello World!". 
    
    
    program Project1;
     begin
       UARTInit;
    
       repeat
         if UARTReadChar = #32 then  // #32 = space character
           begin  
              UARTSendString('Hello World!'#13#10); 
           end;
       until 1 = 2;
     end.
    

## ATtiny2313

The registers on the ATtiny2313 have different names than on the ATmega328p. 

  * The 0 is omitted in the name.
  * UBRR0 must be broken down into UBRRH and UBRRL .



It will look like this: 
    
    
     UBRRH := divider shr 8;
     UBRRL := byte(divider);
    
     UCSRB := (1 shl TXEN ) or (1 shl RXEN);
     UCSRC := (%011 shl UCSZ);
    

(The code has not been tested) 

# UART interrupt

With UART polling, a character may be lost. This can happen when a character arrives and you cannot pick it up in time. For this reason it is advisable to have the reception of characters controlled by an interrupt. At higher baud rates, this is mandatory. 

A simple 16-character ring buffer is used. 

The examples show this using an ATmega328p (Arduino Uno/Nano). 

## Receive buffer

### Declaration

Declare a character buffer: here a 16 character buffer is created. 
    
    
     const
       Crxlen = 16;    // Buffer size
     var
       RXBuf   : array[0..Crxlen - 1] of byte;
       RXread  : byte = 0;
       RXwrite : byte = 0;
    

### Initialization

This is similar to polling, the difference is that **RXCIE0** is used to indicate that an interrupt is triggered when a character is received. 
    
    
     procedure UARTInit;
       begin
         UBRR0  := divider;
         UCSR0A := (0 shl U2X0);
         UCSR0B := (1 shl TXEN0) or (1 shl RXEN0) or (1 shl RXCIE0);
         UCSR0C := %011 shl UCSZ0;
       end;
    

### Interrupt

When the interrupt is triggered, the character is copied from the UART into the character buffer. 
    
    
     procedure UART_RX_Receiving; public name 'USART__RX_ISR'; interrupt;
       var
         b : byte;
       begin
         // Read characters
         b := UDR0;
    
         // Write characters to buffer
         if b <> 0 then 
           begin
             RxBuf[Rxwrite] := b;
             Inc(RXwrite);
    
             if RXwrite >= Crxlen then 
               begin
                 RXwrite := 0;
               end;
           end;
       end;
    

### Main loop

The main loop checks whether something is in the receive buffer, and if so, a character is read from it. In this example, the characters received are converted to uppercase letters and then output. 
    
    
     begin
       // Initialize UART which is interrupt controlled
       UARTInit;
    
       // Enable interrupts
       asm
         sei
       end;
    
       // Main loop
       repeat
    
         // Is there a character in the buffer?
         while RXwrite <> Rxread do 
           begin
             // Disable interrupts
             asm 
               cli
             end;
    
             // Read character from buffer
             ch := RXBuf[rxread];
             Inc(RXread);
    
             if RXread >= Crxlen then 
               begin
                 RXread := 0;
               end;
    
             // Enable interrupts
             asm 
               sei
             end;
    
             // Convert lowercase letters, into uppercase ones
             if ch in[$61..$7A] then 
               begin
                 Dec(ch, 32);
               end;
    
             // Output character
             UARTSendChar(char(ch));
           end;
       until 1 = 2;
     end.
    

## Send buffer

### Declaration

Declare a send buffer: here a 16 character buffer is created. 
    
    
     const
       CTXlen = 16;    // Buffer size
     var
       TXBuf   : array[0..CTXlen - 1] of byte;
       TXread  : byte = 0;
       TXwrite : byte = 0;
    

### Initializarion

This is similar to the receive buffer. Important: the interrupt function must not be activated yet, because there is nothing to send yet. 
    
    
     procedure UARTInit;
       begin
         UBRR0  := divider;
         UCSR0A := (0 shl U2X0);
         UCSR0B := (1 shl TXEN0) or (1 shl RXEN0);
         UCSR0C := %011 shl UCSZ0;
       end;
    

### Interrupt

The interrupt is triggered as soon as a character has been sent to the UART. 

As soon as the ring buffer is empty, the interrupt function is deactivated again. 

The query of **UDRE0** is not needed here, since the interrupt is only triggered when the buffer is empty. 
    
    
     procedure UART_UDRE_Send; public name 'USART__UDRE_ISR'; interrupt;
       begin
         // Send character
         UDR0 := TXBuf[TXread];
        
         // Move ring buffer
         Inc(TXread);
    
         // When pointer is at the end, start again
         if TXread >= CTXlen then 
           begin
             TXread :=0;
           end;
    
         // If the ring buffer is empty, an interrupt is no longer necessary
         if TXread = TXwrite then 
           begin
             UCSR0B := UCSR0B and not (1 shl UDRIE0);
           end;
       end;
    

### Send string

As soon as something is in the send buffer, the interrupt is not activated. 
    
    
     procedure UARTSendString(s : ShortString);
       var
         i : byte;
       begin
         // Fill the ring buffer with data
         for i := 1 to Length(s) do 
           begin
             TXBuf[TXwrite] := byte(s[i]);
             Inc(TXwrite);
             
             if TXwrite >= CTXlen then 
               begin
                 TXwrite := 0;
               end;
         end;
    
         // Enable send interrupt
         UCSR0B := UCSR0B or (1 shl UDRIE0);
       end;
    

### Main loop

The string is output in the background. In the example, an LED flashes during output. 
    
    
     var
       z : byte;
       s : ShortString;
     begin
       DDRB := DDRB or (1 shl 5);
    
       // Initialize UART that is interrupt controlled
       UARTInit;
    
       // Enable interrupts
       asm 
         sei
       end;
    
       // Main loop
       repeat
         UARTSendString('Hello World');
         UARTSendString('!'#13#10);
    
         PORTB := PORTB or (1 shl 5);
         mysleep(sl);
    
         Str(z : 3, s);
         UARTSendString(s + '.');
         Inc(z);
    
         PORTB := PORTB and not (1 shl 5);
         mysleep(sl);
    
       until 1 = 2;
     end.
    

# UART emulation in software

It is also possible to emulate a UART interface using software. 

This is practical if you have an AVR which has no/too few hardware UARTs. 

Note: This code only works with a 16MHz CPU clock and 9600 baud. Simply adjusting baud and CPU_Clock is not enough because the formula still has an error. 

Important: Interrupts must be blocked during the transmission. 

Reading and writing at the same time does not work because there is no buffer. 

Ideally, this can be used to output errors to a UART terminal. 

## Constants

In this example, Pins 2 and 3 from Port D are used. 
    
    
     const
       CPU_Clock = 16000000; // Arduino clock frequency, default 16MHz.
       Baud = 9600;          // Baud rate
    
       TXpin = (1 shl 3);    // Pin 3 (receive)
       RXpin = (1 shl 2);    // Pin 2 (send)
    
       softDivider = CPU_Clock div(Baud * 22);  // bit counter
    

## Set the receive pin to input
    
    
     begin
       DDRD := DDRD or TXpin;
       ...
       // loop
       ...
     end.
    

Note: If you want to use the same pins as for the hardware UART, pins 0 and 1 must be enabled on the Arduino. 
    
    
     begin
       UCSR0B := 0;  // Release blocked pins 0 and 1
       ...
       // loop
       ...
     end.
    

## Send characters
    
    
     procedure UARTSoftSendByte(c : byte);
       var
         i, data : int16;
         j : Int8;
       begin
         asm 
           cli  // Disable interrupts
         end;  
    
         Data := (c shl 1) or $FE00;
     
         for j := 0 to 9 do  // Output bits, including stop bit
           begin
             if(Data and 1) = 1 then 
               PORTD := PORTD or TXpin       // HIGH
             else 
               PORTD := PORTD and not TXpin; // LOW
           
             Data := Data shr 1;
    
             for i := 0 to softDivider do    // One bit break
               ;  
           end;
    
         asm 
           sei
         end;  // Enable interrupts
       end;
    

## Receive characters

Note: If no character is received, the loop hangs forever. 
    
    
      function UARTSoftReadByte : byte;
       var
         i : Int16;
         j : Int8;
       begin
         asm 
           cli   // Disable interrupts
         end;
    
         Result := 0;
    
         while PIND and RXpin > 0 do  // Wait until the first falling edge
           ;  
    
         for i := 0 to softDivider div 2 do  // Half a bit break
           ;
    
         for j : = 0 to 7 do 
           begin
             for i := 0 to softDivider do    // One bit break
               ;
           
             Result := Result shr 1;
    
             if PIND and RXpin <> 0 then 
               Inc(Result, $80);
           end;
    
         for i := 0 to softDivider           // One bit break
           ;
    
         asm 
           sei   // Enable interrupts
         end;
       end;
    

# See also

  * [AVR Embedded Tutorials](<AVR_Embedded_Tutorial.md> "AVR Embedded Tutorial") \- Overview

---

_Source: [https://wiki.freepascal.org/AVR_Embedded_Tutorial_-_UART](https://web.archive.org/web/20250323152420/https://wiki.freepascal.org/AVR_Embedded_Tutorial_-_UART)_
