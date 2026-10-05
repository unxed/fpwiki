# Windows API examples

[![Windows logo - 2012.svg](https://upload.wikimedia.org/wikipedia/commons/thumb/5/5f/Windows_logo_-_2012.svg/50px-Windows_logo_-_2012.svg.png)](</File:Windows_logo_-_2012.svg>)

This article applies to [Windows](</Category:Windows> "Category:Windows") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

This is a collection of Lazarus forum posts from user **440bx** , about Windows API examples. Source codes were [collected in GitHub](<https://github.com/Alexey-T/Lazarus-WinAPI-examples>). 

## Contents

  * 1 Post 1
  * 2 Post 2
  * 3 Post 3
  * 4 Post 4
  * 5 Post 5
  * 6 Post 6
  * 7 Post 7
  * 8 Post 8
  * 9 Post 9



## Post 1

Attached to this post are six (6) "teaser" API programs that give an idea of what these programs/examples are about. In addition, there is a "Windows Bitness" program that is more an illustration of how to get the Windows bitness than how to use a specific API. 

The teaser examples do _not_ include source at this time. I will include the source when the time comes to give an example of the API they demonstrate. The "Windows Bitness" includes full source to give an idea of what the source code for a typical example looks like. 

I will first give a quick description of the purpose of the "teaser" programs before discussing details of the "Windows Bitness" program. The teaser program/examples are: 

**ExpandEnvironmentStrings**

This is an example of the many trivial API functions in Windows. These are APIs that don't have much to say about them. IOW, you read its documentation in MSDN, code the call and you're done. 

Roughly (iow, an estimate) 50% of the examples fall in that category. Their main reason to exist was to test their performance and/or behavior in unusual cases. (those were my personal reasons.) 

  
**SetConsoleWindowInfo**

The description of SetConsoleWindowInfo in MSDN isn't particularly enlightening and the MSDN example's purpose seems to be to confuse the reader (just in case you understood the questionable description that preceded it.) 

The teaser example is a visual representation of how that API works. It shows the parameters before the call, the parameters after the call and the effects of the call. 

Roughly, 10% of the examples fall in that category (API operation visualization). 

I won't go into a detailed explanation at this time. It will be included along with the program source when it is its turn. 

  
**ExtTextOut** (two (2) examples) 

**InvalidateRect**

**SelectClipRgn**

These examples, in addition to demonstrating what the API does and how it works are related by one common goal which is, reducing or eliminating _flicker_ when redrawing. 

The InvalidateRect example flickers when the mouse coordinates are updated in the area dedicated to them. Selecting "Invalidate entire area" from the InvalidateRect menu shows how the size of the invalid area makes the flicker more noticeable. 

  
**ExtTextOut** (v1) 

That example is almost identical to the InvalidateRect example but, it demonstrates how to use ExtTextOut to significantly reduce flicker. In the case of the example, to a level that is almost imperceptible. 

  
**ExtTextOut** (v0) 

This example also demonstrates how to significantly reduce flicker but in a larger area. It also demonstrates the clipping abilities of ExtTextOut. The example also shows how to slow a GUI program down by using a console and, how to use the console to help in debugging. 

  
**SelectClipRgn**

This example shows one the ways available to totally eliminate flicker (zero flicker without using double buffering.) Resize the window horizontally and/or vertically. Control flicker/no flicker with the menu options under "Flicker". 

  
**Window Bitness**

Consider a program, let's call it program "A" that can be compiled as a 32 bit and 64 bit versions. 

The 64 bit version of the program has it easy. All it has to do is determine that it is itself a 64 bit program, once it knows that, it must be running on a 64 bit version of Windows (until MS releases a 128 bit version of Windows - won't happen tomorrow.) To determine if it is itself a 64 bit program, all it has to do is check the size of a pointer, i.e, sizeof(pointer) and if the size is 8 then it knows that it is a 64 bit program and since it is running, it must be running under a 64 bit version of Windows. Easy as pie. 

The 32 bit version cannot use that "trick" since its pointer size will be four (4) whether or not it is running under a 32 bit or 64 bit version of Windows. 

MS would have the programmer use the IsWowProcess() or IsWowProcess2() API to make the determination but, those two APIs have a few annoyances associated with them. First, they may not be present (IsWowProcess() requires XP SP2 or higher) and, even when present, the user running the program must have some privileges, that while not high, he/she may not have. 

There is a way to determine the bitness without using either or these APIs that do not require the user to have any "special" privileges. The "trick" resides in two details. The first one is that the Windows server is what determines the Windows bitness, the second one is that LoadLibraryEx will load almost anything you want as long as you don't intend to run it. 

Using the first observation, if Windows is 32 bit then its csrss.exe (client service runtime subsystem) will also be 32 bit and, it will always be 64 bit in a 64 bit version of Windows. 

Using the second observation, we can simply ask LoadLibraryEx to load csrss.exe as a datafile and unless the user is totally "privilege destitute", LoadLibraryEx will serve csrss.exe in a sliver platter. 

All that's left is for the program to rummage in csrss.exe to determine its bitness and whatever its bitness is, that's the Windows bitness too :) Done! 

That's what the program below does (full source in the attachment) 

On line 192, the program uses LoadLibraryEx to load csrss.exe. On line 211, it checks the address returned by LoadLibraryEx, which fiddles with the LSB to indicate that it was loaded as a data file and adjusts the address to turn it into the real load address. 

On line 219, using the real load address, it determines the module bitness (see GetModuleBitness on line 75.) If anything does not go according to plan, the program states it could not determine the Windows bitness, otherwise it outputs it. Easy as 32 bit pie. :) 

  
My intention is to start with the simplest most trivial API and build from there. This way those who follow the examples can gain the necessary knowledge to understand more subtle and complex use the API. 

NOTE: There is an example dedicated to LoadLibraryEx by itself which shows how it behaves under various circumstances. 

  
About 1% (maybe slightly more) of the examples are like the one above, where one API along with "peripheral" knowledge can be used to extract information out of Windows. 

I suggest creating a "WinAPI" directory and under that directory, create directories named "Techniques" (which the above example would go into), ntdll, kernel32, gdi32 and user32. Of course, you're free the arrange the examples however you like but, I have found that way to make the large number of programs easier to manage and find. 

## Post 2

The examples will be presented roughly in order of complexity, that is, from simplest to more complex. The simplest are by far those APIs which should NOT be used since no example will be provided for those. 

Amond the APIs that should be avoided and preferably never used are: 

  * GlobalCompact
  * GlobalFix
  * GlobalFlags
  * GlobalHandle
  * GlobalLock
  * GlobalReAlloc
  * GlobalSize
  * GlobalUnfix
  * GlobalUnlock
  * GlobalUnWire
  * GlobalWire
  * LocalAlloc
  * LocalCompact
  * LocalFlags
  * LocalFree
  * LocalHandle
  * LocalLock
  * LocalReAlloc
  * LocalShrink
  * LocalSize
  * LocalUnlock



All of the above are Windows 16 bit memory management functions which are _not_ well suited for a 32 bit O/S. Use the 32 bit memory management functions instead. 

As mentioned previously, no example for any of the above APIs will be given. 

**GlobalAlloc**

**GlobalFree**

These two (2) APIs are still commonly found in C programs but, are not as flexible as HeapAlloc() and HeapFree() which should be preferred. 

No example of GlobalAlloc and GlobalFree will be given. An example of HeapAlloc and HeapFree will be given in the future. 

  
**GlobalMemoryStatus**

GlobalMemoryStatus returns incorrect information on systems with more than 4GB of memory. Use GlobalMemoryStatusEx instead. 

An example for both will be given in the future. 

  
**GetTextExtentPoint**

Use GetTextExtentPoint32 instead which, according to MS, is more accurate. 

  
**WinAPI - GetTextExtentPoint** (gdi) 

**WinAPI - GetTextExtentPoint32** (gdi) 

Both of these APIs return the width and height in pixels of a specified string 

  
**WinAPI - SetBkMode** (gdi) 

Sets the background color to opaque or transparent. When the background is opaque then whatever color was chosen as background will be visible. 

  
**WinAPI - SetTextAlign** (gdi) 

Sets the text alignment. This example visually demonstrates the effects of some of the SetTextAlign options. See MSDN for all available options. 

  
**WinAPI - TextOut** (gdi) 

Outputs a string at the specified location, using the currently selected font, background color, and text color. 

  
**WinAPI - GetClientRect** (user) 

Retrieves the coordinates of the client area. This example draws a frame around the client area. It also serves as an example of CreatePen, SetCapture, ReleaseCapture, InvalidateRect, SelectObject, Rectangle, lstrcpy, lstrlen and DeleteObject. 

NOTE: this example does not include code to eliminate or minimize flicker,which would not be hard to implement (left for future example.) 

  
**WinAPI - Windows Bitness** (technique) 

This version includes minor editing of the comments and loads csrss.exe as an image resource instead of a data file. Since Windows uses the low bits of the load address to indicate the mode the file was loaded in, this version aligns the load address instead of simply subtracting one (1) which is valid only when the module is loaded as a data file. Using alignment works regardless of the way the module is loaded. 

recommendation : replace the previous version with this version. 

  
What follows is the code for the GetClientRect example (the source for the remaining examples is present in the zip file attached to this post.) 

## Post 3

Here are some examples of simple, yet often very useful API functions. 

**WinAPI - DebugBreak**

This example demonstrates using DebugBreak() to transfer control to a debugger. The example should be executed both, under a debugger and directly. 

This example also serves to demonstrate how a program can detect if it is running under a debugger without using the IsDebuggerPresent() API. 

Use the "DebugBreak()" menu item to cause the program execution to fall under debugger control. Use F7/F8/F9 to control execution. 

DebugBreak() is very useful to create conditional breakpoints with very complex conditions that are beyond the capabilities of an application debuggers. 

Also, DebugBreak() is useful to set breakpoints where it is usually not possible to set them. For instance, during the loading of a program. Windows automatically loads and calls the associated DLLs entry points. A strategically placed DebugBreak instruction allows a debugger to gain control during the DLLs load process. This combined with the use of a system debugger, such as SoftICE, allows debugging almost any code in the executable. 

  
In addition to that, combining the use of DebugBreak() with conditional compilation directives allows having the breakpoints, which in a large program may be numerous, in the program itself. This prevents "losing" breakpoints by exceeding the "breakpoint history" capacity in most debuggers and aids in future maintenance as, at least in theory, calls to DebugBreak() should only appear (or be left) in the most important or complex section of the code. 

DebugBreak() can also be useful to make reverse engineering a program more cumbersome. By littering the executable with a fairly large number of calls to it (say about 60 or more) this can make reverse engineering the application very tedious when it is run under a debugger. 

(Code skipped...) The above is the "critical" part of the code in the example. By executing DebugBreak() in a try/except, the application can detect whether or not it is running under a debugger. If the code in the exception handler is _not_ executed then, the program is not being debugged because, if it were, the DebugBreak() would have transferred control to the debugger instead of activating the exception handler. 

To make reverse engineering more cumbersome, a program could place code that _should be_ executed in the exception handler. If the program is running under a debugger, the piece of code that is actually part of the normal operation of the program is not executed thus causing the program to later fail in some way. Put around fifty (50) of those cases around in the program and, that will try the patience of whoever is trying to find out how the code works. :) 

This works particularly well when present in functions/procedures that are conditionally executed and/or are not executed often (it increases the chances of one or more of them being missed.) 

  
**WinAPI - IsDebuggerPresent**

This example demonstrates how to use the API IsDebuggerPresent() to determine if a program is running under a debugger or not. If the program is running under a debugger, selecting "Break into the debugger" from the IsDebuggerPresent() menu, will cause a break into the debugger, otherwise the request to break into the debugger will be ignored. 

It also demonstrates the difference between using DebugBreak() and an inline int 3. When using DebugBreak(), application debuggers will stop at the DebugBreak() line because the _entire_ DebugBreak() procedure has _not_ yet been executed, specifically, it stops at the "ret" that is inside DebugBreak(). 

When using an inline "int 3" the debugger will stop at the next source line because the "int 3" has been fully executed unlike the DebugBreak() procedure. 

IsDebuggerPresent() is particularly useful to guard calls to DebugBreak(). That way, it is not necessary to remove the calls to DebugBreak() in the "release" executable since they are triggered only when the executable is run under a debugger (normal users rarely run a program under a debugger and, many of them don't even know what a debugger is anyway.) 

  
**WinAPI - QueryPerformanceCounter**

**WinAPI - QueryPerformanceFrequency**

QueryPerformanceCounter along with QueryPerformanceFrequency can be used to measure elapsed time very accurately. That's the combined purpose of these two APIs. 

To determine elapsed time in seconds, simply subtract the first query from the second query and divide the result by the frequency. 

The attached examples only show how to call each of these functions. They do not include how to calculate elapsed time (it is reasonable to believe that programmers can subtract one number from another and divide the result by yet another number, i.e (b - a) div c) 

  
**WinAPI - SetLastError**

SetLastError can be used by an application's procedures to indicate whether or not they successfully performed their task. Note that it is strongly recommended to mark these custom application errors (the example shows how that is done.) 

Also, it is important NOT to call any Windows API functions after setting the last error as it is very likely that the API function will overwrite the last error set by the application. 

Fun experiment : 

1\. Start the SetLastError example 

2\. Select "LoadLibrary failure" from the "SetLastError" menu 

3\. Notice the value of the error (should be $A1) 

4\. Select "About SetLastError" from the "Help" menu 

5\. Click the OK button on the About box 

6\. Notice the value of the error (should still be $A1 

Now repeat step 1 through 4 above 

5\. Move the About box vertically a couple of pixels up and click the OK button 

6\. Notice the value of the error (should be $0 instead of $A1) 

7\. Why did moving the About box cause the value of SetLastError to change ? 

  
**WinAPI - Sleep**

The MSDN documentation states quite a few things about the Sleep API. This test program demonstrates what _actually_ happens when the sleep interval changes. 

RECOMMENDATION : _NEVER_ use an interval of zero (0). An interval of zero (0) causes most versions of Windows (maybe all) to consume significantly more CPU than expected. Run the example while watching the CPU usage in Task Manager and selecting various values from the menu. 

To eliminate the overhead of repainting the screen, uncomment the NOPAINT define. (painting does not perceptibly alter the cpu consumption, the DEFINE is there to test and prove that is the case. 

## Post 4

This post includes examples for the following APIs : 

**WinApi - CreateStatusWindow** (example is named StatusBar) 

This example shows the use of the obsolete CreateStatusWindow API and a "quirk" in the number of characters a status bar displays. CreateWindow or CreateWindowEx should be used instead of CreateStatusWindow. The fact that the API is obsolete and that the example demonstrates a quirk in the status bar is the reason the example is not named after the API. 

The sample program attempts to display a string that is longer than 137 characters in the status bar. The string to be displayed consists of a sentence that is repeated three times. If the status bar does not display the same sentence three times then truncation occurred. 

To show where the truncation took place, the text from the status bar is retrieved using SB_GETTEXT and subsequently displayed in the listbox. If the listbox displays the sentence three times (as expected), it indicates that the status bar did save the entire string but displayed only part of it. 

In spite of the fact that the status bar will only display 137 characters, it does keep a full copy of the string that was sent to it using SB_SETTEXT. 

Under build 950 of Windows 95 and later versions as well, the status bar control will not display more than 137 characters. It is not known whether this limit is intentional or not but, it does not seem to be documented anywhere. 

  
**WinAPI - GetEnvironmentStrings**

This example shows how to call the GetEnvironmentStrings to, as its name indicates, obtain the environment strings. It also shows how to parse the block of memory where the strings reside. As each individual string is identified it loads it into a listbox. 

NOTE: this example takes a "shortcut" when it comes to determining the listbox horizontal extent. In production code, the horizontal extent should be calculated as the width of the longest string added into the listbox. This simple program uses a hard coded estimate which in many cases will result in the "PATH" string not being fully visible. 

Future examples will show how to calculate the horizontal extent properly. 

Also, this example doesn't even bother to free the memory block created by GetEnvironmentStrings which should be done to avoid a potential memory leak. See the FreeEnvironmentStrings example below. 

This example also shows how to create a status bar at the top of a window as well as at its bottom (the more common case.) However, note that the API CreateStatusWindow is now obsolete. CreateWindow or CreateWindowEx should be prefered. Anything CreateStatusWindow can do, those APIs can do just as well. 

Also, this example does not show how to workaround a bug in the listbox control. The bug _sometimes_ causes the contents of the listbox to be garbled when the window is resized and the horizontal scroll thumb is not at the origin, i.e, horizontal scrolling has taken place. The reason the workaround code is not present is because the window isn't resizable. See the StatusBar example, which uses a resizable window, for the workaround. Note, that the presence of this bug is likely dependent on the version of Windows and isn't triggered consistently. 

  
**WinAPI - FreeEnvironmentStrings**

This example calls the GetEnvironmentStrings API immediately followed by a call to FreeEnvironmentStrings. 

In Windows 9x the FreeEnvironmentStrings function does not free the environment strings. It verifies that the parameter passed does indeed point to the process' environment strings. If it does, the function returns TRUE otherwise it returns FALSE but it never frees the block. 

In other versions of Windows, FreeEnvironmentStrings does free a memory block that contains the environment strings. 

  
**WinAPI - ExpandEnvironmentStrings**

This example shows how to retrieve the value of a known environment string. 

  
**WinAPI - GetModuleHandle**

This example shows how to call GetModuleHandle to retrieve the load address of modules in the process including the .exe itself. 

It also serves as an example for the **WinAPI - ModifyMenu**. The items under the "GetModuleHandle" menu change depending on the bitness of the program. The change is done using ModifyMenu. Notice that the items in the menu are _not_ those declared in the .rc resource file and differ depending on the program's bitness. 

It can also serve as an example for the **WinAPI - GetMenu** and **WinAPI - GetSubMenu**. In order to modify a specific menu item, first the handle to the menu must be obtained using GetMenu and after that the handle to the popup menu associated with the menu item must be obtained using GetSubMenu. Once the submenu handle is known then ModifyMenu can be called to modify an item in the popup menu. 

Note unrelated to any Windows API: the width and precision stated in the StrFmt function is ignored by StrFmt if necessary (it is when the program is 64 bit.) 

  
**WinAPI - GetModuleHandleEx**

This example shows how to call GetModuleHandleEx to determine a module's load address based on any address that resides in the module. For instance, knowing that some dll function resides at a specific address, GetModuleHandleEx when given that address can return the Dll's load address. 

This ability can often be useful to debuggers. 

This example also demonstrates how to "pin" a dll in memory, that is, ensuring that the dll is not unloaded at an undesirable time. 

There are, at least, two cases when that capability is useful/required. 

The first is in a multi-threaded environment where one thread obtains a dll's module handle while another thread _may_ unload the dll thus invalidating the handle obtained by the first thread. Note, this situation quite likely indicates very poor programming, a module should _not_ be loaded by one thread and unloaded in another. IOW, the thread that loads a dll should be the only thread responsible for unloading it. 

The second case is not as common as the first one described above. When a program injects a dll into another process and the injected dll in turn traps functions in application specific dlls then, it is not only conceivable but, even likely that the application may decide to unload one of it dlls that have functions trapped by the injected dll. When this happens the injected dll will call into the void since the dll functions it was trapping are gone. 

Pinning down a dll prevents this very undesirable situation from taking place. 

To see the effects of pinning down a dll use the "Module32First" example (source for it will be given in a future post) and follow these steps : 

1\. After starting the GetModuleHandleEx example, start the Module32First program. 

2\. Search and select "GetModuleHandleEx.exe" on the left hand side pane of Module32First 

3\. Note the values of "USG" and "usg" for the "GetModuleHandleExDll" (they should both be two (2) 

4\. Select "Pin and attempt to unload the dll" from the "GetModuleHandleEx" menu. 

5\. Select "Update snapshot" in the Module32First program. 

6\. Repeat steps 2 and 3 above. The values under "USG" and "usg" should read 65535 indicating that the dll is pinned. 

Every time the "Pin and attempt to unload the dll" option is executed, the program attempts to (unsuccessfully) unload the library 100 times. What should be noted is that 100 is _not_ subtracted from the "USG" and "usg" fields. Once those fields are set to high(word) they are no longer reference counted. 

The interesting code portions of this program are: 
    
    
          case Msg of
            WM_CREATE:
            begin
              { load the supporting dll                                               }
         
              ModuleAddressA := LoadLibrary(DllToLoad);
         
              FunctionAddress := GetProcAddress(ModuleAddressA,
                                                pchar(DLL_FUNCTION_INDEX));
         
              { get the module address again but this time using the function address }
         
              if not GetModuleHandleEx(GET_MODULE_HANDLE_EX_FLAG_FROM_ADDRESS,
                                       FunctionAddress,
                                       ModuleAddressB) then
              begin
                LastError := GetLastError();
         
                MessageBox(Wnd,
                          'GetModuleHandleEx call failed in WM_CREATE',
                          'Main Window',
                           MB_ICONERROR or MB_OK);
         
                WndProc := -1;
         
                exit;
              end;
         
              { format and save the values obtained                                   }
         
              StrFmt(Values[1], ' %p', [FunctionAddress]);
         
              { the next two addresses should be the same                             }
         
              StrFmt(Values[2], ' %p', [pointer(ModuleAddressA)]);
              StrFmt(Values[3], ' %p', [pointer(ModuleAddressB)]);
         
              exit;
            end;
    

  
On line 13, GetModuleHandleEx is used to obtain the dll's load address using the address of one of the functions it contains. During WM_PAINT (not shown here) both addresses, the one obtained this way and the one obtained using GetModuleHandleEx are output, thereby making it easy to verify that both addresses are the same (as they should be) 

The other interesting part is : 
    
    
              case LOWORD(wParam) of
                IDM_BYADDRESSANDPIN:
                begin
                  if not GetModuleHandleEx(GET_MODULE_HANDLE_EX_FLAG_PIN,
                                           DllToLoad,
                                           ModuleAddressB) then
                  begin
                    LastError := GetLastError();
         
                    MessageBox(Wnd,
                              'GetModuleHandleEx call failed in IDM_BYADDRESSANDPIN',
                              'Main Window',
                               MB_ICONERROR or MB_OK);
         
                    WndProc := -1;
         
                    exit;
                  end;
         
                  for i := 1 to 100 do FreeLibrary(ModuleAddressB);
         
                end;
    

  
On line 4, GetModuleHandleEx is used to pin down the dll in memory then in line 20 FreeLibrary is called 100 times to unload the dll. Use Module32First to verify that the dll is not freed/unloaded. 

  
**WinAPI - Module32First**

For the time being, this example is only included to help visualize what GetModuleHandleEx does. Its source code will be included in a future example. 

## Post 5

This post includes examples for the following APIs : 

**WinAPI - GetFileSize**

**WinAPI - GetFileSizeEx**

These two examples show how to get a file's size. GetFileSize should be avoided because the results it returns can be ambiguous for large files. For this reason, it is preferable to always use GetFileSizeEx. 

  
**WinAPI - GetSystemInfo**

**WinAPI - GetNativeSystemInfo**

These two functions are very similar, almost identical. The critical difference is in how the Processor Architecture is reported. 

Presuming the program is run on a 64 bit intel/AMD CPU, A 32 bit program that uses GetSystemInfo will always see the processor architecture as PROCESSOR_ARCHITECTURE_INTEL, while a 32 bit program using GetNativeSystemInfo will see PROCESSOR_ARCHITECTURE_INTEL when running under a 32 bit version of Windows and see PROCESSOR_ARCHITECTURE_AMD64 when running under a 64 bit version of Windows. The reported processor type will also be different in the above mentioned cases, one case reporting PROCESSOR_INTEL_PENTIUM and the other reporting PROCESSOR_AMD_X8664. 

In spite of that, MS documentation claims that using GetNativeSystemInfo is _not_ a reliable method to determine the O/S bitness. 

  
**WinAPI - GetVersion** (deprecated) 

**WinAPI - GetVersionEx** (deprecated) 

**WinAPI - RtlGetNtVersionNumbers** (undocumented) 

Both GetVersion and GetVersionEx should be avoided not only because they are deprecated but, because these functions (and a few others) don't report the _true_/real Windows version the program is running under. 

On the other hand, RtlGetNtVersionNumbers reports the _true_ and accurate Windows version. 

The function (in NTDLL) is as simple as can, and should be: 
    
    
                                 public RtlGetNtVersionNumbers
        RtlGetNtVersionNumbers proc near
         
                        test    rcx, rcx            ; ensure pointer not nil
                        jz      short loc_78E9517B
                        mov     dword ptr [rcx], 6
         
        loc_78E9517B:
                        test    rdx, rdx            ; ensure pointer not nil
                        jz      short loc_78E95186
                        mov     dword ptr [rdx], 1
         
        loc_78E95186:
                        test    r8, r8              ; ensure pointer not nil
                        jnz     loc_78EB7C9D        ; see below
                        rep retn
        RtlGetNtVersionNumbers endp
         
        loc_78EB7C9D:
                        ; F000 -> free build
                        ; 1DB1 -> build number
         
                        mov     dword ptr [r8], 0F0001DB1h
                        retn
    

Before any other consideration, it is worth pointing out that while this function is undocumented, some important MS code depends on its existence, notably some standard C dlls. Obviously, deciding for or against using undocumented functions is programmer's choice. In the case of this function, it's likely that it's here to stay. 

The example RtlGetNtVersionNumbers comes in two "flavors", GUI and Console. From a programming viewpoint, there is nothing particularly remarkable in the GUI version. The console version has some remarkable characteristics, not due to the API but, due to how FPC manages consoles and, getting a console application to display a dialog box by selecting a menu item appended to the system menu. 

As far as how FPC manages consoles, the FPC RTL assumes that there can only be one console during the life of a program. That assumption is incorrect. Consoles, just like any other "resource" can come and go at any time and multiple times. 

The functions in console.inc are "general" in nature (actually, only somewhat general.) They are meant to be used by a GUI program which creates and deletes multiple consoles during its execution. Something the RtlGetNtVersionNumbers example does _not_ do. 

For a discussion of how the FPC RTL manages consoles, see the thread : <https://forum.lazarus.freepascal.org/index.php/topic,44828.msg315468.html#msg315468> The last post includes an example program. 

  
The other purpose of console.inc is to provide a mechanism for a console application to display a dialog box (usually with author's name, copyright, etc.) This sounds like it would be a trivial thing to do but, I could not find a simple and straightforward solution that worked reliably. The solution in console.inc is pure brute force (but it works :)). It creates a thread specifically with the goal of figuring out if the "About ..." item in the system menu was selected and, if so, presents the dialog box. 

If someone can come up with a simpler solution that works reliably to display the dialog box, that would be a very welcome and appreciated improvement. 

If it weren't for the code to manage the creation and destruction of multiple consoles and displaying a dialog box, the console version of this API example would be totally trivial 

## Post 6

This post includes examples for the following APIs : 

  
The following two APIs are in one program named GetDesktopWindow 

**WinAPI - GetDesktopWindow**

**WinAPI - GetWindowThreadProcessId**

This example gets the handle of the desktop window and the process and thread ids associated with it. It also provides the option to hilight the desktop window in its "GetDesktopWindow" menu. 

Note that the desktop window and the shell window are different windows in spite of the fact that they cover the same rectangle. 

  
**WinAPI - GetShellWindow**

This example is almost identical to the GetDesktopWindow except that it shows and highlights the shell window instead of the desktop window. 

  
**WinAPI - GetTaskmanWindow** (undocumented) 

Also very similar to the above mentioned examples but showing the Task manager window. 

  


The following two APIs are in one program named GetWindowTextLength 

**WinAPI - GetWindowText**

**WinAPI - GetWindowTextLength**

This example shows how to use GetWindowText and GetWindowTextLength as well as drawing a frame around a window, creating a pen, offsetting a rectangle, locking out paint updates on a window, (using GetDCEx), SetROP2, SetWindowPos, GetCapture, ReleaseCapture, GetCursorPos, IsWindow and, LockWindowUpdate among others. 

The example shows the window text and text length that is associated with the window that resides under the mouse. 

  
**WinAPI - IsTopLevelWindow** (undocumented) 

Functionally similar to the GetWindowTextLength example. It determines if the window that is currently under the cursor is a top level window or not. 

  
**WinAPI - GetTopLevelWindow** (undocumented) 

The same as the IsTopLevelWindow example but displays the top level window that is associated with the window that is currently under the cursor. 

  
**WinAPI - SetWindowLong and SetWindowLongPtr**

This example uses SetWindowLong (in 32bit) and SetWindowLongPtr (in 64 bit) to change its window frame from that of a typical window to one that has a thin dialog box frame without caption. 

It also shows how to process the messages WM_NCHITTEST and WM_NCLBUTTONDBLCLK to switch from one frame style to another. 

It also shows how to use a window's client area to drag it around the screen (feature is purposely active only when the window has no caption.) 

## Post 7

This post includes examples for a number of API functions but because they are also meant to show how to deal with flicker, the main APIs used in each example is listed. The APIs are: 

  


  * WinAPI - Rectangle (in Rectangle)
  * WinAPI - CreateSolidBrush
  * WinAPI - CreatePen
  * WinAPI - CheckMenuRadioItem
  * WinAPI - GetStockObject
  * WinAPI - DeleteObject



  


  * WinAPI - CreateWindowEx (in FlickerMax)
  * WinAPI - GetClassInfoEx
  * WinAPI - RegisterClassEx
  * WinAPI - BeginPaint
  * WinAPI - EndPaint
  * WinAPI - SelectObject
  * WinAPI - SetBkColor
  * WinAPI - GetTextExtentPoint32
  * WinAPI - SetTextAlign
  * WinAPI - TextOut
  * WinAPI - DialogBox
  * WinAPI - EndDialog
  * WinAPI - DestroyWindow
  * WinAPI - PostQuitMessage



  


  * WinAPI - CopyRect (in FlickerMin)
  * WinAPI - Rectangle



  


  * WinAPI - GetClassLongPtr (in HREDRAW_VREDRAW)
  * WinAPI - SetClassLongPtr
  * WinAPI - GetMenuState
  * WinAPI - CheckMenuItem



  


  * WinAPI - CreateSolidBrush (in Gradient)
  * WinAPI - DeleteObject
  * WinAPI - GetDC
  * WinAPI - ReleaseDC
  * WinAPI - MulDiv
  * WinAPI - FillRect
  * WinAPI - PostMessage
  * WinAPI - CheckMenuRadioItem
  * WinAPI - InvalidateRect



  


  * WinAPI - GetClipBox (in GetClipBox)
  * WinAPI - CopyRect
  * WinAPI - SetRectEmpty
  * WinAPI - SetTimer
  * WinAPI - KillTimer
  * WinAPI - BringWindowToTop
  * WinAPI - SetForegroundWindow
  * WinAPI - GetWindow
  * WinAPI - GetWindowDC
  * WinAPI - UpdateWindow
  * WinAPI - Rectangle



  


  * WinAPI - GetClipBox (in IsWindowFullyCovered)
  * WinAPI - SetTimer
  * WinAPI - KillTimer
  * WinAPI - BringWindowToTop
  * WinAPI - SetForegroundWindow
  * WinAPI - InvalidateRect
  * WinAPI - UpdateWindow
  * WinAPI - GetClassLongPtr
  * WinAPI - CopyRect
  * WinAPI - Rectangle



  


  * WinAPI - GetParent (in GetParent)
  * WinAPI - GetAncestor
  * WinAPI - GetConsoleWindow
  * WinAPI - GetConsoleProcessList
  * WinAPI - GetDC
  * WinAPI - ReleaseDC
  * WinAPI - GetWindowRect
  * WinAPI - GetTopLevelWindow
  * WinAPI - IsTopLevelWindow
  * WinAPI - SetWindowPos
  * WinAPI - MoveWindow



  


  * WinAPI - EnumWindows (in EnumWindows)
  * WinAPI - GetWindowThreadProcessId
  * WinAPI - IsWindowVisible
  * WinAPI - GetWindowText
  * WinAPI - SendMessage
  * WinAPI - InitCommonControls
  * WinAPI - CreateStatusWindow
  * WinAPI - GetClassLongPtr
  * WinAPI - SetClassLongPtr
  * WinAPI - CreateWindowEx
  * WinAPI - GetDlgItem
  * WinAPI - MoveWindow
  * WinAPI - GetScrollPos



  


  * WinAPI - EnumChildWindows (in EnumChildWindows)
  * WinAPI - EnumWindows
  * WinAPI - GetAncestor
  * WinAPI - GetDesktopWindow
  * WinAPI - IsWindowVisible
  * WinAPI - GetWindowText
  * WinAPI - GetWindowThreadProcessId



  
in addition to the APIs listed above, this post and the following ones focus on the reduction or elimination of flicker. 

Flicker is mostly caused by three (3) characteristics, they are: the presence of CS_HREDRAW and CS_VREDRAW in the window class and the processing of the WM_ERASEBKGND (erase background) window message. 

The CS_HREDRAW and CS_VREDRAW cause the window's entire client area to be invalidated when the window is resized. This in turn causes the entire background of the window to be erased by the default window procedure when a WM_ERASEBKGND message is received. It is the full erasing of the background that is the source of flicker. It causes two different images to appear, the first image is the background (for a very short time) the second one is the image that is supposed to be painted on it. 

Carefully controlling _what_ has to be erased when a WM_ERASEBKGND message is received can significantly reduce flicker and often eliminate it completely. 

There are a number of ways to reduce or eliminate flicker, of those, double buffering is likely the most common one due to the fact that it is fairly simple to implement and it completely eliminates flicker. Double buffering consists of painting on a bitmap that is compatible with the output device (usually but not always the screen) then bitblt-ing the fully drawn bitmap onto the output device without having to erase the background first, which is not required because the bitmap is the full and final image that should appear on the output device. 

Double buffering, while very effective is not without disadvantages, among them, it requires some code to implement it, often specific for each window that uses it and, it can potentially use a fair amount of memory. In addition to that, it has the potential of noticeably lowering the speed at which the windows that use it are repainted. This can be experienced when a program's window feels "elastic" when being resized. The more windows on the user interface, the greater the potential for the window to feel "elastic" when resized. 

  
The **FlickerMax** example is what happens when the window class includes CS_HREDRAW and CS_VREDRAW and the default window procedure is allowed to erase the background when processing the WM_ERASEBKGND. The resulting amount of flicker is substantial and visually offensive. It can be seen in its greatest "glory" when the window is resized horizontally. FlickerMax uses the following code when registering the class: 
    
    
          if not GetClassInfoEx (hInstance, AppName, cls) then
          begin
            with cls do
            begin
              { cbSize has already been initialized as required above                 }
         
              style           := CS_BYTEALIGNCLIENT or CS_VREDRAW or CS_HREDRAW;
              lpfnWndProc     := @WndProc;                    { window class handler  }
              cbClsExtra      := 0;
              cbWndExtra      := 0;
    

  
and does not process the WM_ERASEBKGND thus letting the default window procedure erase the entire client area. 

The **FlickerMin** example is code-wise almost identical to FlickerMax, the difference is that FlickerMin does _not_ allow the default window procedure to erase the background, instead, it erases the rectangles that are above and below the text when processing the WM_PAINT, followed by outputting the line of text. That small difference greatly reduces the amount of flicker. Note that the flicker isn't totally eliminated in this case, resizing the FlickerMin horizontally makes it easy to notice but, for most purposes, it can be considered tolerable. 

FlickerMin is the reason for the Rectangle example. When using the Rectangle API it is important to keep in mind the type of pen that API uses to draw the edges of the rectangle. When the pen is a NULL pen the rectangle drawn will be one (1) pixel smaller horizontally and vertically than the specified rectangle coordinates. This is because Rectangle uses the pen to draw the outer right and bottom edge of the rectangle and, when the pen is NULL this causes those edges not to be drawn resulting in a rectangle that is one (1) pixel short on the right and bottom edges. 

This can be seen with the **Rectangle** example by changing the pen used from a null pen to any other colored pen back and forth. A zoomed image of the bottom right of the resulting rectangle is attached where the difference between using a null pen and a colored pen is easier to see. 

[![WinAPI rectangle zoomed.png](https://wiki.freepascal.org/images/b/ba/WinAPI_rectangle_zoomed.png)](</File:WinAPI_rectangle_zoomed.png>)

When using the Rectangle API to erase parts of the client area background, the simplest method consists of specifying a null pen and increasing the Right and Bottom coordinates of the rectangle. This is what FlickerMin does to erase the part of the client area that is above and below the text without erasing any portion where the new text should appear. 

The little bit of flicker seen in FlickerMin is due to its reliance on ExtTextOut to clear the narrow and wide rectangle where the text is displayed. It is possible to make the flicker imperceptible by using Rectangle to erase the area on the left and right of where the text will appear (in addition to above and below) and instruct ExtTextOut to confine its output to the rectangle that is snug to the text. (that improvement is left as an exercise for the reader ;) - simply two additional calls to Rectangle and a change in the rectangle used by ExtTextOut) 

FlickerMin uses the same code as FlickerMax when registering the window class but, when it gets a WM_ERASEBKGND it tells Windows that it took care of erasing the background by using the following code: 
    
    
            WM_ERASEBKGND:
            begin
              { tell Windows we erased the background                                 }
         
              WndProc := 1;
         
              exit;
            end;
    

then when processing the WM_PAINT it uses: 
    
    
              {-----------------------------------------------------------------------}
              { clear the rectangles above and below the text                         }
         
              { this could have been done when processing the WM_ERASEBKGND message   }
              { but it's more convenient to do here.                                  }
         
              CopyRect(ClearRect, ClientRect);
         
              { clear the rectangle above the text                                    }
         
              with ClearRect do
              begin
                { Left, Right and Top are already set                                 }
         
                Bottom := TextRect.Top;
                Rectangle(ps.hdc, Left, Top, Right, Bottom);
              end;
         
              { clear the rectangle below the text                                    }
         
              with ClearRect do
              begin
                { Left and Right are already set                                      }
         
                Top    := TextRect.Bottom;
                Bottom := ClientRect.Bottom;
         
                Rectangle(ps.hdc, Left, Top, Right, Bottom);
              end;
    

which erases the areas above and below the text. A couple of additional calls to rectangle could be used to erase the areas to the right and left of the text that _will_ be output to make flicker imperceptible. 

  
One of the most important decisions is determining if CS_HREDRAW and CS_VREDRAW are necessary when registering the window class. It is, unfortunately, somewhat common to see windows include those two (2) styles in the window class when they aren't really necessary. 

The HREDRAW_VREDRAW is an example. it does NOT include the CS_HREDRAW and CS_VREDRAW styles, yet it updates the window correctly and without a trace of flicker without using any methods to prevent flicker. The example includes the option of adding CS_HREDRAW and CS_VREDRAW in the class styles, when that is done, there will be a very significant amout of flicker. 

Again, HREDRAW_VREDRAW does NOT include any code at all to prevent flicker. The exclusion of CS_HREDRAW and CS_VREDRAW is all that is needed to have no flicker whatsoever when resizing the window. 

The CS_HREDRAW and CS_VREDRAW styles need to be included when what is displayed in the client area changes depending on the size of the window. The Gradient is an example of this. Specifically, the color used at a particular location changes depending on the size of the window, because of this, the entire window needs to be invalidated and repainted whenever its size changes. 

The **Gradient** program draws everything when processing the WM_ERASEBKGND and tells Windows that it took care of erasing the background. This is just to show that, at least in some cases, it is possible to repaint the entire client area when processing the WM_ERASEBKGND. However, a "normal" program would simply return one (1) when processing WM_ERASEBKGND and redraw the gradient when dealing with WM_PAINT. 

NOTE: the gradient program uses functions to draw the vertical and horizontal gradients. It would be fairly easy to roll them into a single function that uses an "orientation" parameter to select vertical or horizontal. Gradient uses different functions to use a different number of bands depending on the orientation. The vertical orientation uses fewer bands resulting in noticeable banding while the horizontal orientation uses more bands thereby making the gradient band-less (much smoother.) Also, the difference in smoothness is more apparent when the gradient is blue than when it is grey. 

Continued on next post ... 

The **GetClipBox** example is meant to show that the mechanisms Windows uses to paint a window's client area changed after Windows XP. When run on WinXP the example will work as expected but won't on Vista and later versions UNLESS its compatibility setting has "Disable visual themes" checked. 

if "Disable visual themes" is not checked then GetClipBox will report "COMPLEX_REGION" whenever the window is partially visible and "NULL_REGION" when the window is fully covered by other windows. 

if "Disable visual themes" is checked then GetClipBox can also report "SIMPLE_REGION" if the window's visible area is a rectangle (which may be the entire client area or a portion of the client area.) NOTE that when "Disable visual themes" is checked the window caption will look like a Win95 caption instead of the stylish captions shown by Windows Vista and later. 

Given that it is unreasonable to require an application to only operate properly when "Disable visual themes" is checked, the current usefulness of GetClipBox is rather limited. 

GetClipBox is still useful, regardless of the "Disable visual themes" setting, to determine if a window is fully covered by other windows or not. When the window is fully covered by other windows then GetClipBox will always return "NULL_REGION", this can be useful to applications that update the contents of the client area on a regular basis, using a timer, such as the "performance" tab of the task manager (or the GetClipBox program itself.) If the window is fully covered then there is obviously no need to update the client area thus sparing Windows from wasting clock cycles to determine that whatever the application is painting should be ignored. 

The GetClipBox example also shows how to eliminate flicker by telling Windows the background has been erased and using the Rectangle API to erase only what is required of the background. The result, unlike in FlickerMin that uses the same method, is totally flicker free. 

In addition to that, it shows how to use BringWindowToTop and SetForegroundWindow to bring the main window to the foreground when a mouse click occurs in the popup's client area. To see this working, have some window cover the GetClipBox main window then click anywhere in the popup's client area. That will cause the main window to be placed back at the top of the Z order. 

NOTE: the popup's topmost attribute is set in CreateWindowEx. 

  
The **IsWindowFullyCovered** example is an "abbreviated" version of GetClipBox that simply reports if the window is fully covered or not. 

It is simply the "home" of a function "surprisingly" namedd IsWindowFullyCovered which can be copy/pasted into any other program that needs the functionality. 

  
The **GetParent** program is to show how the APIs GetParent, GetAncestor and GetWindow behave depending on the type of window (toplevel, child, popup) where it is being called. The MSDN documentation, while it tries hard, it doesn't make it easy to visualize what the result of those APIs is going to be depending on the type of window using it. The GetParent program _shows_ the result in each case. 

The GetParent program is actually three (3) programs in one. The first shows the results when the owner of the popup window is zero (0). That occurs when none of the {$define} in the code are active/set. The second one shows the results when a grandchild window is set as the popup owner, controlled by the {$define POPUP_OWNER_IS_GRANDCHILD}, the third one shows the results when the owner is set using the TopLevelWindow API, controlled by the {$define POPUP_OWNER_IS_TOPLEVELWINDOW}. 

In each case note carefully, the values shown GetParent, GetWindow(..., GW_OWNER) and the values returned by the various GetAncestor options. Particularly when the popup's owner is the grandchild window. 

The GetParent example also shows how to pass a parameter to the WM_CREATE message using the lParam parameter of CreateWindow/CreateWindowEx (their last parameter.) GetParent uses only one (1) window procedure and only one class for the main window, child window, grandchild window and popup window. This causes Window's default window procedure to call the window procedure recursively for the creation of each window. The recursion is controlled and ended using the lParam which determine the type of window to create (main, child, grandchild or popup.) 

It also uses the WM_ACTIVATE message to make the three (3) windows to be brought up to the top of the Z order whenever either the main window or the popup window is activated. For this it uses SetWindowPos. It is worth noting that using SetWindowPos always works with the GUI windows but, for some unknown reason, does NOT always work with console windows (it only works _most_ of the time.) It does this using the following code: 
    
    
            WM_ACTIVATE:
            begin
              if LOWORD(wParam) <> 0 then    { window is being activated              }
              begin
                if Wnd = MainWnd then
                begin
                  { the main window is being activated, make the popup window visible }
         
                  SetWindowPos(PopupWnd,
                               HWND_TOP,
                               0,
                               0,
                               0,
                               0,
                               SWP_NOACTIVATE or SWP_NOMOVE or SWP_NOSIZE);
         
                  if ConsoleWnd <> 0 then
                  begin
                    { NOTE: with consoles SetWindowPos works only MOST of the time.   }
                    {       the reason why it doesn't work all the time is unknown.   }
         
                    SetWindowPos(ConsoleWnd,
                                 HWND_TOP,
                                 0,
                                 0,
                                 0,
                                 0,
                                 SWP_ASYNCWINDOWPOS or
                                 SWP_NOACTIVATE     or SWP_NOMOVE or SWP_NOSIZE);
                  end;
         
                  exit;
                end;
         
                if Wnd = PopupWnd then
                begin
                  { the popup window is being activated, make the main window visible }
         
                  SetWindowPos(MainWnd,
                               HWND_TOP,
                               0,
                               0,
                               0,
                               0,
                               SWP_NOACTIVATE or SWP_NOMOVE or SWP_NOSIZE);
         
                  if ConsoleWnd <> 0 then
                  begin
                    { NOTE: with consoles SetWindowPos works only MOST of the time.   }
                    {       the reason why it doesn't work all the time is unknown.   }
         
                    SetWindowPos(ConsoleWnd,
                                 HWND_TOP,
                                 0,
                                 0,
                                 0,
                                 0,
                                 SWP_ASYNCWINDOWPOS or
                                 SWP_NOACTIVATE     or SWP_NOMOVE or SWP_NOSIZE);
                  end;
         
                  exit;
                end;
              end;
            end;
    

Note that SWP_NOACTIVATE is specified in the call to SetWindowPos, failing to specify that flag will cause the main window and popup window to recursively activate each other, leading to an undesirable situation. 

Another "interesting" feature of GetParent is how it determines whether or not it should move the console window. If GetParent is started from a command line prompt, it will NOT move the console it was started from. However, if it was NOT started from a command line prompt, it creates its own console (automatically done by Windows because in that case GetParent is a console application) and proceeds to move it to uncover it (make it visible.) It makes the determination using the following code: 
    
    
        procedure PositionDebugConsole(Wnd : HWND);
          { positions the console window to ensure it is not obscured by this         }
          { program's main window or its popup.  NOTE: this procedure is NOT general  }
          { it uses knowledge of the total height of the text output in the windows.  }
        var
          ProcessCount : DWORD;
          ProcessList  : array[1..10] of DWORD;
         
          WindowRect   : TRECT;
          ConsoleRect  : TRECT;
         
        begin
          { if the main window has not yet been created, there is no need to move the }
          { console window.                                                           }
         
          if Wnd = 0 then exit;
         
          { the main window exists, figure out if we need to move the console         }
         
          if GetConsoleWindow() = 0 then
          begin
            { there is no console, therefore there is nothing to position             }
         
            exit;
          end;
         
          ZeroMemory(@ProcessList, sizeof(ProcessList));
         
          ProcessCount := 0;
          ProcessCount := GetConsoleProcessList(@ProcessList, high(ProcessList));
         
          if ProcessCount > 1 then
          begin
            { we were started from a pre-existing console, not from a console we own  }
            { therefore, don't alter the position of the console, it would not be     }
            { "polite" to move a pre-existing console.                                }
         
            exit;         { existing console remains whereever it currently is        }
          end;
         
          { we own the console, move it to be right next to our window and below the  }
          { popup.                                                                    }
         
          ConsoleWnd := GetConsoleWindow();         { save the handle to the console  }
         
          GetWindowRect(Wnd,        WindowRect);
          GetWindowRect(ConsoleWnd, ConsoleRect);
         
          with ConsoleRect do
          begin
            MoveWindow(ConsoleWnd,                  { our dedicated console window    }
                       WindowRect.Right,            { x snug to the main window       }
                       2 * LabelsHeight,            { below the popup window          }
                       Right  - Left,               { no change in width              }
                       Bottom - Top,                { no change in height             }
                       TRUE);                       { repaint it                      }
          end;
        end;
    

It makes the determination using GetConsoleProcessList which will return a value greater than 1 if it was started from an existing console and 1 otherwise (it is a new console.) 

  
GetParent has another notable and unexpected "peculiarity". For some reason, the messages (hints) emitted by the compiler (v3.0.4) are off by one. Specifically, the line numbers are one greater than the real line number. This is probably the cause why when, in Lazarus, placing a breakpoint at some line, execution stops one line _after_ the breakpoint instead of the line that contains the breakpoint. Reason for this behavior is unknown but, it might be a bug in FPC v3.0.4. 

  
**WinAPI - EnumWindows**

**WinAPI - EnumChildWindows**

The EnumChildWindows program is _very close_ to the same program/example recently posted in another thread. However, there are two important differences. 

The first one is that one of the constants used by GetAncestor was misspelled, this didn't cause a problem because it was defined for Delphi v2.0 only and it was not used anyway. 

The second difference is that the original example did NOT account for Windows ever returning a handle whose value exceeds high(DWORD) (all window handles should fit in a DWORD even in a 64 bit installation because window handles can and are shared among 32bit and 64bit programs.) For some unknown reason, there are times when EnumWindows and EnumChildWindows will return a window handle that has the high DWORD set to $FFFFFFFF. MS' spy++ also sees the presence of $FFFFFFFF in the high DWORD thus proving it is not a result of a bug in the example. 

The presence of $FFFFFFFF in the high DWORD is a problem in EnumWindows and EnumChildWindows because it messes up the alignment of the output in the listbox. 

This version of EnumChildWindows unceremoniously sets the high DWORD of a window handle to zeroes when necessary thus ensuring the window handle values never exceed high(DWORD) thus not messing up the alignment in the listbox. 

  


**ABOUT HINTS and UNUSED PARAMETERS**

This set of programs aim to minimize the number of unwanted hints emitted by FPC, specifically those related to some variable not being initialized because it presumes that "var" parameters should be initialized and, those caused by unused parameters. 

Removing the unwanted hints about variables not being initialized due to parameters being "var" instead of "out" is solved by correcting the definition. This set of example uses corrected definitions whenever necessary. 

Removing the unwanted hints about parameters being unused is done using the following code: 
    
    
        {$ifdef FPC}
          procedure UNUSED_PARAMETER(UNUSED_PARAMETER : ptrint); inline; begin end;
        {$endif}
    

and using that procedure to actually use the otherwise unused parameter, as in: 
    
    
          {$ifdef FPC}
            UNUSED_PARAMETER(lParam);           { "declare" lParam as unused          }
          {$endif}
    

that will still cause one (1) hint to be emitted due to the procedure not using the parameter but, that procedure can be used for every unused parameter in the program (in a large program, there can be a good number of them) thus getting only one (1) hint instead of one per unused parameter. 

Additionally, since the procedure is inline and is empty, it generates no code. 

Bottom line: it is very similar to using UNREFERENCED_PARAMETER in C. 

  
**OTHER CONSIDERATIONS:**

There are a good number of ways to avoid and/or minimize flicker. This set of examples will be followed by another set that uses other methods/techniques. 

## Post 8

This post includes examples for a number of API functions and how they can be used to prevent/eliminate flicker. This is the second and last post related to preventing/eliminating flicker. Some of the examples are not directly about flicker, they are included because they are used in other examples that are about reverting/eliminating flicker. 

The examples are: 

  * WinAPI - ExtTextOut (ExtTextOut in folder Ex1)
  * WinAPI - CopyRect
  * WinAPI - Rectangle
  * WinAPI - WM_GETMINMAXINFO



  


  * WinAPI - ExtTextOut (ExtTextOut in folder Ex2)
  * WinAPI - GetDC
  * WinAPI - ReleaseDC
  * WinAPI - OffsetRect
  * WinAPI - CopyRect
  * WinAPI - InflateRect
  * WinAPI - InvalidateRect
  * WinAPI - HeapAlloc
  * WinAPI - GetCursorPos
  * WinAPI - WindowFromPoint
  * WinAPI - IsWindow
  * WinAPI - UpdateWindow
  * WinAPI - SetBkColor



  


  * WinAPI - InvalidateRect (in InvalidateRect)
  * WinAPI - GetWindowRect
  * WinAPI - OffsetRect
  * WinAPI - GetDC
  * WinAPI - GetDCEx
  * WinAPI - SetROP2
  * WinAPI - Rectangle
  * WinAPI - ReleaseDC
  * WinAPI - SetRectEmpty
  * WinAPI - HeapAlloc
  * WinAPI - CreatePen
  * WinAPI - PostMessage
  * WinAPI - SendMessage
  * WinAPI - SetCapture
  * WinAPI - ReleaseCapture
  * WinAPI - SetWindowPos
  * WinAPI - LockWindowUpdate
  * WinAPI - UpdateWindow
  * WinAPI - GetCursorPos
  * WinAPI - IsWindow
  * WinAPI - GetParent
  * WinAPI - WindowFromPoint
  * WinAPI - UpdateWindow



  


  * WinAPI - GetCharacterPlacement (GetCharacterPlacement in folder Ex1)
  * WinAPI - ExtTextOut
  * WinAPI - RtlZeroMemory



  


  * WinAPI - GetCharacterPlacement (GetCharacterPlacement in folder Ex2)
  * WinAPI - ExtTextOut
  * WinAPI - RtlZeroMemory



  


  * WinAPI - BitBlt (in BitBlt)
  * WinAPI - LoadBitmap
  * WinAPI - CreateCompatibleDC
  * WinAPI - GetObject
  * WinAPI - DeleteDC



  


  * WinAPI - StretchBlt (in StretchBlt)
  * WinAPI - LoadBitmap
  * WinAPI - CreateCompatibleDC
  * WinAPI - GetObject
  * WinAPI - DeleteDC



  


  * WinAPI - CreateCompatibleBitmap(in CreateCompatibleBitmap)
  * WinAPI - CreateCompatibleDC
  * WinAPI - DeleteDC
  * WinAPI - CreateFont
  * WinAPI - MoveWindow
  * WinAPI - GetSysColor
  * WinAPI - BitBlt



  


  * WinAPI - SelectClipRgn (in SelectClipRgn)
  * WinAPI - CreateFont
  * WinAPI - GetDC
  * WinAPI - ReleaseDC
  * WinAPI - BeginPath
  * WinAPI - EndPath
  * WinAPI - PathToRegion
  * WinAPI - GetRgnBox
  * WinAPI - OffsetRgn
  * WinAPI - ExtSelectClipRgn
  * WinAPI - SelectClipRgn



  


  * WinAPI - CallWindowProc (in ColorMixer)
  * WinAPI - CopyRect
  * WinAPI - CreateSolidBrush
  * WinAPI - CreateRectRgnIndirect
  * WinAPI - ExtSelectClipRgn
  * WinAPI - SelectClipRgn
  * WinAPI - ExtTextOut
  * WinAPI - FrameRect
  * WinAPI - GetWindowLongPtr
  * WinAPI - InvalidateRect
  * WinAPI - MoveWindow
  * WinAPI - Rectangle
  * WinAPI - SetFocus
  * WinAPI - SetScrollInfo
  * WinAPI - SetScrollPos
  * WinAPI - SetTextColor
  * WinAPI - TextOut
  * WinAPI - WM_CTLCOLORSCROLLBAR
  * WinAPI - WM_GETMINMAXINFO
  * WinAPI - WM_VSCROLL



  
**WinAPI - ExtTextOut** (example 1 of 2) 

This first example of the ExtTextOut API is similar to the FlickerMin example in post 7 (see post 7 for the source.) 

It demonstrates ExtTextOut's ability to clip text to a rectangle. It clears the background areas as necessary when processing the WM_PAINT message instead of letting the default window procedure clear it when processing the WM_ERASEBKGND. It produces less flicker than FlickerMin because it uses a smaller font. (NOTE: the flicker is easier to see when resizing the window horizontally only.) 

Also note that it does not specify CS_HREDRAW nor CS_VREDRAW in the class styles, instead it invalidates the client area, _without erasing it_ when processing the WM_SIZE message. 

The rectangle coordinates used to erase the background can be output onto a console by recompiling the program with the "DEBUG" define active. This is useful to see/verify the coordinates of the background rectangles that are erased when painting. 

Use the options under the "ExtTextOut" menu to control the clipping of text. 

It also demonstrates how to process the WM_GETMINMAXINFO to control the minimum width and height of the window. 

  
**WinAPI - InvalidateRect**

Carefully controlling which parts of the client area are invalidated can significantly reduce flicker. 

This example shows the effect of invalidating the entire client area compared to the effect of invalidating a smaller area that is the one that needs to be updated. When "Invalidate entire area" is selected in the "InvalidateRect" menu, the amount of flicker will be very noticeable. When "Invalidate coordinate area" is selected, the amount of flicker is drastically reduced and confined to a much smaller area. 

The flicker left is due to the use of TextOut and specifying TRUE in the call to InvalidateRect when processing the WM_MOUSEMOVE message: 
    
    
             InvalidateRect(Wnd, @UpdateRect, TRUE);
    

This causes the coordinate area to be erased thus showing a clear background before TextOut outputs the new coordinate values. 

NOTE: All the coordinates shown are relative to the _client area_ of the respective windows. 

  
**WinAPI - ExtTextOut** (example 2 of 2) 

This example is the InvalidateRect example but with all flicker removed. The absence of flicker is due to the different way the coordinate rectangle is invalidated when processing the WM_MOUSEMOVE message: 
    
    
             InvalidateRect(Wnd, @CoordinateRect, FALSE);  { NO background erase!    }
    

Note, that InvalidateRect is instructed not to require the background to be erased by specifying FALSE (third parameter.) This combined with the fact that ExtTextOut repaints the background and the text in one operation eliminates all flicker in the coordinates area. 

However, it is still possible to see some occasional flicker when first pressing the left mouse button. The default processing of the WM_LBUTTONDOWN message Invalidates the entire client area with the statement: 
    
    
                 InvalidateRect(Wnd, nil, TRUE);
    

This will cause some flicker to be noticeable if the left mouse button is very quickly pressed down and released (about 7 to 10 times per seconds) for a few seconds (5 seconds should normally enough.) 

That occasional flicker can be removed by selecting "Exclude bottom line from invalidated area" which will cause the call to InvalidateRect to be: 
    
    
              MenuItemState := GetMenuState(GetMenu(Wnd), IDM_EXCLUDE, MF_BYCOMMAND);
         
              case MenuItemState <> 0 of
                true  :
                begin
                  { menu item is checked, exclude the bottom of the client area       }
         
                  GetClientRect(Wnd, ClientRect);
         
                  ClientRect.Bottom := ClientRect.Bottom - Exclude;
                  InvalidateRect(Wnd, @ClientRect, TRUE);
                end;
    

thus excluding the area where the ExtTextOut definition is drawn causing all flicker to be eliminated. 

NOTE: the ExtTextOut attachment contains both examples. 

**WinAPI - GetCharacterPlacement** (example 1 of 2) 

ExtTextOut is a powerful function and when it is combined with GetCharacterPlacement its power surfaces. 

GetCharacterPlacement has a large number of options and the two examples presented here do not do it justice but, will give a _small_ taste of what it enables ExtTextOut to do. For all the available options, please see MSDN. 

This example simply takes a string and uses GetCharacterPlacement to determine how to "spread" it over a specified number of pixels. GetCharacterPlacement returns an array of inter-character distances that are used by ExtTextOut to display the string. 

  
**WinAPI - GetCharacterPlacement** (example 2 of 2) 

The first example above is a bit deceiving. The same example using a slightly different criteria allows converting a proportionally spaced font into a fixed space font. Normally, such a conversion is undesirable because the additional inter-character whitespace is unsightly but, there are exceptions and, this example shows one of them. 

One such case is when outputting the value of registers in a debugger. Most fixed fonts have digits that are not particularly easy to read whereas proportional fonts usually have aesthetically pleasing and easy to read digits. 

This example turns any hex number consisting of digits 0-F from proportional to fixed and, for comparison, outputs them as they would in their proportional state on the right hand side and their fixed state on the left hand side. 

NOTE: the GetCharacterPlacement attachment contains both examples. 

  
**WinAPI - BitBlt**

Likely the most common method to eliminate flicker is to paint on a bitmap then bitblt the fully drawn bitmap onto the client area. This removes all flicker. 

This example is the simplest example of BitBlt-ing meant to serve as a pre-amble to a more sophisticated example that uses it to remove flicker. 

Note that this example does NOT use CS_HREDRAW nor CS_VREDRAW and that it uses a NULL_BRUSH as the background color, making it unnecessary to process the WM_ERASEBKGND to tell Windows the program erased the background when it actually didn't. Windows will erase the background with a NULL_BRUSH which does no erasing at all. 

  
**WinAPI - StretchBlt**

StretchBlt is included in this set of examples only because it is very closely related to BitBlt. Since they are close cousins and it is immoral to break a family, they have been kept together as they should be. 

  


**WinAPI - CreateCompatibleBitmap**

This example demonstrates double buffering. The top part of the window is drawn with TextOut (and optionally ExtTextOut by re-compiling - see source) thus causing flicker while, the bottom part of the window is first drawn on a bitmap then BitBlt-ed onto the bottom half of the window. The result is that the bottom part of the window is totally flicker free. 

To really appreciate the difference, resize the window. When resizing, the top part of the window will flicker significantly while the bottom part won't flicker at all. 

  
**WinAPI - SelectClipRgn**

This example demonstrates how to use regions to eliminate flicker. Note that it does NOT use double buffering, only regions. 

It creates a region that is excluded from the "paintable" region when drawing the background gradient. After the background gradient has been painted then that region is made the "paintable" region and a different gradient is painted in it. 

The inclusion and exclusion of the region is controlled by the options under the "Flicker" menu. When the region is excluded, there will be no flicker and, when it is included, flicker will be noticeable when resizing the window. 

  
**Utility - ColorMixer**

This example which doubles as a simple color mixing/selection utility demonstrates how to eliminate flicker using ExtTextOut, Regions and InvalidateRect. 

It uses all the methods to prevent flicker except double buffering. 

In addition to that, it demonstrates how to use scroll bars as controls, how to subclass a window (in this case the scroll bars), how to handle keyboard input to set and select which scroll bar has the focus, as well as, setting the numeric value for each color (use arrow keys, home/end, page up/down), and how to process the WM_GETMINMAXINFO to set the minimum, maximum and maximized sizes of a window. 

Aside from the 3 scroll bars that control the settings of the Red, Green and, blue colors, there is only _one_ window. The right and left hand side are _not_ separate windows. 

The example provides the option of displaying the coordinates of the various rectangles used to eliminate flicker by enabling the SHOW_RECTANGLE_COORDINATES define. 

It also allows controlling whether or not the sample text will flicker by enabling or disabling the define NO_SAMPLE_TEXT_FLICKER. 

The numeric values of Red, Green and Blue shown below the scroll bars are _not_ protected against flicker. Grabbing any of the scroll bar thumbs and moving it up and down quickly for several seconds while paying close attention to the numbers will make a very slight amount of flicker visible. Because the amount of flicker is usually not noticeable, there is no code to prevent it. However, the same method used to ensure the sample text does not flicker can be used with the Red, Green and Blue values for a totally flicker free utility. 

ColorMixer uses its own definitions for most of the APIs it uses. The purpose of the definitions is to minimize the number of useless hints and give better parameter names, which are used by CODETOOLS, thereby providing better guidance when programming. 

  
[![winapi ColorMixer.png](https://wiki.freepascal.org/images/6/6f/winapi_ColorMixer.png)](</File:winapi_ColorMixer.png>)

  


**Dead BUG**

There is a "dead" bug in DrawWindowFrame. That function appears in multiple examples presented in Post 6. The bug is as follows: 
    
    
          SelectObject(dc, OldPen);          { restore the original pen               }
          ReleaseDC(Wnd, dc);
          DeleteObject(Pen);                 { get rid of the pen                     }
         
          {---------------------------------------------------------------------------}
          { release the window dc                                                     }
         
          ReleaseDC(dc, Wnd);
        end;
    

The last statement "ReleaseDC(dc, Wnd);" is erroneous for two reasons. First, the dc has already been released (just a few lines above) and second, the parameters in that call are reversed. The erroneous statement has no effect on the proper operation of the examples in which it appears but should be removed. Corrected code should read: 
    
    
          SelectObject(dc, OldPen);          { restore the original pen               }
          ReleaseDC(Wnd, dc);
         
          DeleteObject(Pen);                 { get rid of the pen                     }
        end;
    

The examples affected by the above bug are : 

  * GetDesktopWindow.lpr
  * GetShellWindow.lpr
  * GetTaskmanWindow.lpr
  * GetTopLevelWindow.lpr
  * IsTopLevelWindow.lpr
  * GetWindowTextLength.lpr



Corrected versions of the source files above are attached to this post in the file named "CorrectionsToPost6Examples.7z" (NOTE: only examples presented in Post 6 are affected.) 

  


## Post 9

(to be filled later)

---

_Source: [https://wiki.freepascal.org/Windows_API_examples](https://web.archive.org/web/20241212111219/https://wiki.freepascal.org/Windows_API_examples)_
