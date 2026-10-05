# Drag and Drop sample

│ **[English (en)](<../en/Drag_and_Drop_sample.md> "Drag and Drop sample")** │  **русский (ru)** │    
****

Перетаскивание и бросание элементов интерфейса - это обычная операция, которая делает интерфейс удобным для пользователя: пользователь может перетаскивать информацию в элементы управления вместо того, чтобы вводить и т.д. 

В следующем примере объясняются основы перетаскивания. Для получения подробной информации обратитесь к другим статьям вики и справочной документации. 

Обратите внимание, поскольку LCL частично совместим с Delphi VCL, некоторые статьи/примеры о перетаскивании Delphi могут также относиться к LCL. 

## Contents

  * 1 Drag and Drop
  * 2 Примеры
    * 2.1 Перетаскивание между элементами управления
    * 2.2 Перетаскивание внутри элемента управления
  * 3 Подсказки
  * 4 Перетаскивание из других приложений
    * 4.1 Файлы
    * 4.2 Текст и т.д.
      * 4.2.1 Windows
  * 5 См. также



## Drag and Drop

Несмотря на простоту работы с точки зрения пользователя, неопытному разработчику она может доставить немало хлопот. 

Для кода операция перетаскивания всегда состоит как минимум из трех шагов: 

  1. Некоторый элемент управления запускает операцию перетаскивания. Он называется Source (Источником)
  2. Пользователь таскает курсор мыши над другими элементами управления или над самим источником. Теперь перетаскиваемый элемент управления должен решить, может ли он принимать перетаскиваемые данные.
  3. Бросание происходит, если элемент управления соглашается принять перетаскиваемые данные. Принимающий элемент управления называется Sender (Приемник).



Для упрощения перетаскивания в LCL предусмотрен «автоматический» режим. Это не означает, что LCL выполняет все операции перетаскивания за вас, но он будет обрабатывать низкоуровневое управление объектами перетаскивания (которое не рассматривается в этой статье). 

## Примеры

