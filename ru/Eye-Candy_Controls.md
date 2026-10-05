# Eye-Candy Controls

│ **[English (en)](<../en/Eye-Candy_Controls.md> "Eye-Candy Controls")** │  **[español (es)](</Eye-Candy_Controls/es> "Eye-Candy Controls/es")** │  **русский (ru)** │    
****

**Eye Candy Controls** (сокращенно ECControls или EC-Controls) - набор визуальных элементов управления, написанных для Lazarus. Их дизайн основан на Themes, поэтому они выглядят очень нативными везде, независимо от того, какой виджет вы используете. 

Каждый релиз аннонсируется на форуме Lazarus в разделе Third Party Announcements(Сторонние объявления). 

Всегда есть прикрепленные файлы `README.txt` (список всех известных проблем) и `CHANGELOG.txt` (список всех изменений из предыдущего выпуска). 

Лицензия

GNU Lesser General Public License 2.0 с исключением ссылок (a.k.a. Модифицированный LGPL). Каждый файл EC-Controls содержит заголовок лицензии. Кроме того, файлы `COPYING.modifiedLGPL.txt` и `COPYING.LGPL.txt` связаны с каждым архивом. 

Авторство

Все компоненты написаны [Blaazen](<https://forum.lazarus.freepascal.org/index.php?action=profile;u=39483>). Уведомление об авторском праве и настоящее имя упоминаются в заголовке каждого блока. Вы можете связаться с автором на форуме Lazarus (псевдоним: Blaazen) в любой теме о EC-Controls (email author). Если вы авторизованы на форуме, вы можете получить адрес автора электронной почты или отправить ему личное сообщение. 

Credits: Выравнивание составных компонентов (TECSpinEdit, TECSpinPosition, TECEditBtn, TECColorBtn, TECComboBtn, TECColorCombo) основано на идее Flávio, опубликованной в списке рассылки [[1]](<http://lists.lazarus.freepascal.org/pipermail/lazarus/2013-March/079971.html>)

Класс [TBaseScrollControl](</index.php?title=TBaseScrollControl&action=edit&redlink=1> "TBaseScrollControl \(page does not exist\)") основан на [TScrollingControl](<https://github.com/theo222/lazarus-thumbviewer/blob/master/scrollingcontrol.pas>) от Theo. 

Загрузка

  * Последний релиз: 0.9.44 на 1 июня 2020 (включая demo); UltraShare не работает. Этот выпуск был протестирован с Lazarus 2.1
  * Предыдущий релиз: 0.9.30 на 1 марта 2018 (включая demo); UltraShare не работает. Этот выпуск был протестирован с Lazarus 1.9 и 1.8
  * Предыдущий релиз: 0.9.24.6 на 24 октября 2017 (без demo); TECGrid - _Release candidate_ ; UltraShare не работает.
  * Предыдущий релиз: 0.9.20 на 31 июля 2017 (без demo)
  * Предыдущий релиз: 0.9.6 на 24 мая 2016 (включая demo)
  * SourceForge: <https://sourceforge.net/projects/eccontrols/>
  * UltraShare: <http://ultrashare.net/hosting/fl/8c275ee97f> (прямая ссылка на 0.9.6, выпущенный 24.05.2016; включая demo)
  * UltraShare: <http://ultrashare.net/hosting/fl/a8838060fb> (прямая ссылка на 0.9.4.16, выпущенный 9.04.2016; без demos)
  * UltraShare: <http://ultrashare.net/hosting/fl/f523032cb4> (прямая ссылка на 0.9.4.14, выпущенный 6.04.2016; без demo)



UltraShare - альтернатива, потому что SourceForge заблокирован в некоторых странах. О новых выпусках всегда объявляется в разделе Third Party на форуме Lazarus. 

Установка в Lazarus

Открываем меню `Package => Open package File (*.lpk) ...` и выбираем файл `eccontrols.lpk`. Щелкаем `Compile` (ждем, пока закончится компиляция), затем выбираем `Use >> Install`. Lazarus спросит "Do you want to rebuild Lazarus now?"(Хотите пересобрать Lazarus сейчас?). Жмем `Yes`, чтобы установить пакет. 

## Contents

  * 1 Компоненты
    * 1.1 Визуальные
      * 1.1.1 TECBevel
      * 1.1.2 TECLink
      * 1.1.3 TECImageMenu
      * 1.1.4 TECSpinBtns
      * 1.1.5 TECSpinEdit
      * 1.1.6 TECSwitch
      * 1.1.7 TECSpeedBtn
      * 1.1.8 TECBitBtn
      * 1.1.9 TECEditBtn
      * 1.1.10 TECColorBtn
      * 1.1.11 TECComboBtn
      * 1.1.12 TECColorCombo
      * 1.1.13 TECHeader
      * 1.1.14 TECCheckListBox
      * 1.1.15 TECSlider
      * 1.1.16 TECProgressBar
      * 1.1.17 TECPositionBar
      * 1.1.18 TECSpinPosition
      * 1.1.19 TECRuler
      * 1.1.20 TECRadioGroup
      * 1.1.21 TECCheckGroup
      * 1.1.22 TECTabCtrl
      * 1.1.23 TECAccordion
      * 1.1.24 TECTriangle
      * 1.1.25 TECGrid
      * 1.1.26 TECLightView
      * 1.1.27 TECConfCurve
      * 1.1.28 TECScheme
    * 1.2 Невизуальные компоненты
      * 1.2.1 TECSpinController
      * 1.2.2 TECTimer
    * 1.3 Другие части EC-Controls
      * 1.3.1 Class TECScale
      * 1.3.2 Модуль ECTypes
      * 1.3.3 TBaseScrollControl
  * 2 Демки
    * 2.1 ECC-Demo
    * 2.2 ECScale-Demo
    * 2.3 ECConfCurve-Demo
    * 2.4 ECSchemeDesc
    * 2.5 SchemeDesigner



## Компоненты

EC-Controls устанавливаются на вкладку EC-C палитры компонентов Lazarus. 

[![ecpalette2.png](https://wiki.freepascal.org/images/c/c4/ecpalette2.png)](</File:ecpalette2.png>)

Новые изображения (начиная с версии 0.9.24.6). Файлы ресурсов (* .lrs) также содержат изображения _150 и _200 для настольных компьютеров с высоким разрешением. 

[![eccpalette4.png](https://wiki.freepascal.org/images/b/b7/eccpalette4.png)](</File:eccpalette4.png>)

Компоненты ниже перечислены в порядке их появления на палитре компонентов. Все скриншоты взяты из KDE4 или Plasma5 (Lazarus + Qt). 

### Визуальные

#### TECBevel

Альтернатива [TBevel](<../en/TBevel.md> "TBevel"). 

[![tecbevel.png](https://wiki.freepascal.org/images/d/d5/tecbevel.png)](</File:tecbevel.png>)

[TECBevel](</index.php?title=TECBevel&action=edit&redlink=1> "TECBevel \(page does not exist\)") может рисовать непрямоугольные формы или непрямые линии. 

#### TECLink

[TECLink](</index.php?title=TECLink&action=edit&redlink=1> "TECLink \(page does not exist\)") предоставляет веб-ссылку. Эти же ссылки хорошо известны из веб-браузеров. 

[![teclink.png](https://wiki.freepascal.org/images/2/2d/teclink.png)](</File:teclink.png>)

Метка, которая меняет свой вид при наведении на нее курсора мыши (по умолчанию становится подчеркнутой и синей). 

Она может открывать URL-адрес в браузере по умолчанию, почтовом клиенте по умолчанию, файл в связанном приложении или просто запускать событие OnClick. 

#### TECImageMenu

Вертикальное меню с изображениями. Подобный компонент часто используется в приложениях KDE4 и Outlook97. 

[![tecimagemenu.png](https://wiki.freepascal.org/images/8/81/tecimagemenu.png)](</File:tecimagemenu.png>)

[TECImageMenu](</index.php?title=TECImageMenu&action=edit&redlink=1> "TECImageMenu \(page does not exist\)") может находиться в фокусе, и к нему можно перейти с помощью клавиши Tab (когда `TabStop = True`, что по умолчанию). Может управляться мышью, клавиатурой или кодом. 

Мышь

  * Щелчок левой кнопкой мыши по пункту меню эквивалентен нажатию по нему.
  * Щелчок средней или правой кнопкой мыши выбирает элемент, но не нажимает его.
  * Колесико мыши перемещает выделение и не нажимает его.



Клавиатура 

  * Пробел и Enter: нажимает выбранный пункт меню.
  * Arrow Up, Arrow Down, Page Up, Page Down, Home и End: перемещают выделение, но не нажимают на пункт меню.
  * Быстрые клавиши (Alt + Key) выбирают и нажимают соответствующий пункт (нет необходимости фокусировать меню) ..



#### TECSpinBtns

Продвинутая альтернатива [TUpDown](<../en/TUpDown.md> "TUpDown"). 

[![tecspinbtns.png](https://wiki.freepascal.org/images/3/34/tecspinbtns.png)](</File:tecspinbtns.png>)

[TECSpinBtns](</index.php?title=TECSpinBtns&action=edit&redlink=1> "TECSpinBtns \(page does not exist\)") основан на переменных двойной точности. 

TECSpinBtns не имеет фокуса. Управляется мышью или кодом. 

Мышь

TECSpinBtns состоит из 9 маленьких кнопок. 

  * Щелчок левой кнопкой мыши(ЛКМ) на BtnMin, BtnBigDec, BtnDec, BtnMiddle, BtnMenu, BtnInc, BtnBigInc и BtnMax задает `Value` к `Min`, уменьшает `Value` на величину `PageSize`, уменьшает `Value` на величину `Increment`, задает `Value` к `Middle`, запускает событие `OnMenuClick`, увеличивает `Value` на величину `Increment`, увеличивает `Value` на величину `PageSize` и задает `Value` к `Max` соответственно.
  * Щелчок средней кнопкой мыши(СКМ) задает `Value` к `Middle` или запускает событие `OnMenuClick` \- зависит от свойства `MenuControl`. Для перетаскивания также могут быть использованы другие кнопки мыши, зависит от свойства `DragControl`. Перетаскивание в основном зависит от свойств: `DragOrientation`, `MouseIncrementX`, `MouseIncrementY`, `MouseStepPixelsX`, `MouseStepPixelsY` и `Reversed`.



Программно

Просто присвоив любое значение с плавающей точкой свойству `Value`: 
    
    
    Value := 10.5;
    

Если значение выходит за пределы диапазона (меньше `Min` или больше `Max`), тогда `Value` будет равно `Min` или `Max`. 

Приоритет отрисовки: 

  1. Наивысший приоритет имеет событие `OnDrawGlyph`.
  2. Второй - `Caption`. Он должен быть коротким (один или два символа).
  3. Третий - изображение из `Images`. Изображения должны быть присвоены, а `ImageIndex` должен быть больше или равен нулю, и меньше, чем `Images.Count`
  4. Если `OnDrawGlyph` не назначен, `Caption` является пустой строкой, а `ImageIndex` равен -1, тогда используется встроенный глиф. Есть пять наборов стилей, их можно выбрать с помощью свойства `GlyphStyle`.



#### TECSpinEdit

Продвинутая альтернатива TSpinEdit и TFloatSpinEdit. Это [TEdit](<../en/TEdit.md> "TEdit"), соединенный с TECSpinBtns. 

[![tecspinedit.png](https://wiki.freepascal.org/images/4/40/tecspinedit.png)](</File:tecspinedit.png>)

[TECSpinEdit](</index.php?title=TECSpinEdit&action=edit&redlink=1> "TECSpinEdit \(page does not exist\)") может иметь фокус, и к нему можно перейти с помощью клавиши Tab (когда `TabStop = True`, что по умолчанию). Может управляться мышью, клавиатурой или кодом. 

Мышь

См. [TECSpinBtns](<Eye-Candy_Controls.md> "Eye-Candy Controls/ru"). 

Клавиатура

(строка редактирования должна быть в фокусе) 

Текстовое значение можно ввести непосредственно в строку редактирования. Если введенное значение меньше `TECSpinBtnsPlus.MinInEdit` или больше `TECSpinBtnsPlus.MaxInEdit`, тогда значение будет изменено, чтобы соответствовать этим границам. Изменение значения выполняется в событии `OnEditingDone`. 

  * Arrow Up/Down "щелкают" по BtnInc/BtnDec*.
  * PgUp/PgDn "щелкают" по BtnBigInc/BtnBigDec*.
  * Ctrl + Home/End "щелкают" по BtnMax/BtnMin*.
  * Ctrl + Space "щелкают" по BtnMiddle.
  * Ctrl + Enter "щелкают" по BtnMenu.



*) справедливо для `Reversed = False`. `Reversed = True` делает наоборот. 

Программно

Простое присвоение любого значения с плавающей точкой свойству `Value`: 
    
    
    Value := 10.5;
    

Если значение выходит за пределы диапазона (меньше `Min` или больше `Max`), тогда `Value` будет соответственно `Min` или `Max`. 

#### TECSwitch

Альтернатива TCheckBox. Аналогичный компонент существует в GTk3. 

[![tecswitch.png](https://wiki.freepascal.org/images/1/19/tecswitch.png)](</File:tecswitch.png>)

[TECSwitch](</index.php?title=TECSwitch&action=edit&redlink=1> "TECSwitch \(page does not exist\)") может иметь фокус, и к нему можно перейти с помощью клавиши Tab (когда `TabStop = True`, что по умолчанию). Может управляться мышью, клавиатурой или кодом. 

Мышь

  * Щелчок левой кнопкой мыши(ЛКМ) на области переключателя (вне кнопки) изменит `State`*.
  * Щелчок левой кнопкой мыши(ЛКМ) и удержание ее нажатой на кнопке захватит курсор мыши, и кнопку можно будет перемещать, даже если курсор мыши покинет область переключателя.



Клавиатура

  * Пробел или Enter изменяют `State`* (только если переключатель в фокусе).
  * Быстрые клавиши (Alt + Key) изменяют `State`* (нет необходимости перемещать фокус на переключатель).



Программно

Простое присвоение любого значения свойствам `State`* или `Checked`: 
    
    
    Checked := True; //False
    State := cbChecked; //cbGrayed, cbUnchecked
    

*) Свойство `State` всегда изменяет значение от `checked` к `unchecked`, от `grayed` к `unchecked` или от `unchecked` к `checked`. 

#### TECSpeedBtn

Кнопка с некоторыми расширенными функциями и встроенными глифами. Альтернатива TSpeedButton и TToggleBox. 

[![tecspeedbtn.png](https://wiki.freepascal.org/images/c/c3/tecspeedbtn.png)](</File:tecspeedbtn.png>)

[TECSpeedBtn](</index.php?title=TECSpeedBtn&action=edit&redlink=1> "TECSpeedBtn \(page does not exist\)") не имеет фокуса. 

Особенности и отличия от [TSpeedButton](<../en/TSpeedButton.md> "TSpeedButton"): 

  * TSpeedButton имеет свойство `Glyph: TBitmap`. TECSpeedBtn вместо этого имеет свойства `ImageIndex: Integer` и `Images: [TImageList](<../en/TImageList.md> "TImageList")`.
  * TECSpeedBtn имеет свойство `Delay` и встроенный таймер. Поэтому она может работать как кнопка задержки (Delay>0) или как переключатель (Delay<0).
  * TECSpeedBtn имеет более 80 встроенных глифов (нарисованных с помощью помощника [TCanvas](<../en/TCanvas.md> "TCanvas")). Глифы могут быть разными для состояния checked и unchecked.
  * Подобно TSpeedButton, TECSpeedBtn имеет свойства `GroupIndex`, `Checked` и `AllowAllUp`, поэтому кнопки можно сгруппировать в радиогруппу.
  * TECSpeedBtn не может получать фокус, но может быть нажата быстрой клавишей (Alt + [подчеркнутая клавиша]).
  * TECSpeedBtn также может быть связана с [TAction](<../en/TAction.md> "TAction").



Приоритеты отрисовки: 

  1. Наивысший приоритет имеет событие `OnDrawGlyph`.
  2. Второй - `Caption` и изображение из `Images`. Изображения должны быть присвоены, и хотя бы одно из `ImageIndex` и `ImageIndexChecked` должно быть больше или равно нулю и меньше, чем `Images.Count`.
  3. Когда событие `OnDrawGlyph` не назначено и оба свойства `ImageIndex` и `ImageIndexChecked` имеют значение -1, то используется встроенный глиф (свойства `GlyphDesign` и `GlyphDesignChecked`). Если `ImageIndex` корректен, `Image` и `ImageIndexChecked` имеет значение -1 или только `GlyphDesign` является некоторым глифом, а `GlyphDesignChecked` имеет значение `egdNone`, то `ImageIndex` или `GlyphDesign` также используются для состояния проверки (и наоборот).



#### TECBitBtn

То же, что и TECSpeedBtn, но производное от TCustomControl, поэтому может иметь фокус. Альтернатива TBitBtn и TToggleBox. 

[![tecbitbtn.png](https://wiki.freepascal.org/images/b/b4/tecbitbtn.png)](</File:tecbitbtn.png>)

#### TECEditBtn

Альтернатива [TEditButton](</index.php?title=TEditButton&action=edit&redlink=1> "TEditButton \(page does not exist\)"). Это [TEdit](<../en/TEdit.md> "TEdit"), объединенное с [TECSpeedBtn](</index.php?title=TECSpeedBtn&action=edit&redlink=1> "TECSpeedBtn \(page does not exist\)"). 

[![teceditbtn.png](https://wiki.freepascal.org/images/7/79/teceditbtn.png)](</File:teceditbtn.png>)

#### TECColorBtn

Визуальный компонент для выбора цвета. При редактировании строки отображается цветовой код в различных форматах, а соответствующая кнопка запускает диалог выбора цвета. 

[![teccolorbtn.png](https://wiki.freepascal.org/images/c/cb/teccolorbtn.png)](</File:teccolorbtn.png>)

Цвет глифа на кнопке соответствует цвету в строке редактирования. 

Свойство `Text` не является публикуемым ([прим.перев](</User:Zoltanleo> "User:Zoltanleo"): не видно в редакторе свойств). Если текст изменен с помощью кода, необходимо вызвать `EditingDone` для подтверждения изменения. 

#### TECComboBtn

Поле со списком совмещенный с кнопкой. Это [TComboBox](<../en/TComboBox.md> "TComboBox"), соединенный с [TECSpeedBtn](</index.php?title=TECSpeedBtn&action=edit&redlink=1> "TECSpeedBtn \(page does not exist\)"). 

[![teccombobtn.png](https://wiki.freepascal.org/images/2/23/teccombobtn.png)](</File:teccombobtn.png>)

#### TECColorCombo

Визуальный компонент для выбора цвета. Поле со списком содержит цвета в различных форматах, а соответствующая кнопка вызывает диалоговое окно цвета. 

[![teccolorcombo.png](https://wiki.freepascal.org/images/1/11/teccolorcombo.png)](</File:teccolorcombo.png>)

Цвет глифа на кнопке соответствует цвету, выбранному в поле со списком. 

Свойство `Text` не является публикуемым ([прим.перев.](</User:Zoltanleo> "User:Zoltanleo"): не видно в редакторе свойств). Если текст изменяется с помощью кода, необходимо вызвать `Validate` для подтверждения изменения. 

#### TECHeader

Альтернатива [THeader](</index.php?title=THeader&action=edit&redlink=1> "THeader \(page does not exist\)"). 

[![techeader.png](https://wiki.freepascal.org/images/8/8b/techeader.png)](</File:techeader.png>)

Основная особенность - возможность выравнивания столбцов по левому и правому краю одновременно. Этот компонент разработан для TECCheckListBox. 

#### TECCheckListBox

Альтернатива [TCheckListBox](<../en/TCheckListBox.md> "TCheckListBox"). 

[![tecchecklistbox.png](https://wiki.freepascal.org/images/5/56/tecchecklistbox.png)](</File:tecchecklistbox.png>)

Может иметь несколько столбцов с возможностью их пометки. 

В настоящее время свойство `Sorted` не поддерживается. 

#### TECSlider

Продвинутая альтернатива [TTrackBar](<../en/TTrackBar.md> "TTrackBar"). 

[![tecslider.png](https://wiki.freepascal.org/images/d/d4/tecslider.png)](</File:tecslider.png>)

TECSlider может иметь фокус, и к нему можно перейти с помощью клавиши Tab (когда `TabStop = True`, что по умолчанию). 

TECSlider основан на переменных с двойной точностью. TECSlider можно управлять с помощью мыши, клавиатуры или кода. 

Мышь

  * Щелчок левой кнопкой мыши (ЛКМ) на области Slider (вне ползунка) переместит ползунок на величину `PageSize` (или меньше, если курсор мыши ближе).
  * Двойной щелчок ЛКМ или щелчок средней кнопкой (колесиком) мыши сразу переместит ползунок в позицию курсора мыши (или к Min/Max значению, если щелчок осуществляется за пределами области канавки и шкалы).
  * Щелчок ЛКМ на ползунке и удержание ее нажатой включит режим удержания мышью, что позволит перемещать ползунок, даже если курсор мыши покинет пределы слайдера.
  * Колесико мыши перемещает ползунок вверх/вниз независимо от свойства `Reversed`. В случае горизонтального расположения слайдера прокручивание ползунка вверх/вниз будет равносильно перемещению влево/вправо, независимо от свойства `Reversed`.



Приращение составляет: 

  * колесико мыши: `Increment`*`Mouse.WheelScrollLines`
  * Ctrl + колесико мыши: `Increment`.



Клавиатура

  * Пробел: перемещает ползунок в середину канавки слайдера или в `ProgressMiddlePos` в случае `ProgressFromMiddle = True`
  * 0-9: перемещает ползунок в положение, которое является целочисленным множителем `PageSize` (т.е. 0, 10, ..., 90 для `PageSize = 10`).
  * PgUp: уменьшает `Position` на величину `PageSize`
  * PgUp: увеличивает `Position` на величину `PageSize`
  * Home: перемещает ползунок к `Min`
  * End: перемещает ползунок к `Max`
  * +: увеличивает `Position` на величину `Increment`
  * -: уменьшает `Position` на величину `Increment`
  * Ctrl + ArrowUp: уменьшает* `Position` на величину `Increment`
  * Ctrl + ArrowLeft: уменьшает* `Position` на величину `Increment`
  * Ctrl + ArrowDown: увеличивает* `Position` на величину `Increment`
  * Ctrl + ArrowRight: увеличивает* `Position` на величину `Increment`



*) справедливо для `Reversed = False`. Если `Reversed = True`, работает наоборот. 

Программно

Это можно сделать, просто присвоив свойству `Position` любое значение с плавающей точкой: 
    
    
    Position := 10.5; 
    

Если значение выходит за пределы допустимого диапазона (меньше `Min` или больше `Max`), тогда `Position` будет `Min` или `Max` соответственно. 

#### TECProgressBar

Продвинутая альтернатива [TProgressBar](<../en/TProgressBar.md> "TProgressBar"). 

[![tecprogressbar.png](https://wiki.freepascal.org/images/c/c3/tecprogressbar.png)](</File:tecprogressbar.png>)

TECProgressBar основан на переменных с двойной точностью. TECProgressBar не может иметь фокус. Управлять им можно только с помощью кода. 

#### TECPositionBar

Альтернатива [TTrackBar](<../en/TTrackBar.md> "TTrackBar") или [TScrollBar](<../en/TScrollBar.md> "TScrollBar"). Подобные компоненты используются в [Blender](<https://www.blender.org>) (программа для 3D-графики). 

[![tecpositionbar.png](https://wiki.freepascal.org/images/0/05/tecpositionbar.png)](</File:tecpositionbar.png>)

[TECPositionBar](</index.php?title=TECPositionBar&action=edit&redlink=1> "TECPositionBar \(page does not exist\)") не может иметь фокус и базируется на переменных с двойной точностью. TECPositionBar можно контролировать с помощью мыши или кода. 

Мышь

  * Щелчок левой кнопкой мыши(ЛКМ) сразу же устанавливает позицию в положение курсора мыши (или в положение Min/Max, если щелчок произведен вне области канавки или шкалы).
  * Щелчок средней кнопкой мыши(СКМ): перемещает позицию полоски прогрессбара в середину канавки или в позицию `ProgressMiddlePos` в случае `ProgressFromMiddle = True`
  * Щелчок левой кнопкой мыши(ЛКМ) в конце шкалы и удержание ЛКМ нажатой захватывает курсор мыши, что позволяет изменять позицию шкалы даже если курсор мыши покидает область компонента.
  * Перетаскивание позиции полоски прогрессбара мышью осуществляется на величину `MouseDragPixels` (нажатие только ЛКМ) или на величину `MouseDragPixelsFine` (Ctrl + ЛКМ). По умолчанию это значения 1 и 10, т.е. позиция полоски прогрессбара изменится на 1 пиксел, когда курсор мыши переместится на 1 пиксел (или на 10 пикселей в случае перетаскивания с нажатой клавишей Ctrl).
  * Колесико мыши перемещает ползунок вверх/вниз независимо от свойства `Reversed`. В случае горизонтального расположения слайдера прокрутка вверх/вниз перемещает ползунок влуво/вправо, опять же, независимо от свойства `Reversed`.



Приращение составляет: 

  * Колесико мыши: `MouseDragPixels`*`Mouse.WheelScrollLines`
  * Ctrl + колесико мыши: (`MouseDragPixels`/`MouseDragPixelsFine`)*`Mouse.WheelScrollLines`



Программно

Это можно сделать, просто присвоив любое значение с плавающей токой свойству `PositionSimply`: 
    
    
    Position := 10.5; 
    

Если значение выходит за пределы допустимого диапазона (меньше `Min` или больше `Max`), тогда `Position` будет `Min` или `Max` соответственно. 

#### TECSpinPosition

Альтернатива [TTrackBar](<../en/TTrackBar.md> "TTrackBar") или [TScrollBar](<../en/TScrollBar.md> "TScrollBar"). Подобные компоненты используются в [Krita](<https://krita.org/en>) (программное обеспечение для 2D-графики). 

[![tecspinposition.png](https://wiki.freepascal.org/images/a/ad/tecspinposition.png)](</File:tecspinposition.png>)

#### TECRuler

Продвинутая линейка. 

[![tecruler.png](https://wiki.freepascal.org/images/6/67/tecruler.png)](</File:tecruler.png>)

[TECRuler](</index.php?title=TECRuler&action=edit&redlink=1> "TECRuler \(page does not exist\)") не меет фокуса. Она просто отображает масштаб. По желанию может иметь фиксированный или подвижный указатель. 

#### TECRadioGroup

Альтернатива TRadioGroup. 

[![tecradiogroup.png](https://wiki.freepascal.org/images/e/e1/tecradiogroup.png)](</File:tecradiogroup.png>)

[TECRadioGroup](</index.php?title=TECRadioGroup&action=edit&redlink=1> "TECRadioGroup \(page does not exist\)") может иметь фокус,и к ней можно перейти с помощью клавиши Tab (когда `TabStop = True`, что не по умолчанию). Может управляться мышью, клавиатурой или кодом. 

Мышь

  * Щелчок левой кнопкой мыши(ЛКМ) по любому пункту (вне кнопки) изменяет ее свойство `Checked` на `True` (или на `False`, если этот пункт уже имеет значение `checked`*).
  * Щелчок ЛКМ на TECRadioGroup вне пунктов приводит только к получению компонентом фокуса.



Клавиатура

  * 0: снимает выбор всех пунктов*
  * 1-9: выбирает (или снимает выбор*) пункты 1-9
  * Быстрые клавиши (Alt + Key) изменяют свойство `Checked` на `True` или `False`* (radio group не может иметь фокус).



*) Зависит от того, находится ли `egoAllowAllUp` в `Options`. 

Клавиатура

Это можно сделать, просто определив любой `ItemIndex` или свойство `Items[].Checked` : 
    
    
    ItemIndex := 1; //отмечает второй пункт
    Items[1].Checked := False; //снимает выбор со второго пункта, независимо от egoAllowAllUp в Options
    

#### TECCheckGroup

Альтернатива TCheckGroup. 

[![teccheckgroup.png](https://wiki.freepascal.org/images/f/f6/teccheckgroup.png)](</File:teccheckgroup.png>)

TECCheckGroup может находиться в фокусе, и к ней можно перейти с помощью клавиши Tab (когда `TabStop = True`, что не по умолчанию). Может управляться мышью, клавиатурой или кодом. 

Мышь

  * Щелчок левой кнопкой мыши(ЛКМ) по любому из пунктов (вне кнопки) меняет его свойство `Checked` на `True` (или на `False`, если этот пункт уже был помечен*).
  * Щелчок ЛКМ на TECCheckGroup вне пунктов только переводит фокус на компонент.



Клавиатура

  * 0: снять пометки со всех пунктов*
  * 1-9: пометить (ил снять пометки*) пунктов 1-9
  * Быстрые клавиши (Alt + Key) меняют свойство `Checked` на `True`/`False`* (check group не может иметь фокус).



*) Зависит от того, находится ли `egoAllowAllUp` в `Options`. 

Программно

Simply by assigning any Checked[] or Items[].Checked property: Это можно сделать, просто присвоив какое-либо значение свойству `Checked[]` или `Items[].Checked`: 
    
    
    Checked[1] := True; //пометит второй пункт
    Items[1].Checked := False; //снимет пометку со второго пункта, независимого от того, находится ли egoAllowAllUp в Options.
    

#### TECTabCtrl

См: [TECTabCtrl](<../en/TECTabCtrl.md> "TECTabCtrl")

#### TECAccordion

[TECAccordion](</index.php?title=TECAccordion&action=edit&redlink=1> "TECAccordion \(page does not exist\)") это боковое меню, работает аналогично [TPageControl](<../en/TPageControl.md> "TPageControl"). 

[![tecaccordion.png](https://wiki.freepascal.org/images/e/e1/tecaccordion.png)](</File:tecaccordion.png>)

TECAccordion может иметь фокус и доступно с помощью клавиши Tab (когда `TabStop = True`, что не по умолчанию). Может управляться мышью, клавиатурой или кодом. 

Мышь

Щелчок левой кнопкой мыши(ЛКМ) по любому из пунктов разворачивает/сворачивает его. 

Клавиатура

Быстрые клавиши (Alt + Key) меняют пункт (пункт не может иметь фокус). 

Программно

Можно просто менять свойство `ItemIndex`. 

#### TECTriangle

Баланс трех значений. Это не палитра цветов! 

[![tectriangle.png](https://wiki.freepascal.org/images/9/9f/tectriangle.png)](</File:tectriangle.png>)

[TECTriangle](</index.php?title=TECTriangle&action=edit&redlink=1> "TECTriangle \(page does not exist\)") не может находиться в фокусе и не может быть найден клавишей Tab. Управляется только мышью. 

Мышь

  * Щелчок левой кнопкой мыши(ЛКМ) на области треугольника.
  * Щелчок ЛКМ и удержание метки позволяет перемещать ее.
  * Щелчок ЛКМ по метке устанавливает более точное значение, например [0,333..., 0.333..., 0.333...].
  * Колесико мыши позволяет катать метку вверх и вниз.



Программно

Это можно сделать путем вызова перегруженного метода `SetValues`. Параметры могут быть 1) координатами X и Y или 2) значениями `Top` и `Bottom` (третье значение `Side` является вычисляемой, поэтому сумма значений всегда равна 1). 

#### TECGrid

См.: [TECGrid](<../en/TECGrid.md> "TECGrid")

#### TECLightView

См.: [TECLightView](<TECLightView.md> "TECLightView/ru")

#### TECConfCurve

Компонент для настройки. 

[![tecconfcurve.png](https://wiki.freepascal.org/images/9/91/tecconfcurve.png)](</File:tecconfcurve.png>)

Пользователь может настроить полилинию или кривую Безье с несколькими точками. 

Можно выравнивать с помощью вертикальной и/или горизонтальной линейки (TECRuler). 

См. ECConfCurve-Demo. 

#### TECScheme

Расширенный компонент для настройки общей схемы. 

[![tecscheme.png](https://wiki.freepascal.org/images/3/36/tecscheme.png)](</File:tecscheme.png>)

Пользователь может добавить несколько блоков и соединить их. 

Этот компонент легко настраивается. См. SchemeDesigner и ECSchemeDesc в комплекте с EC-Controls. 

### Невизуальные компоненты

#### TECSpinController

Предназначен для управления свойствами нескольких TECSpinBtns и TECSpinEdit. 

[![tecspincontrollericon.png](https://wiki.freepascal.org/images/f/f2/tecspincontrollericon.png)](</File:tecspincontrollericon.png>)

[TECSpinBtns](</index.php?title=TECSpinBtns&action=edit&redlink=1> "TECSpinBtns \(page does not exist\)") и [TECSpinEdit](</index.php?title=TECSpinEdit&action=edit&redlink=1> "TECSpinEdit \(page does not exist\)") имеют свойства `Controller`. Когда этому свойству назначен какой-либо `SpinController`, этот контроллер может одновременно изменять выбранные свойства всех присвоенных TECSpinEdits и TECSpinBtns. Настраиваемые свойства - например, `TimerDelay`, `TimerRepeat`, ширина отдельных кнопок, `GlyphsStyle` и другие. 

В проекте может быть несколько SpinController. 

#### TECTimer

Продвинутый таймер. 

[![tectimericon.png](https://wiki.freepascal.org/images/d/d8/tectimericon.png)](</File:tectimericon.png>)

Основная особенность в том, что первый интервал (свойство `Delay`) может отличаться от всех последующих интервалов (свойство `Repeat`). 

### Другие части EC-Controls

#### Class TECScale

Масштаб. Это не компонент, но он может рисоваться на любом холсте. 

Этот класс используется в TECRuler, TECSlider, TECProgressBar, TECPositionBar и TECSpinPosition. 

См. демку ECScale-Demo. 

#### Модуль ECTypes

Базовые типы и классы для элементов управления Eye Candy Controls (EC-C). 

Если вы используете EC-Controls в своем проекте, вам может потребоваться добавить этот модуль в раздел **uses**. 

Этот модуль содержит множество объявлений, подпрограмм преобразования цвета и базовых классов, например TBaseScrollControl. 

#### TBaseScrollControl

Является базовым классом для прокручиваемых компонентов (TECScheme является его потомком). 

Вы можете получить свой собственный компонент прокрутки из TBaseScrollControl. На этом же классе основаны [TECScheme](</index.php?title=TECScheme&action=edit&redlink=1> "TECScheme \(page does not exist\)") и [TECGrid](<../en/TECGrid.md> "TECGrid"). 

## Демки

EC-Controls поставляется с несколькими демками. Если некоторые из следующих демоверсий отсутствуют в архиве, это может означать, что изменений не было и демоверсия не была включена. В этом случае используйте демки из предыдущего выпуска. 

#### ECC-Demo

В этой демке показаны все элементы управления EC-Control в различных конфигурациях. 

#### ECScale-Demo

Эта демка показывает, как вы можете использовать [TECScale](</index.php?title=TECScale&action=edit&redlink=1> "TECScale \(page does not exist\)") в вашем собственном визуальном компоненте. 

#### ECConfCurve-Demo

Эта демка показывает возможности [TECConfCurve](</index.php?title=TECConfCurve&action=edit&redlink=1> "TECConfCurve \(page does not exist\)"). 

#### ECSchemeDesc

В этой демке показано, как создать компонент-потомок из [TCustomECScheme](</index.php?title=TCustomECScheme&action=edit&redlink=1> "TCustomECScheme \(page does not exist\)"). 

#### SchemeDesigner

SchemeDesigner - это больше, чем демо. Он показывает вам, как вы можете создать конфигуратор TECScheme в вашем собственном приложении, но его также можно использовать для создания ваших собственных конфигураций и сохранения их в формате *.xml.

---

_Source: [https://wiki.freepascal.org/Eye-Candy_Controls/ru](https://web.archive.org/web/20250418105137/https://wiki.freepascal.org/Eye-Candy_Controls/ru)_
