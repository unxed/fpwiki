# Frames

│ **[English (en)](<../en/Frames.md>)** │  **русский (ru)** │

**[Фреймы](<TFrame.md> "TFrame/ru")** являются именованными контейнерами для компонентов и очень похожи на [ Формы](<TForm.md> "TForm/ru"). Их уникальная способность заключается в том, что они могут быть встроены в формы или другие фреймы в дизайнере. В виде форм они хранятся в двух файлах: код хранится в файле .pas, а дизайн - в файле .lfm. 

## Contents

  * 1 Как создать фрейм?
  * 2 Как поместить фрейм на форму или в другой фрейм?
  * 3 Для чего их можно использовать?
  * 4 Инициализация частных переменных
  * 5 Пример динамического создания
  * 6 См.также



## Как создать фрейм?

Нажмите пункт меню File->New... и откройте диалоговое окно, выбрав пункт "Frame". 

[![Frame new.PNG](https://wiki.freepascal.org/images/6/6d/Frame_new.PNG)](</File:Frame_new.PNG>)

## Как поместить фрейм на форму или в другой фрейм?

Вкладка [Standard](<Standard_tab.md> "Standard tab/ru") в [Палитре компонентов](<Component_Palette.md> "Component Palette/ru") имеет специальный компонентный элемент «[TFrame](<TFrame.md> "TFrame/ru")». Когда вы перетаскиваете его на форму или фрейм, IDE предлагает вам выбрать один из фреймов проекта. Вы должны сначала сохранить проект и новый фрейм, потому что только предварительно сохраненные фреймы могут появиться в списке выбора. Фреймы в пакетах пока не реализованы. Но вы можете создавать фреймы в коде даже в пакетах. Вы не можете создавать циклические фреймы - IDE запретит это, т.е. вы не можете поместить FrameA в FrameA, и вы не можете поместить любой фрейм, который содержит FrameA, в FrameA. 

[![Frame add.PNG](https://wiki.freepascal.org/images/3/35/Frame_add.PNG)](</File:Frame_add.PNG>)

## Для чего их можно использовать?

Они необходимы, когда у вас есть группа компонентов, которые вы хотите повторно использовать в нескольких формах. Группа должна иметь одинаковый набор элементов управления и логику между ними в разных окнах (формах) вашего приложения. Вы можете сгруппировать повторяющиеся элементы управления и логику в один фрейм и использовать этот фрейм в разных местах. Поэтому вам не нужно повторять работу по разметке элементов управления и написанию их логики. 

Например, у вас есть два списка и кнопки для перемещения элементов между ними. Таким образом, вы можете создать фрейм с двумя списками и необходимыми кнопками, написать логику для перемещения элементов, а затем использовать свой фрейм во всех формах, где это необходимо. Более того, если вы обнаружите ошибку в коде фрейма, вы можете исправить ее один раз в коде фрейма вместо того, чтобы исправлять ее n раз среди всех форм. 

Фрейм в дизайнере: 

[![Frame example.PNG](https://wiki.freepascal.org/images/1/13/Frame_example.PNG)](</File:Frame_example.PNG>)

  
Фрейм, размещенный на форме: 

[![Frame embedded.PNG](https://wiki.freepascal.org/images/1/17/Frame_embedded.PNG)](</File:Frame_embedded.PNG>)

Тот же фрейм помещен в другую форму: 

[![Frame embedded2.PNG](https://wiki.freepascal.org/images/8/88/Frame_embedded2.PNG)](</File:Frame_embedded2.PNG>)

## Инициализация частных переменных

TFrame не имеет событий `OnCreate` или `OnDestroy`, в которых могут быть инициализированы и освобождены частные переменные. Для этого необходимо [переопределить](<../en/Override.md> "Override") [constructor](<Constructor.md> "Constructor/ru") и [destructor](<../en/Destructor.md> "Destructor") по умолчанию. 
    
    
    TFrame1 = class(TFrame)
      private
        MyObj: TObject;
      public
        constructor Create(TheOwner: TComponent); override;
        destructor Destroy; override;
      end; 
    
    constructor TFrame1.Create(TheOwner: TComponent);
    begin
      inherited Create(TheOwner);
      MyObj := TObject.Create;
    end;
    
    destructor TFrame1.Destroy;
    begin
      MyObj.Free
      inherited Destroy;
    end;
    

## Пример динамического создания

Еще проще. Поскольку компонент TFrame из палитры компонентов не требуется. И вы можете создать приложение типа изменения страницы (например, приложение для смартфона) с сохранением ресурса. 
    
    
    unit Unit1;
    
    {$mode objfpc}{$H+}
    
    interface
    
    uses
      Classes, SysUtils, FileUtil, Forms, Controls, Graphics, Dialogs, StdCtrls;
    
    type
    
      { TForm1 }
    
      TForm1 = class(TForm)
        Button1: TButton;
        GroupBox1: TGroupBox;
        procedure Button1Click(Sender: TObject);
        procedure FormCreate(Sender: TObject);
      private
        { private declarations }
        Frame: TFrame;
      public
        { public declarations }
      end;
    
    var
      Form1: TForm1;
    
    implementation
    uses
      Unit2{TFrame1}, Unit3{TFrame2}, Unit4{TFrame3};
    
    {$R *.lfm}
    
    { TForm1 }
    
    procedure TForm1.FormCreate(Sender: TObject);
    begin
      Frame := TFrame1.Create(GroupBox1);
      Frame.Parent := GroupBox1;
    end;
    
    procedure TForm1.Button1Click(Sender: TObject);
    begin
      if not Assigned(Frame) then
      begin
        Frame := TFrame1.Create(GroupBox1);
        Frame.Parent := GroupBox1;
      end else if Frame is TFrame1 then begin
        Frame.Free;
        Frame := TFrame2.Create(GroupBox1);
        Frame.Parent := GroupBox1;
      end else if Frame is TFrame2 then begin
        Frame.Free;
        Frame := TFrame3.Create(GroupBox1);
        Frame.Parent := GroupBox1;
      end else begin
        FreeAndNil(Frame);
      end;
    end;
    
    end.
    

## См.также

  * [TFrame](<TFrame.md> "TFrame/ru")

---

_Source: [https://wiki.freepascal.org/Frames/ru](https://web.archive.org/web/20230701000000/https://wiki.freepascal.org/Frames/ru)_
