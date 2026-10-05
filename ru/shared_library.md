# shared library

│ **[Deutsch (de)](</shared_library/de> "shared library/de")** │  **[English (en)](<../en/shared_library.md> "shared library")** │  **[español (es)](</shared_library/es> "shared library/es")** │  **[한국어 (ko)](</shared_library/ko> "shared library/ko")** │  **русский (ru)** │    
****

Общая библиотека - это скомпилированный фрагмент кода, который может использоваться разными программами. Он предоставляет функции и процедуры, которые могут вызывать другие программы. Она отличается от статической библиотеки (которая связана с исполняемым файлом и становится его частью) или исполняемого файла. Общая библиотека в этой статье обозначается как библиотека для включения как Linux .so, так и Windows .dll, если явно не указано иное, например, «разделяемая библиотека unix» или «dll». 

Эта страница пытается задокументировать, как общие библиотеки работают в сочетании с FPC. То есть: как сейчас работают разделяемые библиотеки, как они должны работать, а также как с ними работает Delphi. 

Начнем с простого наброска того, какие формы поддерживает Delphi, потому что мы, конечно, постараемся быть максимально совместимыми (ограниченными требованием работать на нескольких платформах). 

В этой статье не показаны подробности Kylix; пожалуйста, не стесняйтесь добавлять детали для Kylix, если они все еще актуальны. 

## Contents

  * 1 Delphi
  * 2 Пространства имен компоновщика
  * 3 Подробности реализации Sharemem
  * 4 VMT duplication
  * 5 Initialization and finalization sections and RTL
  * 6 Shared Exe Memory manager
  * 7 Converting from Delphi
    * 7.1 dllproc
  * 8 Using shared libraries from FPC
    * 8.1 Initialization order
  * 9 Using FPC libraries from other languages
    * 9.1 Returning string results as PChars
    * 9.2 Exposing objects using a wrapper
    * 9.3 .NET
    * 9.4 Java
  * 10 See also
  * 11 References



## Delphi

Delphi, насколько мне известно, знает три (или четыре) способа динамического связывания. 

  1. создание автономной разделяемой библиотеки (компиляция модуля библиотеки без выбранных «пакетов времени выполнения»). 
     * Это означает, что RTL будет связан с общей библиотекой
     * Это также означает, что менеджер памяти будет отдельным островком. Использование автоматизированных типов в функциях, которые взаимодействуют с ним, невозможно.
     * Использование классов невозможно: и программа, и разделяемая библиотека имеют свои собственные копии VMT, которые разделены, например, операторами _is_ и _as_
  2. создание автономной разделяемой библиотеки (компиляция модуля библиотеки без выбранных "пакетов времени выполнения"), но пока используется модуль **sharemem**) 
     * Сравнимо с предыдущим случаем, но диспетчер памяти переключен в COM-совместимый режим. Это означает, что другие компоненты/программы, которые также переключают свой диспетчер памяти на **sharemem** , могут вызывать функции, использующие автоматические типы, потому что не имеет значения, кто возвращает блок в систему управления памятью COM.
     * Обратите внимание, что (насколько мне известно) диспетчер памяти COM довольно медленный. Этот путь, вероятно, нежелателен, если вы действительно не хотите возиться с COM, или ваша компонентность более важна, чем скорость.
  3. [Пакеты библиотек](<../en/packages.md> "packages") \- это разделяемые библиотеки, для которых известны все зависимости на уровне Паскаля (какие модули они содержат и от которых они зависят), и их можно рассматривать как части основной программы, которые находятся в DLL. 
     * RTL находится в отдельном пакете (DLL), и его используют как основная программа, так и пакет. Следовательно, существует только одна копия любого модуля, включая системный.
     * Это также означает, что существует только один менеджер памяти, по крайней мере, для основной программы и используемых ею пакетов. Общие библиотеки IOW, которые не являются пакетами, могут по-прежнему использовать собственный RTL и диспетчер памяти.
     * Пакет может зависеть только от модулей в своем собственном пакете или в других пакетах (отдельное требование компиляции)
     * Поскольку никакие модули не дублируются, существует только одна копия каждого VMT, что делает использование классов прозрачным.
     * Возможно, пакеты также могут переключиться на **sharemem** , что сделает его совместимым с другими системами, использующими **sharemem**. Тем не менее, необходимо найти способ инициализировать **sharemem** как можно раньше (делает ли это Delphi? Возможный тест для этого - передать `ansistring`, созданный в разделе инициализации модуля в пакете, другому **sharemem** , используя общую библиотеку, которая не является пакетом) - **будет определено позднее**



