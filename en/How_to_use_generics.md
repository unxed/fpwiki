# How to use generics

An example of how to use generics to write a `function gmax()` that takes the maximum of two not-yet-typed [variables](<Variable.md> "Variable"). Note that while the functions here are namespaced by the classname, FPC versions from 3.1.1 onwards also support fully free-standing generic methods. 
    
    
    program UseGenerics;
    
    {$mode objfpc}{$H+}
    
    type
      generic TFakeClass<_GT> = class
        class function gmax(a,b: _GT):_GT;
      end;
    
      TFakeClassInt = specialize TFakeClass<integer>;
      TFakeClassDouble = specialize TFakeClass<double>;
    
      class function TFakeClass.gmax(a,b: _GT):_GT;
      begin
        if a > b then 
          result := a
        else 
          result := b;
      end;
    
    begin
        // show max of two integers
      writeln( 'Integer GMax:', TFakeClassInt.gmax( 23, 56 ) );
        // show max of two doubles
      writeln( 'Double GMax:', TFakeClassDouble.gmax( 23.89, 56.5) );
      readln();
    end.

---

_Source: [https://wiki.freepascal.org/How_to_use_generics](https://web.archive.org/web/20210302082656/https://wiki.freepascal.org/How_to_use_generics)_
