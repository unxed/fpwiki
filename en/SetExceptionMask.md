# SetExceptionMask

Q: I'm trying to "merge" an existing MS Visual C++ project with Lazarus by converting it to a DLL and then calling some of its entry points from the Lazarus app. Everything is OK until some floating-point operation used and I get the exception "0xC00002B5 Multiple floating point traps". 

A (forum member _PascalDragon_): C code doesn't usually expect the floating point exceptions that FPC enables. Use [**SetExceptionMask**](<https://www.freepascal.org/docs-html/rtl/math/setexceptionmask.html>) to disable the corresponding exceptions (especially overflow and underflow). 

**Set8087CW** procedure only sets the control word for the x87 FPU, but in case of x86_64 Windows only SSE is used, thus the corresponding SSE exceptions wouldn't be disabled. And on ARM64 it doesn't exist at all. So the correct solution is SetExceptionMask (which handles both the x87 and SSE) and not Set8087CW directly. 

Example from GTK2 Lazarus widgetset: 
    
    
    uses Math;
    //...
    begin
      SetExceptionMask(GetExceptionMask + [exOverflow,exZeroDivide,exInvalidOp]);
    

When creating a cross-platform LCL application, it is important to be aware of different ExceptionMask settings with gtk2 (and gtk3 perhaps?) -- more details here: [Multiplatform_Programming_Guide#Gtk2_and_masking_FPU_exceptions](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide").

---

_Source: [https://wiki.freepascal.org/SetExceptionMask](https://web.archive.org/web/20250323134816/https://wiki.freepascal.org/SetExceptionMask)_
