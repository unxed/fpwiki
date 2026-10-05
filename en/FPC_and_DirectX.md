# FPC and DirectX

│ **English (en)** │

## Introduction

First of all, DirectX is only suited for Microsoft Windows. 

DirectX programming in Free Pascal is fast. Calling a DirectX function is not different from other languages like C++ (with some small exceptions). 

To use DirectX you will need to know some basic stuff about the language. A valuable resource is Google, and the [MSDN network](<http://msdn.microsoft.com>). As most of the tutorials and examples are in C++, it is recommended to have some basic C++ knowledge, but translating to Pascal is not very difficult: you will get the hang of it in no time. This page is going to explain the basics of how to setup DirectX with Free Pascal, where to obtain the Pascal headers etc. 

## Obtaining DirectX headers for FPC

You will need pascal headers to be able to use DirectX with FPC . You can download them from <http://www.clootie.ru/fpc/index.html>. In the downloadable file you will find the headers, and also the DLL files that need to go with your application. They always go together. 

## Examples

It's convenient to use a program like Lazarus to create DirectX applications, because it needs a window to draw all graphics and programming directly in Win32 is very awkward. In Lazarus you can just connect all your drawing objects to the Form and everything works fine. 

You just place the units you will use in your program in your uses section. Be sure to place the units in your program folder. Like this: 
    
    
    interface
    uses  
      Windows, Classes, SysUtils, LResources, Forms,  Dialogs,  ExtCtrls,  // standard stuff
      Direct3D8, // the DirectX units
      D3DX8,
      Directinput,
      DirectMusic,
      DirectSound;
    

If you have placed the DLLs in your program folder you now can use all DirectX calls in your program.

---

_Source: [https://wiki.freepascal.org/FPC_and_DirectX](https://web.archive.org/web/20230331061350/https://wiki.freepascal.org/FPC_and_DirectX)_
