# Python4Delphi

│ **English (en)** │  **[русский (ru)](<../ru/Python4Delphi.md> "Python4Delphi/ru")** │ 

## Contents

  * 1 Overview
  * 2 Lazarus port
  * 3 See also
  * 4 FreeBSD



# Overview

Current home: <https://github.com/pyscripter/python4delphi>

From that page: 

> Python for Delphi (P4D) is a set of free components that wrap up the Python dll into Delphi and Lazarus (FPC). They let you easily execute Python scripts, create new Python modules and new Python types. You can create Python extensions as dlls and much more. P4D provides different levels of functionality: 
> 
>   * Low-level access to the python API
>   * High-level bi-directional interaction with Python
>   * Access to Python objects using Delphi custom variants (VarPyth.pas)
>   * Wrapping of Delphi objects for use in python scripts using RTTI (WrapDelphi.pas)
> 

> 
> P4D makes it very easy to use python as a scripting language for Delphi applications. It comes with an extensive range of demos and tutorials. 

The [commit log](<https://github.com/pyscripter/python4delphi/commits/master/>) indicates development is active as of Sep 2024. Lazarus lpk files and demos exist. 

# Lazarus port

Lazarus port: [Using Python in Lazarus on Windows/Linux](<Using_Python_in_Lazarus_on_Windows/Linux.md> "Using Python in Lazarus on Windows/Linux")

# See also

  * Old wiki (~2006) with lots of examples at [py4d.pbworks.com](<http://py4d.pbworks.com/w/page/9174525/FrontPage>)
  * [Yahoo group](<https://groups.yahoo.com/neo/groups/pythonfordelphi/info>)
  * Notes from a [Python for Delphi talk](<http://www.atug.com/andypatterns/pythonDelphiTalk.htm>)



# FreeBSD

I was able to get this working on FreeBSD without relying on Lazarus design-time components: 
    
    
    program simplefpcdemo;
    uses PythonEngine, dynlibs;
    
    var eng : TPythonEngine;
    begin
      eng := TPythonEngine.Create(Nil);
      eng.LoadDll;
      if eng.IsHandleValid then
        begin
          WriteLn(' evens: ', eng.EvalStringAsStr('[x*2 for x in range(10)]'));
          eng.ExecString('print "powers:", [x**2 for x in range(10)]');
        end
      else writeln('invalid library handle!', dynlibs.GetLoadErrorStr);
    end.
    

The output: 
    
    
        evens: [0, 2, 4, 6, 8, 10, 12, 14, 16, 18]
       powers: [0, 1, 4, 9, 16, 25, 36, 49, 64, 81]
    

My personal working copy (including a Python wrapper for my terminal library) is <https://github.com/tangentstorm/py4d> .

---

_Source: [https://wiki.freepascal.org/Python4Delphi](https://web.archive.org/web/20241217170648/https://wiki.freepascal.org/Python4Delphi)_
