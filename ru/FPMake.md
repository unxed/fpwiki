# FPMake

│ **[English (en)](<../en/FPMake.md>)** │  **русский (ru)** │

**FPMake** \- это система сборки, специализированная для кода на Паскале. Его можно сравнить с другими системами сборки, такими как Make, cons, scons и т.д. 

С помощью FPMake код разбивается на пакеты. Пакет FPMake может содержать модули, двоичные файлы, примеры и документацию. Каждый пакет может зависеть от других пакетов, и в этом случае пакет может использовать модули своих зависимостей. FPMake использует два разных местоположения для поиска пакетов: общесистемное (глобальное) и локальное местоположение. По умолчанию глобальное расположение указывает на местоположение установленного fpc, а локальное расположение зависит от пользовательских настроек. 

Некоторые части самого fpc скомпилированы с использованием FPMake. (Пакеты fastcgi и fcl-web в fpc 2.6 и все пакеты, утилиты и ide в fpc 2.7 и выше). 

Менеджер пакетов [fppkg](<fppkg.md> "fppkg/ru") может использоваться для управления пакетами FPMake и их установки из удаленного репозитория. 

  


## Contents

  * 1 Как это работает
  * 2 Сборка пакетов FPMake
  * 3 Установка пакетов FPMake
  * 4 Как создать FPMake-пакеты
    * 4.1 Простой пример fpmake.pp
    * 4.2 Более сложный пример fpmake.pp
    * 4.3 Изменение рабочего каталога
    * 4.4 Добавление каталогов
    * 4.5 Добавление параметров компилятора
  * 5 Распространенные сообщения об ошибках
    * 5.1 Unknown target for unit "[unitname]" in dependencies for [targetname] in package [packagename]
    * 5.2 Could not find unit directory for dependency package "rtl"



## Как это работает

FPMake работает с Pascal-приложениями, называемыми fpmake, каждое из которых может скомпилировать и установить один или несколько FPMake-пакетов. Все настройки, расположение исходников и инструкции определены внутри исходного кода этого исполняемого файла fpmake. Этот исходный файл обычно сохраняется с именем fpmake.pp. Файл fpmake.pp компилируется в исполняемый файл fpmake, и этот исполняемый файл может создавать и устанавливать конечные пакеты. 

## Сборка пакетов FPMake

Самое простое - использовать [fppkg](<fppkg.md> "fppkg/ru") для сборки пакета FPMake. Но это также можно сделать вручную. Найдите исходники пакета, который вы хотите установить, и файл fpmake.pp. Теперь скомпилируйте файл fpmake.pp: 
    
    
    fpc fpmake.pp
    

Типичный вывод выглядит так: 
    
    
     
     Free Pascal Compiler version 2.7.1 [2014/11/26] for i386
     Copyright (c) 1993-2014 by Florian Klaempfl and others
     Target OS: Darwin for i386
     Compiling fpmake.pp
     Assembling (pipe) fpmake.s
     Linking fpmake
     203 lines compiled, 3.6 sec
    

Теперь запустите новый исполняемый файл fpmake(.exe), чтобы собрать пакеты. Этот пример содержит только один пакет. 

На nix'ах или аналогичных ОСях: 
    
    
    ./fpmake build --globalunitdir=/usr/local/lib/fpc/2.7.1
    

На Windows или аналогичных ОСях: 
    
    
    fpmake.exe build --globalunitdir=c:\pp
    

Типичный вывод выглядит так: 
    
    
     
     Start compiling package fcl-base for target i386-darwin.
           Compiling src/ascii85.pp
           Compiling src/avl_tree.pp
           Compiling src/base64.pp
           Compiling src/fpmimetypes.pp
     [100%] Compiled package fcl-base
    

Параметр `--globalunitdir` указывает местоположение уже установленных глобальных пакетов. Таким образом, FPMake может найти пакеты, от которых зависит этот пакет. Когда есть второе место, где установлены пакеты, кроме того, можно использовать параметр `--localunitdir`. 

Если вы хотите узнать, какой путь вы должны использовать для параметра `--global(local)unitdir`, то [поищите] каталог, который содержит папку с именем 'fpmkinst'. 

Чтобы удалить все файлы, сгенерированные во время сборки, и сделать «чистую» сборку, выполните очистку. 

На nix'ах или аналогичных ОСях: 
    
    
    ./fpmake clean
    

На Windows или аналогичных ОСях: 
    
    
    fpmake.exe clean
    

Используйте параметр `-h`, чтобы получить обзор всех других доступных команд и опций. 

## Установка пакетов FPMake

Как и при сборке пакетов, использование [fppkg](<fppkg.md> "fppkg/ru") для установки пакетов FPMake является самым простым. Но это можно сделать вручную. Сначала скомпилируйте пакет, а затем вызовите исполняемый файл fpmake для установки пакета. 

На никсах и аналогичных ОСях: 
    
    
     ./fpmake install --globalunitdir=/usr/local/lib/fpc/2.7.1 --prefix=/usr/local --baseinstalldir=/usr/local/lib/fpc/2.7.1
    

На Windows и аналогичных ОСях: 
    
    
     fpmake.exe install --globalunitdir=c:\pp --prefix=c:\pp --baseinstalldir=c:\pp
    

Типичный вывод выглядит так: 
    
    
     Installing package fcl-base
     Installation package fcl-base for target i386-darwin succeeded
    

Globalunitdir содержит местоположение, в котором нужно искать пакеты, от которых может зависеть пакет FPMake, как при сборке пакета. Baseinstalldir - это место, куда должен быть установлен пакет. В общем, это тот же путь, который используется для global- или local-unitdir. Некоторые пакеты также устанавливают файлы за пределами baseinstalldir. как файлы конфигурации. Эти пакеты чаще всего также нуждаются в префиксе, поэтому рекомендуется его указывать. 

