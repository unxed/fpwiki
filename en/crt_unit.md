# Crt

│ **English (en)** │

CRT is a unit providing subroutines for programming in text mode. It is similar to ncurses C library. It's intention is to be compatible with the Borland Pascal / Turbo Pascal 7 CRT unit. 

  


## Contents

  * 1 Procedures and Functions
    * 1.1 Keyboard:
    * 1.2 Display:
    * 1.3 Cursor Control:
    * 1.4 Sound:
    * 1.5 Delay: (Typically used with sound)
    * 1.6 File:
  * 2 Variables and Constants
    * 2.1 Color Constants:



## Procedures and Functions

### Keyboard:
    
    
    Function KeyPressed: Boolean;          
    Function ReadKey: Char;
    

### Display:
    
    
    Procedure TextMode (Mode: word);        
    Procedure Window(X1,Y1,X2,Y2: Byte);    
    Procedure Window32(X1,Y1,X2,Y2: DWord); 
    Procedure ClrScr;                       
    Procedure ClrEol;                       
    Procedure InsLine;                      
    Procedure DelLine;                      
    Procedure TextColor(Color: Byte);       
    Procedure TextBackground(Color: Byte);  
    Procedure LowVideo;                     
    Procedure HighVideo;                    
    Procedure NormVideo;
    

### Cursor Control:
    
    
    Procedure cursoron;                     
    Procedure cursoroff;                    
    Procedure cursorbig;                    
    Procedure GotoXY(X,Y: tcrtcoord);       
    Procedure GotoXY32(X,Y: DWord);         
    Function WhereX: tcrtcoord;            
    Function WhereY: tcrtcoord;            
    Function WhereX32: DWord;              
    Function WhereY32: DWord;
    

### Sound:
    
    
    Procedure Sound(Hz: Word);              
    Procedure NoSound;
    

### Delay: (Typically used with sound)
    
    
    Procedure Delay(MS: Word);
    

### File:
    
    
    Procedure AssignCrt(var F: Text);
    

## Variables and Constants

### Color Constants:
    
    
    const
      Black        =   0;
      Blue         =   1;
      Green        =   2;
      Cyan         =   3;
      Red          =   4;
      Magenta      =   5;
      Brown        =   6;
      LightGray    =   7;
      DarkGray     =   8;
      LightBlue    =   9;
      LightGreen   =  10;
      LightCyan    =  11;
      LightRed     =  12;
      LightMagenta =  13;
      Yellow       =  14;
      White        =  15;
      Blink        = 128;

---

_Source: [https://wiki.freepascal.org/crt_unit](https://web.archive.org/web/20250425031620/https://wiki.freepascal.org/crt_unit)_
