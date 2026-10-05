# How to use procedural variables

│ **English (en)** │

Copy the text below and it will demonstrate the use of procedural variables, this is a fully working program. You don't even need to understand how it works. The syntax is pretty simple. 
    
    
    program Test;
    
    {$mode objfpc}{$H+}
    uses
      {$IFDEF UNIX}{$IFDEF UseCThreads}
      cthreads,
      {$ENDIF}{$ENDIF}
      Classes;
    
    // Make the Types the type corresponds to a function signature
    type
      TFuncNoArgsString = function(): String;
      TFuncOneArgsString = function(x: string): string;
    
    // Example functions
    function Hello: String;
    begin
      Result := 'Hello There';
    end;
    
    function Woah(G: String): String;
    begin
      Result := 'Woah ' + G;
    end;
    
    // Overloaded function takes the two types of function
    // pointers created above
    procedure Take(f: TFuncNoArgsString); overload;
    begin
      WriteLn(f());
    end;
    
    procedure Take(f: TFuncOneArgsString); overload;
    begin
      WriteLn(f('there!!!'));
    end;
    
    var
      ptr: Pointer;
      List: TList;
    begin
      // the "@" symbol turns the variable into a pointer.
      // This must be done in order pass a function as a
      // parameter.  This also demonstrates that pascal
      // keeps track of the pointer type so the overloading works!
    
      Take(@Hello);
      Take(@Woah);
    
      // Now put a function in an untyped pointer
      ptr := @Hello;
      // Type the pointer and call it all at the same time
      WriteLn(TFuncNoArgsString(ptr));
      // A TList Example
      List := TList.Create;
      List.Add(@Hello);
      WriteLn(TFuncNoArgsString(List[0]));
      ReadLn;
    end.
    

With `{$modeswitch classicprocvars+}` the [`@`-address-operator](<@.md> "@") is not necessary to refer to methods. Also, if you are using the `@`-address-operator usage of `{$typedaddress on}` is advised in order to prevent programming mistakes.

---

_Source: [https://wiki.freepascal.org/How_to_use_procedural_variables](https://web.archive.org/web/20250324221548/https://wiki.freepascal.org/How_to_use_procedural_variables)_
