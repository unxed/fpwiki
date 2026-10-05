# DebugLn

**DebugLn** is a [LazLogger](<LazLogger.md> "LazLogger") procedure that can assist debugging. 

In your main program use code like: 
    
    
    program main;
    uses
    {$ifdef DEBUG}
      LazLogger;
    {$end}
    
    ...
    
    end.
    

In units where you need debugln-style debugging: 
    
    
    uses
    {$ifdef DEBUG}
      LazLoggerBase;
    {$else}
      LazLoggerDummy;
    {$end}
    
    procedure dosomething();
    var
      s: string;
    begin
    ...
      debugln( s );
    ...
    end;
    

Syntax of `debugln` is comparable to `[writeln](<writeln.md> "writeln")` syntax.

---

_Source: [https://wiki.freepascal.org/DebugLn](https://web.archive.org/web/20240225153810/https://wiki.freepascal.org/DebugLn)_