Помимо этого, Delphi также может создавать библиотеки DLL, которые являются компонентами ActiveX. **будет описано/определено**

Темы о пакетах библиотек по большей части перенесены с этой страницы в отдельную статью [пакеты](<../en/packages.md> "packages"), чтобы не запутать обсуждение. 

## Пространства имен компоновщика

Одна из больших проблем при переносе динамических библиотек из Windows (например, DLL) в Linux и FreeBSD заключается в том, что Windows и OS X имеют пространство имен компоновщика для каждого модуля (например, для общей библиотеки или двоичного файла), при этом таблицы экспорта тщательно регулируют экспортируемые символы, а Linux и FreeBSD имеют только одно пространство имен компоновщика. 

Это является проблемой, особенно в многоязычных проектах. 

Я получил некоторые отзывы об этом от хакера FreeBSD, который порекомендовал изучить атрибут видимости ELF и/или управление версиями API: 

  * <http://gcc.gnu.org/wiki/Visibility>
  * <http://people.freebsd.org/~deischen/symver/freebsd_versioning.txt>



Обратите внимание: хотя последний URL-адрес посвящен FreeBSD, он упоминает, что Linux делает то же самое. 

## Подробности реализации Sharemem

Как уже говорилось, `sharemem` переключает менеджер памяти на глобальный. Под Windows это, насколько мне известно, COM-менеджер. В *nix подобный `memmanager` не всегда существует (архитектуры компонентов Gnome и KDE могут иметь нечто подобное), поэтому это не гарантируется. 

Вероятно, можно было бы просто использовать `cmem` во всех программах, что могло бы быть реализацией «уровня 0» для всего, что выполняется в этом процессе. 

Я прямо упоминаю об этом, потому что может потребоваться ввести собственный порядок инициализации (независимо от инициализации разделяемой библиотеки ОС), чтобы позволить основной программе инициализировать модули сразу после инициализации RTL (модуля `system`), но перед другими библиотеками, например другой менеджер памяти. 

## VMT duplication

The basic problem of VMT duplication is mostly support for the _is_ parameter and similar methods of _TObject_ and _TClass_ like _inheritsfrom_. This is only solved for packages, IOW for the other two types named above, the _is_ operator doesn't work across libraries/binaries. 

In earlier discussions about packages, there was some confusion about this topic. People assumed that packages would somehow tap into the RTL VMTs in the main binary. I'm however pretty sure this is not the case, at least not in Delphi, since if you use packages, the RTL always is a package, too. Also, every dependency of a unit in a package must be in the same package or in a package it has a dependency on. This means a unit (and thus, the VMTs declared in it) only exists once in the greater program (main program + its packages) 

## Initialization and finalization sections and RTL

(The initialization order of units in packages is moved to the packages page). 

The problem with libraries is usually that libraries are initialized as a whole. This can lead to problems with plugin units modifying RTL behaviour (cwstrings, cmem, FV drivers etc) 

For stand-alone libraries with internal copies of RTL to function, the RTL needs to be initialized, and the initialization section of the library needs to be called as well. 

Roughly there are two options here: 

  * Use (e.g. on ELF platforms) the ABI .init and .fini sections or similar constructs in other binary formats.
  * Leave the above initializers mostly empty sections, and enforce an own order using a set of own initializers and finalizers. The ELF init and fini sections merely register the real initializers.



Mixed forms are also possible, e.g. initialize standalone shared libraries via the initializer sections, but with packages use an own definition. 

## Shared Exe Memory manager

**Lars says:** regarding a single memory manager for BPL style packages: what about using the executable and exporting its memory manager? I have had problems trying to use CMEM but have successfully shared the same memory manager using SetMemoryManager/GetMemoryManager tricks. 

