# Developing Python Modules with Pascal

│ **English (en)** │  **[русский (ru)](<../ru/Developing_Python_Modules_with_Pascal.md>)** │

## Contents

  * 1 Introduction
  * 2 Minimum Python API
  * 3 A simple module example
  * 4 Using your module in a host application
  * 5 See also



## Introduction

Python is a popular scripting language that is often used to add functionality to other applications such as OpenOffice and Quantum GIS. Your computer may already have a version of Python installed. If not, you can download Python from the official Website: <http://www.python.org/>. 

You can extend Python by developing compiled libraries (called modules) that add functions to Python. This topic discusses how to create a library with Pascal (Delphi or Free Pascal). 

This article describes a very low level, "from scratch" approach. For an established python-pascal bridge, see [Python4Delphi](<Python4Delphi.md> "Python4Delphi"). 

## Minimum Python API

Copy and paste this code into a text editor and save it as file PyAPI.pas: 
    
    
    unit PyAPI;
     
    { 
     
      Minimum set of Python function declarations for module libraries.
     
      Author: Phil (MacPgmr at fastermac.net).
     
      To add other Python function declarations, see the Python header
       files (.h) included with every Python distribution.
     
    }
     
    {$IFDEF FPC}
     {$MODE Delphi}
    {$ENDIF} 
     
    interface
    
    {$DEFINE IS32BIT}
    {$IFDEF CPUX64}  {Delphi}
     {$UNDEF IS32BIT}
    {$ENDIF}
    {$IFDEF CPU64}  {FPC}
     {$UNDEF IS32BIT}
    {$ENDIF}
     
    const
    {$IFDEF MSWINDOWS}
     {$IFDEF USE_PYTHON23}
      PythonLib = 'python23.dll';
     {$ELSE}
      PythonLib = 'python27.dll';
     {$ENDIF}
    {$ENDIF} 
     
    {$IFDEF LINUX}
     {$IFDEF USE_PYTHON23}
      PythonLib = 'python23.so';
     {$ELSE}
      PythonLib = 'python27.so';
     {$ENDIF}
    {$ENDIF} 
     
    {$IFDEF DARWIN}
      PythonLib = '';  //Link against Python.framework (-k'-framework Python').
                       // To link against a specific version of Python, pass the
                       // full path to that version's library instead, for example,
                       //  -k'/System/Library/Frameworks/Python.framework/Versions/2.6/Python'
    {$ENDIF} 
     
    type
    {$IFDEF IS32BIT}
      c_long = LongInt;
      c_ulong = LongWord;
      c_int  = LongInt;
    {$ELSE}
      c_long = Int64;
      c_ulong = UInt64;
      c_int = Int64;  //"int" also appears to be 8 bytes with 64-bit Python
    {$ENDIF}
     
      PyMethodDef = packed record
        name  : PAnsiChar;  //Python function name
        meth  : Pointer;    //Address of function that implements it
        flags : c_int;      //METH_xxx flags; describe function's arguments
        doc   : PAnsiChar;  //Description of funtion
        end;
     
      PyObject = Pointer;
    
    const
    {$IFDEF USE_PYTHON23}
      PYTHON_API_VERSION = 1012;  //Also used with Python 2.4
    {$ELSE}
      PYTHON_API_VERSION = 1013;
    {$ENDIF}
      METH_VARARGS = 1;
     
    function Py_InitModule(    name    : PAnsiChar;
                           var methods : PyMethodDef;
                               doc     : PAnsiChar = nil;
                               self    : PyObject = nil;
                               apiver  : c_int = PYTHON_API_VERSION) : PyObject; cdecl; 
              external PythonLib name {$IFDEF IS32BIT}'Py_InitModule4'{$ELSE}'Py_InitModule4_64'{$ENDIF};
     
    function PyArg_ParseTuple(args   : PyObject; 
                              format : PAnsiChar) : c_int; cdecl; varargs; external PythonLib;
     //Note varargs allows us to simulate C variable number of arguments (...).
     
    function PyInt_FromLong(along : c_long) : PyObject; cdecl; external PythonLib;
     
    function PyLong_FromLong(along : c_long) : PyObject; cdecl; external PythonLib;
     
    function PyLong_FromUnsignedLong(aulong : c_ulong) : PyObject; cdecl; external PythonLib; 
    
    function PyString_FromString(astr : PAnsiChar) : PyObject; cdecl; external PythonLib;
     
    implementation
     
     
    end.
    

If you have a different version of Python, define USE_PYTHON23 to link against version 2.3 or just change the library name (PythonLib). If you need other Python API functions, just add them to PyAPI.pas following the example of PyInt_FromLong. 

## A simple module example

