# OpenGL

│ **[English (en)](<../en/OpenGL.md>)** │  **русский (ru)** │

OpenGL (**Open** **G** raphics **L** ibrary) - это кроссплатформенный API для создания трехмерной компьютерной графики. Большинство современных видеокарт обеспечивают аппаратную поддержку OpenGL, что делает OpenGL хорошим решением для написания передового графического программного обеспечения. 

## Contents

  * 1 Модули OpenGL во Free Pascal
  * 2 Модули OpenGL в Lazarus
  * 3 Сторонние OpenGL модули
  * 4 Учебник
  * 5 Учебник OpenGL 3.3 (современный OpnGL )
  * 6 Вопросы/ответы по использованию привязок OpenGL



## Модули OpenGL во Free Pascal

Интерфейс FreePascal OpenGL состоит из следующих модулей: 

  * **gl** : Этот модуль содержит основные функции, такие как рисование полигонов, применение преобразований, установка цветов и материалов, ... Процедуры всегда начинаются с предлога "gl".
  * **glu** : Этот модульк содержит утилиты OpenGL. Хотя в нем есть некоторые полезные функции, этот модуль не является необходимым, поскольку вы можете реализовать все процедуры glu с функциональностью блока gl. Процедуры всегда начинаются с предлога "glu".
  * **glext** : Производители могут предоставлять дополнительные функциональные возможности через расширения. Используйте этот модуль для использования этих расширений. 

Функциональность, характерная для более новых версий OpenGL (1.2 и выше), также доступна в этом модуле. Инициализация этой функциональности аналогична инициализации обычных расширений OpenGL: просто вызовите функцию Load_GL_version_X_Y. Если версия вашей библиотеки OpenGL старее X.Y, Load_GL_version_X_Y вернет false.

  * **glut** : Этот модуль обеспечивает функциональность для создания окна OpenGL. Хотя это кроссплатформенный модуль, большинство операционных систем Windows по умолчанию не имеют glut DLL.
  * **glx** : glx предоставляет функциональность для настройки окна OpenGL в системе X Window. Процедуры всегда начинаются с предлога "glx". Очевидно, что вы не можете использовать это устройство в не-X Window системах, таких как Windows.



## Модули OpenGL в Lazarus

Lazarus включает в себя TOpenGLControl - элемент управления LCL с контекстом OpenGL. Пакет Lazarus LazOpenGLContext находится в каталоге lazarus/components/opengl/lazopenglcontext.lpk. Пример можно найти в lazarus/examples/openglcontrol/openglcontrol_demo.lpi. 

Если вы хотите изменить реализацию TOpenGLControl, например, чтобы добавить новые свойства, такие как ColorBits и AuxBuffers, см. [Расширение TOpenGLControl](<../en/Extending_TOpenGLControl.md> "Extending TOpenGLControl"). 

## Сторонние OpenGL модули

  * [GLScene](<GLScene.md> "GLScene/ru") \- пакет Lazarus с множеством дополнительных функций для приложений OpenGL.
  * [Castle Game Engine](<../en/Castle_Game_Engine.md> "Castle Game Engine") \- позволяет перемещаться и визуализировать трехмерные миры (в VRML, X3D и других трехмерных форматах).



## Учебник

  * [OpenGL Tutorial](<OpenGL_Tutorial.md> "OpenGL Tutorial/ru")



## Учебник OpenGL 3.3 (современный OpnGL )

  * [Lazarus OpenGL 3.3 - Tutorial](<https://wiki.delphigl.com/index.php/Lazarus_-_OpenGL_3.3_Tutorial>)\- (нем.) (внешний ресурс)



## Вопросы/ответы по использованию привязок OpenGL

**В: Я не понимаю, почему версия ARB не работает. Насколько мне известно, она имеет те же точки входа, что и основные функциональные возможности, и для поддержки функций ARB-версии требуются версии OpenGL, даже если в ядре сейчас не версия ARB.**

О:Нет. OpenGL не должен поддерживать точки входа расширения, даже если он уже поддерживает те же функции в ядре. Поставщик может свободно удалять расширения и поддерживать только «основные» функции. 

Кроме того, иногда, когда функциональность продвигается из расширения ARB в ядро, оно немного изменяется (хотя в 99% случаев оно остается прежним). Примером являются шейдеры GLSL, которые предоставляют (очень немного) различные API через расширения ARB и спецификацию ядра 2.0. 

По сути, то, что раскрывается через расширения, полностью перпендикулярно тому, что раскрывается через основные функции, поэтому их следует инициализировать отдельно. В особых случаях вы можете делать такие трюки, как 
    
    
    if glIsBufferARB = nil then glIsBufferARB := @glIsBuffer;
    

но это следует оставить конечному программисту, который знает, что поведение «ARB_vertex_buffer_object» идентично тому, как оно работает в ядре. 

**В: Когда я пытаюсь использовать функцию gluTessCallback из модуля glu, либо компилятор жалуется, либо я получаю ошибку сегментации, когда пытаюсь запустить свою программу. Как я могу заставить это работать должным образом?**

О: Функция gluTessCallback определяется следующим образом в модуле **glu** : 
    
    
    {$IFDEF Windows}
      {$DEFINE extdecl := stdcall}
    {$ELSE}
      {$DEFINE extdecl := cdecl}
    {$ENDIF}
    
    gluTessCallback: procedure(tess: PGLUtesselator; which: GLenum; fn: TCallBack); extdecl;
    

**Примечание для Windows:** Общая проблема заключается в том, что соглашение о вызовах функций glu, по-видимому, варьируется в нескольких разных сборках DLL GLUT для Windows. В некоторых это stdcall, в некоторых cdecl. Наиболее часто используемым является stdcall. 

Вы должны определить свои функции обратного вызова следующим образом: 
    
    
    procedure MyTessBegin(which: GLEnum); 
    {$IFDEF Windows} stdcall; {$else} cdecl; {$endif}
    begin
      glBegin(which);
    end;
    
    procedure MyTessEnd; 
    {$IFDEF Windows} stdcall; {$else} cdecl; {$endif}
    begin
      glEnd();   // Требуется пустые ()
    end;
    
    procedure MyTessVertex(data: Pointer);
    {$IFDEF Windows} stdcall; {$else} cdecl; {$endif}
    begin
      glVertex3dv(PGLDouble(data));
    end;
    

Обратите внимание на модификаторы cdecl/stdcall; они необходимы. Затем вы вызываете gluTessCallback следующим образом: 
    
    
    gluTessCallback(tess, GLU_TESS_BEGIN, TCallBack(@MyTessBegin));
    gluTessCallback(tess, GLU_TESS_END, TCallBack(@MyTessEnd));
    gluTessCallback(tess, GLU_TESS_VERTEX, TCallBack(@MyTessVertex));
    

Убедитесь, что указатель на функцию обратного вызова приведен к типу TCallBack, как показано выше. 

В качестве альтернативы, если вы хотите напрямую обратиться к функциям OpenGL, используйте следующее соглашение о вызовах: 
    
    
    gluTessCallback(tess, GLU_TESS_BEGIN, TCallBack(glBegin));
    gluTessCallback(tess, GLU_TESS_END, TCallBack(glEnd));
    gluTessCallback(tess, GLU_TESS_VERTEX, TCallBack(glVertex3dv));

---

_Source: [https://wiki.freepascal.org/OpenGL/ru](https://web.archive.org/web/20240807122607/https://wiki.freepascal.org/OpenGL/ru)_
