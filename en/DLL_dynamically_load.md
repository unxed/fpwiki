# DLL dynamically load

[![Windows logo - 2012.svg](https://upload.wikimedia.org/wikipedia/commons/thumb/5/5f/Windows_logo_-_2012.svg/50px-Windows_logo_-_2012.svg.png)](</File:Windows_logo_-_2012.svg>)

This article applies to [Windows](</Category:Windows> "Category:Windows") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

│ **[Deutsch (de)](</DLL_dynamically_load/de> "DLL dynamically load/de")** │  **English (en)** │  **[русский (ru)](<../ru/DLL_dynamically_load.md> "DLL dynamically load/ru")** │  **[中文（中国大陆）‎ (zh_CN)](</DLL_dynamically_load/zh_CN> "DLL dynamically load/zh CN")** │    
****

The tutorial shows how a DLL (Dynamic Link Library) is loaded dynamically . 

The DLL (DLLTest.dll) referred to in the example below: 
    
    
     library info;
    
     {$mode objfpc} {$H+}
    
     uses
       SysUtils;
    
     {$R *.res}
    
     // The DLL subroutine
     function funStringBack(strIn : string) : PChar;
       begin
         funStringBack := PChar(UpperCase(strIn));
       end ;
    
    
     // Exported subroutine(s)
     exports
       funStringBack;
    
     begin
     end.
    

What should I do: 

  * Define memory 
    * A data type must be created that corresponds exactly to the (external) subroutine that is to be imported from the DLL.
  * Reserve memory 
    * Memory must be reserved for a variable (data field) to which the above data type is assigned.
    * Memory must be reserved for a handle, to which the DLL handle will be assigned later.
  * Assign DLL and external subroutine and accept the data 
    * Call the DLL and assign the handle of the DLL to the handle.
    * The pointer for the variables must be changed to the memory of the external subroutine.
    * The result of the external subroutine is to be accepted.
  * Free all memory 
    * The pointer for the variables must point to an invalid memory area again (:= nil) to release the external subroutine.
    * The memory of the DLL must be released again.



Integrate, use and release the DLL subroutine in your own program: 
    
    
     uses
       Windows, ...;
    
       ...
    
     Include function funDll : string;
     type
       // Definition of the subroutine to be called, as defined in the DLL to be used
       TfunStringBack = function(strIn : string) : PChar;  stdcall;
    
     var
       // Creates a suitable variable (data field) for the DLL subroutine
       funStringBack : TfunStringBack;
       // Creates a handle for the DLL
       LibHandle : THandle;
    
     begin
       // Get the handle of the library to be used
       LibHandle := LoadLibrary(PChar('DLLTest.dll'));
    
       // Checks whether loading the DLL was successful
       if LibHandle <> 0 then
         begin
           // Assigns the address of the subroutine call to the variable funStringBack
           // 'funStringBack' from the DLL DLLTest.dll
           Pointer(funStringBack) := GetProcAddress(LibHandle, 'funStringBack');
    
           // Checks whether a valid address has been returned
           if @funStringBack <> nil then
             Result := funStringBack('hello world');
         end;
    
       // release memory
       funStringBack := nil;
       FreeLibrary(LibHandle);
    
     end;
    
       ...

---

_Source: [https://wiki.freepascal.org/DLL_dynamically_load](https://web.archive.org/web/20230701000000/https://wiki.freepascal.org/DLL_dynamically_load)_
