# ZenGL Tutorial 2

│ **[English (en)](<../en/ZenGL_Tutorial_2.md>)** │  **русский (ru)** │

  
[ZenGL/ru](<ZenGL.md> "ZenGL/ru") | [Tutorial 1](<ZenGL_Tutorial.md> "ZenGL Tutorial/ru") | Tutorial 2 | [Tutorial 3](</index.php?title=ZenGL_Tutorial_3/ru&action=edit&redlink=1> "ZenGL Tutorial 3/ru \(page does not exist\)") | [Edit](<http://wiki.lazarus.freepascal.org/index.php?title=Template:ZenGL_Tutorial_Index/ru&action=edit>)

## Contents

  * 1 Создание шрифта ZenGL
  * 2 Создание программы
  * 3 Исходный код
  * 4 Результат



## Создание шрифта ZenGL

Создадим шрифт ZenGL. Для решения поставленной задачи нам потребуется скачать генератор шрифтов [ZenFont](<http://zengl.org/download.html>). Запустив программу, вы увидите что-то похожее на картинку ниже: 

[![zenfont.png](https://wiki.freepascal.org/images/2/2b/zenfont.png)](</File:zenfont.png>)

## Создание программы

  * Создаем пустой проект и ссылаемся на ZenGL как это было описано в предыдущей статье.


  * Создадим директории:



`projectname\bin `

`projectname\data `

`projectname\project`

  * Сохраним проект по пути: "_projectname\project_ ".


  * Скопируем шрифт (.ZFI файл) в "_projectname\data_ "


  * Идем в _Project > Options > Paths_ и в _Target file name_ допишем "_..\bin\project1_ ".



## Исходный код
    
    
    var
      dirRes     : String {$IFNDEF DARWIN} = '../data/' {$ENDIF}; // это путь к ресурсам
      fnt        : zglPFont; // это шрифт, который мы будем использовать
    

Загрузим шрифт: 
    
    
    procedure Init;
    begin
      fnt := font_LoadFromFile( dirRes + 'Agency FB-Regular-18pt.zfi' );
    end;
    

Процедура рисования (тут мы рисуем текст нашим шрифтом): 
    
    
    procedure Draw;
    var
      rect: zglTRect;
    begin
      text_Draw( fnt, 0, 0, 'Sample Text. Press ESC to EXIT.' );
    
      text_DrawEx( fnt, 32, 32, 1.5, 0, 'Sample Text with DrawEx - Scale 1.5 - Alpha 150', 150 );
    
      rect.H:=128;
      rect.W:=400;
      rect.X:=0;
      rect.Y:=96;
    
      pr2d_rect(rect.X,rect.Y,rect.W,rect.H,$FFFFFF,100);
    
      text_DrawInRect(fnt,rect,
      'Sample multiline text in rect.' + #10 +
      'Sample multiline text in rect.'+ #10 +
      'Sample multiline text in rect.');
    end;
    

Код выхода из программы по нажатию ESC: 
    
    
    procedure Timer;
    begin
      if key_Press( K_ESCAPE ) Then zgl_Exit();
      key_ClearState();
    end;
    

## Результат

Теперь вы можете увидеть текст напечатанный нашим шрифтом 

[![zengltext.png](https://wiki.freepascal.org/images/2/2b/zengltext.png)](</File:zengltext.png>)

Конечный код будет выглядет примено так: 
    
    
    program project1;
    
    {$IFDEF WINDOWS}
      {$R *.res}
    {$ENDIF}
    {$DEFINE STATIC}
    
    uses
      {$IFNDEF STATIC}
      zglHeader
      {$ELSE}
      zgl_main,
      zgl_screen,
      zgl_window,
      zgl_timers,
      zgl_keyboard,
      zgl_font,
      zgl_text,
      zgl_textures,
      zgl_textures_tga,
      zgl_primitives_2d,
      zgl_utils,
      zgl_math_2d
      {$ENDIF}
      ;
    
    var
      dirRes     : String {$IFNDEF DARWIN} = '../data/' {$ENDIF};
      fnt        : zglPFont;
    
    procedure Init;
    begin
      fnt := font_LoadFromFile( dirRes + 'Agency FB-Regular-18pt.zfi' );
    end;
    
    procedure Draw;
    var
      rect: zglTRect;
    begin
      text_Draw( fnt, 0, 0, 'Sample Text. Press ESC to EXIT.' );
    
      text_DrawEx( fnt, 32, 32, 1.5, 0, 'Sample Text with DrawEx - Scale 1.5 - Alpha 150', 150 );
    
      rect.H:=128;
      rect.W:=400;
      rect.X:=0;
      rect.Y:=96;
    
      pr2d_rect(rect.X,rect.Y,rect.W,rect.H,$FFFFFF,100);
    
      text_DrawInRect(fnt,rect,
      'Sample multiline text in rect.' + #10 +
      'Sample multiline text in rect.'+ #10 +
      'Sample multiline text in rect.');
    end; 
    
    procedure Timer;
    begin
      if key_Press( K_ESCAPE ) Then zgl_Exit();
      key_ClearState();
    end;
    
    Begin
      {$IFNDEF STATIC}
      zglLoad( libZenGL );
      {$ENDIF}
    
      timer_Add( @Timer, 16 );
    
      zgl_Reg( SYS_LOAD, @Init );
      zgl_Reg( SYS_DRAW, @Draw );
    
      zgl_Enable( APP_USE_UTF8 );
    
      wnd_SetCaption( 'Sample Text' );
    
      wnd_ShowCursor( TRUE );
    
      scr_SetOptions( 800, 600, REFRESH_MAXIMUM, FALSE, FALSE );
    
      zgl_Init();
    End.

---

_Source: [https://wiki.freepascal.org/ZenGL_Tutorial_2/ru](https://web.archive.org/web/20230207123422/https://wiki.freepascal.org/ZenGL_Tutorial_2/ru)_
