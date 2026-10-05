# VirtualTreeview Example for Lazarus

│ **[English (en)](<../en/VirtualTreeview_Example_for_Lazarus.md> "VirtualTreeview Example for Lazarus")** │  **[español (es)](</VirtualTreeview_Example_for_Lazarus/es> "VirtualTreeview Example for Lazarus/es")** │  **[français (fr)](</VirtualTreeview_Example_for_Lazarus/fr> "VirtualTreeview Example for Lazarus/fr")** │  **[polski (pl)](</VirtualTreeview_Example_for_Lazarus/pl> "VirtualTreeview Example for Lazarus/pl")** │  **русский (ru)** │    
****

  
Вот несколько примеров того, как использовать [VirtualTreeview](<../en/VirtualTreeview.md> "VirtualTreeview") для Lazarus (проверено на win32). Они в основном собираются из Интернета, написанного для Delphi, и из учебника/документов Philipp Frenzel и Mike Lischke. 

Учебник / документы можно загрузить с <http://www.soft-gems.net>. Ниже кто-либо найдет только быстрый способ использовать VirtualTreeview на Lazarus, а не объяснения. Для пояснения и множества других функций/методов, получите официальные документы и учебник. 

## Contents

  * 1 Основной список дерева с тремя столбцами
  * 2 Checkbox
  * 3 Картинки
  * 4 Цвет шрифта
  * 5 Добавление Combobox'а
  * 6 Если правки ячейки не отображаются
  * 7 Внешние ссылки



# Основной список дерева с тремя столбцами

1\. Установите компонент. Запустите lazarus. 

2\. Бросьте [на форму] компонент TVirtualStringTree (вкладка Virtual Controls). 

3\. Перейдите в редактор исходного кода (нажмите F12). В разделе Uses добавьте модуль - VirtualTrees (если она еще не существует, и это не VirtualStringTree). Это может выглядеть так: 
    
    
    uses
      Classes, SysUtils, LResources, Forms, Controls, Graphics, Dialogs,
      VirtualStringTree, VirtualTrees;
    

4\. Войдите в конструктор форм (нажмите F12). Выберите компонент Virtual Tree. В Инспекторе объектов нажмите Name, введите VST и нажмите enter. Кликните на заголовок (разверните его) -> Columns, нажмите маленькую кнопку рядом с "0 items". Нажмите 3 раза кнопку Add, чтобы добавить 3 столбца. Не закрывайте это окно. 

5\. В окне редактирования столбца теперь выбирается третий столбец. Войдите в Инспектор объектов. Кликните Options (разверните его) -> установите для параметра `coAllowClick` значение `False`. 

6\. Нажмите на Text. Введите Column2. 

7\. Нажмите `Width`, введите 100 и нажмите enter. 

8\. Перейдите в окно редактирования столбцов, выберите 1-й и 2-й столбцы и установите их свойство, как указано выше (для текстового поля используйте разные имена, то есть Column0, Column1). 

9\. Закройте окно редактирования столбцов. Выберите компонент Virtual Tree в форме. В Object Inspector перейдите к Header -> Options ([раскройте спиcок]). Установите значение `hoVisible` в значение True. 

10\. Прокрутите вниз до Style, чтобы установить его в hsFlatButtons. 

