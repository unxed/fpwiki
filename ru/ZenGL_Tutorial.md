# ZenGL Tutorial

│ **[English (en)](<../en/ZenGL_Tutorial.md>)** │  **русский (ru)** │

  
[ZenGL/ru](<ZenGL.md> "ZenGL/ru") | Tutorial 1 | [Tutorial 2](<ZenGL_Tutorial_2.md> "ZenGL Tutorial 2/ru") | [Tutorial 3](</index.php?title=ZenGL_Tutorial_3/ru&action=edit&redlink=1> "ZenGL Tutorial 3/ru \(page does not exist\)") | [Edit](<http://wiki.lazarus.freepascal.org/index.php?title=Template:ZenGL_Tutorial_Index/ru&action=edit>)

## Contents

  * 1 Скачать
  * 2 Установка
    * 2.1 Настройка путей
  * 3 Сборка
    * 3.1 Windows DLL
  * 4 Первая программа
    * 4.1 Результат



## Скачать

[ZenGL](<../en/ZenGL.md> "ZenGL") для Linux, Windows и Mac можно скачать [тут](<http://zengl.org/download.html>). 

## Установка

Для инсталляции достаточно распаковать скачанный архив в удобную директорию. 

### Настройка путей

Перед использованием модулей не забудьте добавить в пути к исходным кодам путь к исходникам ZenGL 

Откройте "_Project > Project Options_". В "_Compiler Options > Paths_" добавьте путь к "_Other Unit Files_ ": 
    
    
    headers
    extra
    src
    src\Direct3D
    lib\jpeg\$(TargetCPU)-$(TargetOS)
    lib\msvcrt\$(TargetCPU)
    lib\ogg\$(TargetCPU)-$(TargetOS)
    lib\zlib\$(TargetCPU)-$(TargetOS)
    

## Сборка

Приложение можно собрать с ZenGL статически либо с использованием динамических библиотек (so/dll/dylib). 

Подробнее можно прочесть в [compilation](<http://zengl.org/wiki/doku.php?id=compilation>) на [ZenGL Wiki](<http://zengl.org/wiki/>). 

Статическая сборка: 
    
    
    {$DEFINE STATIC}
    

Сборка с использованием загружаемых библиотек: 
    
    
    //{$DEFINE STATIC}
    

#### Windows DLL

Откройте "_src\Lazarus\ZenGL.lpi_ " а потом: "_Run > Compile (Ctrl + F9)_". 

После, в директории "_src\_ " вы можете увидеть файл "_ZenGL.dll_ ": скопируйте его в директорию "_bin\i386_ " При использовании динамической линковки, вам придется постоянно копировать DLL в выходную директорию проекта. 

## Первая программа

Сперва, создайте новое приложение Free Pascal ("Free Pascal Program"), измените пути к исходным кодам, как было указано выше. Также, смените _Syntax Mode_ в _Project > Project Options > Compiler Options > Parsing_ на _Delphi (-Mdelphi)_. Запомните: если вы используете динамическую линковку (so/dll/dylib), скопируйте необходимые библиотеки в директорию выполнения программы. 

Добавьте ресурсы, назовите программу: 
    
    
    program demo01;
    
    {$R *.res}
    

Выберите режим компиляции, для использования so/dll/dylib: 
    
    
    {$DEFINE STATIC}
    

Добавьте модули ZenGL. 
    
    
    uses
      {$IFNDEF STATIC}
      zglHeader
      {$ELSE}
      zgl_main,
      zgl_screen,
      zgl_window,
      zgl_timers,
      zgl_utils
      {$ENDIF}
      ;
    

Задаем переменные: 
    
    
    var
      DirApp  : String;
      DirHome : String;
    

...Процедуры:: 
    
    
    procedure Init;
    begin
      // Загрузка основных ресурсов
    end;
    
    procedure Draw;
    begin
      // Рисуем то, что необходимо нам
    end;
    
    procedure Update( dt : Double );
    begin
      // Эта функция - лучший метод для реализации плавного движения
    end;
    
    procedure Timer;
    begin
      // Заголовок будет показывать количество кадров в секунду.
      wnd_SetCaption( '01 - Initialization[ FPS: ' + u_IntToStr( zgl_Get( RENDER_FPS ) ) + ' ]' );
    end;
    
    procedure Quit;
    begin
     //
    end;
    

Вот начало программы: 
    
    
    Begin
      {$IFNDEF STATIC}
      zglLoad( libZenGL );
      {$ENDIF}
      // Для загрузки/создания каких-то своих настроек/профилей/etc. можно получить путь к
      // домашенему каталогу пользователя, или к исполняемому файлу(не работает для GNU/Linux).
      DirApp  := u_CopyStr( PChar( zgl_Get( DIRECTORY_APPLICATION ) ) );
      DirHome := u_CopyStr( PChar( zgl_Get( DIRECTORY_HOME ) ) );
    
      // Создаем таймер с интервалом 1000мс.
      timer_Add( @Timer, 1000 );
    
      // Регистрируем процедуру, что выполнится сразу после инициализации ZenGL.
      zgl_Reg( SYS_LOAD, @Init );
      // Регистрируем процедуру, где будет происходить рендер.
      zgl_Reg( SYS_DRAW, @Draw );
      // Регистрируем процедуру, которая будет принимать разницу времени между кадрами.
      zgl_Reg( SYS_UPDATE, @Update );
      // Регистрируем процедуру, которая выполнится после завершения работы ZenGL.
      zgl_Reg( SYS_EXIT, @Quit );
      
      // Т.к. модуль сохранен в кодировке UTF-8 и в нем используются строковые переменные
      // следует указать использование этой кодировки.
      zgl_Enable( APP_USE_UTF8 );
    
      // Устанавливаем заголовок окна.
      wnd_SetCaption( '01 - Initialization' );
    
      // Разрешаем курсор мыши.
      wnd_ShowCursor( TRUE );
    
      // Указываем первоначальные настройки.
      scr_SetOptions( 800, 600, REFRESH_MAXIMUM, FALSE, FALSE );
    
      // Инициализируем ZenGL.
      zgl_Init();
    End.
    

### Результат

Наш результат - _шаблон_ для проектов ZenGL: 
    
    
    program template;
    
    {$DEFINE STATIC}
    
    {$R *.res}
    
    uses
      {$IFNDEF STATIC}
      zglHeader
      {$ELSE}
      zgl_main,
      zgl_screen,
      zgl_window,
      zgl_timers,
      zgl_utils
      {$ENDIF}
      ;
    
    var
      DirApp  : String; DirHome : String;
    
    procedure Init;
    begin
    end;
    
    procedure Draw;
    begin
    end;
    
    procedure Update( dt : Double );
    begin
    end;
    
    procedure Timer;
    begin
      wnd_SetCaption( '01 - Initialization[ FPS: ' + u_IntToStr( zgl_Get( RENDER_FPS ) ) + ' ]' );
    end;
    
    procedure Quit;
    begin
    end;
    
    Begin
      {$IFNDEF STATIC}
      zglLoad( libZenGL );
      {$ENDIF}
      DirApp  := u_CopyStr( PChar( zgl_Get( DIRECTORY_APPLICATION ) ) );
      DirHome := u_CopyStr( PChar( zgl_Get( DIRECTORY_HOME ) ) );
      timer_Add( @Timer, 1000 );
      zgl_Reg( SYS_LOAD, @Init );
      zgl_Reg( SYS_DRAW, @Draw );
      zgl_Reg( SYS_UPDATE, @Update );
      zgl_Reg( SYS_EXIT, @Quit );
      zgl_Enable( APP_USE_UTF8 );
      wnd_SetCaption( '01 - Initialization' );
      wnd_ShowCursor( TRUE );
      scr_SetOptions( 800, 600, REFRESH_MAXIMUM, FALSE, FALSE );
      zgl_Init();
    End.

---

_Source: [https://wiki.freepascal.org/ZenGL_Tutorial/ru](https://web.archive.org/web/20250425021227/https://wiki.freepascal.org/ZenGL_Tutorial/ru)_
