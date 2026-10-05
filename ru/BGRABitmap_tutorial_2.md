# BGRABitmap tutorial 2

│ **[English (en)](<../en/BGRABitmap_tutorial_2.md>)** │  **русский (ru)** │

  
****

[ **Home**](<../en/BGRABitmap_tutorial.md> "BGRABitmap tutorial") | [ **Tutorial 1**](<../en/BGRABitmap_tutorial_1.md> "BGRABitmap tutorial 1") | [ **Tutorial 2**](<../en/BGRABitmap_tutorial_2.md> "BGRABitmap tutorial 2") | [ **Tutorial 3**](<../en/BGRABitmap_tutorial_3.md> "BGRABitmap tutorial 3") | [ **Tutorial 4**](<../en/BGRABitmap_tutorial_4.md> "BGRABitmap tutorial 4") | [ **Tutorial 5**](<../en/BGRABitmap_tutorial_5.md> "BGRABitmap tutorial 5") | [ **Tutorial 6**](<../en/BGRABitmap_tutorial_6.md> "BGRABitmap tutorial 6") | [ **Tutorial 7**](<../en/BGRABitmap_tutorial_7.md> "BGRABitmap tutorial 7") | [ **Tutorial 8**](<../en/BGRABitmap_tutorial_8.md> "BGRABitmap tutorial 8") | [ **Tutorial 9**](<../en/BGRABitmap_tutorial_9.md> "BGRABitmap tutorial 9") | [ **Tutorial 10**](<../en/BGRABitmap_tutorial_10.md> "BGRABitmap tutorial 10") | [ **Tutorial 11**](<../en/BGRABitmap_tutorial_11.md> "BGRABitmap tutorial 11") | [ **Tutorial 12**](<../en/BGRABitmap_tutorial_12.md> "BGRABitmap tutorial 12") | [ **Tutorial 13**](<../en/BGRABitmap_tutorial_13.md> "BGRABitmap tutorial 13") | [ **Tutorial 14**](<../en/BGRABitmap_tutorial_14.md> "BGRABitmap tutorial 14") | [ **Tutorial 15**](<../en/BGRABitmap_tutorial_15.md> "BGRABitmap tutorial 15") | [ **Tutorial 16**](<../en/BGRABitmap_tutorial_16.md> "BGRABitmap tutorial 16") | Edit

Из этого урока Вы узнаете, как загрузить изображение и нарисовать его на форме. 

## Contents

  * 1 Создаём новый проект
  * 2 Загружаем растровое изображение (bitmap)
  * 3 Нарисуем растровое изображение (bitmap)
  * 4 Весь код
  * 5 Запустим программу
  * 6 Сделаем выравнивание по центру изображения
  * 7 Растянем изображение



### Создаём новый проект

Создайте новый проект и добавьте ссылку на модуль [BGRABitmap](<../en/BGRABitmap.md> "BGRABitmap"), так же, как в [первом уроке](<BGRABitmap_tutorial_1.md> "BGRABitmap tutorial 1/ru"). 

### Загружаем растровое изображение (bitmap)

Скопируйте изображение в каталог вашего проекта. Предположим, имя файла _image.png_. 

Добавьте переменную "image" в "private" область основной формы для хранения изображения: 
    
    
      TForm1 = class(TForm)
      private
        { private declarations }
        image: TBGRABitmap;
      public
        { public declarations }
      end;
    

Загрузите изображение при создании формы. Для этого дважды щелкните форму, в редакторе кода должна появиться процедура. И добавьте [инструкцию загрузки изображения из файла](<../en/TBGRACustomBitmap_and_IBGRAScanner.md> "TBGRACustomBitmap and IBGRAScanner") в переменную "image": 
    
    
    procedure TForm1.FormCreate(Sender: TObject);
    begin
      image := TBGRABitmap.Create('image.png');
    end;
    

### Нарисуем растровое изображение (bitmap)

Добавьте обработчик OnPaint. Для этого выберите основную форму, затем перейдите в инспектор объектов на вкладке событий и дважды щелкните строку OnPaint. Затем добавьте код для рисования: 
    
    
    procedure TForm1.FormPaint(Sender: TObject);
    begin
      image.Draw(Canvas,0,0,True);
    end;
    

