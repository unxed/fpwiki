# Using Pascal Libraries with .NET and Mono

**English (en)** | [**italiano (it)**](</Using_Pascal_Libraries_with_.NET_and_Mono/it> "Using Pascal Libraries with .NET and Mono/it") | [**日本語 (ja)**](</Using_Pascal_Libraries_with_.NET_and_Mono/ja> "Using Pascal Libraries with .NET and Mono/ja")   
****

## Contents

  * 1 Introduction
  * 2 A simple Pascal library
  * 3 A simple VB.NET app
  * 4 A simple C# app
  * 5 A simple Pascal app using Oxygene
  * 6 Running the simple app
  * 7 When to use native libraries with .NET/Mono
  * 8 Mono tips



## Introduction

With the .NET framework, Microsoft provided excellent compatibility for "unmanaged" code. While unmanaged code sounds like something wild and dangerous, it just means so-called "legacy" code in the form of native Windows libraries. The [Mono](<http://www.mono-project.com/Main_Page>) project included similar support for native libraries on Linux and macOS. What this means is that you can compile libraries with Free Pascal and use them with .NET apps on Windows and with Mono apps on Linux and macOS. 

## A simple Pascal library

Copy and save this Pascal code to file SimpleLib.pas: 
    
    
    library SimpleLib;
    
    function MySucc(AVal : Int64) : Int64; stdcall;
    begin
      Result := System.Succ(AVal);
    end;
    
    function MyPred(AVal : Int64) : Int64; stdcall;
    begin
      Result := System.Pred(AVal);
    end;
    
    exports
      MySucc,
      MyPred;
    
    end.
    

Now compile the library with Free Pascal: 
    
    
    fpc -Sd SimpleLib.pas
    

On Windows, this will create file simplelib.dll. On macOS, this will create file libsimplelib.dylib. On Linux, this will create file simplelib.so. On macOS and Linux, rename the compiled library file to simplelib.dll: 
    
    
    mv libsimplelib.dylib simplelib.dll
    

## A simple VB.NET app

Copy and save this VB.NET code to file TestLib.vb: 
    
    
    Imports System
    Imports System.Runtime.InteropServices
    
    Public Class TestLib
    
      Const SimpLibName = "simplelib.dll"
    
       'Declare external functions using the DllImport attribute.
      <DllImport(SimpLibName, EntryPoint:="MySucc", _
                 CallingConvention:=CallingConvention.StdCall)> _
                 Public Shared Function Succ(ByVal AVal As Long) As Long
        End Function
    
      <DllImport(SimpLibName, EntryPoint:="MyPred", _
                 CallingConvention:=CallingConvention.StdCall)> _
                 Public Shared Function Pred(ByVal AVal As Long) As Long
        End Function
    
      Public Shared Sub Main()
      
        Dim TestVal As Long
    
        Try
          TestVal = 123
          Console.WriteLine("Value is " & TestVal)
          Console.WriteLine("Successor is " & Succ(TestVal))
          Console.WriteLine("Predecessor is " & Pred(TestVal))
        Catch e As Exception
          Console.WriteLine(e)
        End Try
    
      End Sub  'Main
    
    End Class  'TestLib
    

If you have the .NET framework installed on Windows, you can compile this code as follows: 
    
    
     [pathto]vbc TestLib.vb

where [pathto] is the path to the .NET framework (example: c:\windows\microsoft.net\framework\v1.1.4322\ with .NET 1.1). This will create file TestLib.exe. 

If you have Mono installed, you can also try compiling this code as follows: 
    
    
     vbnc TestLib.vb

You can also try compiling the C# version: 

## A simple C# app

Here's the equivalent C# code. Copy and save it to file TestLib.cs. 
    
    
    using System;
    using System.Runtime.InteropServices;
    
    public class TestLib {
      
      const string SimpLibName = "simplelib.dll";
    
       //Declare external functions using the DllImport attribute.
      [DllImport(SimpLibName, EntryPoint="MySucc",
                 CallingConvention=CallingConvention.StdCall)]
                 public static extern long Succ(long AVal);
    
      [DllImport(SimpLibName, EntryPoint="MyPred", 
                 CallingConvention=CallingConvention.StdCall)] 
                 public static extern long Pred(long AVal);
    
      public static void Main()
      {
        long TestVal;
    
        try
        {
          TestVal = 123;
          Console.WriteLine("Value is " + TestVal);
          Console.WriteLine("Successor is " + Succ(TestVal));
          Console.WriteLine("Predecessor is " + Pred(TestVal));
        }
        catch(Exception e)
        {
          Console.WriteLine(e);
        }
      }
    }
    

With .NET, you can compile it as follows: 
    
    
    [pathto]csc TestLib.cs

With Mono, you can compile it as follows: 
    
    
    mcs TestLib.cs

With both .NET and Mono, compiling will create file TestLib.exe. 

## A simple Pascal app using Oxygene

Copy and save this Pascal code to file TestLib.pas: 
    
    
    namespace TestLib;
    
    interface
    
    uses
      System.Runtime.InteropServices;
      
    const
      SimpleLibName = 'simplelib.dll';
    
    type
      TestLib = class
      private
        [DllImportAttribute(SimpleLibName, EntryPoint:='MySucc',
                            CallingConvention:=CallingConvention.StdCall)]
        class method Succ(AVal : Int64) : Int64; external;  
    
        [DllImportAttribute(SimpleLibName, EntryPoint:='MyPred',
                            CallingConvention:=CallingConvention.StdCall)]
        class method Pred(AVal : Int64) : Int64; external;  
      public
        class method Main;
      end;
        
    implementation
    
    class method TestLib.Main;
    var
      TestVal : Int64;
    begin
      try
        TestVal := 123;
        Console.WriteLine('Value is ' + TestVal);
        Console.WriteLine('Successor is ' + Succ(TestVal));
        Console.WriteLine('Predecessor is ' + Pred(TestVal));
      except
        on E: Exception do
          Console.WriteLine(E.Message);
      end;
    end;
    
    end.
    

The [Oxygene](<Oxygene.md> "Oxygene") Object Pascal compiler is available from [RemObjects Software](<http://www.remobjects.com>). 

With both .NET and Mono, you can compile TestLib.pas as follows: 
    
    
    [pathto]oxygene TestLib.pas

With both .NET and Mono, this will create file testlib.exe. 

## Running the simple app

With .NET, just run the compiled app as follows: 
    
    
    testlib

If .NET can find simplelib.dll, the app will output this to the console: 
    
    
    Value is 123
    Successor is 124
    Predecessor is 122
    

With Mono, run the compiled app as follows: 
    
    
     mono TestLib.exe

Mono will also look for file simplelib.dll -- that's the reason why we renamed the compiled library above on Linux and macOS. 

Note that .NET/Mono ".exe" files don't contain actual native compiled code. Instead, they contain bytecode that is executed by the .NET/Mono runtime. As a result, you can take a .NET .exe file created on Windows and run it with Mono on macOS and Linux (and vice versa) without recompiling it. You _will_ have to recompile your Pascal libraries for each platform you want to support. 

## When to use native libraries with .NET/Mono

Why not just code everything in VB.NET or C#? There are several situations where this might not be a good idea: 

  * When you have a large or complicated Pascal codebase that you don't want to rewrite (and re-debug) in C#.
  * When you have native libraries that need to be used by both native and .NET/Mono apps.
  * When there are performance reasons (presumably compiled FPC or Delphi native code is faster than the equivalent compiled bytecode run under .NET/Mono).
  * When you need low-level operating system access not available easily with .NET/Mono.
  * When you need to quickly develop code in a language you're more proficient in for a multi-developer .NET/Mono project.



## Mono tips

**1.** Since Visual Basic is pretty foreign to Linux and macOS, you'll probably want to develop .NET and Mono apps using C#, which is similar to C++ (and Object Pascal). Or use the Oxygene compiler and develop your .NET and Mono apps with familiar Object Pascal. 

**2.** Mono on macOS includes a script for uninstalling it. You can run this script before installing a newer version of Mono: 

  * In Finder, navigate to /Library/Receipts.
  * Ctrl+click on MonoFramework.pkg and choose Show Package Contents from the popup menu.
  * Double-click the Contents folder.
  * Double-click the Resources folder.
  * Drag a copy of uninstallMono.sh to the desktop.
  * Open a Terminal window.
  * cd desktop
  * sudo ./uninstallMono.sh and enter your password.



**3**. In general, don't use any Lazarus units in your libraries. Libraries usually don't have a user interface. Confine your use to Free Pascal units like System, SysUtils, StrUtils, DateUtils, Classes, Variants and Math. 

**4.** If you need a user interface to go with your library, program it as a Windows Forms application. Here's a simple form example. Copy and save this code to file MyForm.cs. 
    
    
    using System;
    using System.Windows.Forms;
    
    public class MyForm: Form
    {
      public MyForm ()
      {
        this.Text = "A .NET/Mono Windows Forms App";
        this.Width = 400;
    
        Button b = new Button();
        b.Left = 15;
        b.Top = 20;
        b.Width = this.Width-30;
        b.Height = 30;
        b.Text = "Click Me";
        b.Click += new EventHandler(button_Click);
        this.Controls.Add(b);
      }
      void button_Click(object sender, EventArgs e)
      {
        MessageBox.Show("Hello");
      }
    }
    
    public class MyApp
    {
      public static void Main()
      {
        MyForm aForm = new MyForm();
        Application.Run(aForm);
      }
    }
    

Compile this code on Mono as follows: 
    
    
    mcs -r:System.Windows.Forms MyForm.cs

Run the app as follows: 
    
    
    mono MyForm.exe

On macOS, you can also create a normal app bundle using the macpack utility: 
    
    
    macpack -n MyForm -m winforms MyForm.exe

This creates the MyForm.app bundle, which can be double-clicked, dropped onto the Dock, etc. 

**5.** On Linux and macOS, Mono now includes MonoDevelop, a simple IDE for editing and compiling code. 

**6.** If you have any questions or comments about using Pascal libraries with .NET or Mono, please post them to the Lazarus forum.

---

_Source: [https://wiki.freepascal.org/Using_Pascal_Libraries_with_.NET_and_Mono](https://web.archive.org/web/20250324162537/https://wiki.freepascal.org/Using_Pascal_Libraries_with_.NET_and_Mono)_
