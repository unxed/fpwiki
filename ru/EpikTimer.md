# EpikTimer

│ **[Deutsch (de)](</EpikTimer/de> "EpikTimer/de")** │  **[English (en)](<../en/EpikTimer.md> "EpikTimer")** │  **[français (fr)](</EpikTimer/fr> "EpikTimer/fr")** │  **русский (ru)** │    
****

## Contents

  * 1 О компоненте
  * 2 Автор
  * 3 Лицензия
  * 4 Загрузка
  * 5 Статус проекта
  * 6 Change Log
  * 7 Зависимости/Системные требования
  * 8 Установка в Lazarus IDE
  * 9 Использование
    * 9.1 EpikTimer в качестве профайлера
  * 10 Приложение ETDemo
  * 11 Консольное приложение
  * 12 См. также



### О компоненте

EpikTimer - это секундомер для программиста, способный измерять очень короткие события с чрезвычайно высокой точностью в течение длительных периодов времени. Он прост в использовании, практически не потребляет ЦП и требует только 25 B оперативной памяти для реализации экземпляра таймера. Компонент предоставляет один внутренний таймер ... но неограниченное число может быть объявлен извне и связан с одним компонентом EpikTimer на форме. 

Загруженный архив содержит компонент, установочный пакет и демонстрационное приложение, которое иллюстрирует функции компонента, а также некоторые инструменты для оценки источников синхронизации в данной системе. 

Этот компонент был разработан для кроссплатформенных приложений и был написан специально для Lazarus IDE и Free Pascal Compiler, и включает в себя демонстрационное приложение. 

### Автор

[Tom Lisjac](</User:VlxAdmin> "User:VlxAdmin")

### Лицензия