## Как создать FPMake-пакеты

### Простой пример fpmake.pp
    
    
      program fpmake;
    
      uses fpmkunit;
    
      Var
        P: TPackage;
        T: TTarget;
    
      begin
        With Installer do
        begin
          P := AddPackage('my-nice-program');
          P.OSes := [win32,openbsd,netbsd,freebsd,darwin,linux];
          T := P.Targets.AddUnit('myunit');
          T.ResourceStrings := True;
          T := P.Targets.AddUnit('myprogram');
          T.Dependencies.Add('myunit');
          Run;
        end;
      end.
    

### Более сложный пример fpmake.pp
    
    
      program fpmake;
    
      uses fpmkunit;
    
      type TWidgetSet = (wsGDI, wsX, wsCarbon);
    
      var WidgetSet : TWidgetSet;
          P : TPackage;
    
      procedure DetermineWidgetSet;
      Var
        I : Integer;
      begin
        if Defaults.OS in AllWindowsOSes then
          WidgetSet := wsGDI else
        if Defaults.OS = MacOS then
          WidgetSet := wsCarbon else
        if Defaults.OS in AllUnixOSes then
          Widgetset := wsX;
    
        // Проверьте paramstr(), чтобы увидеть, был ли переопределен набор виджетов в командной строке;
        For I := 1 to ParamCount do
          If ParamStr(i) = '--widgetset=X' then
            WidgetSet := wsX;
      end;
    
      begin
        DetermineWidgetSet;
        With Installer do
        begin
          // ...
          Case WidgetSet of
            wsGDI : P.UnitPath.Add('corelib/gdi');
            wsX   : P.UnitPath.Add('corelib/x11');
            // и т.д.
          end;
          //  ...
          Run;
        end;
      end.
    

### Изменение рабочего каталога

Если вы работаете с модулями в подкаталоге относительно ./fpmake, то "Каталог" можно изменить. 
    
    
        With Installer do
        begin
          ...
          Directory:='some/subdir';
          T:= Targets.AddUnit('unitinfolder');
          ...
        end;
    

### Добавление каталогов

Вы можете добавить каталоги с помощью пути к модулю: 
    
    
      Case Defaults.OS of
        Win32, Win64: 
          begin
            T.UnitPath.Add('corelib/gdi');
            T.Dependencies.Add('gfx_gdi');
          end;
        Linux: 
          begin
            T.UnitPath.Add('corelib/x11');
            T.Dependencies.Add('gfx_x11');
          end;
      end;
    

Часто удобнее использовать заданные константы, такие как AllWindowsOS, AllUnixOS, а не конкретные имена ОС. 

### Добавление параметров компилятора

Дополнительные пользовательские параметры компилятора (т.е. параметры командной строки компилятора) могут быть добавлены с помощью TTarget.Options, TPackage.Options или Defaults.Options. 
    
    
      var T : TTarget;
      ...
      T.Options.Append('-dSOMEDEFINE');
      T.Options.Append('-xyzAnythingYouNeed');
    

Или 
    
    
      var P : TPackage;
      ...
      P.Options.Append('-dSOMEDEFINE');
    

Или 
    
    
      Defaults.Options.Append('-dSOMEDEFINE');
    

**Примечание** : До FPC 2.4.0 свойство Options представляло собой простую строку и, похоже, поддерживало передачу только одного параметра компилятору. Начиная с FPC 2.4.0 (точнее, начиная с svn revision 13223), Options - это экземпляр TStrings, поэтому он гораздо более гибкий. Это также означает, что вы должны использовать $ifdefs, если вы хотите использовать Options и хотите, чтобы ваш fpmake.pp был совместим с FPC < 2.4.0 и >= 2.4.0. 

## Распространенные сообщения об ошибках

Несколько общих сообщений об ошибках объяснены здесь 

### Unknown target for unit "[unitname]" in dependencies for [targetname] in package [packagename]

Неизвестная цель для модуля "[unitname]" в зависимостях для [targetname] в пакете [packagename]

\- Это указывает на проблему в файле fpmake.pp. Существует зависимость от модуля, добавляемая с помощью Target.Dependencies.AddUnit('unitname'). Но нет соответствующей цели, добавленной в пакет с таким же именем модуля. 

Другими словами: для каждой зависимости от модуля должна быть соответствующая цель в том же пакете. Если существует зависимость от модуля в другом пакете, добавьте зависимость для этого пакета. Зависимость от одного файла в другом пакете не будет работать. 

Также обратите внимание, что имя модуля должно быть без расширения соответствующего файла. (Target.Dependencies.AddUnit('unitnams.pas') не будет работать, в то время как Targets.Addunit требует наличия расширения). 

### Could not find unit directory for dependency package "rtl"

Не удалось найти каталог модуля для зависимостей пакета "rtl"

\- В общем, система fpmake сканирует места, обычно используемые для установки fpc. В случае, если fpmake не найдет каталог FPC, вы получите вышеупомянутую ошибку. В таком случае установите для переменной среды FPCDIR базовый каталог установки FPC, например: 
    
    
    export FPCDIR=~/src/lib/fpc/2.6.4
    

или 
    
    
    set FPCDIR=C:\lazarus\fpc\2.6.4\
    

Это также может быть полезно, если вам необходимо устранить неоднозначность в нескольких установках FPC.

---

_Source: [https://wiki.freepascal.org/FPMake/ru](https://web.archive.org/web/20250117052151/https://wiki.freepascal.org/FPMake/ru)_
