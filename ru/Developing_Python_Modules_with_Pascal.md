# Developing Python Modules with Pascal

│ **[English (en)](<../en/Developing_Python_Modules_with_Pascal.md> "Developing Python Modules with Pascal")** │  **русский (ru)** │ 

## Contents

  * 1 Введение
  * 2 Минимальное Python API
  * 3 Пример простого модуля
  * 4 Использование вашего модуля в серверных приложениях
  * 5 Смотрите также



## Введение

Python — популярный скриптовый язык, который часто используется для добавления функциональности другими приложениями, такими как [OpenOffice](<https://ru.wikipedia.org/wiki/OpenOffice.org>) и [Quantum GIS](<https://ru.wikipedia.org/wiki/QGIS>). На вашем компьютере уже может быть установлена какая-то версия Python. Если нет, можно загрузить Python с официального вебсайта: <http://www.python.org/>. 

Можно расширять Python путём разработки скомпилированных библиотек (называемых модулями), которые добавляют функции в Python. В этом разделе рассматривается как создать библиотеку на Pascal (Delphi или Free Pascal). 

Эта статья описывает очень низкий уровень, подход «с нуля». Для установки моста Python-Pascal, смотрите [Python4Delphi/ru](<Python4Delphi.md> "Python4Delphi/ru"). 

## Минимальное Python API

Скопируйте и сохраните с помощью текстового в файл PyAPI.pas: 
    
    
    unit PyAPI;
     
    { 
     
      Минимальный набор деклараций функций Python для библиотек модулей.
     
      Author: Фил (MacPgmr на fastermac.net).
     
      Для добавления других деклараций функций Python, смотрите заголовочные файлы (.h), включённые в любой дистрибутив Python.
     
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
      PythonLib = '';  //Связывает с Python.framework (-k'-framework Python').
                       // Для ссылки на конкретную версию Python, передайте
                       // полный путь к этой версии библиотеки, например,
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
      c_int = Int64;  //"int" также будет 8-байтным в 64-битном Python
    {$ENDIF}
     
      PyMethodDef = packed record
        name  : PAnsiChar;  //имя функции Python
        meth  : Pointer;    //Адрес реализуемой функции
        flags : c_int;      //METH_xxx флаги; описывает аргументы функции
        doc   : PAnsiChar;  //Описание функции
        end;
     
      PyObject = Pointer;
    
    const
    {$IFDEF USE_PYTHON23}
      PYTHON_API_VERSION = 1012;  //Используется также вместе с Python 2.4
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
     //Заметьте, что varargs позволяет нам эмулировать переменное количество аргументов в  C (...).
     
    function PyInt_FromLong(along : c_long) : PyObject; cdecl; external PythonLib;
     
    function PyLong_FromLong(along : c_long) : PyObject; cdecl; external PythonLib;
     
    function PyLong_FromUnsignedLong(aulong : c_ulong) : PyObject; cdecl; external PythonLib; 
    
    function PyString_FromString(astr : PAnsiChar) : PyObject; cdecl; external PythonLib;
     
    implementation
     
     
    end.
    

Если у вас разные версии Python, определите USE_PYTHON23 как ссылку на версию 2.3 или просто измените имя библиотеки (PythonLib). Если вам нужны другие функции API Python, просто добавьте их PyAPI.pas, следуя примеру PyInt_FromLong. 

## Пример простого модуля

Вот простая библиотека, которая использует модуль Python API. Скопируйте этот код и сохраните его в файле PyMinMod.dpr: 
    
    
    library PyMinMod;
     
    {
     
      Минимальный модуль (библиотека) Python, включающий простые функции.
     
      Автор: Фил (MacPgmr на fastermac.net).
     
      Для компиляции этого модуля:
        - На Delphi: Откройте этот файл .dpr и скомпилируйте.
        - На Lazarus: Откройте файл .lpi и скомпилируйте.
     
      Для раздачи модуля:
        - С Delphi: Переименуйте скомпилированный .dll в .pyd.
        - С Lazarus на Windows: Переименуйте скомпилированный .so в .pyd.
        - С Lazarus на OS X и Linux: Оставьте расширение .so extension.
     
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
      PyArg_ParseTuple(Args, 'ii', @Arg1, @Arg2);  //Получает 2 аргумента типа int
      Result := PyInt_FromLong(Arg1 + Arg2);  //Суммируем их и возвращаем sum
    //  Result := PyLong_FromLong(Arg1 + Arg2);
    //  Result := PyLong_FromUnsignedLong(Arg1 + Arg2);
    end;
     
     
    function ConcatTwoStrings(Self : PyObject;
                              Args : PyObject) : PyObject; cdecl;
     {Из документации Python про формат «s»: «Вы не должны выделять память под саму строку; указатель 
      на существующую строку сохранён в переменной типа указатель на символ (character pointer) чей адрес вы передаёте.»
      Из документации Python по PyString_FromString: «Вернуть новый строковый объект с копией строки v в случае успеха».
     }
    var
      Arg1 : PAnsiChar;
      Arg2 : PAnsiChar;
    begin
      PyArg_ParseTuple(Args, 'ss', @Arg1, @Arg2);  //Получить два строковых аргумента
      Result := PyString_FromString(PAnsiChar(AnsiString(Arg1) + AnsiString(Arg2)));  
                 //Соединить и вернуть строку
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
    

Можно добавить в этот модуль ещё функций по примеру в initPyMinMod. 

На Delphi, просто откройте PyMinMod.dpr и скомпилируйте. 

На FPC, просто скомпилируйте из командной строки. Например, чтобы создать 64-битный модуль на OS X: 
    
    
    ppcx64 -Sd -k'-framework Python' -oPyMinMod.so PyMinMod.dpr
    

На Lazarus, вы, возможно, захотите создать файл проекта. Можете сделать его сами или просто скопировать этот проект и сохранить его в файле PyMinMod.lpi: 
    
    
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
    

После компиляции модуля переименуйте его, если необходимо, как указано в комментариях к PyMinMod.dpr. Затем протестируйте модуль, создав простой файл test.py, который содержит эти три строки: 
    
    
    import PyMinMod
    print "Value returned by SumTwoIntegers: " + str(PyMinMod.SumTwoIntegers(1, 2))
    print "Value returned by ConcatTwoStrings: " + PyMinMod.ConcatTwoStrings("Hey ", "there")
    

Учтите, что Python различает большие и малые буквы, так что если ваш скомпилированный модуль в нижнем регистре, измените соответственно ссылку на "PyMinMod". 

Теперь откройте окно терминала и запустите такой скрипт: 
    
    
    python test.py
    

Скрипт должен выдать такую строку: 
    
    
    Value returned by SumTwoIntegers: 3
    Value returned by ConcatTwoStrings: Hey there
    

Здесь мы создали простой модуль Python, реализующий две функции Python. После того как вы импортировали модуль в свой скрипт Python, вы можете использовать функции так же, как и встроенные функции Python. 

## Использование вашего модуля в серверных приложениях

Если у вас установлены OpenOffice или NeoOffice, можете протестировать ваш модуль с помощью макроопределений на Python. 

Сохраните этот скрипт в файл test_minmod.py, поместите его в папку, указанную в скрипте. Возможно вам придётся создать папку python\Library1 в разделе Scripts. 
    
    
    # Макроопределение Python, тестирующее модуль на Pascal. Создаёт новый документ и вставляет результат функции модуля.
    
    import sys, os
    # Укажем Python где искать наш модуль на Pascal
    if sys.platform == 'win32':
      sys.path.append(os.path.expanduser('~\Application Data\OpenOffice.org2\user\Scripts\python\Library1'))
    elif sys.platform == 'darwin':
      sys.path.append(os.path.expanduser('~/Library/Preferences/NeoOffice-2.2/user/Scripts/python/Library1'))
    
    # Импортировать модуль Pascal, содержащий функцию SumTwoIntegers
    import PyMinMod
    
    import uno
    
    def TestMinMod():
      ctx = uno.getComponentContext()
      smgr = ctx.ServiceManager
      desktop = smgr.createInstance('com.sun.star.frame.Desktop')
      doc = desktop.loadComponentFromURL('private:factory/swriter', '_blank', 0, ())
      textCursor = doc.Text.createTextCursor()
      doc.Text.insertString(textCursor, 'Sum of 1 + 2 = ' + str(PyMinMod.SumTwoIntegers(1, 2)), 0)
    

Поскольку OpenOffice вероятно включает Python 2.3, компилируйте модуль для версии 2.3, дополнив на вкладку Other диалога Lazarus Compiler Options: 
    
    
    -dUSE_PYTHON23
    

В Delphi, введите USE_PYTHON23 на вкладке Directories/Conditionals диалога Project Options. 

Если надо переименуйте свой модуль, поместите его в ту же папку, что и test_minmod.py. Теперь проверьте, запустив его из OpenOffice выбрав Tools | Macros | Organize Macros | Python и запустив макроопределение TestMinMod. 

## Смотрите также

  * [Python4Delphi/ru](<Python4Delphi.md> "Python4Delphi/ru")

---

_Source: [https://wiki.freepascal.org/Developing_Python_Modules_with_Pascal/ru](https://web.archive.org/web/20241207104947/https://wiki.freepascal.org/Developing_Python_Modules_with_Pascal/ru)_