See <http://www.freepascal.org/contrib/delete.php3?ID=543> for an example of sharing memory between exe and dll without using CMEM. Powtils CGI library also uses this trick for dynpwu.pas so that ansistrings can be used without any sharemem or cmem unit. The memory manager is exported from the executable or a single library and shared with all the other DLL/EXE's that are connecting to the single module. 

**Marco's answer:** First, library packages have more features than just shared memory managers. Unique VMTs (try to use the _is_ operator in your example on a class created in the other lib/exe) and no need to handcraft proper initialization. One just groups units into libs, and the compiler does the rest. No special code required. So what must be done for (library) packages is different (and non competing) to the more general shared library case. And that is what this page is about. 

From this general "shared library" case, a subdivision can be separated that has a shared memory manager. 

Note that there can be a bunch of memory managers used for this, and multiple units that work like sharemem. Nearly any memory manager applies. However it is logical to reserve the "sharemem" unit name for the memory manager that is the most compatible for inter-process work on the given platform (COM on Windows, cmem on Unix). So, I'm talking about the default case here, which doesn't mean that other people can't roll their own. 

Also, in-compiler (RTL) support must be as universal as possible, and also work for libraries that are not _loadlibrary_ -ed, but autoloaded at binary startup, and not require handcoding e.g. initialization. Also it mustn't stand in the way of handcoded solutions (like the one you reference), in other words it must be overridable, since otherwise in-compiler (RTL) support would stand in the way of what people build on top of FPC. 

It would be useful though, if somebody investigate if it is possible to call exported symbols from the binary from an autoloaded library (for the Tier 1 targets, Linux/FreeBSD - Windows - OS X). This because then a library could maybe indeed plug in the mother binary. 

In short the first objective is to simply have a default scheme that works like Delphi's sharemem to put in the "sharemem" unit. 

**Marco, added later after some new insights**

Moreover a difference is also if the units in the dll are accessed directly, or only over a separately (additionally) defined interface. 

**Lars says:** Well another idea: instead of exporting the memory manager from the exe I was thinking about making a fpsharemem.dll that exports one single common FPC memory manager on each platform for all the programs and DLLs to use, instead of using CMEM. i.e. all units put fpsharemem.dll in their uses clause. In fact, that's actually what the demo does but instead of a separate fpsharemem.dll it is just inside that single DLL included with the demo and all the other code. i.e. why use CMEM if we can use freepascal memory manager? The freepascal mem manager is available on all platforms isn't it, as long as there is a FreePascal DLL that is created for each platform? 

I guess to answer my own questions it must have to do with COM/IPC (inter process communication) issues/compatibilities. Well anyway, still a very interesting discussion. Doing it by hand the way I did was just a demo to show the concept in action and to prove to people that ansistrings and automated types can be used in regular old DLLs. 

**Marco:** If you have to make one (thing? memory manager?) mandatory, make it cmem, because otherwise you might not be able to interface with some shared libs. If you can somehow make it user selectable it would be better. But keep in mind that it is already hard to pack two precompiled sets of units into a release (increasing to 200MB installed size or so), let alone more. 

Roughly this scheme is what sharemem does, communication over COM/IPC to make sure that a second instance of a library doesn't instantiate its state twice. It avoids the DLL with the memmanager itself by using COM as memory manager. (Something that wouldn't work on Unix, since there is no such global memory manager). 

I've thought about this in the past too, but once you start adding a few more things (like _is_ operator support, and something end-user supportable for people that can't write a header to a DLL), you end up with effectively packages again. It is really not that much more, runtime, just a few tables to govern proper unit initialization. 

## Converting from Delphi

### dllproc

In Windows DLLs, Delphi allows you to assign your own procedure to the DLLProc hook to function as a DLL entry point. That procedure can handle: 

  * DLL_PROCESS_ATTACH
  * DLL_PROCESS_DETACH
  * DLL_THREAD_ATTACH
  * DLL_THREAD_DETACH



FreePascal does not have this functionality, but on Windows you can handle three of them: 

  * Dll_Process_Detach_Hook
  * Dll_Thread_Attach_Hook
  * Dll_Thread_Detach_Hook



## Using shared libraries from FPC

