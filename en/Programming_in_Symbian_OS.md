# Programming in Symbian OS

This page is a tutorial about how one can use the Symbian OS API to write software 

## Overview

### Hello World

**Description**

When started, this software will display a 'My Pascal Hello' message on the screen and then quit. The message will remain for about 5 seconds. 

**Notes**

  * User_InfoPrint will return a integer with a error message, but we just ignore it on this hello world
  * We are using the procedural bindings, througth the symbian.pas unit, and not the object-oriented bindings



**Code**

QPasHello.ini 
    
    
    [Main]
    EXENAME=QPasHello.exe
    TARGETTYPE=EXE
    UID2=0x100039CE
    UID3=0xE1000002
    
    [Files]
    source0=QPasHello.pas
    

QPasHello.pas 
    
    
    program qpashello;
    
    {$mode objfpc}{$H+}
    
    uses symbian;
    
    var
      aMsg: array[0..255] of Char;
    begin
      aMsg := 'My Pascal Hello';
      User_InfoPrint(aMsg);
    end.
    

## See Also

  * [SymbianOS](<SymbianOS.md> "SymbianOS")
  * [Symbian OS Internals](<Symbian_OS_Internals.md> "Symbian OS Internals")

---

_Source: [https://wiki.freepascal.org/Programming_in_Symbian_OS](https://web.archive.org/web/20240920204100/https://wiki.freepascal.org/Programming_in_Symbian_OS)_
