# OpenGL Tutorial

│ **[English (en)](<../en/OpenGL_Tutorial.md>)** │  **русский (ru)** │

## Contents

  * 1 Введение
    * 1.1 GLUT
    * 1.2 GLFW
    * 1.3 LCL
    * 1.4 LCL / GLFW / GLUT
  * 2 Примеры
    * 2.1 Ваша первая LCL-программа
    * 2.2 Ваша первая GLUT-программа
    * 2.3 Рисование простой фигуры
    * 2.4 Использование списков отображения
    * 2.5 Анимация в полноэкраном режиме
    * 2.6 Освещение
    * 2.7 Растровые шрифты
    * 2.8 Текстуры
    * 2.9 Мультитекстурирование (расширения)
    * 2.10 Рендеринг в текстуру
    * 2.11 Массив вершин
  * 3 Современный OpenGL с использованием MacOS
  * 4 See also
  * 5 Внешние ссылки



# Введение

OpenGL является средой для разработки интерактивных 2D и 3D графических приложений. OpenGL позволяет быстро разрабатывать приложения, включающие в себя рендеринг, текстурирование, спецэффекты и другие мощные функции визуализации. Разработчики могут использовать OpenGL на всех популярных платформах и операционных системах. 

Более подробную информацию о OpenGL можно узнать [здесь](<http://www.opengl.org/about/overview/>). 

## GLUT

GLUT - библиотека для использования OpenGL, реализующий простой оконный интерфейс API. GLUT делает изучение OpenGL более легким. Это кроссплатформенный API, так что вы можете написать программу OpenGL, которая будет работать на большинстве операционных систем. 

Подробнее о GLUT вы можете почитать [здесь](<http://www.opengl.org/resources/libraries/glut/>). 

Многие ОС поставляются с GLUT, но если у вас её не оказалось, можно поискать её в Интернете с помощью [Google](<http://www.google.com/>). 

Версию для Windows можно скачать на [www.xmission.com](<http://www.xmission.com/~nate/glut.html>). 

Про GLUT unit можно почитать в статье [OpenGL](<../en/OpenGL.md> "OpenGL"). 

## GLFW

См. <http://www.glfw.org/>

## LCL

The Lazarus Component Library также может быть использована при работе с OpenGL. Lazarus включает в себя TOpenGLControl. Пакет LazOpenGLContext можно найти в lazarus/components/opengl/lazopenglcontext.lpk, примеры использования - в lazarus/examples/openglcontrol/openglcontrol_demo.lpi. 

## LCL / GLFW / GLUT

Когда нужно использовать GLUT, а когда LCL? 

  * GLUT лучше применим, если вы хотите сделать все сами.
  * LCL лучше использовать для обычных приложений. Например, 3D-редактор должен иметь несколько окон OpenGL и прочие обыкновенные элементы: кнопки, выпадающие списки, окна, модальные окна и т. д.



Часть OpenGL почти такая же. Для GLUT нужны dll под windows, а LCL обычно работает "из коробки", но и исполняемый файл имеет больший размер. 

# Примеры

## Ваша первая LCL-программа

Использование LCL, как правило, является самым простым способом доступа к OpenGL с помощью Lazarus. Поскольку GLUT устарела, использование LCL - это хорошая идея для нового проекта OpenGL Lazarus. Большинство примеров кода, описанных ниже для GLUT, легко перевести в код LCL, хотя вам нужно будет найти эквиваленты для функций с префиксом 'glut', например, вместо "glutSwapBuffers" мы будем использовать LCL-свойство "SwapBuffers" для отображения нашего рендеринга. Единственная замечательная функция, которую обеспечивает GLUT и которую трудно сделать с LCL - это отображение текста на экране (см. Раздел «Растровые шрифты» GLUT ниже). Однако, поскольку это ваша первая LCL-программа, мы немного упростим ее, не отображая текст. 

Lazarus поставляется с примером программы OpenGL, вы можете найти ее в папке _Lazarus/Examples/openglcontrol_. Этот пример демонстрирует множество мощных функций для создания анимированного изображения OpenGL. Тем не менее, это также относительно сложная программа. Ниже приведен минимальный проект Lazarus, который имитирует некоторые функции, описанные в примерах GLUT, описанных ниже. Чтобы создать его, запустите Lazarus и выберите Project/NewProject, чтобы создать новое приложение. Выберите пункт меню Project/ProjectInspector, нажмите кнопку 'Add..' (Добавить ...), перейдите к 'New Requirement' (Новое требование) и пакету «LazOpenGLContext». Затем вставьте приведенный ниже код в ваш 'unit1.pas'. Затем щелкните форму и на вкладке событий инспектора объектов свяжите событие «OnCreate» с функцией «FormCreate». Теперь вы сможете запустить новое приложение, выбрав пункт меню Run/Run (Выполнить/запустить). 

Код создает новую панель OpenGL, которая заполняет форму. OpenGL рисует простой треугольник на форме. Обратите внимание, что при запуске приложения вы можете изменить размер формы, и треугольник, чтобы заполнить форму, масштабируется пропорционально. 
    
    
    unit Unit1;
    
    {$mode objfpc}{$H+}
    
    interface
    
    uses
      Classes, SysUtils, FileUtil, Forms, Controls, Graphics, Dialogs, OpenGLContext, gl;
    
    type
      TForm1 = class(TForm)
        procedure FormCreate(Sender: TObject);
        procedure GLboxPaint(Sender: TObject);
      private
        GLBox: TOpenGLControl;
      public
      end;
    
    var
      Form1: TForm1;
    
    implementation
    
    {$R *.lfm}
    
    procedure TForm1.GLboxPaint(Sender: TObject);
    begin
      glClearColor(0.27, 0.53, 0.71, 1.0); // Задаем синий фон
      glClear(GL_COLOR_BUFFER_BIT or GL_DEPTH_BUFFER_BIT);
      glLoadIdentity;
      glBegin(GL_TRIANGLES);
        glColor3f(1, 0, 0);
        glVertex3f( 0.0, 1.0, 0.0);
        glColor3f(0, 1, 0);
        glVertex3f(-1.0,-1.0, 0.0);
        glColor3f(0, 0, 1);
        glVertex3f( 1.0,-1.0, 0.0);
      glEnd;
      GLbox.SwapBuffers;
    end;
    
    procedure TForm1.FormCreate(Sender: TObject);
    begin
      GLbox := TOpenGLControl.Create(Self);
      GLbox.AutoResizeViewport := true;
      GLBox.Parent             := Self;
      GLBox.MultiSampling      := 4;
      GLBox.Align              := alClient;
      GLBox.OnPaint            := @GLboxPaint; // для "mode delphi" должно быть "GLBox.OnPaint := GLboxPaint"
      GLBox.invalidate;
    end;
    
    end.
    

## Ваша первая GLUT-программа

Для того чтобы использовать GLUT, сначала необходимо инициализировать его. Это делается с помощью функции **glutInit**. Эта функция может разобрать командную строку и установить параметры для главного окна, но она ожидает, что ввод будет в стиле C/C++. Вы можете написать собственную функцию, которая будет преобразовывать ParamCount и ParamStr в стиль C/C++. 
    
    
     procedure glutInitPascal(ParseCmdLine: Boolean); 
     var
       Cmd: array of PChar;
       CmdCount, I: Integer;
     begin
       if ParseCmdLine then
         CmdCount := ParamCount + 1
       else
         CmdCount := 1;
       SetLength(Cmd, CmdCount);
       for I := 0 to CmdCount - 1 do
         Cmd[I] := PChar(ParamStr(I));
       glutInit(@CmdCount, @Cmd);
     end;
    

По сути, вы создаете массив и заполняете его строками из ParamStr. Эта функция также имеет параметр, который указывает то, что передается glutInit -- вся команда или просто имя исполняемого файла. 

ToDo: вероятно достаточно **glutInit(@argc, @argv);**. 

  
Подробнее о glutInit: <http://www.opengl.org/resources/libraries/glut/spec3/node10.html>

Дальше нужно создать главное окно. Установите режим отображения для главного окна использованием `glutInitDisplayMode`. Она принимает только один параметр, который является комбинацией флагов. Обычно `GLUT_DOUBLE or GLUT_RGB or GLUT_DEPTH` \- это всё, что необходимо. 

Подробнее о glutInitDisplayMode: <http://www.opengl.org/resources/libraries/glut/spec3/node12.html>

Положение и размер окна изменяются с помощью `glutInitWindowPosition` и `glutInitWindowSize`. Они принимают 2 параметра: X и Y координаты в первой функции, ширина и высота в последней. Вы можете использовать `glutGet`, чтобы найти размер экрана и поместить центр окна. 

ToDo: неверный перевод 

Подробнее о `glutInitWindowPosition`, `glutInitWindowSize` и `glutGet`: <http://www.opengl.org/resources/libraries/glut/spec3/node11.html> <http://www.opengl.org/documentation/specs/glut/spec3/node70.html>

Наконец, окна должны быть созданы с использованием функции **glutCreateWindow**. Это создаст его с названием, переданным в параметре. Функция возвращает handle окна. Это можно использовать в других функциях, которые требуют handle. 

Подробнее о glutCreateWindow: <http://www.opengl.org/resources/libraries/glut/spec3/node16.html>

Прежде чем программа войдёт в главный цикл, нужно установить некоторые функции для обратной связи. Это нужно для рисования окна, для изменения размеров и для получения нажатий клавиш на клавиатуре. Она устаналивается с помощью `glutDisplayFunc`, `glutReshapeFunc` и `glutKeyboardFunc`. 

Подробнее: <http://www.opengl.org/resources/libraries/glut/spec3/node45.html#SECTION00080000000000000000>

Функция рисования может выглядеть следующим образом: 
    
    
     procedure DrawGLScene; cdecl;
     begin
       glClear(GL_COLOR_BUFFER_BIT or GL_DEPTH_BUFFER_BIT);
       glutSwapBuffers;
     end;
    

Она только очистит окно, зальёт его цветом фона и сбросит ZBuffer (не волнуйтесь о zbuffer... об этом позже). 

Функция изменения размера может выглядеть так: 
    
    
     procedure ReSizeGLScene(Width, Height: Integer); cdecl;
     begin
       if Height = 0 then
         Height := 1;
     
       glViewport(0, 0, Width, Height);
       glMatrixMode(GL_PROJECTION);
       glLoadIdentity;
       gluPerspective(45, Width / Height, 0.1, 1000);
     
       glMatrixMode(GL_MODELVIEW);
       glLoadIdentity;
     end;
    

С помощью этого кода вы говорите OpenGL, где в окне будет происходить отрисовка и задаете матрицу (матричные функции будут описаны позже). 

Ввод с клавиатуры: 
    
    
     procedure GLKeyboard(Key: Byte; X, Y: Longint); cdecl;
     begin
       if Key = 27 then
         Halt(0);
     end;
    

Эта функция завершит работу программы при нажатии клавиши ESC. Это единственный способ остановить программу. Если вы закроете окно по-другому, оно исчезнет, но сама программа продолжит работать бесконечно долго. 

Для запуска основного цикла вызовите `glutMainLoop`. Это запустит бесконечный цикл, в котором содержатся все ваши функции для обратной связи. 

Основная часть программы может выглядеть так: 
    
    
     const 
       AppWidth = 640; 
       AppHeight = 480; 
     
     procedure InitializeGL; 
     begin 
       glClearColor(0.18, 0.20, 0.66, 0); 
     end; 
     
     var 
       ScreenWidth, ScreenHeight: Integer; 
     begin 
       glutInitPascal(True); 
       glutInitDisplayMode(GLUT_DOUBLE or GLUT_RGB or GLUT_DEPTH); 
       glutInitWindowSize(AppWidth, AppHeight); 
       ScreenWidth := glutGet(GLUT_SCREEN_WIDTH); 
       ScreenHeight := glutGet(GLUT_SCREEN_HEIGHT); 
       glutInitWindowPosition((ScreenWidth - AppWidth) div 2,
         (ScreenHeight - AppHeight) div 2); 
       glutCreateWindow('OpenGL Tutorial 1'); 
     
       InitializeGL; 
     
       glutDisplayFunc(@DrawGLScene); 
       glutReshapeFunc(@ReSizeGLScene); 
       glutKeyboardFunc(@GLKeyboard); 
     
       glutMainLoop; 
     end.
    

Следующий урок добавит некоторый код, который будет рисовать простую фигуру. 

Скачать исходный код или исполняемые файлы linux/windows с [Lazarus CCR SourceForge](<http://sourceforge.net/project/showfiles.php?group_id=92177&package_id=199145>). 

## Рисование простой фигуры

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Примечание:** Следующие детали описывают в основном только код OpenGL, поэтому они будут работать как с GLUT, так и с LCL. Вы можете узнать функции GLUT по префиксу 'glu'.

Мы добавим только несколько строк кода и сосредоточим внимание на объяснение некоторых функций OpenGL. 

Поясним следующий код. 
    
    
     begin 
       glMatrixMode(GL_PROJECTION);
       glLoadIdentity;
       gluPerspective(45, Width / Height, 0.1, 1000);
     
       glMatrixMode(GL_MODELVIEW);
       glLoadIdentity;
     end;
    

Использование функции `glMatrixMode` указывает, какую матрицу вы хотите установить. OpenGL работает с 3 матрицами: `GL_MODELVIEW`: это используется при перемещении вершин в пространстве модели. `GL_PROJECTION`: это используется для преобразования 3D-координат в 2D-координаты. `GL_TEXTURE`: это используется для изменения координат текстуры. 

После того, как вы выбрали матрицу, которую хотите изменить, вы можете вызвать функции, которые изменяют значения матриц. `glLoadIdentity` сбросит матрицу. Поскольку почти все матричные функции умножают текущую матрицу на сгенерированную, иногда необходимо очистить матрицу с помощью этой функции. 

Чтобы установить перспективу матрицы, используйте функцию `gluPerspective`. Четыре параметра задают поле зрения, соотношение сторон, передний и задний план. Это просто. 

Теперь вы измените матрицу модели ... на этот раз вы просто установите ее на идентичность. 

Код рисования для первой фигуры: 
    
    
     procedure DrawGLScene; cdecl;
     begin
       glClear(GL_COLOR_BUFFER_BIT or GL_DEPTH_BUFFER_BIT);
     
       glLoadIdentity;
       glTranslatef(0, 0, -5);
     
       glBegin(GL_TRIANGLES);
         glColor3f(1, 0, 0);
         glVertex3f(-1, -1, 0);
     
         glColor3f(0, 1, 0);
         glVertex3f(1, -1, 0);
     
         glColor3f(0, 0, 1);
         glVertex3f(0, 1, 0);
       glEnd;
     
       glutSwapBuffers;
     end;
    

Мы использовали функцию glClear. Она просто сбрасывает буферы. Следующие две функции пока пропустим. 

`glBegin` обозначает начало рисования в блок. После нее можно начать ввод вершин. Параметр описывает, что мы будем рисовать: 

`GL_POINTS`: просто рисуем n точек. Нужно указать n вершин. 

`GL_LINES`: каждая пара вершин задаст линию. Вершины 2n-1 и 2n задают одну линию. При использовании n вершин получится n/2 линий. 

`GL_LINE_STRIP`: рисует отрезки из первой вершины через все в последнюю. Получится n-1 линий. 

`GL_LINE_LOOP`: то же самое, но добавляется отрезок из последней вершины в первую. Получится n линий. 

`GL_TRIANGLES`: каждые три точки зададут треугольник. Получится n/3 треугольников. 

`GL_TRIANGLE_STRIP`: рисует связанные треугольники. Каждая вершина после первых двух будет задавать треугольник. При нечетном n вершины n, n+1 и n+2 зададут треугольник. При четном n - n+1, n и n+2. Получится n-2 треугольника. 

`GL_TRIANGLE_FAN`: тоже рисует связанные треугольники. Каждая вершина после первых двух будет задавать треугольник. Вершины 1, n+1 и n+2 зададут треугольник. Получится n-2 треугольников. 

`GL_QUADS`: каждые четыре вершины рисуют четырехугольник. Получится n/4 четырехугольников. 

`GL_QUAD_STRIP`: рисует связанные четырехугольники. Каждая пара вершин после первой пары будет задавать четырехугольник. Вершины 2n-1, 2n, 2n+2 и 2n+1 задают четырехугольник n. n/2-1 четырехугольников получится. Обратите внимание, что порядок, в котором вершины используются для построения четырехугольника из ленты данных отличается от используемого с независимыми данными. 

`GL_POLYGON`: рисует один выпуклый многоугольник. Вершины определяют этот полигон. 

[![SimpleShapePic1.jpg](https://wiki.freepascal.org/images/f/f3/SimpleShapePic1.jpg)](</File:SimpleShapePic1.jpg>)

[](</File:SimpleShapePic1.jpg> "Enlarge")

Нарисуем один треугольник с помощью `GL_TRIANGLES`. Функция `glVertex3f` задаёт положение вершины. Есть еще glVertex* функции. Единственное отличие состоит в количестве и типе параметров, которые они принимают. Например, `glVertex2i` принимает два параметра (x и y) типа integer. `glVertex3f` это обычно то, что вам нужно. 

Перед `glVertex` можно установить цвет, материал, текстуру... Для простоты просто укажем цвет для каждой вершины. Цвет задаётся с помощью функции `glColor3f`. `glColor` также может принимать разные параметры, аналогично `glVertex`. 

В коде мы можем увидеть, что Z имеет значение 0 для всех вершин. Поскольку передний план установлен как 0.1, треугольник не будет видно. Тут как раз нужны те две функции, которые мы пропустили. Вы уже знаете, что функция `glLoadIdentity` сбрасывает матрицу. `glTranslatef` перемещает треугольник на X, Y и Z, которые вы зададите. Если установить Z как -5 (отрицательный Z будет находиться дальше от камеры), то все вершины будут на 5 единиц дальше от камеры, и треугольник будет видно. 

После завершения рисования вызовите функцию `glEnd`. Если хотите, то можете начать новое рисование с помощью `glBegin`. 

Скачать исходный код, исполняемые файлы linux или windows с [Lazarus CCR SourceForge](<http://sourceforge.net/project/showfiles.php?group_id=92177&package_id=199145>). 

## Использование списков отображения

Иногда вам будет необходимо отобразить один и тот же объект на сцене несколько раз. OpenGL имеет возможность создавать **списки отображения** , которые также создают рисунок немного быстрее. Создать этот список очень легко. Просто задать вершины и заключить их между `glNewList` и `glEndList`. 
    
    
     const
       LIST_OBJECT = 1;
     
     procedure CreateList;
     begin
       glNewList(LIST_OBJECT, GL_COMPILE);
         glBegin(GL_TRIANGLE_FAN);
           glColor3f(1, 0, 0);
           glVertex3f(0, 0.5, 0);
     
           glColor3f(1, 1, 0);
           glVertex3f(-0.5, -0.5, 0.5);
     
           glColor3f(1, 1, 1);
           glVertex3f(0.5, -0.5, 0.5);
     
           glColor3f(0, 1, 1);
           glVertex3f(0.5, -0.5, -0.5);
     
           glColor3f(0, 0, 1);
           glVertex3f(-0.5, -0.5, -0.5);
     
           glColor3f(0, 1, 0);
           glVertex3f(-0.5, -0.5, 0.5);
         glEnd;
     
         glBegin(GL_QUADS);
           glColor3f(1, 1, 0);
           glVertex3f(-0.5, -0.5, 0.5);
     
           glColor3f(1, 1, 1);
           glVertex3f(0.5, -0.5, 0.5);
     
           glColor3f(0, 1, 1);
           glVertex3f(0.5, -0.5, -0.5);
     
           glColor3f(0, 0, 1);
           glVertex3f(-0.5, -0.5, -0.5);
     
           glColor3f(0, 1, 0);
           glVertex3f(-0.5, -0.5, 0.5);
         glEnd;
       glEndList;
     end;
    

`glNewList` создаёт новый список, и все функции рисования будут записаны, пока не вызовется `glEndList`. 

Первый параметр функции `glNewList` задаёт ID списка. Каждый список определяется по его идентификатору. Если список с заданным ID уже существует, то он очистится перед записью. Если второй параметр `GL_COMPILE`, то все функции рисования просто записываются, если это `GL_COMPILE_AND_EXECUTE`, то они записываются и выполняются автоматически. 

Функция `glIsList` поможет вам со списками. Она сообщает, существует ли список с таким ID. 

Еще одна полезная функция - это `glGenLists`. Он создает несколько пустых списков отображения. Вы задаёте количество нужных списков и получаете номер первого из них. Если вам нужно создать n списков и получить r ID, получатся списки: r, r+1, r+2,..., r+n-1 

Все созданные списки должны быть удалены. Для этого при выходе из программы вы должны сделать следующее: 
    
    
     procedure GLKeyboard(Key: Byte; X, Y: Longint); cdecl;
     begin
       if Key = 27 then
       begin
         glDeleteLists(LIST_OBJECT, 1);
         Halt(0);
       end;
     end;
    

`glDeleteLists` принимает 2 параметра, ID списка отображения и количество списков для удаления. Если ID - r и количество списков для удаления n, удаляются следующие листы: r, r+1, r+2,..., r+n-1 

Теперь вы знаете, как создавать и удалять списки отображения. Попробуем их применить: 
    
    
     procedure DrawGLScene; cdecl;
     begin
       glClear(GL_COLOR_BUFFER_BIT or GL_DEPTH_BUFFER_BIT);
     
       glLoadIdentity;
       glTranslatef(-2, 0, -5);
       glRotatef(40, 1, 0, 1);
       glCallList(LIST_OBJECT);
     
       glLoadIdentity;
       glTranslatef(1, -2, -10);
       glRotatef(62, 0, 1, 0);
       glCallList(LIST_OBJECT);
     
       glLoadIdentity;
       glTranslatef(-4, 0.5, -15);
       glRotatef(200, 1, 0, 0);
       glCallList(LIST_OBJECT);
     
       glutSwapBuffers;
     end;
    

[![DisplayListsPic1.jpg](https://wiki.freepascal.org/images/a/a5/DisplayListsPic1.jpg)](</File:DisplayListsPic1.jpg>)

[](</File:DisplayListsPic1.jpg> "Enlarge")

Используйте `glCallList` для рисования одного списка. Прежде, чем делать список отображения, вы меняли модели матрицы и рисовали объект в разных местах. 

Иногда нужно рисовать несколько списков одновременно. Это возможно при использовании функции `glCallLists`. Она принимает количество списков, которые вы хотите нарисовать, тип массива, в котором содержатся ID и массив с идентификаторами списков. Тип может принимать одно из следующих значений: 

`GL_BYTE`: список обрабатывается как массив байтов со знаком, каждый в диапазоне от -128 до 127. 

`GL_UNSIGNED_BYTE`: список обрабатывается как массив байтов без знака, каждый в диапазоне от 0 до 255. 

`GL_SHORT`: Список обрабатывается как массив двухбайтовых целых чисел со знаком, каждое из которых находится в диапазоне от -32768 до 32767. 

`GL_UNSIGNED_SHORT`: Список обрабатывается как массив двухбайтовых целых чисел без знака, каждое из которых находится в диапазоне от 0 до 65535. 

`GL_INT`: списки обрабатываются как массив четырехбайтовых целых чисел со знаком. 

`GL_UNSIGNED_INT`: Список обрабатывается как массив беззнаковых четырехбайтовых целых чисел. 

`GL_FLOAT`: Список обрабатывается как массив четырехбайтовых значений с плавающей точкой. 

`GL_2_BYTES`: Список обрабатывается как массив байтов без знака. Каждая пара байтов определяет один идентификатор списка отображения. Значение пары вычисляется как 256-кратное значение без знака первого байта плюс значение без знака второго байта. 

`GL_3_BYTES`: Список обрабатывается как массив байтов без знака. Каждый триплет байтов определяет один идентификатор списка отображения. Значение триплета вычисляется как 65536, умноженное на значение без знака первого байта, плюс 256 раз без знака без знака второго байта, плюс значение без знака третьего байта. 

`GL_4_BYTES`: Список обрабатывается как массив байтов без знака. Каждая четверка байтов задает один идентификатор списка отображения. Значение четверки вычисляется как 16777216 раз значение без знака первого байта, плюс 65536 раз значение без знака второго байта, плюс 256 раз значение без знака третьего байта, плюс значение без знака четвертого байта. 

Это пока. Следующий урок покажет, как создать маленькую планетную систему. Мы поговорим о матрицах и о том, как сделать анимированную сцену, которая не зависит от количества кадров в секунду. 

Скачать исходный код, исполняемые файлы linux или windows с [Lazarus CCR SourceForge](<http://sourceforge.net/project/showfiles.php?group_id=92177&package_id=199145>). 

## Анимация в полноэкраном режиме

Переход в полноэкранный режим в GLUT прост. Измените основную часть программы: 
    
    
     const
       FSMode = '800x600:32@75';
     
     begin
       glutInitPascal(False);
       glutInitDisplayMode(GLUT_DOUBLE or GLUT_RGB or GLUT_DEPTH);
       glutGameModeString(FSMode);
       glutEnterGameMode;
       glutSetCursor(GLUT_CURSOR_NONE);
     
       InitializeGL;
     
       glutDisplayFunc(@DrawGLScene);
       glutReshapeFunc(@ReSizeGLScene);
       glutKeyboardFunc(@GLKeyboard);
       glutIdleFunc(@DrawGLScene);
     
       glutMainLoop;
     end.
    

Так как нам не нужно, чтобы GLUT разбирал командную строку, то на этот раз на glutInitPascal стави параметр false. Также не нужен код для создания окна. Функцией `glutEnterGameMode` GLUT автоматически создаст окно на весь экран. Чтобы указать желаемый режим, через функцию `glutGameModeString`передаём нужные параметры экрана. Format of that string is: 

Так как в этот раз мы не хотим, чтобы GLUT анализировал командную строку, мы вызываем `glutInitPascal` с параметром False. Как видите, нет кода для создания окна. GLUT имеет `glutEnterGameMode`, который создает полноразмерное окно. Чтобы указать, какой полноэкранный режим вы хотите, вы вызываете функцию `glutGameModeString`, которая принимает строку, которая определяет режим, который вам нравится. 

Формат этой строки: 
    
    
     [ширина "x" высота][":" цвет. палитра]["@" Гц]
    

В строке FSMode мы задали разрешение экрана 800x600, 32-битнкю цветовую палитру и частоту обновления экрана 75Гц. Можно пропустить один из элементов. Если вы пропустите разрешение, GLUT будет пытаться использовать текущее или самый низкое из тех, которые можно установить. То же для других параметров. 

В строке `FSMode` мы объявили, что полноэкранный режим должен быть 800x600, с 32-битной палитрой и обновлением 75 Гц. Можно пропустить одну из групп. Если вы опустите размер, GLUT попытается использовать текущий или первый наименьший, который может работать. Эта политика используется и для других параметров. 

Обычно в полноэкранном режиме курсор не виден. Чтобы скрыть курсор, вы должны использовать функцию `glutSetCursor`. Она принимает только один параметр, который описывает курсор, который вы хотели бы видеть: 
    
    
     GLUT_CURSOR_RIGHT_ARROW
     GLUT_CURSOR_LEFT_ARROW
     GLUT_CURSOR_INFO
     GLUT_CURSOR_DESTROY
     GLUT_CURSOR_HELP
     GLUT_CURSOR_CYCLE
     GLUT_CURSOR_SPRAY
     GLUT_CURSOR_WAIT
     GLUT_CURSOR_TEXT
     GLUT_CURSOR_CROSSHAIR
     GLUT_CURSOR_UP_DOWN
     GLUT_CURSOR_LEFT_RIGHT
     GLUT_CURSOR_TOP_SIDE
     GLUT_CURSOR_BOTTOM_SIDE
     GLUT_CURSOR_LEFT_SIDE
     GLUT_CURSOR_RIGHT_SIDE
     GLUT_CURSOR_TOP_LEFT_CORNER
     GLUT_CURSOR_TOP_RIGHT_CORNER
     GLUT_CURSOR_BOTTOM_RIGHT_CORNER
     GLUT_CURSOR_BOTTOM_LEFT_CORNER
     GLUT_CURSOR_FULL_CROSSHAIR
     GLUT_CURSOR_NONE
     GLUT_CURSOR_INHERIT
    

`glutIdleFunc` определяет функцию обратного вызова, которую вы захотите вызывать каждый раз, когда у вашей программы нет сообщений для обработки. Так как мы просто хотим рендерить новый кадр, если ничего не нужно делать, просто установите функцию ожидания в `DrawGLScene`. Некоторые другие учебные пособия показывают, что функция бездействия должна отправлять сообщение об обновлении вместо рисования, но в этом случае у меня на 50-100 кадров меньше, чем при использовании описанного мной метода. 

Теперь давайте посмотрим на завершение программы, где вам нужно выйти из полноэкранного режима: 
    
    
     procedure GLKeyboard(Key: Byte; X, Y: Longint); cdecl;
     begin
       if Key = 27 then
       begin
         glutLeaveGameMode;
         Halt(0);
       end;
     end;
    

Как видите, все, что вам нужно сделать, это вызвать `glutLeaveGameMode`. 

Теперь мы представим некоторые новые матричные функции. Во-первых, давайте изменим функцию `ReSizeGLScene`: 
    
    
     procedure ReSizeGLScene(Width, Height: Integer); cdecl;
     begin
       .
       .
       .
     
       glMatrixMode(GL_MODELVIEW);
       glLoadIdentity;
       gluLookAt(0, 20, 25, 0, 0, 0, 0, 1, 0);
     end;
    

`gluLookAt` создает матрицу, которая будет определять, откуда вы смотрите на объекты. Первые 3 параметра - это координаты X, Y и Z положения камеры. Следующие 3 параметра - это координаты X, Y и Z точки, на которую смотрит камера, а последние 3 параметра определяют вектор «вверх» (где «вверх» для камеры). Обычно up - это положительная ось y. 

Хорошо, давайте сейчас порисуем. Поскольку вы устанавливаете матрицу с помощью `gluLookAt`, которая должна использоваться со всеми объектами, вы не можете просто использовать `glLoadIdentity` для сброса матрицы для следующего объекта ... вы сохраните предыдущее состояние матрицы и восстановите его после отрисовки объекта: 
    
    
     procedure DrawGLScene; cdecl;
     var
       T: Single;
     begin
       T := glutGet(GLUT_ELAPSED_TIME) / 1000;
       glClear(GL_COLOR_BUFFER_BIT or GL_DEPTH_BUFFER_BIT);
     
       glPushMatrix;
         glRotatef(5 * T, 0, 1, 0);
         glColor3f(1, 1, 0);
         glutWireSphere(2, 20, 20);
       glPopMatrix;
     
       glPushMatrix;
         glRotatef(90 * T, 0, 1, 0);
         glTranslatef(5, 0, 0);
         glRotatef(40 * T, 0, 1, 0);
         glColor3f(1, 0, 0);
         glutWireSphere(0.6, 10, 10);
       glPopMatrix;
     
       glPushMatrix;
         glRotatef(60 * T, 0, 1, 0);
         glTranslatef(-3, 0, 9);
         glRotatef(50 * T, 0, 1, 0);
         glColor3f(0, 1, 0);
         glutWireSphere(1, 16, 16);
     
         glPushMatrix;
           glRotatef(360 * T, 0, 1, 0);
           glTranslatef(-1.7, 0, 0);
           glRotatef(50 * T, 0, 1, 0);
           glColor3f(0, 0, 1);
           glutWireSphere(0.4, 10, 10);
         glPopMatrix;
     
       glPopMatrix;
     
       glutSwapBuffers;
     end;
    

[![FullScreenAnimationPic1.jpg](https://wiki.freepascal.org/images/5/51/FullScreenAnimationPic1.jpg)](</File:FullScreenAnimationPic1.jpg>)

[](</File:FullScreenAnimationPic1.jpg> "Enlarge")

`glPushMatrix` и `glPopMatrix` используются для сохранения и восстановления состояния матрицы. Как видите, мы сохраняем состояние матрицы, затем меняем матрицу, чтобы нарисовать объект в нужном месте, а затем восстанавливаем старое состояние матрицы. 

Вы можете спросить, для чего нужна переменная `T`? Ну, она используется для определения скорости анимации. Каждое изменение, зависящее от времени, умножается на `T`. Таким образом, скорость анимации постоянна на каждой частоте кадров. 

Функция `glutGet` с параметром `GLUT_ELAPSED_TIME` возвращает время в миллисекундах из `glutInit`. Разделив ее значение на 1000, мы получим время в секундах. 

Функция `glRotatef` создает матрицу вращения. Первый параметр - это угол в градусах, а последние 3 параметра определяют ось, вокруг которой будет выполняться вращение. Поскольку вы умножили угол на `T`, объект будет повернут на этот угол ровно за 1 секунду. 

Загрузите исходный код, исполняемый файл linux или исполняемый файл windows из [Lazarus CCR SourceForge](<http://sourceforge.net/project/showfiles.php?group_id=92177&package_id=199145>). 

## Освещение

Этот урок познакомит вас с освещением сцены. Вы сделаете вращающийся куб и один источник света, который добавит реалистичности сцене, но сначала давайте сделаем несколько вспомогательных модулей. 

На данный момент урок будет иметь только базовые функции, которые помогут нам получить текущее время и дельту (время, прошедшее от одного рендеринга до другого вызова рендеринга), и рассчитать количество кадров в секунду. 
    
    
     unit utils;
     
     {$mode objfpc}{$H+}
     
     interface
     
     uses
       glut;
     
     function GetTotalTime: Single;
     function GetDeltaTime: Single;
     procedure FrameRendered(Count: Integer = 1);
     function GetFPS: Single;
     
     implementation
     
     var
       OldTime: Integer = 0;
       FPSTime: Integer = 0;
       FPSCount: Integer = 0;
     
     function GetTotalTime: Single;
     begin
       Result := glutGet(GLUT_ELAPSED_TIME) / 1000;
     end;
     
     function GetDeltaTime: Single;
     var
       NewTime: Integer;
     begin
       NewTime := glutGet(GLUT_ELAPSED_TIME);
       Result := (NewTime - OldTime) / 1000;
       OldTime := NewTime;
     end;
     
     procedure FrameRendered(Count: Integer);
     begin
       Inc(FPSCount, Count);
     end;
     
     function GetFPS: Single;
     var
       NewTime: Integer;
     begin
       NewTime := glutGet(GLUT_ELAPSED_TIME);
     
       Result := FPSCount / ((NewTime - FPSTime) / 1000);
     
       FPSTime := NewTime;
       FPSCount := 0;
     end;
     
     end.
    

Как видите, в этом модуле нет ничего сложного. Время просто сохраняется между вызовами, а разница возвращается. FrameRendered должен вызываться каждый раз, когда вы рисуете сцену, чтобы функция могла рассчитывать FPS. 

Теперь давайте повеселимся с освещением. В OpenGL есть несколько типов света: ambient (окружающий), diffuse (рассеянный), point(точечный), spot (пятно), specular (зеркальный) и emissive (излучение). 

  
Окружающий свет (ambient) - это что-то вроде Солнца. Когда солнечные лучи проходят через окно комнаты, они ударяются о стены и отражаются во всех направлениях, что в среднем осветляет всю комнату. Все вершины освещены окружающим светом. 

Рассеянный свет(diffuse) может быть представлен как параллельные световые лучи, исходящие издалека. Они будут освещать только вершины, ориентированные на источник света. 

Точечный свет(point) освещает все вокруг себя. Он похож на огненный шар, он посылает световые лучи вокруг себя и освещает вершины, которые ориентированы на источник света и достаточно близки. 

Пятно (spot) - как свет от фонарика. Это просто точечный источник света с небольшим радиусом светового конуса. Все вершины, которые попадают внутрь конуса и находятся достаточно близко, подсвечиваются. 

Зеркальный свет (specular), как и рассеянный свет, является направленным типом света. Это происходит из одного конкретного направления. Разница между ними заключается в том, что зеркальный свет отражается от поверхности острым и равномерным образом. Рендеринг зеркального света зависит от угла между зрителем и источником света. С точки зрения зрителя зеркальный свет создает выделенную область на поверхности наблюдаемого объекта, известную как зеркальное выделение или зеркальное отражение. 

Излучение (emissive) - свет немного отличается от любых других ранее объясненных компонентов света. Этот свет исходит от объекта, который вы рисуете, но не освещаете другие объекты поблизости. 

Для простоты мы будем использовать только рассеянный свет в этом уроке. Позже, некоторые другие источники света могут появиться в уроках :) 

Давайте посмотрим, как включить освещение в сцене: 
    
    
     const
       DiffuseLight: array[0..3] of GLfloat = (0.8, 0.8, 0.8, 1);
     
       glEnable(GL_LIGHTING);
       glLightfv(GL_LIGHT0, GL_DIFFUSE, DiffuseLight);
       glEnable(GL_LIGHT0);
    

Как видите, мы включаем подсветку в OpenGL, чтобы свет влиял на сцену, которую вы рендерили. Параметры освещения задаются функцией `glLightfv`. Потребуется 3 параметра ... один для числа освещения, которое вы хотите изменить (OpenGL поддерживает до 8 источников света), другой сообщает OpenGL, какой параметр освещения изменить, а последний - новый параметр для освещения. В этом уроке вы установите только рассеянный свет для освещения. После этого вы можете включить освещение, и на сцене будет свет ... но ... это еще не все. 

Подробнее о `glLightfv`: <http://www.opengl.org//documentation/specs/man_pages/hardcopy/GL/html/gl/light.html>

Если вы хотите использовать источники света, вы не можете просто задать цвет для вершины ... вы также должны задать и материал для вершин. Давайте настроим материал для рисования: 
    
    
     glEnable(GL_COLOR_MATERIAL);
     glColorMaterial(GL_FRONT, GL_AMBIENT_AND_DIFFUSE);
    

[![LightPic1.jpg](https://wiki.freepascal.org/images/e/ed/LightPic1.jpg)](</File:LightPic1.jpg>)

[](</File:LightPic1.jpg> "Enlarge")

Вы ожидали чего-то более сложного? :) Ну, этот код позволит нам использовать функцию `glColor`, чтобы задать материал для вершин. Используя функцию `glEnable` и флаг `GL_COLOR_MATERIAL`, вы можете определить, какие свойства материала будут изменять `glColor`. `glColorMaterial (GL_FRONT, GL_AMBIENT_AND_DIFFUSE)` сообщает OpenGL, что `glColor` изменяет окружающий и рассеянный материал. Мы обсудим материалы больше в следующих уроках. 

Еще одна вещь, которая важна при использовании источников света ... каждая вершина должна иметь нормаль, связанную с ней. Нормаль используется для определения направления вершины, чтобы свет можно было правильно рассчитать. Вы будете использовать функцию GLUT, чтобы нарисовать куб, и он предоставит нам нормали, так что на этот раз мы просто пройдемся по нормали. 

После всех этих настроек, свет будет сиять в вашем кубе :) 

Часть текста скопирована из [The OpenGL Light Bible](<http://www.falloutsoftware.com/tutorials/gl/gl8.htm>)

Скачать исходный код, исполняемые файлы linux или windows с [Lazarus CCR SourceForge](<http://sourceforge.net/project/showfiles.php?group_id=92177&package_id=199145>). 

## Растровые шрифты

Игры и программы, как правило, должны писать текст на экране. GLUT предоставляет несколько функций, которые не зависят от платформы, для рисования символов. 

Сначала мы покажем, как использовать стандартные растровые шрифты. Почти все дополнения кода будут сделаны в модуле utils.pas. 

Поскольку текст будет отображаться в 2D, нам нужно знать ширину и высоту области просмотра ... поэтому мы напишем две функции для этого: 
    
    
     function glGetViewportWidth: Integer;
     var
       Rect: array[0..3] of Integer;
     begin
       glGetIntegerv(GL_VIEWPORT, @Rect);
       Result := Rect[2] - Rect[0];
     end;
     
     function glGetViewportHeight: Integer;
     var
       Rect: array[0..3] of Integer;
     begin
       glGetIntegerv(GL_VIEWPORT, @Rect);
       Result := Rect[3] - Rect[1];
     end;
    

Мы просто получаем left/right, top/bottom и вычисляем width/height, вычитая их. 

Должны быть функции для входа и выхода из режима 2D: 
    
    
     procedure glEnter2D;
     begin
       glMatrixMode(GL_PROJECTION);
       glPushMatrix;
       glLoadIdentity;
       gluOrtho2D(0, glGetViewportWidth, 0, glGetViewportHeight);
     
       glMatrixMode(GL_MODELVIEW);
       glPushMatrix;
       glLoadIdentity;
     
       glDisable(GL_DEPTH_TEST);
     end;
     
     procedure glLeave2D;
     begin
       glMatrixMode(GL_PROJECTION);
       glPopMatrix;
       glMatrixMode(GL_MODELVIEW);
       glPopMatrix;
     
       glEnable(GL_DEPTH_TEST);
     end;
    

При входе в режим 2D мы сохраняем текущие матрицы и устанавливаем 2D-матрицу с помощью функции `gluOrtho2D`. Таким образом, если мы нарисуем какую-либо штуку в координатах (100,100), она будет нарисована точно на 100 пикселей от левого края окна и на 100 пикселей от нижнего края формы (положительные значения по оси Y направлены вверх). Также мы отключаем ZBuffer. Таким образом, текст не изменит ZBuffer. 

Выход из режима 2D просто возвращает старые матрицы и включает ZBuffer. 

Теперь мы можем создать функцию для рисования текста: 
    
    
     procedure glWrite(X, Y: GLfloat; Font: Pointer; Text: String);
     var
       I: Integer;
     begin
       glRasterPos2f(X, Y);
       for I := 1 to Length(Text) do
         glutBitmapCharacter(Font, Integer(Text[I]));
     end;
    

`glutBitmapCharacter` может рисовать только один символ выбранного шрифта. Первый параметр - желаемый шрифт (GLUT_BITMAP_9_BY_15, GLUT_BITMAP_8_BY_13, GLUT_BITMAP_TIMES_ROMAN_10, GLUT_BITMAP_TIMES_ROMAN_24, GLUT_BITMAP_HELVETICA_10, GLUT_BITMAP_HELVETICA_12 или GLUT_BITMAP_HELVETICA_18) и другие - являются символом. 

Символ будет нарисован в текущей позиции растра. Чтобы установить желаемую позицию растра, мы вызываем функцию `glRasterPos`. `glRasterPos` может обрабатывать различные количества и типы параметров так же, как функция `glVertex`. Указанная координата преобразуется моделью и матрицей проекции, чтобы получить двухмерную координату, в которой будет находиться новая позиция растра. Поскольку мы вошли в режим 2D, координаты X и Y являются фактическими координатами 2D, в которых будет происходить рисование. 

Эти новые функции сделают рисование текста очень простым: 
    
    
     procedure DrawGLScene; cdecl;
     begin
       glClear(GL_COLOR_BUFFER_BIT or GL_DEPTH_BUFFER_BIT);
     
       glLoadIdentity;
       glTranslatef(0, 0, -5);
       glRotatef(GetTotalTime * 10, 0, 0.5, 0.5);
     
       glColor3f(1, 0, 0);
       glutSolidCube(2);
     
       glEnter2D;
     
       glColor3f(0.2, 0.8 + 0.2 * Sin(GetTotalTime * 5), 0);
       glWrite(20, glGetViewportHeight - 20, GLUT_BITMAP_8_BY_13,
         Format('OpenGL Tutorial :: Bitmap Fonts :: FPS - %.2f FPS', [FPS]));
     
       glColor3f(1, 1, 1);
       glWrite(50, glGetViewportHeight - 60, GLUT_BITMAP_9_BY_15, 'GLUT_BITMAP_9_BY_15');
       glWrite(50, glGetViewportHeight - 90, GLUT_BITMAP_8_BY_13, 'GLUT_BITMAP_8_BY_13');
       glWrite(50, glGetViewportHeight - 120, GLUT_BITMAP_TIMES_ROMAN_10, 'GLUT_BITMAP_TIMES_ROMAN_10');
       glWrite(50, glGetViewportHeight - 150, GLUT_BITMAP_TIMES_ROMAN_24, 'GLUT_BITMAP_TIMES_ROMAN_24');
       glWrite(50, glGetViewportHeight - 180, GLUT_BITMAP_HELVETICA_10, 'GLUT_BITMAP_HELVETICA_10');
       glWrite(50, glGetViewportHeight - 210, GLUT_BITMAP_HELVETICA_12, 'GLUT_BITMAP_HELVETICA_12');
       glWrite(50, glGetViewportHeight - 240, GLUT_BITMAP_HELVETICA_18, 'GLUT_BITMAP_HELVETICA_18');
     
       glColor3f(0.5, 0.5, 1);
       glWrite(
         glGetViewportWidth - glutBitmapLength(GLUT_BITMAP_9_BY_15, LazText) - 5,
         10, GLUT_BITMAP_9_BY_15, LazText);
     
       glLeave2D;
     
       glutSwapBuffers;
     
       FrameRendered;
     end;
    

[![BitmapFontsPic1.jpg](https://wiki.freepascal.org/images/b/ba/BitmapFontsPic1.jpg)](</File:BitmapFontsPic1.jpg>)

[](</File:BitmapFontsPic1.jpg> "Enlarge")

Мы рисуем красный куб и поворачиваем его, а также немного текста, чтобы показать, как выглядят различные растровые шрифты. Функция `glutBitmapLength` используется для определения ширины строки, чтобы ее можно было выровнять вправо. Код может быть легко изменен для выравнивая текста по центру. 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Примечание:** Посмотрите, как куб выглядит без света.

Загрузите исходный код, исполняемый файл под Linux или Windows с [Lazarus CCR SourceForge](<http://sourceforge.net/project/showfiles.php?group_id=92177&package_id=199145>). 

## Текстуры

Пришло время использовать текстуры :) 

Этот урок покажет, как рисовать текстурированные полигоны и как смешивать текстуры, используя технику многопроходности. Поскольку в OpenGL нет встроенного механизма загрузки текстур, мы будем использовать внешнюю библиотеку: [Vampyre Imaging Library](<http://imaginglib.sourceforge.net/>). Мы будем использовать только вспомогательные функции OpenGL, но вам может пригодиться эта библиотека для некоторых других целей. 

Давайте начнем ... мы создадим список отображения для рисования текстурированного прямоугольника: 
    
    
     procedure CreateList;
     begin
       glNewList(LIST_OBJECT, GL_COMPILE);
         glBegin(GL_QUADS);
           glTexCoord2f(1, 0);
           glVertex3f( 2, 2, 0);
           glTexCoord2f(0, 0);
           glVertex3f(-2, 2, 0);
           glTexCoord2f(0, 1);
           glVertex3f(-2,-2, 0);
           glTexCoord2f(1, 1);
           glVertex3f( 2,-2, 0);
         glEnd;
       glEndList;
     end;
    

Обратите внимание на функции `glTexCoord`. Они используются, чтобы указать, какая часть текстуры назначена вершине. Координаты, определенные в этих функциях, имеют значения от 0 до 1 (допустимы значения больше 1, но они могут давать разные результаты). 0 - это первый пиксель, а 1 - последний. Таким образом, 0.5 будет прямо в середине текстуры. 

Загрузка текстур чрезвычайно проста с Vampyre Imaging Library: 
    
    
     var
       Tex1, Tex2: GLuint;
     
     procedure InitializeGL;
     begin
       glClearColor(0, 0, 0, 0);
       Tex1 := LoadGLTextureFromFile('ashwood.bmp');
       Tex2 := LoadGLTextureFromFile('Flare.bmp');
       glEnable(GL_TEXTURE_2D);
     end;
    

`LoadGLTextureFromFile` загружает текстуру из файла и возвращает ее ID. Когда текстура загружена, она уже настроена на рендеринг. Последняя строка просто включает 2D текстуры. 

Чтобы нарисовать текстурированный многоугольник, вы должны связать текстуру и установить координаты текстуры (координаты текстуры задаются в списке отображения в этом уроке): 
    
    
       ...
       glLoadIdentity;
       glTranslatef(-5, 0, -15);
       glBindTexture(GL_TEXTURE_2D, Tex1);
       glCallList(LIST_OBJECT);
       ...
    

Функция `glBindTexture` используется для выбора текстуры. Когда вы рисуете полигоны, на них будет выделена текстура. Это так просто :) 

Итак, использовать одну текстуру легко ... но как смешать две текстуры? Обычно вы рисуете полигон один раз с одной текстурой, настраиваете параметры смешивания и еще раз рисуете полигон с другой текстурой. Таким способом вы можете смешивать текстуры. Давайте посмотрим, как выглядит код для этого: 
    
    
       ...
       glLoadIdentity;
       glTranslatef(5, 0, -15);
       glBindTexture(GL_TEXTURE_2D, Tex1);
       glCallList(LIST_OBJECT);
     
       glEnable(GL_BLEND);
       glBlendFunc(GL_ZERO, GL_SRC_COLOR);
       glLoadIdentity;
       glTranslatef(5, 0, -15);
       glBindTexture(GL_TEXTURE_2D, Tex2);
       glCallList(LIST_OBJECT);
       glDisable(GL_BLEND);
     ...
    

Как видите, полигон рисуется впервые, как мы уже знаем. Перед вторым рисованием мы включаем смешивание, вызывая `glEnable(GL_BLEND)`. Смешивание означает, что конечный цвет пикселя рассчитывается следующим образом: 
    
    
     DrawingColor * SRCBLEND + BackgroundColor * DESTBLEND
    

`SRCBLEND` и `DESTBLEND` определяются с помощью функции `glBlendFunc`. В этом уроке мы устанавливаем для `SRCBLEND` значение `GL_ZERO` (ноль) и для `DESTBLENT` значение `GL_SRC_COLOR (DrawingColor)`, а затем окончательный цвет: 
    
    
     DrawingColor * 0 + BackgroundColor * DrawingColor
    

[![TexturesPic1.jpg](https://wiki.freepascal.org/images/d/d4/TexturesPic1.jpg)](</File:TexturesPic1.jpg>)

[](</File:TexturesPic1.jpg> "Enlarge")

Это означает, что фон будет темнее, когда вы рисуете темными цветами ... когда вы рисуете белым цветом, цвет фона не изменится. Результат будет выглядеть следующим образом. 

В следующий раз мы будем использовать расширения, чтобы показать, как использовать однопроходное мультитекстурирование. 

Загрузите исходный код, исполняемый файл для linux или windows с [Lazarus CCR SourceForge](<http://sourceforge.net/project/showfiles.php?group_id=92177&package_id=199145>). 

## Мультитекстурирование (расширения)

Когда вы уже знаете многопроходное мультитекстурирование, однопроходный процесс очень прост. Текстурирование разделено на этапы. Первый этап настраивает и рисует первую текстуру, второй этап рисует еще одну и так далее. Все, что вам нужно сделать, это настроить этапы текстуры и визуализации объекта. 

Давайте посмотрим, как выглядит код: 
    
    
     procedure InitializeGL;
     begin
       Load_GL_ARB_multitexture;
       glClearColor(0, 0, 0, 0);
       Tex1 := LoadGLTextureFromFile('Lazarus.bmp');
       Tex2 := LoadGLTextureFromFile('Mask.bmp');
       glActiveTextureARB(GL_TEXTURE0_ARB);
       glEnable(GL_TEXTURE_2D);
       glBindTexture(GL_TEXTURE_2D, Tex1);
       glActiveTextureARB(GL_TEXTURE1_ARB);
       glEnable(GL_TEXTURE_2D);
       glBindTexture(GL_TEXTURE_2D, Tex2);
     end;
    

Для начала нам нужно загрузить расширение OpenGL, которое позволит нам использовать мультитекстурные функции. `Load_GL_ARB_multitexture` попытается загрузить эти расширения и вернет TRUE, если операция прошла успешно. 

Чтобы выбрать стадию текстуры, над которой вы хотите работать, используйте функцию `glActiveTextureARB`. Она принимает только один параметр, который определяет, какая стадия вам нужна. После этого все текстурные функции (включение, отключение, связывание, создание ...) будут влиять на этот этап. 

Поскольку мы настраиваем все в функции инициализации, все, что нам нужно сделать, это нарисовать объект: 
    
    
     procedure DrawGLScene; cdecl;
     begin
       glClear(GL_COLOR_BUFFER_BIT or GL_DEPTH_BUFFER_BIT);
     
       glLoadIdentity;
       glTranslatef(0, 0, -5);
     
       glBegin(GL_QUADS);
         glMultiTexCoord2fARB(GL_TEXTURE0_ARB, 1, 0);
         glMultiTexCoord2fARB(GL_TEXTURE1_ARB, 1, 0);
         glVertex3f(2.516, 2, 0);
         glMultiTexCoord2fARB(GL_TEXTURE0_ARB, 0, 0);
         glMultiTexCoord2fARB(GL_TEXTURE1_ARB, 0, 0);
         glVertex3f(-2.516, 2, 0);
         glMultiTexCoord2fARB(GL_TEXTURE0_ARB, 0, 1);
         glMultiTexCoord2fARB(GL_TEXTURE1_ARB, 0, 1);
         glVertex3f(-2.516,-2, 0);
         glMultiTexCoord2fARB(GL_TEXTURE0_ARB, 1, 1);
         glMultiTexCoord2fARB(GL_TEXTURE1_ARB, 1, 1);
         glVertex3f(2.516,-2, 0);
       glEnd;
     
       glutSwapBuffers;
     end;
    

[![MultitexturePic1.jpg](https://wiki.freepascal.org/images/5/52/MultitexturePic1.jpg)](</File:MultitexturePic1.jpg>)

[](</File:MultitexturePic1.jpg> "Enlarge")

Как видите, разница только в определении текстурных координат. Теперь мы используем функцию `glMultiTexCoord2fARB`, которая принимает стадию текстуры и координаты текстуры. Все остальное без изменений. 

Сегодня почти все графические карты поддерживают как минимум 2 стадии текстур. Использование однопроходного мультитекстурирования выполняется быстрее, чем многопроходная версия, поскольку объекты рисуются только один раз за раз. Если оборудование поддерживает однопроходное мультитекстурирование (`Load_GL_ARB_multitexture` возвращает TRUE), используйте его. 

Загрузите исходный код, исполняемый файл под linux или windows с [Lazarus CCR SourceForge](<http://sourceforge.net/project/showfiles.php?group_id=92177&package_id=199145>). 

## Рендеринг в текстуру

Этот будет кратко. OpenGL может захватывать текущую сцену в текстуру, чтобы вы могли использовать ее для текстурирования других объектов (экран телевизора, зеркало или что-то еще). Итак, просто визуализируйте сцену в текстуру и примените ее к вращающейся плоскости. 

Сначала мы должны создать пустую текстуру, которую мы будем использовать для захвата сцены: 
    
    
     procedure SetupRenderTexture;
     var
       Data: Pointer;
     begin
       GetMem(Data, 256*256*3);
       glGenTextures(1, @RenderTexture);
       glBindTexture(GL_TEXTURE_2D, RenderTexture);
       glTexImage2D(GL_TEXTURE_2D, 0, 3, 256, 256, 0, GL_RGB, GL_UNSIGNED_BYTE, Data);
       glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_MAG_FILTER, GL_LINEAR);
       glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_MIN_FILTER, GL_LINEAR);
       FreeMem(Data);
     end;
    

Создается буфер для изображения размером 256 * 256 RGB, который используется для настройки 2D-текстуры. 

Основная часть - в функции рисования: 
    
    
     procedure DrawGLScene; cdecl;
     var
       TotalTime: Single;
     begin
       glClearColor(0, 0, 0, 0);
       glClear(GL_COLOR_BUFFER_BIT or GL_DEPTH_BUFFER_BIT);
       glEnable(GL_LIGHTING);
       glDisable(GL_TEXTURE_2D);
       glViewport(0, 0, 256, 256);
     
       TotalTime := GetTotalTime;
     
       glLoadIdentity;
       glTranslatef(0, 0, -5);
       glRotatef(50 * TotalTime, 1, 0, 0);
       glRotatef(100 * TotalTime, 0, 1, 0);
       glRotatef(50 * TotalTime, 0, 0, 1);
     
       glColor3f(1, 1, 1);
       glutSolidCube(2);
     
       glCopyTexImage2D(GL_TEXTURE_2D, 0, GL_RGB, 0, 0, 256, 256, 0);
     
       glClearColor(0.18, 0.20, 0.66, 0);
       glClear(GL_COLOR_BUFFER_BIT or GL_DEPTH_BUFFER_BIT);
       glDisable(GL_LIGHTING);
       glEnable(GL_TEXTURE_2D);
       glViewport(0, 0, AppWidth, AppHeight);
     
       glLoadIdentity;
       glTranslatef(0, 0, -7);
       glRotatef(20 * TotalTime, 1, 0, 0);
       glRotatef(50 * TotalTime, 0, 1, 0);
     
       glBegin(GL_QUADS);
         glTexCoord2f(1, 0);
         glVertex3f(2, 2, 0);
         glTexCoord2f(0, 0);
         glVertex3f(-2, 2, 0);
         glTexCoord2f(0, 1);
         glVertex3f(-2,-2, 0);
         glTexCoord2f(1, 1);
         glVertex3f(2,-2, 0);
       glEnd;
     
       glutSwapBuffers;
     end;
    

[![RenderToTexturePic1.jpg](https://wiki.freepascal.org/images/6/6e/RenderToTexturePic1.jpg)](</File:RenderToTexturePic1.jpg>)

[](</File:RenderToTexturePic1.jpg> "Enlarge")

Во-первых, все настроено для сцены, которая будет захвачена. Окно просмотра уменьшено до 256*256, поэтому оно будет вписываться в текстуру, и сцена будет прорисована. `glCopyTexImage2D` используется для захвата сцены в текущую выбранную текстуру. 

Когда у нас есть сцена, захваченная в текстуру, все может быть очищено снова, область просмотра может быть возвращена к исходному размеру, а финальная сцена рисуется с использованием предыдущей сцены в качестве текстуры. 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Примечание:** Захваченную текстуру можно сохранить с помощью функции `SaveGLTextureToFile` из [Vampyre Imaging Library](<http://imaginglib.sourceforge.net/>).

Загрузите исходный код, исполняемый файл для linux или windows с [Lazarus CCR SourceForge](<http://sourceforge.net/project/showfiles.php?group_id=92177&package_id=199145>). 

## Массив вершин

OpenGL способен отображать примитивы, используя данные, которые хранятся в буферах, вызванных вызовом `glVertex`. Буферы могут использоваться для определения координат вершин и текстур, а также цветов (индекс и RGBA), нормалей и флагов ребер. 

В этом уроке будут использоваться только вершинные и цветовые буферы, и мы покажем неиндексированное и индексированное рисование. Неиндексированный режим рисует буферы в виде потоков. Индексированный режим будет рисовать элементы буфера в порядке, определенном в индексе буфера. Но хватит говорить ... давайте начнем кодить. 

Для начала давайте определим некоторые типы и константы: 
    
    
     type
       TVertex3f = record
         X, Y, Z: Single;
       end;
     
       TColor3f = record
        R, G, B: Single;
       end;
     
       VertexBuffer: array [0..5] of TVertex3f = (
         (X : 1; Y : 1; Z : 0),
         (X : -1; Y : 1; Z : 0),
         (X : -1; Y : -1; Z : 0),
         (X : 1; Y : 1; Z : 0),
         (X : -1; Y : -1; Z : 0),
         (X : 1; Y : -1; Z : 0)
       );
       ColorBuffer: array [0..5] of TColor3f = (
         (R : 1; G : 0; B : 1),
         (R : 0; G : 0; B : 1),
         (R : 0; G : 1; B : 0),
         (R : 1; G : 0; B : 1),
         (R : 0; G : 1; B : 0),
         (R : 1; G : 1; B : 0)
       );
    

У нас есть два буфера. Один для координат вершин и один для цветов вершин. Эти 6 вершин определяют 2х треугольников, которые образуют прямоугольник. 

Рисовать примитивы с помощью буферов легко: 
    
    
       glEnableClientState(GL_VERTEX_ARRAY);
       glEnableClientState(GL_COLOR_ARRAY);
       glVertexPointer(3, GL_FLOAT, 0, @VertexBuffer[0]);
       glColorPointer(3, GL_FLOAT, 0, @ColorBuffer[0]);
     
       glDrawArrays(GL_TRIANGLES, 0, Length(VertexBuffer));
     
       glDisableClientState(GL_VERTEX_ARRAY);
       glDisableClientState(GL_COLOR_ARRAY);
    

Сначала мы включаем буферы, которые мы хотим использовать, используя функцию `glEnableClientState`. Затем мы можем выбрать буферы, которые мы хотим использовать. Каждый тип буфера имеет собственную функцию для выбора (`glColorPointer`, `glEdgeFlagPointer`, `glIndexPointer`, `glNormalPointer`, `glTexCoordPointer`, `glVertexPointer`). 

Первый параметр в этих функциях определяет, сколько чисел содержит каждый элемент. Например, давайте возьмем буфер вершин. Если этот параметр равен 2, то OpenGL ожидает, что каждый элемент в буфере содержит координаты x и y. Если этот параметр равен, например, 4, то каждый элемент должен содержать координаты x, y, z и w. Следующий параметр определяет, какой тип элемента данных содержит (GL_BYTE, GL_UNSIGNED_BYTE, GL_SHORT, GL_UNSIGNED_SHORT, GL_INT, GL_UNSIGNED_INT, GL_FLOAT или GL_DOUBLE). Далее определяется количество байтов между каждым элементом. Таким образом, вы можете иметь буфер, который содержит координаты вершины и некоторые пользовательские данные. 

Для произвольного типа данных этот параметр может быть рассчитан следующим образом: 
    
    
     type
       TBufferData = record
         DataBefore: TDataBefore;
         Vertex: TVertex;
         DataAfter: TDataAfter;
       end;
    
    
    
    Байты между элементами = SizeOf(TDataBefore) + SizeOf(TDataAfter)
    

Последний параметр - указатель на начало буфера. 

Когда буферы выбраны, мы можем нарисовать их, используя функции `glDrawArrays`. Все включенные буферы используются для рисования примитивов. Тип создаваемых полигонов определяется в первом параметре (так же, как в функции `glBegin`). Следующие два определяют подмножество буфера, который используется для рисования (start и count). 

Когда буферы не нужны, вы можете отключить их. 

Чтобы продемонстрировать индексированный режим, я создал простой класс сетки, который может загружать данные вершин, цветов и индексов из внешних файлов: 
    
    
     type
       TMesh = class
       private
         FVertices: array of TVertex3f;
         FColors: array of TColor3f;
         FIndices: array of Integer;
         procedure FreeBuffers;
       public
         constructor Create;
         destructor Destroy; override;
         procedure LoadMesh(FileName: String);
         procedure DrawMesh;
       end;
    

Поле FVertices будет содержать данные о вершинах, поле FColors - данные о цвете, и поле FIndices - данные об индексах при загрузке внешнего файла. 

Сначала мы напишем некоторый код, который занимается созданием и уничтожением класса: 
    
    
     procedure TMesh.FreeBuffers;
     begin
       FVertices := nil;
       FColors := nil;
       FIndices := nil;
     end;
     
     constructor TMesh.Create;
     begin
       FreeBuffers;
     end;
     
     destructor TMesh.Destroy;
     begin
       FreeBuffers;
       inherited Destroy;
     end;
    

Файл, который будет содержать данные сетки, является простым текстовым файлом. Первая строка будет содержать количество вершин и индексов, разделенных пробелом. После этого ряда появятся строки для каждой вершины и цвета. X, Y, Z, R, G и B все разделены пробелом. В конце будут строки для индексов ... каждый номер индекса будет записан в своей собственной строке ... поэтому для одного треугольника файл данных будет выглядеть примерно так: 
    
    
     3 3
     -1 -1 0 1 1 1
     1 -1 0 1 1 1
     0 1 0 1 1 1
     0
     1
     2
    

Это означает, что в файле определены 3 вершины и 3 индекса. Первая вершина имеет значения -1, -1, 0 и цвет, имеющий значения 1, 1, 1 и т.д. Индексы определяют тот порядок, в котором рисуются вершины (в этом случае вершины рисуются в том же порядке, в котором они определены). 

Код для загрузки этих данных будет выглядеть так: 
    
    
     procedure TMesh.LoadMesh(FileName: String);
     var
       MeshFile: TextFile;
       VertexCount, IndexCount: Integer;
       iV, iI: Integer;
     begin
       FreeBuffers;
     
       AssignFile(MeshFile, FileName);
       Reset(MeshFile);
     
       ReadLn(MeshFile, VertexCount, IndexCount);
     
       SetLength(FVertices, VertexCount);
       SetLength(FColors, VertexCount);
       SetLength(FIndices, IndexCount);
     
       for iV := 0 to VertexCount - 1 do
         ReadLn(MeshFile,
           FVertices[iV].X, FVertices[iV].Y, FVertices[iV].Z,
           FColors[iV].R, FColors[iV].G, FColors[iV].B);
     
       for iI := 0 to IndexCount - 1 do
         ReadLn(MeshFile, FIndices[iI]);
     
       CloseFile(MeshFile);
     end;
    

После загрузки данных у нас есть все для рисования: 
    
    
     procedure TMesh.DrawMesh;
     begin
       glEnableClientState(GL_VERTEX_ARRAY);
       glEnableClientState(GL_COLOR_ARRAY);
       glVertexPointer(3, GL_FLOAT, 0, @FVertices[0]);
       glColorPointer(3, GL_FLOAT, 0, @FColors[0]);
     
       glDrawElements(GL_TRIANGLES, Length(FIndices), GL_UNSIGNED_INT, @FIndices[0]);
     
       glDisableClientState(GL_VERTEX_ARRAY);
       glDisableClientState(GL_COLOR_ARRAY);
     end;
    

Как видите, почти все то же самое, что и для неиндексированного рисования, за исключением функции, которая фактически рисует многоугольники. В этом случае мы используем функцию `glDrawElements`. Для этого мы указываем, какой тип полигонов мы хотим нарисовать, сколько индексов находится в буфере индекса, тип данных в буфере индекса и указатель на начало буфера индекса. 

[![VertexArrayPic1.jpg](https://wiki.freepascal.org/images/7/77/VertexArrayPic1.jpg)](</File:VertexArrayPic1.jpg>)

[](</File:VertexArrayPic1.jpg> "Enlarge")

Полный исходный код поставляется с файлом данных сетки, который этот класс может использовать для создания прямоугольника, идентичного прямоугольнику, нарисованному в неиндексированном режиме. Файл данных сетки выглядит так: 
    
    
     4 6
     1 1 0 1 0 1
     -1 1 0 0 0 1
     -1 -1 0 0 1 0
     1 -1 0 1 1 0
     0
     1
     2
     0
     2
     3
    

Как видите, есть данные только для 4 вершин и 6 индексов. Итак, первый треугольник определяется вершинами 0, 1 и 2, а второй - вершинами 0, 2 и 3. Используя индексированный режим, нам не нужно дублировать вершины. 

Загрузите исходный код, исполняемый файл для linux или windows с [Lazarus CCR SourceForge](<http://sourceforge.net/project/showfiles.php?group_id=92177&package_id=199145>). 

# Современный OpenGL с использованием MacOS

Отличная особенность Lazarus - «написав однажды, компилируй где угодно», где код должен работать как на MacOS, так и на Linux или Windows. Однако для поддержки всех трех из этих систем вам необходимо решить, поддерживать ли только устаревший OpenGL или использовать профиль «OpenGL Core». 

Для пользователей Linux и Windows последние версии OpenGL представляют собой расширенный набор старых версий. Поэтому пользователь Linux может смешивать и сопоставлять старый код OpenGL с современными шейдерами. Но только **не** для случая с пользователями Macintosh MacOS (OSX). 

MacOS обеспечивает две формы поддержки: унаследованный режим поддерживает все функции OpenGL до 2.1 (и GLSL 1.2). Кроме того, пользователь может выбрать современную **core** -версию OpenGL (тогда как большинство пользователей Linux и Windows имеют доступ к «совместимым» версиям OpenGL). 

[**Сore** -версия OpenGL](<https://prideout.net/modern-opengl-prezo/>) удаляет многие устаревшие унаследованные функции. Это означает, что все приведенные выше учебники будут компилироваться только в устаревшем режиме MacOS. Режим ядра не имеет конвейера с фиксированными функциями, и поэтому разработчик должен написать свои собственные шейдеры. Кроме того, режим Core удаляет некоторые базовые примитивы, такие как [`GL_QUADS`](<https://www.khronos.org/opengl/wiki/Legacy_OpenGL>), поэтому квадрат необходимо заменить двумя треугольниками (это действительно имеет смысл: все вершины треугольника копланарны (расположены в одной плоскости), но это не обязательно имеет место для четырехугольника, точно так же, как каждая ножка треноги всегда будет касаться земли, в то время как 4х-ногая табуретка может колебаться, если одна нога короче других). 

Преимущество базовой модели состоит в том, что она проста и, как правило, легко адаптируется к мобильным устройствам (которые используют аналогичную встроенную форму OpenGL). См. [веб-сайт Apple](<Https://developer.apple.com/opengl/capabilities/>), чтобы увидеть OpenGL, поддерживаемый вашей операционной системой. Еще одна проблема заключается в том, что основные режимы OpenGL поддерживаются только в наборе виджетов Cocoa, поэтому вы не можете использовать набор виджетов Carbon (который в настоящее время используется по умолчанию в Lazarus). 

Три демонстрационных проекта доступны на [Github OpenGLCoreTutorials](<https://github.com/neurolabusc/OpenGLCoreTutorials>) и будут компилироваться с использованием Lazarus 1.6.2 или более поздней версии для Linux, Windows или MacOS. 

# See also

  * [Создание привязок для библиотек C](<Creating_bindings_for_C_libraries.md> "Creating bindings for C libraries/ru")
  * [OpenGL](<../en/OpenGL.md> "OpenGL")



# Внешние ссылки

  * [OpenGL Light Tutorial](<http://www.falloutsoftware.com/tutorials/gl/gl8.htm>) \- Учебное пособие по OpenGL, в котором описывается, как создавать источники света и управлять ими, а также справляться с отражениями поверхности полигона с помощью нормалей.
  * [opengles3-book](<https://github.com/danginsburg/opengles3-book>)
  * [opengl-tutorials](<https://github.com/zilongshanren/opengl-tutorials>)

---

_Source: [https://wiki.freepascal.org/OpenGL_Tutorial/ru](https://web.archive.org/web/20241207124941/https://wiki.freepascal.org/OpenGL_Tutorial/ru)_