Обратите внимание, что последний параметр имеет значение True, что означает непрозрачность. Если вы хотите принять во внимание прозрачные пиксели, закодированные в альфа-канале, вы должны использовать вместо этого False. Однако использование прозрачного рисунка на стандартном холсте может быть медленным, поэтому, если в этом нет необходимости, используйте только непрозрачный рисунок. 

### Весь код
    
    
    unit UMain;
    
    {$mode objfpc}{$H+}
    
    interface
    
    uses
      Classes, SysUtils, FileUtil, LResources, Forms, Controls, Graphics, Dialogs,
      BGRABitmap, BGRABitmapTypes;
    
    type
      { TForm1 }
    
      TForm1 = class(TForm)
        procedure FormCreate(Sender: TObject);
        procedure FormPaint(Sender: TObject);
      private
        { private declarations }
        image: TBGRABitmap;
      public
        { public declarations }
      end; 
    
    var
      Form1: TForm1; 
    
    implementation
    
    { TForm1 }
    
    procedure TForm1.FormCreate(Sender: TObject);
    begin
      image := TBGRABitmap.Create('image.png');
    end;
    
    procedure TForm1.FormPaint(Sender: TObject);
    begin
      image.Draw(Canvas,0,0,True);
    end;
    
    initialization
      {$I UMain.lrs}
    
    end.
    

### Запустим программу

Вы должны увидеть форму с изображением в верхнем левом углу. 

[![BGRATutorial2.png](https://wiki.freepascal.org/images/d/db/BGRATutorial2.png)](</File:BGRATutorial2.png>)

### Сделаем выравнивание по центру изображения

Вы можете центрировать изображение в форме. Для этого измените процедуру FormPaint: 
    
    
    procedure TForm1.FormPaint(Sender: TObject);
    var ImagePos: TPoint;
    begin
      ImagePos := Point( (ClientWidth - Image.Width) div 2,
                         (ClientHeight - Image.Height) div 2 );
    
      // test for negative position
      if ImagePos.X < 0 then ImagePos.X := 0;
      if ImagePos.Y < 0 then ImagePos.Y := 0;
    
      image.Draw(Canvas,ImagePos.X,ImagePos.Y,True);
    end;
    

Чтобы вычислить позицию, нам нужно вычислить пространство между изображением и левой границей (координата X) и пространство между изображением и верхней границей (координата Y). Выражение ClientWidth - Image.Width возвращает доступное горизонтальное пространство, и мы делим его на 2, чтобы получить левое поле. 

Результат может быть отрицательным, если изображение больше ширины клиента. В этом случае отступ от края устанавливается на ноль. 

Вы можете запустить программу и посмотреть, работает ли она. Обратите внимание, что произойдет, если мы удалим проверку на отрицательную позицию изображения относительно края. 

### Растянем изображение

Чтобы растянуть изображение, нам нужно создать временное растянутое изображение: 
    
    
    procedure TForm1.FormPaint(Sender: TObject);
    var stretched: TBGRABitmap;
    begin
      stretched := image.Resample(ClientWidth, ClientHeight) as TBGRABitmap;
      stretched.Draw(Canvas,0,0,True);
      stretched.Free;
    end;
    

По умолчанию используется точное копирование. Но вместо этого Вы можете использовать простое растяжение (работает быстрее): 
    
    
    stretched := image.Resample(ClientWidth, ClientHeight, rmSimpleStretch) as TBGRABitmap;
    

Вы также можете указать интерполяционный фильтр с помощью свойства [ResampleFilter](<../en/TBGRACustomBitmap_and_IBGRAScanner.md> "TBGRACustomBitmap and IBGRAScanner"): 
    
    
    image.ResampleFilter := rfMitchell;
    stretched := image.Resample(ClientWidth, ClientHeight) as TBGRABitmap;
    

[Первый урок](<BGRABitmap_tutorial_1.md> "BGRABitmap tutorial 1/ru") | [Следующий урок (Рисование с помощью мыши (No. 3))](<../en/BGRABitmap_tutorial_3.md> "BGRABitmap tutorial 3")

---

_Source: [https://wiki.freepascal.org/BGRABitmap_tutorial_2/ru](https://web.archive.org/web/20250418110545/https://wiki.freepascal.org/BGRABitmap_tutorial_2/ru)_
