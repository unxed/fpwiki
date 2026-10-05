# Logging exceptions

│ **[English (en)](<../en/Logging_exceptions.md>)** │  **русский (ru)** │

  


## Contents

  * 1 Вступление
    * 1.1 Модуль SysUtils
    * 1.2 Модуль System
      * 1.2.1 Выводимая информация
      * 1.2.2 Stabs
      * 1.2.3 DWARF
    * 1.3 Модуль LCLProc
  * 2 Сброс текущего стека вызовов
  * 3 Сброс исключений стека вызовов
  * 4 Обработка исключений
    * 4.1 Ручная обработка исключений
    * 4.2 ExceptProc из модуля System
    * 4.3 Событие TApplication.OnException
    * 4.4 Обработка исключений доп.потока
    * 4.5 Использование map-файла
    * 4.6 Исключения в DLL
  * 5 См.также
  * 6 Внешние ссылки



## Вступление

**Stacktrace** иногда называют **backtrace** или **call stack**. Это список снимков стека, помещенных в стек, содержащих адрес возврата и локальные переменные. Поэтому трассировка стека полезна для отслеживания пути выполнения вашей программы (после вызова процедур). 

Чтобы получить трассировку стека, компилятор сначала должен обратиться к сгенерированной отладочной информации с использованием ключей: 

  * -g - генерировать отладочную информацию (в выходном формате по умолчанию для соответствующей платформы, который является dwarf2 на многих платформах)
  * -gl - генерировать номера строк для отладочной информации
  * -gs - генерировать отладочную информацию для устаревшего режима stabs; не используйте одновременно оба ключа -gs и -gw
  * -gw - генерировать отладочную информацию dwarf2; не используйте одновременно оба ключа -gs и -gw
  * -Xg - использовать внешний файл символов отладки



  


### Модуль SysUtils

Этот модуль содержит некоторые процедуры, полезные для отладки исключений. 
    
    
    { Процедуры обработки исключений }
    function ExceptObject: TObject;
    function ExceptAddr: Pointer;
    function ExceptFrameCount: Longint;
    function ExceptFrames: PPointer;
    

### Модуль System

Этот модуль содержит некоторые связанные со стеком процедуры: 
    
    
    function SysBackTraceStr(Addr:Pointer): ShortString; // Адрес по умолчанию для преобразования строки, назначенный BackTraceStrFunc
    procedure Dump_Stack(var f : text;bp:pointer); // Дамп стека в текстовый файл
    procedure DumpExceptionBackTrace(var f:text); // Дамп backtrace в текстовый файл
    

Процедурная переменная **BackTraceStrFunc** отвечает за преобразование адреса памяти в строковую отладочную информацию. Поведение по умолчанию реализовано с помощью **SysBackTraceStr**. 

#### Выводимая информация

Если выбран вывод отладочной информации в виде строк (переключатель компилятора -gl), в программу автоматически включается модуль **lineinfo**. Этот модуль гарантирует, что отладчики/обработчики исключений смогут найти номера строк выполняющегося кода. Это может быть полезно, если вы не хотите развертывать получающийся исполняемый файл с полной отладочной информацией, но вам нужна полезная информация при возникновении ошибок. 

#### Stabs

Если используется старый формат stabs (-gs), функция **BackTraceStrFunc** переназначается на **StabBackTraceStr**. 

#### DWARF

Если выбран формат отладки dwarf (ключ компилятора -gw), в программу автоматически включается модуль **lnfodwrf** , а функция **BackTraceStrFunc** преобразуется в **DwarfBacktraceStr**. 

### Модуль LCLProc

Этот модуль Lazarus имеет некоторые функции, связанные с отладкой: 
    
    
    //Отладка
    procedure RaiseGDBException(const Msg: string);
    procedure RaiseAndCatchException;
    procedure DumpExceptionBackTrace;
    procedure DumpStack;
    function GetStackTrace(UseCache: boolean): string;
    procedure GetStackTracePointers(var AStack: TStackTracePointers);
    function StackTraceAsString(const AStack: TStackTracePointers;
                                UseCache: boolean): string;
    function GetLineInfo(Addr: Pointer; UseCache: boolean): string;
    