[![ExempleVirtualtreeView1.jpg](https://wiki.freepascal.org/images/7/70/ExempleVirtualtreeView1.jpg)](</File:ExempleVirtualtreeView1.jpg>)

11\. Прокрутите вниз до TreeOptions (раскройте) -> MiscOption (раскройте), установите `toEditable` в значение `True`. Установите `toGridExtensions` в значение `True`. 

12\. Прокрутите вниз до пункта SelectionOptions(раскройте) -> установите toExtendedFocus в значение True. Установите toMultiSelect в значение True. В конструкторе форм измените размер VST (Virtual Tree Component) так, чтобы получить представление обо всех столбцах, если необходимо. 

13\. Теперь добавьте 3 кнопки на форму. Получите их из палитры компонентов - вкладка Standard (ярлык "OK"). 

14\. Нажмите кнопку Button1, изменив в Object Inspector [ее свойство] `Caption` на AddRoot. Нажмите кнопку Button2, измените заголовок на AddChild. Измените заголовок Button3 на Delete. 

[![ExempleVirtualtreeView2.jpg](https://wiki.freepascal.org/images/7/76/ExempleVirtualtreeView2.jpg)](</File:ExempleVirtualtreeView2.jpg>)

15\. Сохраните это и перейдите в редактор исходного кода (нажмите F12). В редакторе исходного кода замените строку: 
    
    
    {$mode objfpc}{$H+}
    

на 
    
    
    {$MODE DELPHI}
    

16\. В разделе "implementation" вставьте следующие строки: 
    
    
    type
      PTreeData = ^TTreeData;
      TTreeData = record
        Column0: String;
        Column1: String;
        Column2: String;
      end;
    

17\. Войдите в конструктор форм (нажмите F12). Выберите VST. Перейдите к Object Inspector, выберите вкладку `Events`, прокрутите вниз до `onChange`. Дважды щелкните значок со списком. Вставьте следующие данные: 
    
    
    procedure TForm1.VSTChange(Sender: TBaseVirtualTree; Node: PVirtualNode);
    begin
      VST.Refresh;
    end;
    

18\. Прокрутите до `onFocusChanged`. Дважды щелкните и вставьте следующее: 
    
    
    procedure TForm1.VSTFocusChanged(Sender: TBaseVirtualTree; Node: PVirtualNode;
      Column: TColumnIndex);
    begin
      VST.Refresh;
    end;
    

19\. Прокрутите до `onFreeNode`. Дважды щелкните и вставьте следующее: 
    
    
    procedure TForm1.VSTFreeNode(Sender: TBaseVirtualTree; Node: PVirtualNode);
    var
      Data: PTreeData;
    begin
      Data := VST.GetNodeData(Node);
      if Assigned(Data) then begin
        Data^.Column0 := '';
        Data^.Column1 := '';
        Data^.Column2 := '';
      end;
    end;
    

20\. Прокрутите вниз до `onGetNodeDataSize`. Дважды кликните и вставьте следующее: 
    
    
    procedure TForm1.VSTGetNodeDataSize(Sender: TBaseVirtualTree; var NodeDataSize: Integer);
    begin
      NodeDataSize := SizeOf(TTreeData);
    end;
    

21\. Прокрутите вниз до `onGetText`. Дважды кликните и вставьте следующее: 
    
    
    procedure TForm1.VSTGetText(Sender: TBaseVirtualTree; Node: PVirtualNode;
     Column: TColumnIndex; TextType: TVSTTextType; var CellText: WideString);
    var
      Data: PTreeData;
    begin
      Data := VST.GetNodeData(Node);
      case Column of
        0: CellText := Data^.Column0;
        1: CellText := Data^.Column1;
        2: CellText := Data^.Column2;
      end;
    end;
    

22\. Нажмите F12 для перехода дизайнер форм. Дважды кликните кнопку AddRoot. Вставьте следующее: 
    
    
    procedure TForm1.Button1Click(Sender: TObject);
    var
      Data: PTreeData;
      XNode: PVirtualNode;
      Rand: Integer;
    begin
      Randomize;
      Rand := Random(99);
      XNode := VST.AddChild(nil);
      
      if VST.AbsoluteIndex(XNode) > -1 then
      begin
        Data := VST.GetNodeData(Xnode);
        Data^.Column0 := 'One ' + IntToStr(Rand);
        Data^.Column1 := 'Two ' + IntToStr(Rand + 10);
        Data^.Column2 := 'Three ' + IntToStr(Rand - 10);
      end; 
    end;
    

23\. Нажмите F9 для запуска проекта с целью проверки. Нажмите AddRoot, чтобы добавить узел. Если все нормально, узел будет добавлен в VST. 

[![ExempleVirtualtreeView3.jpg](https://wiki.freepascal.org/images/8/86/ExempleVirtualtreeView3.jpg)](</File:ExempleVirtualtreeView3.jpg>)

24\. Прервите выполнение. В конструкторе форм кликните дважды кнопку с надписью AddChild. Вставьте следующее: 
    
    
    procedure TForm1.Button2Click(Sender: TObject);
    var
      XNode: PVirtualNode;
      Data: PTreeData;
    begin
      if not Assigned(VST.FocusedNode) then
        Exit;
        
      XNode := VST.AddChild(VST.FocusedNode);
      Data := VST.GetNodeData(Xnode);
        
      Data^.Column0 := 'Ch 1';
      Data^.Column1 := 'Ch 2';
      Data^.Column2 := 'Ch 3';
    
      VST.Expanded[VST.FocusedNode] := True;
    end;
    

25\. В конструкторе форм дважды нажмите кнопку Delete. Вставьте следующее: 
    
    
    procedure TForm1.Button3Click(Sender: TObject);
    begin
      VST.DeleteSelectedNodes;
    end;
    

26\. Запустите проект, нажав F9 для проверки. Добавьте узел, дочерний элемент и удалите их. 

[![ExempleVirtualtreeView4.jpg](https://wiki.freepascal.org/images/3/3d/ExempleVirtualtreeView4.jpg)](</File:ExempleVirtualtreeView4.jpg>)

27\. Попробуйте отредактировать узел. Выберите узел и нажмите F2, напишите новое значение. Если вы видите, что напечатали, тогда все в порядке. Иначе, читайте ниже: [Если правки ячейки не отображаются](<VirtualTreeview_Example_for_Lazarus.md> "VirtualTreeview Example for Lazarus/ru"). 

28\. Чтобы отображалось полученное VST новое значение, введенное после редактирования, перейдите в конструктор форм, выберите VST. В инспекторе объектов [дважды кликаем] по комбобоксу `OnNewText` на вкладке Events. Вставляем следующее: 
    
    
    procedure TForm1.VSTNewText(Sender: TBaseVirtualTree; Node: PVirtualNode;
      Column: TColumnIndex; NewText: WideString);
    var
      Data: PTreeData;
    begin
      Data := VST.GetNodeData(Node);
      case Column of
        0: Data^.Column0 := NewText;
        1: Data^.Column1 := NewText;
        2: Data^.Column2 := NewText;
      end;
    end;
    

Пока что пример базового использования заканчивается здесь. Вы можете бросить [еще] несколько кнопок на форму, чтобы проверить несколько команд, приведенных ниже. Далее будут показаны [примеры работы с] checkbox'ом, изображением, цветом шрифта и добавлением combobox'а на узле. 

  * Другой способ добавления корневого узла


    
    
    procedure TForm1.Button8Click(Sender: TObject);
    begin
      with VST do
        RootNodeCount := RootNodeCount + 1;
    end;
    

  * Другой способ добавления дочернего узла


    
    
    procedure TForm1.Button9Click(Sender: TObject);
    begin
      if Assigned(VST.FocusedNode) then
        VST.ChildCount[VST.FocusedNode] := VST.ChildCount[VST.FocusedNode] + 1;
    end;
    

  * Определение и удаление потомков узла


    
    
    procedure TForm1.Button4Click(Sender: TObject);
    var
      c: Integer;
    begin
      if not Assigned(VST.FocusedNode) then
        Exit;
      if VST.HasChildren[VST.FocusedNode] then
      begin
        c := VST.ChildCount[VST.FocusedNode];
        VST.DeleteChildren(VST.FocusedNode);
        ShowMessage('Number of deleted child:' + #13#10 + IntToStr(c));
      end;
    end;
    

  * Удаление узла


    
    
    procedure TForm1.Button5Click(Sender: TObject);
    begin
    {VST.Clear;  //Удаляет все узлы}
      if not Assigned(VST.FocusedNode) then
        Exit;
      VST.DeleteNode(VST.FocusedNode);
    end;
    

  * Поиск и выделение узла


    
    
    procedure TForm1.Button6Click(Sender: TObject);
    bar
      XNode: PVirtualNode;
      Data: PTreeData;
    begin
      XNode := VST.GetFirst;
    
      while XNode <> nil do
      begin
        Data := VST.GetNodeData(XNode);
        if Data^.Column0 = '1' then
        begin
          VST.ClearSelection;
          VST.Selected[XNode] := True;
          VST.SetFocus;
          break;
        end else
        XNode := VST.GetNextSibling(XNode);
      end;
    end;
    

  * Определение родителя


    
    
    procedure TForm1.Button13Click(Sender: TObject);
    var
      XNode: PVirtualNode;
    begin
      if not Assigned(VST.FocusedNode) then
        Exit;
      XNode := VST.FocusedNode;
      while VST.GetNodeLevel(XNode) > 0 do
      begin
        XNode := XNode.Parent;
        VST.Selected[XNode] := True;
      end;
      VST.Refresh;
      VST.SetFocus;
    end;
    

  * Поиск всего


    
    
    procedure TForm1.Button7Click(Sender: TObject);
    var
      XNode: PVirtualNode;
      Data: PTreeData;
    begin
      if VST.GetFirst = nil then Exit;
      XNode := nil;
      repeat
        if XNode = nil then 
          XNode := VST.GetFirst 
        else 
          XNode := VST.GetNext(XNode);
        Data := VST.GetNodeData(XNode);
        if (Data^.Column0 = '1') or (Data^.Column1 = '1') or (Data^.Column2 = '1') then
        begin
          ShowMessage('Found at Node Level : ' + IntToStr(VST.GetNodeLevel(XNode)));
          break;
        end;
      until XNode = VST.GetLast();
    end;
    

  * Поиск следующего


    
    
    procedure TForm1.Button8Click(Sender: TObject);
    var
      XNode: PVirtualNode;
      Data: PTreeData;
    begin
      if not Assigned(VST.GetFirst) then
        Exit
      else
        XNode := VST.GetFirst;
      repeat
        XNode := VST.GetNext(XNode);
        Data := VST.GetNodeData(XNode);
        if Pos(LowerCase(SearchEdit.Text), LowerCase(Data^.Column0)) > 0 then
        begin
          VST.FocusedNode := XNode;
          VST.Selected[XNode] := True;
          if MessageDlg('Item found?', mtConfirmation, mbYesNo, 0) = mrYes then
          begin
            VST.Expanded[XNode] := True;
            VST.Refresh;
            VST.SetFocus;
            Break;
          end;
        end;
      until XNode = VST.GetLast;
    end;
    

  * Вставка узла


    
    
    procedure TForm1.Button12Click(Sender: TObject);
    var
      XNode: PVirtualNode;
    begin
      if Assigned(VST.FocusedNode) then
      begin
        XNode := VST.InsertNode(VST.FocusedNode,amInsertBefore);
        // Вставка после выделенного узла
        {XNode := VST.InsertNode(VST.FocusedNode,amInsertAfter);}
        VST.Refresh;
      end;
    end;
    

  * Установка высоты узла


    
    
    procedure TForm1.Button14Click(Sender: TObject);
    begin
      if Assigned(VST.FocusedNode) then
        VST.NodeHeight[VST.FocusedNode] := 32;
    end;
    

  * Сохранение и загрузка



Простое дерево (без столбцов) можно сохранить и загрузить как: 
    
    
    VST.SaveToFile('filename.dat');
    VST.LoadFromFile('filename.dat');
    

  
Чтобы сохранить и загрузить вышеупомянутый пример, поместите 2 кнопки на форму, переименуйте заголовок в "Save" и "Load". Выберите VST, в Object Inspector -> TreeOptions -> StringOptions убедитесь, что [значение] toSaveCaptions установлено в True. Перейдите на вкладку Events инспектора объектов. Прокрутите вниз до OnLoadNode, дважды щелкните [его] и вставьте: 
    
    
    procedure TForm1.VSTLoadNode(Sender: TBaseVirtualTree; Node: PVirtualNode;
      Stream: TStream);
    var
      Data: PTreeData;
      Len: Integer;
    begin
      Data := VST.GetNodeData(Node);
      Stream.read(Len, SizeOf(Len));
      SetLength(Data^.Column0, Len);
      Stream.read(PChar(Data^.Column0)^, Len);
    
      Stream.read(Len, SizeOf(Len));
      SetLength(Data^.Column1, Len);
      Stream.read(PChar(Data^.Column1)^, Len);
    
      Stream.read(Len, SizeOf(Len));
      SetLength(Data^.Column2, Len);
      Stream.read(PChar(Data^.Column2)^, Len);
    end;
    

Снова перейдите на вкладку Events инспектора объектов - прокрутите до OnSaveNode, дважды щелкните и вставьте: 
    
    
    procedure TForm1.VSTSaveNode(Sender: TBaseVirtualTree; Node: PVirtualNode;
      Stream: TStream);
    var
      Data: PTreeData;
      Len: Integer;
    begin
      Data := VST.GetNodeData(Node);
      Len := Length(Data^.Column0);
      Stream.write(Len, SizeOf(Len));
      Stream.write(PChar(Data^.Column0)^, Len);
    
      Len := Length(Data^.Column1);
      Stream.write(Len, SizeOf(Len));
      Stream.write(PChar(Data^.Column1)^, Len);
    
      Len := Length(Data^.Column2);
      Stream.write(Len, SizeOf(Len));
      Stream.write(PChar(Data^.Column2)^, Len);
    end;
    

В редакторе форм дважды щелкните по кнопке с заголовком Save, вставьте: 
    
    
    procedure TForm1.Button10Click(Sender: TObject);
    begin
      VST.SaveToFile('C:\vst.dat');
    end;
    

В редакторе форм дважды щелкните по кнопке с заголовком Load, вставьте: 
    
    
    procedure TForm1.Button11Click(Sender: TObject);
    begin
      VST.LoadFromFile('C:\vst.dat');
    end;
    

Теперь тест сохраняет и загружает дерево. 

  * Проблемы с прокруткой



Заголовок дерева полностью или частично исчезает при прокрутке. Я не смог найти для этого хорошего решения. Один из способов преодолеть это - установить высоту заголовка в 0, а затем использовать общую надпись для столбцов. Это хорошо, когда имеется несколько столбцов, и все они видны без горизонтальной прокрутки. Или высота заголовка может быть установлена на большее значение, например 25 или 30. VST.Refresh можно добавить в событие OnScroll. 

  * Изменение размера столбцов



Невозможно изменить размер столбца перетаскиванием мышью заголовка VST. Может быть, это ошибка заголовка, или я что-то пропустил. Если это для заголовка, вероятно, будет исправлено в следующей версии Lazarus. См. Эту ссылку: <http://bugs.freepascal.org/view.php?id=11209>   
Во всяком случае, можно изменить размер столбца из кода. [Сделав так:] когда вы зажимаете правую кнопку мыши и крутите колесико мыши вверх, ширина выбранного столбца [будет] увеличиваться; а [когда] зажимаете правую кнопку мыши и крутите колесико мыши вниз, ширина выбранного столбца будет уменьшаться. Для этого:   


1\. Добавьте переменную в исходный код под именем `CurCol: Integer;`, чтобы это выглядело так: 
    
    
    var
      Form1: TForm1; 
      CurCol: Integer; // <- Добавьте только эту строчку.
    
    implementation
    
    { TForm1 }
    

2\. В конструкторе форм дважды щелкните по форме, чтобы создать событие OnCreate. Внутри процедуры OnCreate напишите `Form1.OnMouseWheelUp:=` и нажмите `Ctrl` \+ `⇧ Shift` \+ `C`, это завершит код и сделает скелет события MouseWheelUp. Теперь вернемся к процедуре `TForm1.FormCreate(Sender: TObject);`. Добавьте еще одно событие для MouseWheelDown. Введите `Form1.OnMouseWheelDown:=` и нажмите `Ctrl` \+ `⇧ Shift` \+ `C`, чтобы сгенерировать событие MouseWheelDown. Процедура FormCreate теперь выглядит так: 
    
    
    procedure TForm1.FormCreate(Sender: TObject);
    begin
      Form1.OnMouseWheelUp   := @Form1MouseWheelUp;
      Form1.OnMouseWheelDown := @Form1MouseWheelDown;
    end;
    

3\. Заполните процедуру TForm1.Form1MouseWheelUp как: 
    
    
    procedure TForm1.Form1MouseWheelUp(Sender: TObject; Shift: TShiftState;
      MousePos: TPoint; var Handled: Boolean);
    begin
      if VST.Focused and (ssRight in Shift) then
        VST.Header.Columns[CurCol].Width := VST.Header.Columns[CurCol].Width + 10;
    end;
    

4\. Заполните процедуру TForm1.Form1MouseWheelDown как: 
    
    
    procedure TForm1.Form1MouseWheelDown(Sender: TObject; Shift: TShiftState;
      MousePos: TPoint; var Handled: Boolean);
    begin
      if VST.Focused and (ssRight in Shift) then
        VST.Header.Columns[CurCol].Width := VST.Header.Columns[CurCol].Width - 10;
    end;
    

5\. Перейдите в конструктор форм (нажмите F12), выберите VST, на вкладке Events инспектора объектов прокрутите список до OnFocusChanged, дважды кликните [по нему] и вставьте: 
    
    
    procedure TForm1.VSTFocusChanged(Sender: TBaseVirtualTree;
      Node: PVirtualNode; Column: TColumnIndex);
    begin
      CurCol := Column;
    end;
    

Когда вы запустите [программу], щелкните по столбцу, затем удерживайте зажатой правую кнопку мыши и перемещайте колесико [прокрутки мыши] вверх, чтобы увеличить ширину, и вниз[, чтобы ее] уменьшить. При необходимости вы можете настроить вышеуказанные процедуры. Или добавить событие клавиатуры, что-то вроде _"if (key = 187) and (ssShift в Shift) then"_ для просмотра по `⇧ Shift` \+ `+`. 

# Checkbox

В дизайнере форм выберите VST. Перейдите к:  


  1. Object Inspector -> Properties -> CheckImageKind и выберите ckDarkCheck.
  2. Object Inspector -> Properties -> TreeOptions -> MiscOptions -> toCheckSupport и установите его значение в True.  




Теперь переключитесь на вкладку Events.  


  * Прокрутите до OnInitNode. Дважды кликните [на нем] и вставьте следующее:


    
    
    procedure TForm1.VSTInitNode(Sender: TBaseVirtualTree; ParentNode,
      Node: PVirtualNode; var InitialStates: TVirtualNodeInitStates);
    var
      Level: Integer;
    begin
      Level := VST.GetNodeLevel(Node);//уровень вложенности
    
      case Level of
        0:  Node^.CheckType:= ctCheckBox;//Root (пустой checkbox)
        1:
          begin
            Node^.CheckType  := ctTriStateCheckBox;//checkbox c 3-мя состояниями
            Node^.CheckState := csUncheckedNormal;//пустой checkbox
          end;
        2:  Node^.CheckType := ctRadioButton;//RadioButton
        3:  Node^.CheckType := ctButton;//кнопка выпадающего списка
        4:
          begin
            Node^.CheckType  := ctTriStateCheckBox;//checkbox c 3-мя состояниями
            Node^.CheckState := cscheckedNormal;//отмеченный checkbox
          end;
      end; 
    end;
    

[![vst checkbox.png](https://wiki.freepascal.org/images/9/96/vst_checkbox.png)](</File:vst_checkbox.png>)

Запустите программу, добавьте родительский узел и дочерний, затем дочерний элемент дочернего узла, и проверьте, можете ли вы изменять состояние [checkbox'а]. Если нет, закройте программу. Перейдите на вкладку Events инспектора объектов. 

  * Прокрутите до OnChecked, дважды кликните [на нем] и вставьте:


    
    
    procedure TForm1.VSTChecked(Sender: TBaseVirtualTree; Node: PVirtualNode);
    begin
      VST.Refresh;
    end;
    

  * Прокрутите до OnChecking, дважды кликните [на нем] и вставьте:


    
    
    procedure TForm1.VSTChecking(Sender: TBaseVirtualTree; Node: PVirtualNode;
      var NewState: TCheckState; var Allowed: Boolean);
    begin
      VST.Refresh;
    end;
    

Надеюсь, теперь все в порядке. Определить состояние checkbox'а можно так:  

    
    
    if XNode.CheckState = csCheckedNormal then
    ShowMessage('Checked');
    

  


Другие значения флажов состояний:  

    
    
    csUncheckedNormal = не отмечен и не нажат 
    csUncheckedPressed = не отмечен и нажат
    csCheckedNormal = отмечен и не нажат
    csCheckedPressed = отмечен и нажат
    csMixedNormal = checkbox имеет 3 состояния и не нажат
    csMixedPressed = checkbox имеет 3 состояния и нажат
    

* * *

[Прим.перев:](</User:Zoltanleo> "User:Zoltanleo") визуально свойство `CheckState` выглядит примерно так:  
[![checkstate enum.png](https://wiki.freepascal.org/images/f/f6/checkstate_enum.png)](</File:checkstate_enum.png>)  


* * *

  
Другие типы:  

    
    
    ctNone
    ctTriStateCheckBox
    ctCheckBox
    ctRadioButton
    ctButton
    

* * *

[Прим.перев:](</User:Zoltanleo> "User:Zoltanleo") визуально свойство `CheckType` выглядит примерно так:  
[![checktype enum.png](https://wiki.freepascal.org/images/6/67/checktype_enum.png)](</File:checktype_enum.png>)  


* * *

  


  * Отлов нажатия кнопки Checkbox'а (ctButton)



Перейдите на вкладку Events объекта инспектора. Прокрутите до OnChecked, дважды кликните [на нем] и вставьте: 
    
    
    procedure TForm1.VSTChecked(Sender: TBaseVirtualTree; Node: PVirtualNode);
    begin
      if Node.CheckType = ctButton then
        ShowMessage('Ok.');
      VST.Refresh;
    end;
    

  


# Картинки

Чтобы показать изображение в узлах VST, необходимо создать список изображений. 

  * Перейдите в палитру компонентов -> вкладка Common Controls. Выберите и бросьте на форму компонент TImageList. Щелкните правой кнопкой мыши значок компонента и выберите редактор ImageList. Нажмите кнопку "Add" и выберите какие-нибудь изображения (по крайней мере 3 на данный момент), затем нажмите кнопку "OK", чтобы принять и закрыть редактор ImageList. Кстати, есть несколько хороших изображений, которые вы можете скачать с <http://www.famfamfam.com/lab/icons/silk/>
  * Теперь в дизайнере форм выберите VST и на вкладке Properties инспектора объектов прокрутите до [свойства] Images и выберите ImageList1
  * На вкладке Events инспектора объектов прокрутите до OnGetImageIndex, дважды кликните [на нем] и вставьте:


    
    
    procedure TForm1.VSTGetImageIndex(Sender: TBaseVirtualTree; Node: PVirtualNode;
      Kind: TVTImageKind; Column: TColumnIndex; var Ghosted: Boolean;
      var ImageIndex: Integer);
    begin
      if Kind in [ikNormal , ikSelected] then // либо выбран, либо нет
      begin
        if Column = 0 then                    // если 1-й столбец
          ImageIndex := 0;                    // 1-е изображение ImageList1'а
    
        if Column = 1 then                    // если 2-й столбец
          ImageIndex := 1;                    // 2-е изображение ImageList1'а
    
        if Sender.FocusedNode = Node then     // Показывать, только если в фокусе
          if Column = 2 then                  // если 3-й столбец
            ImageIndex := 2;                  // 3-е изображение ImageList1'а
      end;
      {Sender.NodeHeight[node] := 40; //Если изображение большое}
    end;
    

  


# Цвет шрифта

В дизайнере форм выберите VST и на вкладке Events инспектора объектов прокрутите до OnPaintText, дважды кликните [на нем] и вставьте: 
    
    
    procedure TForm1.VSTPaintText(Sender: TBaseVirtualTree;
      const TargetCanvas: TCanvas; Node: PVirtualNode; Column: TColumnIndex;
      TextType: TVSTTextType);
    var
      Data: PTreeData;
    begin
      Data := VST.GetNodeData(Node);
     
      if Data^.Column0 = 'sky' then
        TargetCanvas.Font.Color := clBlue;
      
      if Column = 1 then
      begin
        TargetCanvas.Font.Color := clRed;
        TargetCanvas.Font.Style := Font.Style + [fsItalic];
      end;
     
      if Column = 2 then
      begin
    //  ImageList1.Draw(Form1.Canvas, -1, -1, 2); {рисует верхнюю левую часть формы, третье изображение ImageList1 ??}
        TargetCanvas.Font.Size  := 9;
        TargetCanvas.Font.Color := clHighlightText;
      end;
    end;
    

  


# Добавление Combobox'а

  * Я предполагаю, что у вас есть открытый проект на Lazarus IDE с VST на нем и возможность редактировать узлы. Если нет, см. выше [Основной список дерева с тремя столбцами](<VirtualTreeview_Example_for_Lazarus.md> "VirtualTreeview Example for Lazarus/ru") и, по крайней мере, выполните шаги с 1 по 21.


  * Ниже приведен файл модуля с именем combo. Скопируйте этот модуль и сохраните его как combo.pas внутри каталога проекта. Добавьте [модуль] комбо в раздел uses вашей программы. Это может выглядеть так:


    
    
    uses
      Classes, SysUtils, LResources, Forms, Controls, Graphics, Dialogs,
      VirtualStringTree, VirtualTrees, combo;
    

  * Модуль combo.pas


    
    
    unit combo;
    
    {$mode delphi}
    
    interface
    
    uses
      Classes, SysUtils, LResources, Forms, Controls, Graphics, Dialogs,
      VirtualStringTree, VirtualTrees, messages, windows, StdCtrls;
    
    type
      TStringEditLink = class(TInterfacedObject, IVTEditLink)
      private
        FEdit: TWinControl;
        FTree: TVirtualStringTree;
        FNode: PVirtualNode;
        FColumn: Integer;
      protected
        procedure EditKeyDown(Sender: TObject; var Key: Word; Shift: TShiftState);
      public
        destructor Destroy; override;
        function BeginEdit: Boolean; stdcall;
        function CancelEdit: Boolean; stdcall;
        function EndEdit: Boolean; stdcall;
        function GetBounds: TRect; stdcall;
        function PrepareEdit(Tree: TBaseVirtualTree; Node: PVirtualNode; Column: TColumnIndex): Boolean; stdcall;
        procedure ProcessMessage(var Message: TMessage); stdcall;
        procedure SetBounds(R: TRect); stdcall;
      end;
    
    implementation
    
    destructor TStringEditLink.Destroy;
    begin
      FEdit.Free;
      inherited;
    end;
    
    procedure TStringEditLink.EditKeyDown(Sender: TObject; var Key: Word; Shift: TShiftState);
    begin
      case Key of
        VK_ESCAPE:
          begin
            FTree.CancelEditNode;
            Key := 0;
            FTree.setfocus;
          end;
        VK_RETURN:
          begin
           PostMessage(FTree.Handle, WM_KEYDOWN, VK_DOWN, 0);
           Key := 0;
           FTree.EndEditNode;
           FTree.setfocus;
          end;
      end; //case
    end;
    
    function TStringEditLink.BeginEdit: Boolean;
    begin
      Result := True;
      //FEdit.Height := (FTree.DefaultNodeHeight - 1); //Нужен для editbox. Не для combo
      FEdit.Show;
      TComboBox(FEdit).DroppedDown := True;
      FEdit.SetFocus;
    end;
    
    function TStringEditLink.CancelEdit: Boolean;
    begin
      Result := True;
      FEdit.Hide;
    end;
    
    function TStringEditLink.EndEdit: Boolean;
    var
      s: String;
    begin
      Result := True;
      s := TComboBox(FEdit).Text;
      FTree.Text[FNode, FColumn] := s;
    
      FTree.InvalidateNode(FNode);
      FEdit.Hide;
      FTree.SetFocus;
    end;
    
    function TStringEditLink.GetBounds: TRect;
    begin
      Result := FEdit.BoundsRect;
    end;
    
    function TStringEditLink.PrepareEdit(Tree: TBaseVirtualTree; Node: PVirtualNode; Column: TColumnIndex): Boolean;
    begin
      Result := True;
      FTree := Tree as TVirtualStringTree;
      FNode := Node;
      FColumn := Column;
    
      FEdit.Free;
      FEdit := nil;
    
      FEdit := TComboBox.Create(nil);
      with FEdit as TComboBox do
        begin
          Visible := False;
          Parent := Tree;
          Items.Add('Google');
          Items.Add('Yahoo');
          Items.Add('Altavista');
          OnKeyDown := EditKeyDown;
        end;
    end;
    
    procedure TStringEditLink.ProcessMessage(var Message: TMessage);
    begin
      FEdit.WindowProc(Message);
    end;
    
    procedure TStringEditLink.SetBounds(R: TRect);
    var
      Dummy: Integer;
    begin
      FTree.Header.Columns.GetColumnBounds(FColumn, Dummy, R.Right);
      FEdit.BoundsRect := R;
    end;
    
    end.
    

  * После сохранения файла в дизайнере форм выберите VST и [на вкладке] Properties инспектора объектов прокрутите до [свойства] TreeOptions -> MiscOptions, установите [параметр] toEdeitable в значение True. Затем перейдите в TreeOptions -> SelectionOptions, установите [параметр] toExtendedFocus в значение True.


  * Переключитесь на вкладку Events инспектора объектов. Прокрутите до OnCreateEditor, дважды кликните [на нем] и вставьте:


    
    
    procedure TForm1.VSTCreateEditor(Sender: TBaseVirtualTree;
      Node: PVirtualNode; Column: TColumnIndex; out EditLink: IVTEditLink);
    begin
      EditLink := TStringEditLink.Create;
    end;
    

  * На вкладке Events инспектора объектов. Прокрутите до OnNewText, дважды кликните [на нем] и вставьте:


    
    
    procedure TForm1.VSTNewText(Sender: TBaseVirtualTree; Node: PVirtualNode;
      Column: TColumnIndex; NewText: WideString);
    var
      Data: PTreeData;
    begin
      Data := VST.GetNodeData(Node);
      case Column of
        0: Data^.Column0 := NewText;
        1: Data^.Column1 := NewText;
        2: Data^.Column2 := NewText;
      end;
    end;
    

Запустите программу, выберите узел и нажмите F2, чтобы получить combobox. При нажатии Enter на узле должно появиться новое значение.  


# Если правки ячейки не отображаются

Откройте файл модуль VirtualStringTree.pas (если вы все еще находитесь в приведенном выше примере проекта, щелкните правой кнопкой мыши на VirtualStringTree в разделе Uses и выберите "Найти объявление". Это откроет файл на следующей вкладке. Перейдите на вкладку этого файла.). Войдите в "function TStringEditLink.BeginEdit: Boolean; stdcall;". Это выглядит как: 
    
    
    function TStringEditLink.BeginEdit: Boolean; stdcall;
    
    // Сообщает ссылку на редактирование, которая может начать редактирование. Потомки могут отменить редактирование узла, 
    // вернув False.
    
    begin
      Result := not FStopping;
      if Result then
      begin
        FEdit.Show;
        FEdit.SelectAll;
        FEdit.SetFocus;
      end;
    end;
    

Теперь добавьте "FEdit.Height:=18;". Это должен выглядеть так: 
    
    
    function TStringEditLink.BeginEdit: Boolean; stdcall;
    
    // Сообщает ссылку на редактирование, которая может начать редактирование. Потомки могут отменить редактирование узла, 
    // вернув False.
    
    begin
      Result := not FStopping;
      if Result then
      begin
        FEdit.Show;
        FEdit.SelectAll;
        FEdit.SetFocus;
        FEdit.Height := 18; // <--- Добавлена эта строчка.
      end;
    end;
    

Сохраните файл (нажмите `Ctrl` \+ `S`). Если вы находитесь в примере проекта, закройте его (Project -> Close Project). Нажмите Tools -> Configure "Build Lazarus" ... Выберите Clean Up + Build All, а затем нажмите кнопку Build. После компиляции Lazarus следует перезапустить. Теперь откройте пример проекта и попробуйте отредактировать узел на VST. На этот раз все должно быть хорошо.  


# Внешние ссылки

  * [[1]](<http://www.soft-gems.net>) \- Virtual Treeview tutorials/docs
  * [Programando en Pascal](<http://es.wikibooks.org/wiki/Programaci%C3%B3n_en_Pascal>) \- Испанский учебник, посвященный FPC/Lazarus, размещается на Wikibooks.

---

_Source: [https://wiki.freepascal.org/VirtualTreeview_Example_for_Lazarus/ru](https://web.archive.org/web/20241104221929/https://wiki.freepascal.org/VirtualTreeview_Example_for_Lazarus/ru)_
