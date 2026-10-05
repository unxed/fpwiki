# Win32/64 Interface

[![Windows logo - 2012.svg](https://upload.wikimedia.org/wikipedia/commons/thumb/5/5f/Windows_logo_-_2012.svg/50px-Windows_logo_-_2012.svg.png)](</File:Windows_logo_-_2012.svg>)

Эта статья относится только к [Windows](</Category:Windows> "Category:Windows").

См. также: [Multiplatform Programming Guide](<../../en/Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

│ **[English (en)](<../../en/Win32/64_Interface.md>)** │  **русский (ru)** │

## Contents

  * 1 Введение
  * 2 Актуальные вопросы
    * 2.1 Скроллинг
    * 2.2 Показ рамки фокуса элементов управления, отображающихся в соответствии с темой
    * 2.3 Навигация по элементам управления с помощью клавиш со стрелками
  * 3 Детали реализации
    * 3.1 Цвет фона стандартных элементов управления
    * 3.2 TCheckListBox
  * 4 FAQ
    * 4.1 Обработка пользовательских сообщений в вашем окне
    * 4.2 Обработка не пользовательских сообщений в вашем окне
      * 4.2.1 Пример
  * 5 Other Interfaces
    * 5.1 Platform specific Tips
    * 5.2 Interface Development Articles



## Введение

Интерфейс win32 / 64 является, пожалуй, самым отлаженным и хорошо разработанным интерфейсом Lazarus, а также наиболее часто используемым, учитывая количество загрузок. Несмотря на то, что он наиболее полный, есть некоторые проблемы, которые нуждаются в исправлении. 

Еще один важный момент, связанный с интерфейсом win32/64, заключается в том, что он в настоящее время находится в процессе [миграции на unicode](<../../en/LCL_Unicode_Support.md> "LCL Unicode Support"). 

Win64: см. предостережение [ здесь](<../../en/Windows_Programming_Tips.md> "Windows Programming Tips") о неиспользовании определенных версий FPC/Lazarus Win64. 

## Актуальные вопросы

### Скроллинг

В настоящее время прокрутка выполняется путем перемещения дочерних элементов управления вместо перемещения клиентской области, как ожидает LCL. Например, в некоторых случаях это выглядит так, как будто дочерние элементы прокручиваются в обратном порядке. Правда такова, что прокрутка в значительной степени сломана. Прокрутка дочерних элементов управления несовместима с другими наборами виджетов, а также имеет такой недостаток: перемещение одного дочернего элемента за другим генерирует несколько сообщений перемещения. LCL получает сообщения и должен реагировать каждый раз (например, он должен перераспределить все привязанные дочерние элементы). Для одного набора виджетов вы можете попытаться преодолеть эту проблему, но решение никогда не будет работать одинаково хорошо для всех других наборов виджетов. Этот подход работает для VCL Delphi, но не для LCL. Поэтому должен быть реализован другой подход: 

**Решение**

Решением было бы вставить окно «клиентской области» между окном каждого дочернего элемента и его родительским окном. Дочерние окна помещаются в окно «клиентской области», и когда прокручиваются дочерние окна, вместо этого перемещается окно «клиентской области». Это уже сделано другими наборами виджетов. 

_Замечание Mattias_ : В конце концов, я это реализую, но мои знания интерфейса winapi ограничены, и у меня уже есть множество других задач Lazarus, поэтому я не могу сказать, когда я это сделаю. 

**Связанные сообщения об ошибках**

  * [10400](<http://bugs.freepascal.org/view.php?id=10400>)
  * [10188](<http://bugs.freepascal.org/view.php?id=10188>)
  * [10471](<http://bugs.freepascal.org/view.php?id=10471>)
  * [10091](<http://bugs.freepascal.org/view.php?id=10091>)



### Показ рамки фокуса элементов управления, отображающихся в соответствии с темой

Элементы управления (CheckBox, Button, TRadioButton и т.д.) теряют рамку фокуса, когда приложение использует темы. 

**Советы по решению**

  * По словам Пола, это не связано с WM_PAINT



**Связанные сообщения об ошибках**

  * <http://bugs.freepascal.org/view.php?id=10685>



### Навигация по элементам управления с помощью клавиш со стрелками

**Связанные сообщения об ошибках**

  * <http://bugs.freepascal.org/view.php?id=10742>



## Детали реализации

Этот список объясняет, как определенные части LCL реализованы в интерфейсе Win32, чтобы помочь людям найти соответствующий код для исправления ошибок. 

### Цвет фона стандартных элементов управления

См. также: [Windows_CE_Development_Notes#TRadioButton_and_TGroupBox](<../../en/Windows_CE_Development_Notes.md> "Windows CE Development Notes")

Можно заметить, что в Windows нет реализации для TWSWin32WinControl.SetColor, а также для большинства стандартных элементов управления (GroupBox, RadioButton, CheckBox и т.д.), даже если эти элементы управления могут изменить свой цвет фона. 

Причина в том, что это реализовано обработкой сообщения WM_CTLCOLOR*. Большинство стандартных элементов управления (GroupBox, RadioButton, CheckBox и т.д.) посылают сообщение WM_CTLCOLORSTATIC. В этом сообщении можно установить цвет фона, и должен быть возвращен дескриптор кисти, используемой для рисования элемента управления. 

Еще одна важная деталь - это то, что дочерние элементы управления посылают свои сообщения WindowProc своего родителя. Итак, если у вас есть Форма с GroupBox и несколькими CheckBoxes внутри GroupBox, сообщения GroupBox будут отправляться в Form, а сообщения CheckBoxes будут отправляться в GroupBox (включая сообщение WM_CTLCOLORSTATIC). Чтобы преодолеть это, набор виджетов win32 использует SetWindowLong для сброса WindowProc элементов управления с дочерними элементами в наш централизованный WindowControl на win32callback.inc. 

### TCheckListBox

См. также: [Windows_CE_Development_Notes#TCheckListBox](<../../en/Windows_CE_Development_Notes.md> "Windows CE Development Notes")

  * _TWin32WSCustomCheckListBox_ реализует некоторые минимальные методы
  * _TWin32WSCustomListBox_ реализует создание дескрипторов и большинство методов
  * _TWin32CheckListBoxStrings_ является основным потомком TStrings для этого класса



В Windows TCheckListBox является обычным окном класса 'LISTBOX', в котором установлен стиль LBS_OWNERDRAWFIXED. Когда будет создан список, будет отправлено сообщение WM_MEASUREITEM, а после, сообщение WM_DRAWITEM будет отправляться всякий раз, когда элемент должен быть отрисован. В обработчике этого сообщения будет создано сообщение LM_DRAWITEM. 

Затем сообщение LM_DRAWITEM перехватывается TWin32WidgetSet.CallDefaultWndHandler и обрабатывается в его внутренней функции: 
    
    
    procedure DrawCheckListBoxItem(CheckListBox: TCheckListBox; Data: PDrawItemStruct);
    

А вот реальный код для рисования элементов TCheckListBox. 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Примечание:** Это некрасиво, может быть, этот код следует перенести в LCL, чтобы у нас был общий код для рисования элементов на случай, если набор виджетов не сделает это сам.

MSDN Docs о LISTBOX: 

  * <http://msdn2.microsoft.com/en-us/library/bb775146(VS.85).aspx>
  * <http://msdn2.microsoft.com/en-us/library/bb775149(VS.85).aspx>



## FAQ

### Обработка пользовательских сообщений в вашем окне

Напишите мне... 

### Обработка не пользовательских сообщений в вашем окне

Для пользовательской обработки сообщений <= WM_USER вы должны использовать SetWindowLong из модуля Windows. Это вернет адрес текущего WndProc, так что вы можете заполучить свой WndProc следующим образом: 
    
    
    begin
     if Msg = WM_COPYDATA then
     ...
     else CallOldWindowProc;
    end;
    

And you don't lose anything. With a clever code you can even use the same wndproc for any control, just take care to call the correct old wndproc in each case. 

И вы ничего не теряете. С умным кодом вы можете даже использовать тот же wndproc для любого элемента управления, просто позаботьтесь, чтобы вызывать правильный старый wndproc в каждом случае. 

#### Пример

Перехватывая сообщение WM_NCHITTEST, вы можете избежать перетаскивания окна. 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Примечание:** Часть, в которой функция взаимодействует с "WM_NCHITTEST", имеет очень странный результат в Windows XP. Вы не сможете переместить окно. В Vista, с другой стороны, вы все еще можете. Комментирование этого раздела, кажется, не приносит вреда, и вы можете переместить окно программы в 

Windows XP.
    
    
    var
      PrevWndProc: WNDPROC;
    ...
    function WndCallback(Ahwnd: HWND; uMsg: UINT; wParam: WParam; lParam: LParam):LRESULT; stdcall;
    begin
      if uMsg=WM_NCHITTEST then
      begin
        result:=Windows.DefWindowProc(Ahwnd, uMsg, WParam, LParam);  //not sure about this one
        if result=windows.HTCAPTION then result:=windows.HTCLIENT;
        exit;
      end;
      result:=CallWindowProc(PrevWndProc,Ahwnd, uMsg, WParam, LParam);
    end;
    
    //устанавливаем наш обработчик сообщений
    procedure TForm1.FormCreate(Sender: TObject);
    begin
      PrevWndProc:=Windows.WNDPROC(SetWindowLongPtr(Self.Handle,GWL_WNDPROC,PtrInt(@WndCallback)));
    end;
    

  


## Other Interfaces

  * [Lazarus known issues (things that will never be fixed)](<../../en/Lazarus_known_issues_\(things_that_will_never_be_fixed\).md> "Lazarus known issues \(things that will never be fixed\)") \- A list of interface compatibility issues
  * [Win32/64 Interface](<../../en/Win32/64_Interface.md> "Win32/64 Interface") \- The Windows API (formerly Win32 API) interface for Windows 95/98/Me/2000/XP/Vista/10, but not CE
  * [Windows CE Interface](<../../en/Windows_CE_Interface.md> "Windows CE Interface") \- For Pocket PC and Smartphones
  * [Carbon Interface](<../../en/Carbon_Interface.md> "Carbon Interface") \- The Carbon 32 bit interface for macOS (deprecated; removed from macOS 10.15)
  * [Cocoa Interface](<../../en/Cocoa_Interface.md> "Cocoa Interface") \- The Cocoa 64 bit interface for macOS
  * [Qt Interface](<../../en/Qt_Interface.md> "Qt Interface") \- The Qt4 interface for Unixes, macOS, Windows, and Linux-based PDAs
  * [Qt5 Interface](<../../en/Qt5_Interface.md> "Qt5 Interface") \- The Qt5 interface for Unixes, macOS, Windows, and Linux-based PDAs
  * [GTK1 Interface](<../../en/GTK1_Interface.md> "GTK1 Interface") \- The gtk1 interface for Unixes, macOS (X11), Windows
  * [GTK2 Interface](<../../en/GTK2_Interface.md> "GTK2 Interface") \- The gtk2 interface for Unixes, macOS (X11), Windows
  * [GTK3 Interface](<../../en/GTK3_Interface.md> "GTK3 Interface") \- The gtk3 interface for Unixes, macOS (X11), Windows
  * [fpGUI Interface](<../../en/fpGUI_Interface.md> "fpGUI Interface") \- Based on the fpGUI library, which is a cross-platform toolkit completely written in Object Pascal
  * [Custom Drawn Interface](<../../en/Custom_Drawn_Interface.md> "Custom Drawn Interface") \- A cross-platform LCL backend written completely in Object Pascal inside Lazarus. The Lazarus interface to Android.



### Platform specific Tips

  * [Android Programming](<../../en/Android_Programming.md> "Android Programming") \- For Android smartphones and tablets
  * [iPhone/iPod development](<../../en/iPhone/iPod_development.md> "iPhone/iPod development") \- About using Objective Pascal to develop iOS applications
  * [FreeBSD Programming Tips](<../../en/FreeBSD_Programming_Tips.md> "FreeBSD Programming Tips") \- FreeBSD programming tips
  * [Linux Programming Tips](<../../en/Linux_Programming_Tips.md> "Linux Programming Tips") \- How to execute particular programming tasks in Linux
  * [macOS Programming Tips](<../../en/macOS_Programming_Tips.md> "macOS Programming Tips") \- Lazarus tips, useful tools, Unix commands, and more...
  * [WinCE Programming Tips](<../../en/WinCE_Programming_Tips.md> "WinCE Programming Tips") \- Using the telephone API, sending SMSes, and more...
  * [Windows Programming Tips](<../../en/Windows_Programming_Tips.md> "Windows Programming Tips") \- Desktop Windows programming tips



### Interface Development Articles

  * [Carbon interface internals](<../../en/Carbon_interface_internals.md> "Carbon interface internals") \- If you want to help improving the Carbon interface
  * [Windows CE Development Notes](<../../en/Windows_CE_Development_Notes.md> "Windows CE Development Notes") \- For Pocket PC and Smartphones
  * [Adding a new interface](<../../en/Adding_a_new_interface.md> "Adding a new interface") \- How to add a new widget set interface
  * [LCL Defines](<../../en/LCL_Defines.md> "LCL Defines") \- Choosing the right options to recompile LCL
  * [LCL Internals](<../../en/LCL_Internals.md> "LCL Internals") \- Some info about the inner workings of the LCL
  * [Cocoa Internals](<../../en/Cocoa_Internals.md> "Cocoa Internals") \- Some info about the inner workings of the Cocoa widgetset

---

_Source: [https://wiki.freepascal.org/Win32/64_Interface/ru](https://web.archive.org/web/20250115000000/https://wiki.freepascal.org/Win32/64_Interface/ru)_
