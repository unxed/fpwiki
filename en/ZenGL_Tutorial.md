# ZenGL Tutorial

│ **English (en)** │  **[русский (ru)](<../ru/ZenGL_Tutorial.md>)** │

  
[ZenGL](<ZenGL.md> "ZenGL") | Tutorial 1 | [Tutorial 2](<ZenGL_Tutorial_2.md> "ZenGL Tutorial 2") | [Tutorial 3](</index.php?title=ZenGL_Tutorial_3&action=edit&redlink=1> "ZenGL Tutorial 3 \(page does not exist\)") | [Edit](<ZenGL_Tutorial.md>)

## Contents

  * 1 Download
  * 2 Installation
    * 2.1 Source Path
  * 3 Compilation
    * 3.1 Compiling ZenGL statically
    * 3.2 ZenGL using a .so/.dll/.dylib
      * 3.2.1 Windows dll
  * 4 First Program
    * 4.1 Resulting Code



## Download

You can get [ZenGL](<ZenGL.md> "ZenGL") for Linux, Windows & Mac [at the ZenGL homepage](<http://zengl.org/download.html>). 

## Installation

You can extract the downloaded compressed archive with an utility like 7-Zip to a folder of your choice. 

### Source Path

Before using any of the modules, make sure you set the correct path to the source code of ZenGL. 

Go to "_Project > Project Options_". In "_Compiler Options > Paths_" and you can add these in "_Other Unit Files_ ": 
    
    
    headers
    extra
    src
    src\Direct3D
    lib\jpeg\$(TargetCPU)-$(TargetOS)
    lib\msvcrt\$(TargetCPU)
    lib\ogg\$(TargetCPU)-$(TargetOS)
    lib\zlib\$(TargetCPU)-$(TargetOS)
    

## Compilation

Application can be compiled with ZenGL statically or with a .so/.dll/.dylib. 

Read more about [compiling](<http://zengl.org/wiki/doku.php?id=compilation:basics>) ZenGL in the [ZenGL Wiki](<http://zengl.org/wiki/>). 

### Compiling ZenGL statically

The advantage of static compilation is a smaller size of your application, but it requires including all units. 
    
    
    {$DEFINE STATIC}
    

### ZenGL using a .so/.dll/.dylib

Using a .so/.dll/.dylib doesn't require you to open source the code of your application. To do this comment out or delete the _$DEFINE STATIC_ compiler directive. You also need to compile the ZenGL library. 
    
    
    //{$DEFINE STATIC}
    

#### Windows dll

Open "_src\Lazarus\ZenGL.lpi_ " then go to "_Run > Compile (Ctrl + F9)_". 

Then in the directory "_src\_ " you should see the file "_ZenGL.dll_ ", copy and paste it in the folder "_bin\i386_ " where all the demo binaries are compiled. You always must copy the libraries in your program output directory if you are using the .dll. 

Now you can compile the demos, commenting out the _$DEFINE STATIC_. 

Other .dll files in the "_bin\_ " folder you can use are: `chipmunk.dll ; libogg-0.dll ; libvorbis-0.dll ; libvorbis-3.dll`

## First Program

This is the first demo program included with ZenGL. First create a new "Free Pascal Program". Add the Source Path as described before. 

You must also change the _Syntax Mode_ in _Project > Project Options > Compiler Options > Parsing_ to _Delphi (-Mdelphi)_. 

Remember, if you are using a .so/.dll/.dylib copy the library binaries to the output folder of your program. 

Program title, add resources: 
    
    
    program demo01;
    
    {$R *.res}
    

Define compilation mode (comment out to use .so/.dll/.dylib): 
    
    
    {$DEFINE STATIC}
    

This adds the ZenGL units: 
    
    
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
    

Variables like in a standard pascal program: 
    
    
    var
      DirApp  : String;
      DirHome : String;
    

Procedures, add your code here: 
    
    
    procedure Init;
    begin
      // Here you can load the main resources.
    end;
    
    procedure Draw;
    begin
      // Here you can "draw" anything.
    end;
    
    procedure Update( dt : Double );
    begin
      // This function is the best way to implement smooth moving of something, because timers are restricted by FPS.
    end;
    
    procedure Timer;
    begin
      // This caption will show the frames per second.
      wnd_SetCaption( '01 - Initialization[ FPS: ' + u_IntToStr( zgl_Get( RENDER_FPS ) ) + ' ]' );
    end;
    
    procedure Quit;
    begin
     //
    end;
    

The program starts here: 
    
    
    Begin
      {$IFNDEF STATIC}
      zglLoad( libZenGL );
      {$ENDIF}
      // For loading/creating your own options/profiles/etc. you can get the path to the user home
      // directory, or to the executable file (does not work on GNU/Linux).
      DirApp  := u_CopyStr( PChar( zgl_Get( DIRECTORY_APPLICATION ) ) );
      DirHome := u_CopyStr( PChar( zgl_Get( DIRECTORY_HOME ) ) );
    
      // Create a timer with an interval of 1000ms.
      timer_Add( @Timer, 1000 );
    
      // Register the procedure that will be executed after ZenGL initialization.
      zgl_Reg( SYS_LOAD, @Init );
      // Register the render procedure.
      zgl_Reg( SYS_DRAW, @Draw );
      // Register the procedure that will get the delta time between the frames.
      zgl_Reg( SYS_UPDATE, @Update );
      // Register the procedure that will be called after ZenGL shuts down.
      zgl_Reg( SYS_EXIT, @Quit );
      
      // Enable usage of UTF-8, because this unit saved in UTF-8 encoding and here used
      // string variables.
      zgl_Enable( APP_USE_UTF8 );
    
      // Set the caption of the window.
      wnd_SetCaption( '01 - Initialization' );
    
      // Show the mouse cursor.
      wnd_ShowCursor( TRUE );
    
      // Set screen options.
      scr_SetOptions( 800, 600, REFRESH_MAXIMUM, FALSE, FALSE );
    
      // Initialize ZenGL.
      zgl_Init();
    End.
    

### Resulting Code

The result is a _template_ for ZenGL projects: 
    
    
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

_Source: [https://wiki.freepascal.org/ZenGL_Tutorial](https://web.archive.org/web/20250217114738/https://wiki.freepascal.org/ZenGL_Tutorial)_