See the FPC documentation (to do: insert link). On Windows, the bitness of the program you are writing (e.g. 64 bit application) must match the bitness of the DLL you are using. 

Debugging FPC libraries called by an FPC program: if you are using Lazarus: 

  * Open your library project
  * Go to Run/Run Parameters, enter the full path of the application that uses your library in Launching Application
  * Check the "Use launching application".
  * Set the breakpoints as usual and Run the program.



### Initialization order

Warning: you should not initialize resources like a db engine that use LoadLibrary to load external libraries in `initialization` sections or finalize them in `finalization` (maybe only as last chance) because this is not supported by the underlying operating system. See [Issue #26801](<https://bugs.freepascal.org/view.php?id=26801>). 

## Using FPC libraries from other languages

The possibility to create a shared library in FPC and being able to use it in other languages (typically but not limited to C/C++) is one of the key goals of having the idea of shared libraries. 

Official documentation: FPC programmer's guide, chapter 12. 

In general 

  * keep you library procedural; do not rely on OOP (Object-Oriented Programming)
  * use simple types in the interface procedures, **avoid** usage of 
    * ANSI / Unicode strings (non OLE-bases). ShortStrings are ok.
    * explicit dynamic strings (open arrays are ok)
    * classes
  * OOP implementation is not portable due to compiler dependent implementation (except for Objective C). Even worse, objects might not be compatible between different versions of the same compiler (i.e. TObject for FPC 2.4.x is not compatible to TObject for 2.6.x)



More specific information about which types can be exported: 

  * var / const / out parameters 
    * var - a variable is always passed by reference
    * const - might be passed as reference (depending on the size of the variable and target CPU platform (ABI))
    * out - a variable is always passed by reference
    * if a parameter is passed by reference, it's typically a pointer reference in C
  * Records - yes, they are compatible with most of the languages. Do keep in mind the "packing" of the record. The best approach is to use "packed record" (the data will be allocated in the most compact way).
  * ShortStrings (fixed size strings) - yes - as a simple array of 256-characters (bytes), the first byte in the array is the number of characters
  * Strings (dynamic strings: AnsiString) - incompatible - do not export; Consider using PChar instead (see example below)
  * WideStrings (Windows ONLY - based on OLE) - could be used. Other languages would need to support OLEStrings as well (and handle them through Windows OLE utilities)
  * WideStrings (non Windows), Unicode strings - incompatible - consider using PWideChar instead;
  * Classes - incompatible. Consider using wrapper functions'
  * Objects (old ones) - could be done, but you'll need to export all methods as separate functions/procedures
  * Open Arrays - yes, but a single "open array" parameter is in fact 2 parameters - a pointer to the array and the size of the array (in elements). Example:


    
    
     // this declaration cannot be converted to C, as C doesn't have "open arrays"
     function Calculation(const params: array of TSomeData; a, b : integer): Integer;
    

could be called as 
    
    
     // this declaration is easily converted to C
     function _Calculation(params: PSomeData; paramscount: Integer; a, b : integer): Integer;
    

  * StaticArrays - yes - _const_ might, if _var_ will, be passed as a reference, rather than an array itself;
  * DynamicArrays - **dynamic arrays are different from open arrays parameter** \- not portable. consider using arrays or create wrappers;
  * Interfaces COM/CORBA - yes.
  * Procedure types - yes - but keep in mind the calling convention, since a callback function call would need to use it as well.



  


### Returning string results as PChars

This example illustrates the use of PChar to return string-type results: 
    
    
    function InLibStringFunction: PChar; cdecl;
    const
      MyString: AnsiString = 'Hello World';
    begin
      result := PChar(MyString);
    end;
    

The DLL's memory manager will allocate memory for the PChar. This means that your application must not free that memory. It needs to be freed by the DLL. This is not a problem if "MyString" does not change through the complete lifetime of your application. 

If you want your application to be able to free the PChars then you can allow your main application to free the memory via the library. 

So in that case use this kind of function: 
    
    
    function InLibStringFunction: PChar; cdecl;
    const
      MyString: AnsiString = 'Hello World'; 
    begin
      Result := GetMem(Length(MyString) + 1);
      Move(@MyString[1], Result, Length(MyString));
      Result[Length(MyString)] := 0;
    end;
    

And, for freeing the allocated memory, you may add this procedure in the library, which you can then call from your main application : 
    
    
    procedure InLibFreeString(aStr: PChar);
    begin
      FreeMem(aStr);
    end;
    

It is a good idea to store the pointer values of the allocated PChar results (e.g. TList or specialize TFPGList<PChar>) so that you can use this inside your exported FreeString function to check whether this is indeed a pointer that you need to free. 

### Exposing objects using a wrapper

If you need to expose an object, you will need to create wrapper functions. These wrapper functions would accept a pointer parameter, cast it to the object and call the method. Sanity checks are advised. 

This is also known as "flattening" the object (to a procedural API/interface). 

Keep in mind that you will need to wrap/flatten: 

  * object methods
  * object properties
  * object fields (Note: BigChimp: what is an object field? Do you mean property?)


    
    
    library libtest;
    
    uses
      Classes, ctypes, Math, SysUtils; 
    
    type
    TMyClass = class(TThread)
      public
      enabled1: boolean;
      int1, int2: shortint; 
      function function1(): integer; cdecl; 
    end;
    
    function TMyClass.function1(): integer; cdecl; 
    begin
      if enabled1 then
        result := int1 + int2 else result := -1;
    end; 
    
    exports
      TMyClass.function1(); 
    end.
    

Chould be "wrapped" like this: 
    
    
    library libtest;
    
    uses
      Classes, ctypes, Math, SysUtils; 
    
    type
    TMyClass = class(TThread)
      public
      enabled1: boolean;
      int1, int2: shortint; 
      function function1(): integer; cdecl; 
    end;
    
    function TMyClass.function1(): integer; cdecl;  
    begin
      if enabled1 then
        result := int1 + int2 
      else 
        result := -1;
    end; 
    
    function MyClassCreate:Pointer; cdecl;
    begin
      Result:=TMyClass.Create;
    end;
    
    function MyClassFree(obj:Pointer); cdecl;
    begin
      TMyClass(Obj).free;
    end;
    
    function MyClassSetEnabled(obj:Pointer; aenabled: Boolean); cdecl;
    begin
      TMyClass(Obj).enabled:=aenabled;
    end;
    
    function MyClassGetEnabled(obj:Pointer): Boolean; cdecl;
    begin
      Result:=TMyClass(Obj).enabled;
    end;
    
    function MyClassSetData(obj:Pointer; i1, i2: shortint); cdecl;
    begin
      TMyClass(Obj).int1:=i1;
      TMyClass(Obj).int2:=i2;
    end;
    
    function MyClassGetData(obj:Pointer; var i1, i2: shortint); cdecl;
    begin
      i1:=TMyClass(Obj).int1;
      i2:=TMyClass(Obj).int2;
    end;
    
    function MyClassFunction1(obj:Pointer): Integer; cdecl;
    begin 
      Result:=TMyClass(Obj).function1
    end;
    
    
    exports
      TMyClassCreate, MyClassFree, MyClassSetEnabled, MyClassGetEnabled
      ,MyClassSetData, MyClassGetData, MyClassFunction1;
    end.
    

By the way, the same "wrapper"/flattening approach is taken when a C++ library (with classes) needs to be accessed from FPC (or any other language) 

### .NET

More information [here](<../en/Using_Pascal_Libraries_with_.md> "Using Pascal Libraries with .NET and Mono")

### Java

More information [Using Pascal Libraries with Java](<../en/Using_Pascal_Libraries_with_Java.md> "Using Pascal Libraries with Java")

## See also

  * [Lazarus/FPC Libraries](<../en/Lazarus/FPC_Libraries.md> "Lazarus/FPC Libraries")



## References

  * FPC programmer's guide, chapter 12: FPC documentation on programming shared libraries
  * <http://www.iecc.com/linker/>
  * <ftp://ftp.freepascal.org/pub/fpc/docs-pdf/CinFreePascal.pdf>

---

_Source: [https://wiki.freepascal.org/shared_library/ru](https://web.archive.org/web/20241104201013/https://wiki.freepascal.org/shared_library/ru)_