## Сброс текущего стека вызовов

См. также справку FPC [для получения] подробностей по сбросу стека/исключений: 

  * [DumpExceptionBackTrace](<http://www.freepascal.org/docs-html/rtl/system/dumpexceptionbacktrace.html>)
  * [dump_stack](<http://www.freepascal.org/docs-html/rtl/system/dump_stack.html>)


    
    
    procedure DumpCallStack;
    var
      I: Longint;
      prevbp: Pointer;
      CallerFrame,
      CallerAddress,
      bp: Pointer;
      Report: string;
    const
      MaxDepth = 20;
    begin
      Report := '';
      bp := get_frame;
      // Этот трюк пропустит элемент SendCallstack
      // bp:= get_caller_frame(get_frame);
      try
        prevbp := bp - 1;
        I := 0;
        while bp > prevbp do begin
           CallerAddress := get_caller_addr(bp);
           CallerFrame := get_caller_frame(bp);
           if (CallerAddress = nil) then
             Break;
           Report := Report + BackTraceStrFunc(CallerAddress) + LineEnding;
           Inc(I);
           if (I >= MaxDepth) or (CallerFrame = nil) then
             Break;
           prevbp := bp;
           bp := CallerFrame;
         end;
       except
         { предотвратить бесконечный сброс, если произошло исключение}
       end;
      ShowMessage(Report);
    end;
    

## Сброс исключений стека вызовов

Стек вызовов исключения может быть получен через функции модуля SysUtils **ExceptAddr** , **ExceptFrames** и **ExceptFrameCount**. 
    
    
    uses SysUtils;
    
    procedure DumpExceptionCallStack(E: Exception);
    var
      I: Integer;
      Frames: PPointer;
      Report: string;
    begin
      Report := 'Program exception! ' + LineEnding +
        'Stacktrace:' + LineEnding + LineEnding;
      if E <> nil then begin
        Report := Report + 'Exception class: ' + E.ClassName + LineEnding +
        'Message: ' + E.Message + LineEnding;
      end;
      Report := Report + BackTraceStrFunc(ExceptAddr);
      Frames := ExceptFrames;
      for I := 0 to ExceptFrameCount - 1 do
        Report := Report + LineEnding + BackTraceStrFunc(Frames[I]);
      ShowMessage(Report);
      Halt; // Конец выполнения программы
    end;
    

## Обработка исключений

### Ручная обработка исключений

Ручное выполнение обработчика исключений может быть вставлено во многие места в коде. 
    
    
    try
      // ... какая-то операция, которая должна вызвать исключение...
      raise Exception.Create('Test error');
    except
      on E: Exception do 
        DumpExceptionCallStack(E);
    end;
    

### ExceptProc из модуля System

Если происходит необработанное исключение, выполняется переменная процедуры ExceptProc. Поведение по умолчанию инициализируется процедурой **InitExceptions** из модуля **System**. Если вы хотите, эту процедуру можно переназначить на собственный обработчик. 

Пример: 
    
    
    procedure CatchUnhandledException(Obj: TObject; Addr: Pointer; FrameCount: Longint; Frames: PPointer);
    var
      Message: string;
      i: LongInt;
      hstdout: ^Text;
    begin
      hstdout := @stdout;
      Writeln(hstdout^, 'An unhandled exception occurred at $', HexStr(PtrUInt(Addr), SizeOf(PtrUInt) * 2), ' :');
      if Obj is exception then
       begin
         Message := Exception(Obj).ClassName + ' : ' + Exception(Obj).Message;
         Writeln(hstdout^, Message);
       end
      else
        Writeln(hstdout^, 'Exception object ', Obj.ClassName, ' is not of class Exception.');
      Writeln(hstdout^, BackTraceStrFunc(Addr));
      if (FrameCount > 0) then
        begin
          for i := 0 to FrameCount - 1 do
            Writeln(hstdout^, BackTraceStrFunc(Frames[i]));
        end;
      Writeln(hstdout^,'');
    end;
    

### Событие TApplication.OnException

Это событие можно использовать для перезаписи глобального обработчика исключений приложения по умолчанию. Пользовательский механизм ведения журнала может обеспечивать показ настраиваемого диалога, запись в файл, консоль, отправку отчета на почту, ведение журнала на HTTP-сервер, например, 
    
    
    procedure TMainForm.CustomExceptionHandler(Sender: TObject; E: Exception);
    begin
      DumpExceptionCallStack;
      Halt; // Конец выполнения программы
    end;   
    
    procedure TMainForm.FormCreate(Sender: TObject);
    begin
      Application.OnException := @CustomExceptionHandler;
    end;
    
    procedure TMainForm.ButtonClick(Sender: TObject);
    begin
      raise Exception.Create('Test');
    end;
    

### Обработка исключений доп.потока

Обработка исключений, возникающих в потоках, должна выполняться вручную. Метод основного потока TThread **Execute** вызывается из функции **ThreadProc** , расположенной в модуле **Classes**. 
    
    
    function ThreadProc(ThreadObjPtr: Pointer): PtrInt;
    begin
      ...
      try
        Thread.Execute;
      except
        Thread.FFatalException := TObject(AcquireExceptionObject);
      end; 
      ...
    end;
    

В этой функции метод Execute заключен в блок try-except, и все исключения обрабатываются присвоением объекта исключения свойству **FatalException** объекта TThread как объекта последнего возникшего исключения. Таким образом, исключения не отображаются для пользователя вообще. 

В каждом потоке приложения должен быть вставлен отдельный блок try-except, чтобы перехватить все необработанные исключения с помощью специального обработчика исключений. 
    
    
    procedure TMyThread.Execute;
    begin
      try
        // какой-нибудь ошибочный код
      except
        on E: Exception do 
          CustomExceptionThreadHandler(Self, E);
      end;
    end;
    

Затем **CustomExceptionThreadHandler** может получить стек вызовов исключений и управлять отображением сообщений об ошибках или протоколированием отчетов журнала в файл. Поскольку обработчик выполняется из потока, показ диалога сообщений должен быть безопасным для потока с использованием метода **Synchronize**. 
    
    
    procedure TMainForm.CustomExceptionHandler(Thread: TThread; E: Exception);
    begin
      Thread.Synchronize(DumpExceptionCallStack);
    end;
    

### Использование map-файла

Используйте ключ компилятора -Xm для генерации map-файла. 

### Исключения в DLL

todo 

## См.также

  * [Profiling](<../en/Profiling.md> "Profiling")
  * [Creating a Backtrace with GDB](<../en/Creating_a_Backtrace_with_GDB.md> "Creating a Backtrace with GDB")
  * [MultiLog](<../en/MultiLog.md> "MultiLog") \- протоколирующий пакет
  * [log4delphi](<../en/log4delphi.md> "log4delphi") \- протоколирующий пакет



## Внешние ссылки

  * [esprinter](<http://www.ciuly.com/tools/programming/esprinter/index.html>) \- инструмент для получения трассировки стека из запущенной программы FreePascal, заданной идентификатором процесса и идентификатором потока (для Win32)
  * [PascalBugReports](<https://github.com/r3code/pascalbugreports>) \- проект, имитирующий EurekaLog / MadExcept, но долгое время не разрабатывался

---

_Source: [https://wiki.freepascal.org/Logging_exceptions/ru](https://web.archive.org/web/20220101000000/https://wiki.freepascal.org/Logging_exceptions/ru)_
