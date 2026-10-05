# maxint

│ **English (en)** │


  
**MAXINT** is a global constant, equal to the upper bound of the [integer](<Integer.md> "Integer") type, [per the ISO 7185 standard](<http://www.moorecad.com/standardpascal/iso7185.html#6.7.2.2%20Arithmetic%20operators>). 

Note that this value changes based on the compiler mode and has nothing to do with the capabilities of the host or target machine. 

If you want to check the size of the address space, check **sizeof(pointer)**. 

If you want to know if you are running on a CPU with 64-bit ALU, check whether the symbol **cpu64** is defined. 

The following example illustrates how these values relate: 
    
    
    program maxvals;
      const width = 20;
    begin
      writeln;
      writeln( 'these change depending on compiler mode:' );
      writeln( '----------------------------------------' );
      writeln( 'maxint:           :', maxint : width );
      writeln( 'high( integer )   :', high( integer ) : width );
      writeln;
      writeln( 'constant, regardless of mode or target: ' );
      writeln( '----------------------------------------' );
      writeln( 'high( int32 )     :', high( int32 ) : width );
      writeln( 'high( int64 )     :', high( int64 ) : width );
      writeln;
      writeln( 'variable, depending on target cpu:' );
      writeln( '----------------------------------------' );
      writeln( 'sizeof( pointer ) :', sizeof( pointer ) : width );
      writeln;
      writeln( 'compile-time definitions:' );
      writeln( '----------------------------------------' );
      {$IFDEF cpu64} writeln( 'cpu64' ); {$ENDIF}
      {$IFDEF cpu32} writeln( 'cpu32' ); {$ENDIF}
      {$IFDEF cpu16} writeln( 'cpu16' ); {$ENDIF}
      writeln;
    end.

Compare the results with **-Mtp** vs **-Mobjfpc** , for example: 
    
    
    fpc -Mtp maxvals.pas && ./maxvals
    fpc -Mobjfpc maxvals.pas && ./maxvals

---

_Source: [https://wiki.freepascal.org/maxint](https://web.archive.org/web/20180309172751/https://wiki.freepascal.org/maxint)_