Here is a simple library that uses this Python API unit. Copy and paste this code into a text editor and save it as file PyMinMod.dpr: 
    
    
    library PyMinMod;
     
    {
     
      Minimal Python module (library) that includes simple functions.
     
      Author: Phil (MacPgmr at fastermac.net).
     
      To compile this module:
        - With Delphi: Open this .dpr file and compile.
        - With Lazarus: Open .lpi file and compile.
     
      To deploy module:
        - With Delphi: Rename compiled .dll to .pyd.
        - With Lazarus on Windows: Rename compiled .so to .pyd.
        - With Lazarus on OS X and Linux: .so extension is okay.
     
    }
     
    uses
      SysUtils,
      PyAPI;
     
     
    function SumTwoIntegers(Self : PyObject;
                            Args : PyObject) : PyObject; cdecl;
    var
      Arg1 : Integer;
      Arg2 : Integer;
    begin
      PyArg_ParseTuple(Args, 'ii', @Arg1, @Arg2);  //Get the two int arguments
      Result := PyInt_FromLong(Arg1 + Arg2);  //Add them together and return sum
    //  Result := PyLong_FromLong(Arg1 + Arg2);
    //  Result := PyLong_FromUnsignedLong(Arg1 + Arg2);
    end;
     
     
    function ConcatTwoStrings(Self : PyObject;
                              Args : PyObject) : PyObject; cdecl;
     {From Python documentation for "s" format: "You must not provide storage for 
       the string itself; a pointer to an existing string is stored into the 
       character pointer variable whose address you pass."
      From Python documentation for PyString_FromString: "Return a new string 
       object with a copy of the string v as value on success".}
    var
      Arg1 : PAnsiChar;
      Arg2 : PAnsiChar;
    begin
      PyArg_ParseTuple(Args, 'ss', @Arg1, @Arg2);  //Get the two string arguments
      Result := PyString_FromString(PAnsiChar(AnsiString(Arg1) + AnsiString(Arg2)));  
                 //Concatenate and return string
    end;
     
     
    var
      Methods : packed array [0..2] of PyMethodDef;
     
    procedure initPyMinMod; cdecl;
    begin
      Methods[0].name := 'SumTwoIntegers';
      Methods[0].meth := @SumTwoIntegers;
      Methods[0].flags := METH_VARARGS;
      Methods[0].doc := 'Tests passing ints to and from module function';
     
      Methods[1].name := 'ConcatTwoStrings';
      Methods[1].meth := @ConcatTwoStrings;
      Methods[1].flags := METH_VARARGS;
      Methods[1].doc := 'Tests passing strings to and from module function';
     
      Methods[2].name := nil;
      Methods[2].meth := nil;
      Methods[2].flags := 0;
      Methods[2].doc := nil;
     
      Py_InitModule('PyMinMod', Methods[0]);
    end;
     
     
    exports
      initPyMinMod;
     
    end.
    

You can add more functions to the module by following the example in initPyMinMod. 

With Delphi, just open PyMinMod.dpr and compile. 

With FPC, just compile from the command line. For example, to create a 64-bit module on OS X: 
    
    
    ppcx64 -Sd -k'-framework Python' -oPyMinMod.so PyMinMod.dpr
    

With Lazarus, you'll probably want to create a project file. You can do that yourself or just copy and paste this project file into a text editor and save it as file PyMinMod.lpi: 
    
    
    <?xml version="1.0"?>
    <CONFIG>
      <ProjectOptions>
        <PathDelim Value="/"/>
        <Version Value="6"/>
        <General>
          <MainUnit Value="0"/>
          <IconPath Value="./"/>
          <TargetFileExt Value=".exe"/>
          <UseAppBundle Value="False"/>
          <ActiveEditorIndexAtStart Value="0"/>
        </General>
        <PublishOptions>
          <Version Value="2"/>
          <IgnoreBinaries Value="False"/>
          <IncludeFileFilter Value="*.(pas|pp|inc|lfm|lpr|lrs|lpi|lpk|sh|xml)"/>
          <ExcludeFileFilter Value="*.(bak|ppu|ppw|o|so);*~;backup"/>
        </PublishOptions>
        <RunParams>
          <local>
            <FormatVersion Value="1"/>
            <LaunchingApplication PathPlusParams="/usr/X11R6/bin/xterm -T 'Lazarus Run Output' -e $(LazarusDir)/tools/runwait.sh $(TargetCmdLine)"/>
          </local>
        </RunParams>
        <Units Count="1">
          <Unit0>
            <Filename Value="PyMinMod.dpr"/>
            <IsPartOfProject Value="True"/>
            <UnitName Value="PyMinMod"/>
            <CursorPos X="1" Y="1"/>
            <TopLine Value="1"/>
            <EditorIndex Value="0"/>
            <UsageCount Value="20"/>
            <Loaded Value="True"/>
            <SyntaxHighlighter Value="Delphi"/>
          </Unit0>
        </Units>
        <JumpHistory Count="0" HistoryIndex="-1"/>
      </ProjectOptions>
      <CompilerOptions>
        <Version Value="8"/>
        <Target>
          <Filename Value="PyMinMod.so"/>
        </Target>
        <Parsing>
          <SyntaxOptions>
            <SyntaxMode Value="Delphi"/>
            <CStyleOperator Value="False"/>
            <AllowLabel Value="False"/>
            <CPPInline Value="False"/>
          </SyntaxOptions>
        </Parsing>
        <CodeGeneration>
          <Checks>
            <IOChecks Value="True"/>
            <RangeChecks Value="True"/>
            <OverflowChecks Value="True"/>
            <StackChecks Value="True"/>
          </Checks>
        </CodeGeneration>
        <Linking>
          <Options>
            <PassLinkerOptions Value="True"/>
            <LinkerOptions Value="-framework Python"/>
            <Win32>
              <GraphicApplication Value="True"/>
            </Win32>
            <ExecutableType Value="Library"/>
          </Options>
        </Linking>
        <Other>
          <CompilerPath Value="$(CompPath)"/>
        </Other>
      </CompilerOptions>
      <Debugging>
        <Exceptions Count="2">
          <Item1>
            <Name Value="ECodetoolError"/>
          </Item1>
          <Item2>
            <Name Value="EFOpenError"/>
          </Item2>
        </Exceptions>
      </Debugging>
    </CONFIG>
    

Once you've compiled the module, rename it if necessary per the comments in PyMinMod.dpr. Then test the module by creating a simple test.py file that contains these three lines: 
    
    
    import PyMinMod
    print "Value returned by SumTwoIntegers: " + str(PyMinMod.SumTwoIntegers(1, 2))
    print "Value returned by ConcatTwoStrings: " + PyMinMod.ConcatTwoStrings("Hey ", "there")
    

Note that Python is case sensitive so if your compiled module is in lower-case, change the "PyMinMod" references accordingly. 

Now open a terminal window and run the script like this: 
    
    
    python test.py
    

The script should output the following line: 
    
    
    Value returned by SumTwoIntegers: 3
    Value returned by ConcatTwoStrings: Hey there
    

What we've done here is create a simple Python module that implements two Python functions. Once you've imported the module into your Python script, you can use the functions in the same way that you use built-in Python functions. 

## Using your module in a host application

If you have OpenOffice or NeoOffice installed, you can test running your module from a Python macro. 

Copy and paste this script into a text editor and save it as file test_minmod.py, then place it in the folder specified in the script. You may have to create the python\Library1 folder under Scripts. 
    
    
    # Python macro that tests Pascal module by creating new document and inserting module function result.
    
    import sys, os
    # Tell Python where to look for our Pascal module
    if sys.platform == 'win32':
      sys.path.append(os.path.expanduser('~\Application Data\OpenOffice.org2\user\Scripts\python\Library1'))
    elif sys.platform == 'darwin':
      sys.path.append(os.path.expanduser('~/Library/Preferences/NeoOffice-2.2/user/Scripts/python/Library1'))
    
    # Import Pascal module that contains SumTwoIntegers function
    import PyMinMod
    
    import uno
    
    def TestMinMod():
      ctx = uno.getComponentContext()
      smgr = ctx.ServiceManager
      desktop = smgr.createInstance('com.sun.star.frame.Desktop')
      doc = desktop.loadComponentFromURL('private:factory/swriter', '_blank', 0, ())
      textCursor = doc.Text.createTextCursor()
      doc.Text.insertString(textCursor, 'Sum of 1 + 2 = ' + str(PyMinMod.SumTwoIntegers(1, 2)), 0)
    

Since OO probably includes Python 2.3, compile your module against 2.3 by adding this on the Lazarus Compiler Options dialog's Other tab: 
    
    
    -dUSE_PYTHON23
    

With Delphi, enter USE_PYTHON23 on the Project Options dialog's Directories/Conditionals tab. 

Rename your compiled module if necessary, then place it in the same folder as test_minmod.py. Now test running it from OO by choosing Tools | Macros | Organize Macros | Python and running the TestMinMod macro. 

## See also

  * [Python4Delphi](<Python4Delphi.md> "Python4Delphi")

---

_Source: [https://wiki.freepascal.org/Developing_Python_Modules_with_Pascal](https://web.archive.org/web/20250420133117/https://wiki.freepascal.org/Developing_Python_Modules_with_Pascal)_