[![DnDTest.PNG](https://wiki.freepascal.org/images/3/30/DnDTest.PNG)](</File:DnDTest.PNG>)

Пример охватывает функцию автоматического перетаскивания между двумя элементами управления (Edit->Treeview), а также внутри одного элемента управления (Treeview->Treeview). 

  * Запустите новое приложение.
  * Поместите компонент TreeView и Edit в форму.
  * Включите автоматический режим перетаскивания (Automatic drag-and-drop) для TreeView и редактирования в инспекторе объектов:



**DragMode: dkAutomatic**

Теперь вы можете запустить приложение и попробовать перетащить что-нибудь. У вас пока не должно ничего работать. Но если вы нажмете левую кнопку мыши в Treeview, вы, вероятно, увидите, что значок курсора изменился, но при отпускании мыши ничего не происходит. 

### Перетаскивание между элементами управления

Сделаем операцию перетаскивания между Edit и TreeView. Там содержимое Edit будет «перетащено» в TreeView и будет создан новый узел дерева. 

Чтобы инициировать перетаскивание, у элементов управления есть специальный метод: `BeginDrag()`

  
Создайте событие OnMouseDown для Edit: 
    
    
    procedure TForm1.Edit1MouseDown(Sender: TObject; Button: TMouseButton; 
      Shift: TShiftState; X, Y: Integer);
    begin
      if Button = mbLeft then   {проверяем, была ли нажата левая кнопка мыши}
        Edit1.BeginDrag(true);  {начинаем операцию перетаскивания}
    end;
    

Если вы запустите приложение прямо сейчас и попытаетесь перетащить его, вы заметите, что Edit запускает операцию, но по-прежнему ничего не происходит, когда вы пытаетесь тащить элемент к TreeView. 

Это потому, что TreeView не принимает данные. Ни один из элементов управления не принимает данные по умолчанию, поэтому вам всегда нужно предоставлять соответствующий обработчик событий. 

Создайте код в событии TreeView.OnDragOver: 
    
    
    procedure TForm1.TreeView1DragOver(Sender, Source: TObject; X, Y: Integer; 
      State: TDragState; var Accept: Boolean);
    begin
      Accept := true;
    end;
    

Конечно, в некоторых случаях TreeView может запретить перетаскивание (если Source или данные не могут быть обработаны), но на данный момент TreeView всегда принимает перетаскивание. 

Запустите приложение и протестируйте. Теперь все должно быть лучше, хотя при бросании элемента все равно ничего не происходит. 

Создайте код в событии TreeView.OnDragDrop: 
    
    
    procedure TForm1.TreeView1DragDrop(Sender, Source: TObject; X, Y: Integer);
    var
      tv     : TTreeView; 
      iNode  : TTreeNode;  
    begin
      tv := TTreeView(Sender);      { Sender - это TreeView, где данные удаляются               }
      iNode := tv.GetNodeAt(x,y);   { х, y - координаты перетаскивания (относительно  Sender)   }   
                                    { поскольку Sender - это TreeView, мы можем оценить         }
                                    { дерево в координатах X, Y                                 } 
                                    
      { TreeView также может быть Source'ом! Так что мы должны убедиться, }                                
      { что этим Source'ом является TEdit, до получения им текста         }    
    
      if Source is TEdit then       
        tv.Items.AddChild(iNode, TEdit(Source).Text); {Теперь мы можем добавить новый узел с текстом из Source }
    end;
    

Запускаем и тестируем. Теперь он должен работать. Перетаскивание текста из Edit в TreeView должно создать новый узел. 

### Перетаскивание внутри элемента управления

Sender и Source могут быть одним и тем же элементом управления! Это ни в коем случае не запрещено. Давайте добавим возможность TextView изменять расположение его узлов. Поскольку TextView находится в автоматическом режиме DragMode, вам не нужно начинать перетаскивание с помощью `DragBegin()`. Он запускается автоматически при перемещении мыши с удержанием левой кнопки. 

Убедитесь, что у вас "Accept:=true;" внутри операции DragOver для источника TreeView. 

Измените обработчик события DragDrop следующим образом: 
    
    
    procedure TForm1.TreeView1DragDrop(Sender, Source: TObject; X, Y: Integer);
    var
      tv     : TTreeView; 
      iNode  : TTreeNode;  
    begin
      tv := TTreeView(Sender);      { Sender - это TreeView, где данные удаляются             }
      iNode := tv.GetNodeAt(x,y);   { х, y - координаты перетаскивания (относительно  Sender) }   
                                    {поскольку Sender - это TreeView, мы можем оценить        }
                                    { дерево в координатах X, Y                               } 
                                    
      { TreeView также может быть Source'ом! Так что мы должны убедиться, }                                
      { что этим Source'ом является TEdit, до получения им текста         }      
      if Source is TEdit then       
        tv.Items.AddChild(iNode, TEdit(Source).Text) {Теперь мы можем добавить новый узел с текстом из Source }
        
      else if Source = Sender then begin         { бросание элемента интерфейса происходит внутри TreeView   }
        if Assigned(tv.Selected) and             { проверяем, был ли выбран какой-либо узел                  }
          (iNode <> tv.Selected) then            { и мы переходим к другому узлу                             }
        begin
          if iNode <> nil then 
            tv.Selected.MoveTo(iNode, naAddChild) { завершаем операцию перетаскивания, переместив выбранный узел }
          else
            tv.Selected.MoveTo(iNode, naAdd); { завершаем операцию перетаскивания, переместившись в корень TreeView }
        end;
      end;
    end;
    

Вот и все. Если вы запустите приложение сейчас, у вас должны работать обе функции. 

  * Добавление нового узла путем перетаскивания текста из Edit в TreeView
  * Перетаскивание узлов внутри древовидной структуры



## Подсказки

  * Можете (не можете) ли вы использовать некоторые глобальные данные, чтобы проверить, что сейчас перетаскивается? Используйте для этого не глобальные переменные, а только поля вашего класса формы. 
    * Помещайте туда данные при запуске перетаскивания
    * Проверяйте данные во время перетаскивания элемента управления и соответствующим образом изменяйте Accept-флаг элемента .
    * Читайте и используйте данные в событии перетаскивания



## Перетаскивание из других приложений

Вы можете перетащить/отпустить 

### Файлы

Файлы можно легко бросать и обрабатывать, реализуя событие FormDropFiles после того, как для формы свойство AllowDropFiles задано как `True`: 
    
    
    procedure TForm1.FormDropFiles(Sender: TObject; const FileNames: array of String);
    var FileName : String;
    begin
      for FileName in FileNames do
      begin
        ShowMessage(FileName);
      end;
    end;
    

* * *

[Прим.перев.](</User:Zoltanleo> "User:Zoltanleo"): Наиболее наглядно пример реализован [здесь](<http://lazplanet.blogspot.com/2013/05/drag-drop-files-lazarus-form.html>). 

* * *

### Текст и т.д.

Вы можете перетащить, например текст из другого приложения (например, блокнота) в элемент управления в вашем приложении. Способ реализации зависит от платформы. 

#### Windows

Этот код позволяет перетаскивать выделенный текст из других приложений в элемент управления редактированием. 
    
    
    unit Unit1;
    
    {$mode objfpc}{$H+}
    
    interface
    
    uses
      Classes, SysUtils, FileUtil, Forms, Controls, Graphics, Dialogs, StdCtrls,
      Windows, ActiveX, ComObj;
    
    type
    
      { TForm1 }
    
      TForm1 = class(TForm, IDropTarget)
        Memo1: TMemo;
        procedure FormCreate(Sender: TObject);
        procedure FormDestroy(Sender: TObject);
      private
        // IDropTarget
        function DragEnter(const dataObj: IDataObject; grfKeyState: DWORD; pt: TPoint; var dwEffect: DWORD): HResult;StdCall;
        function DragOver(grfKeyState: DWORD; pt: TPoint; var dwEffect: DWORD): HResult;StdCall;
        function DragLeave: HResult;StdCall;
        function Drop(const dataObj: IDataObject; grfKeyState: DWORD; pt: TPoint; var dwEffect: DWORD):HResult;StdCall;
        // IUnknown
        // Ignore referance counting
        function _AddRef: Integer; stdcall;
        function _Release: Integer; stdcall;
      public
        { public declarations }
      end;
    
    
    var
      Form1: TForm1;
    
    implementation
    
    {$R *.lfm}
    
    { TForm1 }
    
    procedure TForm1.FormCreate(Sender: TObject);
    begin
      OleInitialize(nil);
      OleCheck(RegisterDragDrop(Handle, Self));
    end;
    
    procedure TForm1.FormDestroy(Sender: TObject);
    begin
      RevokeDragDrop(Handle);
      OleUninitialize;
    end;
    
    function TForm1.DragEnter(const dataObj: IDataObject; grfKeyState: DWORD;
      pt: TPoint; var dwEffect: DWORD): HResult; StdCall;
    begin
      dwEffect := DROPEFFECT_COPY;
      Result := S_OK;
    end;
    
    function TForm1.DragOver(grfKeyState: DWORD; pt: TPoint; var dwEffect: DWORD
      ): HResult; StdCall;
    begin
      dwEffect := DROPEFFECT_COPY;
      Result := S_OK;
    end;
    
    function TForm1.DragLeave: HResult; StdCall;
    begin
      Result := S_OK;
    end;
    
    function TForm1._AddRef: Integer; stdcall;
    begin
       Result := 1;
    end;
    
    function TForm1._Release: Integer; stdcall;
    begin
       Result := 1;
    end;
    
    function TForm1.Drop(const dataObj: IDataObject; grfKeyState: DWORD;
      pt: TPoint; var dwEffect: DWORD): HResult; StdCall;
    var
      aFmtEtc: TFORMATETC;
      aStgMed: TSTGMEDIUM;
      pData: PChar;
    begin
      {Убеждаемся, что рендеринг данных доступен}
      if (dataObj = nil) then
        raise Exception.Create('IDataObject-Указатель недействителен!');
      with aFmtEtc do
      begin
        cfFormat := CF_TEXT;
        ptd := nil;
        dwAspect := DVASPECT_CONTENT;
        lindex := -1;
        tymed := TYMED_HGLOBAL;
      end;
      {Получаем данные}
      OleCheck(dataObj.GetData(aFmtEtc, aStgMed));
      try
        {Заблокируем дескриптор глобальной памяти, чтобы получить указатель на данные}
        pData := GlobalLock(aStgMed.hGlobal);
        { Заменяем текст }
        Memo1.Text := pData;
      finally
        {Завершаем с указателем}
        GlobalUnlock(aStgMed.hGlobal);
        {Освобождаем память}
        ReleaseStgMedium(aStgMed);
      end;
      Result := S_OK;
    end;
    
    
    end.
    

Источник на форуме: [этот тред](<http://forum.lazarus.freepascal.org/index.php/topic,25769.msg156933.html#msg156933>)

## См. также

  * [LCL Drag Drop](<LCL_Drag_Drop.md> "LCL Drag Drop/ru")

---

_Source: [https://wiki.freepascal.org/Drag_and_Drop_sample/ru](https://web.archive.org/web/20231203211224/https://wiki.freepascal.org/Drag_and_Drop_sample/ru)_