[LGPL](<http://www.opensource.org/licenses/lgpl-license.php>)(пожалуйста, свяжитесь с автором, если LGPL не совместима с лицензированием вашего проекта) 

### Загрузка

**Исходный код** можно загрузить из Git-репозитория следующим образом: 
    
    
     git clone <https://github.com/graemeg/epiktimer.git>
    

Если вы не хотите устанавливать Git-клиент, Github также позволяет загружать последний код в виде ZIP-архива. Просто перейдите по ссылке [Github EpikTimer](<https://github.com/graemeg/epiktimer>) и нажмите кнопку «Загрузить ZIP» справа. 

Все **tagged releases** (помеченные выпуски) можно скачать в формате .zip или .tar.gz, перейдя по этой ссылке [Github Releases](<https://github.com/graemeg/epiktimer/releases>) и выбрав zip или tar.gz ссылка для любого релиза, который вы хотите скачать. 

* * *

[Прим.перев.](</User:Zoltanleo> "User:Zoltanleo"): Боюсь ошибиться, но в релизах Лазаруса после v.2.0.0 точно, EpikTimer можно скачать и установить посредством **O** nline **P** ackage **M** anager. 

* * *

### Статус проекта

  * В производстве/Стабильный



### Change Log

  * Initially written on 24-06-2003 TL
  * Pre-release 30-06-2003 TL - Needs testing on the BSD's and Win32
  * Version 0.1 1-7-2003 TL


  1. initial beta release


  * Version 0.2 3-7-2003 TL


  1. Revised logic around hardware detection to prevent executing extended instructions if the HasCapabilityData call returns false.
  2. Removed exposed low level diagnositic functions from unit interface.
  3. Revised demo.


  * Version 0.3 15-11-2005


  1. Updated 0.2 version to make it compile on the latest Lazarus (0.9.10).
  2. Added LCL to the required packages.
  3. Changed mode to Delphi in order to compile.
  4. Changed windows timebase to use QueryPerformanceCounter, because has a much greater precision then GetSystemTime.
  5. Added changes to ensure the component compiles on Delphi 7.0
  6. Made tests on Windows and Linux
  7. Small changes to the demo to make it compile


  * Version 1.0 06-10-2006


  1. Changes for 64 bits operation
  2. Added units BaseUnix, Unix and UnixUtil, removed oldlinux (obsolete)
  3. Gettimeofday -> fpGettimeofday
  4. Changed systemsleep for 64 bits systems
  5. Some changes in timeval
  6. Tested on AMD64 (linux)



For any later changes, please see the Git commit log. 

### Зависимости/Системные требования

  * Разрешение Nanosecond поддерживается в версиях Intel Pentium с счетчиком Timestamp.
  * Микросекундные системные часы являются временной базой по умолчанию.



Протестировано на: 

  * Pentium IV 3,2Ghz на Windows и Linux с большой точностью (~ 3,220,000,000 тиков/сек)
  * Pentium 233 МГц с чертовски маленьким Linux. Прекрасно работает с аппаратными часами.
  * Mobile AMD 64 Athlon 3200+ на Windows XP Home (32 bit), Lazarus 9.13
  * Процессор AMD 64, работающем под управлением 64-битной версии Ubuntu Linux, Lazarus 9.18.
  * Intel i7-3770K, работающем под управлением 64-разрядной версии FreeBSD 9.1, использует FPC 2.6.2
  * ARM CPU, использующем Raspberry Pi и FPC 2.6.2



### Установка в Lazarus IDE

  * В Components/Open Package File, откройте etpackage.lpk
  * Скомпилируйте пакет и закройте окно диалога пакета
  * В Components -> Open Package File, откройте etpackage_dsgn.lpk
  * Скомпилируйте пакет.
  * Нажмите кнопку "Install" и пересоберите Lazarus
  * Компонент появится в системной палитре (значок секундомера-линейки)



### Использование

Перетащите компонент на форму. Компонент содержит один экземпляр таймера, и вызовы без параметров для запуска, остановки, истечения и очистки будут неявно ссылаться на него. 

**Когда таймер называется ET**
    
    
    uses epiktimer;
    
    var
      ET: TEpikTimer;
    
    procedure InitTimer;
    begin
      ET := TEpikTimer.Create(Application);
    end;
    
    procedure InstrumentedCall;
    Begin
      ET.Clear; // необязательно ... таймер очищается при создании
      ET.Start;
      ExecuteFirstTimedSection;
      ET.Stop; // таймер фактически приостановлен и может быть перезапущен позже
      TimedSection1:=ET.Elapsed; // запоминаем прошедшее время вообще
      MakeAnUntimedOverheadCall; // не учитывается в таймере
      ET.Start; //возобновляем таймер ... продолжаем накапливать тики
      CallTimedSection2;
      TimedSection2:=ET.Elapsed; //таймер продолжает работать ... мы просто сэмплируем его (т.е. формируем репрезентатитвную выборку).
      CallTimedSection3;
      CallSomethingElse;
      TimedSection3:=ET.Elapsed; //продолжаем считать ... фиксируем прошедшее время
      CallTimedSection4;
      TimedSection4:=ET.Elapsed; //продолжаем считать ... фиксируем прошедшее время
      ET.clear // готово ... таймер остановлен и обнулен
    end;
    

Вы также можете создать любое количество таймеров из одного компонента в форме, объявив запись TimerData и передав ее в качестве параметра для запуска, остановки, истечения и очистки с использованием перегруженных методов в компоненте. Примером может быть: 
    
    
    Function TimedExecution: Extended;
    Var 
      DiskAccessTime:TimerData;
    Begin
      ET.Clear(DiskAccessTimer); // Объявленные таймеры *должны* быть очищены перед использованием. 
      ET.Start(DiskAccessTimer);
      ExecuteTheTimedSection;
      Result:=ET.Elapsed(DiskAccessTimer); // таймер продолжает работать ...
      и т.д. ...
    

Одним из конкретных применений EpikTimer является проверка того, прошло ли заданное количество времени, прежде чем продолжить выполнение задачи или выполнить другую итерацию той же задачи. 
    
    
    DelayInSeconds := 8.5; // или любое значение
    OldTime := ET.Elapsed;
    
    while ((ET.Elapsed - OldTime) < DelayInSeconds) do
    begin
      ET.SystemSleep(10);
    end;
    
    // DelayInSeconds истек, и теперь вы можете продолжить выполнение кода
    

Смотрите etdemo.pas для дополнительных примеров использования компонентов 

#### EpikTimer в качестве профайлера

EpikTimer может быть использован для профилирования скорости процедур: 
    
    
    uses
      {$ifdef PROFILING}epiktimer,{$endif}
      ...
    
    type
      TMyClass = class
      private
        {$ifdef PROFILING}
        profiler: Tepiktimer;
        {$endif}
        ...
      end;
    
    implementation
    
    constructor TMyClass.Create(AOwner: TComponent);
    begin
      inherited Create(AOwner);
      ...
      {$ifdef CARDROOM_PROFILING}
      profiler := TEpikTimer.Create(Self)
      {$endif}
    end;
    
    procedure TMyClass.MethodToBeProfiled;
    begin
      {$ifdef PROFILING}
      profiler.Clear;
      profiler.Start;
      {$endif}
      // do stuff
      {$ifdef PROFILING}
      OPDebugLn('TMyClass.MethodToBeProfiled ' + FloatToStr(profiler.Elapsed));
      {$endif}
    end;
    

### Приложение ETDemo

Приложение ETDemo не требует установки EpikTimer для компиляции и работы. Мне никогда не нравилось устанавливать палитру, полную компонентов, только для того, чтобы узнать, что мне не понравился ни один из них! :) 

**Установка**

  * Окройте etdemo.lpi
  * Скомпилируйте
  * Запустите



### Консольное приложение

Если у вас нет экземпляра объекта «Приложение», описанного выше, просто создайте экземпляр EpikTimer следующим образом. Он должен быть освобожден вручную, когда в нем отпадет нужда: 
    
    
      ET := TEpikTimer.Create(nil)
      ... здесь ваш код ...
      ET.Free;
    

### См. также

  * [FPTimer included with FPC/Lazarus](<http://lazarus-ccr.sourceforge.net/docs/fcl/fptimer/index.html> "doc:fcl/fptimer/index.html")
  * [Yet another Running Timer](<../en/Yet_another_Running_Timer.md> "Yet another Running Timer")

---

_Source: [https://wiki.freepascal.org/EpikTimer/ru](https://web.archive.org/web/20250418093813/https://wiki.freepascal.org/EpikTimer/ru)_
