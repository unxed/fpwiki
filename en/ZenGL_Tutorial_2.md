# ZenGL Tutorial 2

│ **English (en)** │  **[русский (ru)](<../ru/ZenGL_Tutorial_2.md>)** │

  
[ZenGL](<ZenGL.md> "ZenGL") | [Tutorial 1](<ZenGL_Tutorial.md> "ZenGL Tutorial") | Tutorial 2 | [Tutorial 3](</index.php?title=ZenGL_Tutorial_3&action=edit&redlink=1> "ZenGL Tutorial 3 \(page does not exist\)") | [Edit](<ZenGL_Tutorial_2.md>)

This tutorial show how to create a ZenGL Font (.zfi) and how to draw text. 

## Contents

  * 1 Create ZenGL Font
  * 2 Create program
  * 3 Add code
  * 4 Result



## Create ZenGL Font

To create a ZenGL Font you need to download [ZenFont](<http://zengl.org/download.html>) that is a Font generator. 

When you got ZenFont open it and you will see something like this: 

[![zenfont.png](https://wiki.freepascal.org/images/2/2b/zenfont.png)](</File:zenfont.png>)

_Agency FB-Regular-18pt.zfi_

Use the options to customize your font and then press "Save font" and chose the filename you want (some filename is generated automatically with 

the font name and size). The files generated are the .zfi file with some .tga images. 

Once we have a font created we need to follow those steps to draw in the ZenGL window. 

## Create program

  * Create a new pascal program and add reference to ZenGL like was explained in the first tutorial.


  * Create those directories:



`projectname\bin `

`projectname\data `

`projectname\project`

  * Save your project in "_projectname\project_ ".


  * Put the font you has created in "_projectname\data_ "


  * Go to _Project > Options > Paths_ in _Target file name_ write "_..\bin\project1_ ".



## Add code

Variables: 
    
    
    var
      dirRes     : String {$IFNDEF DARWIN} = '../data/' {$ENDIF}; // this is the directory where resources are placed
      fnt        : zglPFont; // this is the font we will use
    

Load the font in a variable: 
    
    
    procedure Init;
    begin
      fnt := font_LoadFromFile( dirRes + 'Agency FB-Regular-18pt.zfi' );
    end;
    

Draw procedure (here we will draw our text with the loaded font): 
    
    
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
    

This is some code to exit our application pressing ESC: 
    
    
    procedure Timer;
    begin
      if key_Press( K_ESCAPE ) Then zgl_Exit();
      key_ClearState();
    end;
    

## Result

You can see text with the font we created and other text scaled with alpha. 

[![zengltext.png](https://wiki.freepascal.org/images/2/2b/zengltext.png)](</File:zengltext.png>)

The resulting code is something like this: 
    
    
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

_Source: [https://wiki.freepascal.org/ZenGL_Tutorial_2](https://web.archive.org/web/20250217112700/https://wiki.freepascal.org/ZenGL_Tutorial_2)_
