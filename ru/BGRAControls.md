# BGRAControls

│ **[English (en)](<../en/BGRAControls.md>)** │  **русский (ru)** │

[![bgracontrols.png](https://wiki.freepascal.org/images/6/6e/bgracontrols.png)](</File:bgracontrols.png>)

## Contents

  * 1 Установка
    * 1.1 Дополнительные компоненты
    * 1.2 Веб-сайт
  * 2 Элементы управления BGRA
    * 2.1 TBCButton
    * 2.2 TBCButtonFocus
    * 2.3 TBCGameGrid
    * 2.4 TBCImageButton
    * 2.5 TBCXButton
    * 2.6 TBCLabel
    * 2.7 TBCMaterialDesignButton
    * 2.8 TBCPanel
    * 2.9 TBCRadialProgressBar
    * 2.10 TBCToolBar
    * 2.11 TBCTrackBarUpdown
    * 2.12 TBGRAFlashProgressBar
    * 2.13 TBGRAGraphicControl
    * 2.14 TBGRAImageList
    * 2.15 TBGRAImageManipulation
    * 2.16 TBGRAKnob
    * 2.17 TBGRAResizeSpeedButton
    * 2.18 TBGRAShape
    * 2.19 TBGRASpeedButton
    * 2.20 TBGRASpriteAnimation
    * 2.21 TBGRAVirtualScreen
    * 2.22 TDTAnalogClock
    * 2.23 TDTAnalogGaugue
    * 2.24 TDTThemedClock
    * 2.25 TDTThemedGauge
    * 2.26 TPSImport_BGRAPascalScript
  * 3 BGRA Custom Drawn
    * 3.1 TBCDButton
    * 3.2 TBCDEdit
    * 3.3 TBCDStaticText
    * 3.4 TBCDProgressBar
    * 3.5 TBCDSpinEdit
    * 3.6 TBCDCheckBox
    * 3.7 TBCRadioButton
  * 4 Образцы кода
    * 4.1 Библиотека паскалевского скрипта
    * 4.2 Пользовательские BGRA Ribbon компоненты
    * 4.3 [Каталог] Tests
    * 4.4 [Каталог] Tests Extra
  * 5 Другие модули
    * 5.1 BCEffect
    * 5.2 BCFilters
    * 5.3 BGRAScript
  * 6 Статьи по теме



# Установка

Используйте [Online Package Manager](<../en/Online_Package_Manager.md> "Online Package Manager") для получения BGRABitmap и BGRAControls. 

Обратите внимание, что вы должны проверять только пакеты "bgrabitmappack.lpk" и "bgracontrols.lpk" в онлайн-менеджере пакетов. Другие пакеты являются необязательными и могут потребоваться сторонние пакеты / библиотеки для работы (OpenGL и PascalScript). 

## Дополнительные компоненты

Начиная с версии 4.4, компоненты TBCDefaultThemeManager, TBCKeyboard и TBCNumericKeyboard не устанавливаются по умолчанию, чтобы позволяет пользователям Linux получить беспроблемную установку с помощью Online Package Manager без установки сторонних компонентов. Если вы хотите, чтобы эти компоненты [установлены], подключите [их] в "Register unit" в опциях пакета для каждого файла (bcdefaulthememanager.pas, bckeyboard.pas, bcnumerickeyboard.pas), затем скомпилируйте и пересоберите Lazarus. В Linux вам нужно сначала установить [пакеты] libxtst-dev и libgl-dev. 

## Веб-сайт

BGRABitmap Organization on GitHub: <https://github.com/bgrabitmap/>

# Элементы управления BGRA

Элементы управления BGRA - это набор графических элементов пользовательского интерфейса, которые можно использовать с приложениями Lazarus LCL. Под Linux вам нужно установить [пакеты] libxtst-dev и libgl-dev. 

### TBCButton

[![bcbutton.png](https://wiki.freepascal.org/images/6/63/bcbutton.png)](</File:bcbutton.png>)

Элемент управления - кнопка, который можно стилизовать через свойства для каждого состояния, например StateClicked, StateHover, StateNormal, с такими настройками, как градиенты, границы и текст с тенями. Вы можете назначить уже созданный стиль через свойство AssignStyle. 

### TBCButtonFocus

[![TBCButtonFocus.png](https://wiki.freepascal.org/images/5/5b/TBCButtonFocus.png)](</File:TBCButtonFocus.png>)

Аналогичен TBCButton, но поддерживает фокусировку как обычный TButton. 

### TBCGameGrid

[![bcgamegrid.png](https://wiki.freepascal.org/images/7/7c/bcgamegrid.png)](</File:bcgamegrid.png>)

Сетка с пользовательской шириной и высотой элементов и любым количеством горизонтальных и вертикальных ячеек, которые можно нарисовать с помощью BGRABitmap непосредственно с событием OnRenderControl. 

### TBCImageButton

  * [![samplebgraimagebutton.png](https://wiki.freepascal.org/images/5/56/samplebgraimagebutton.png)](</File:samplebgraimagebutton.png>)


  * [![samplebgraimagebuttonalpha.png](https://wiki.freepascal.org/images/d/d9/samplebgraimagebuttonalpha.png)](</File:samplebgraimagebuttonalpha.png>)



Элемент управления - кнопка, который можно стилизовать с помощью одного файла изображения, содержащего рисунок для каждого состояния Normal(«Обычный»), Hovered(«Наведенный»), Active(«Активный») и Disabled(«Отключенный»). Он поддерживает функцию 9-фрагментного масштабирования. Он поддерживает приятную анимацию затухания, которую можно включить. 

### TBCXButton

[![bcxbutton.png](https://wiki.freepascal.org/images/7/7e/bcxbutton.png)](</File:bcxbutton.png>)

Элемент управления - кнопка, который может быть стилизован [при помощи кода] в событии OnRenderControl. Или даже лучше создать свой собственный дочерний элемент управления, наследующийся от этого класса. 

### TBCLabel

[![bclabel.png](https://wiki.freepascal.org/images/7/79/bclabel.png)](</File:bclabel.png>)

Элемент управления - label, у который можно [настраивать] стиль через свойства, он поддерживает тени, настраиваемые границы и фон. 

### TBCMaterialDesignButton

[![TBCMaterialDesignButton 01.png](https://wiki.freepascal.org/images/0/0b/TBCMaterialDesignButton_01.png)](</File:TBCMaterialDesignButton_01.png>) [![TBCMaterialDesignButton 02.png](https://wiki.freepascal.org/images/2/2a/TBCMaterialDesignButton_02.png)](</File:TBCMaterialDesignButton_02.png>) [![TBCMaterialDesignButton 03.gif](https://wiki.freepascal.org/images/9/9e/TBCMaterialDesignButton_03.gif)](</File:TBCMaterialDesignButton_03.gif>)

Элемент управления - кнопка - с эффектом анимации в соответствии с рекомендациями Google Material Design. Он поддерживает пользовательский цвет для фона и для зацикленной анимации, также вы можете настраивать тень. 

### TBCPanel

[![bcpanel.png](https://wiki.freepascal.org/images/3/3f/bcpanel.png)](</File:bcpanel.png>)

Элемент управления - панель, у который можно [настраивать] стиль через свойства. Вы можете назначить уже созданный стиль через свойство AssignStyle. 

### TBCRadialProgressBar

[![TBCRadialProgressBar.png](https://wiki.freepascal.org/images/5/57/TBCRadialProgressBar.png)](</File:TBCRadialProgressBar.png>)

Индикатор выполнения с радиальным стилем. Вы можете установить цвет и свойства текста, как вам нравится. 

### TBCToolBar

TToolBar с событием OnRedraw, чтобы нарисовать его, используя BGRABitmap. Он также поддерживает OnPaintButton по умолчанию для настройки рисования кнопок. По умолчанию это стиль панели инструментов проводника, похожий на Windows 7. 

### TBCTrackBarUpdown

[![TBCTrackBarUpdown.png](https://wiki.freepascal.org/images/8/8f/TBCTrackBarUpdown.png)](</File:TBCTrackBarUpdown.png>)

Элемент управления для ввода числовых значений, работает как трекбар и spinedit в одном элементе управления. 

### TBGRAFlashProgressBar

[![BC-Bgraflashprogressbar.png](https://wiki.freepascal.org/images/0/0b/BC-Bgraflashprogressbar.png)](</File:BC-Bgraflashprogressbar.png>)

Индикатор выполнения со стилем по умолчанию в духе старого стиля диалогового окна прогресс-бара Flash Player Setup для Windows. Вы можете изменить свойство color, чтобы оно имело разные стили, а также использовать событие OnRedraw, чтобы рисовать на нем собственные стили, такие как текст, или переопределять весь рисунок по умолчанию. 

### TBGRAGraphicControl

Подобен компоненту paintbox. С помощью этого элемента управления вы можете рисовать с прозрачностью, используя событие OnRedraw. 

### TBGRAImageList

[![after-TBGRAImageList.png](https://wiki.freepascal.org/images/3/3b/after-TBGRAImageList.png)](</File:after-TBGRAImageList.png>)

Список изображений, который поддерживает альфа[-канал] на всех поддерживаемых платформах. 

### TBGRAImageManipulation

[![bgraimagemanipulation.jpg](https://wiki.freepascal.org/images/6/6a/bgraimagemanipulation.jpg)](</File:bgraimagemanipulation.jpg>)

Инструмент для манипуляциями с изображениями, посмотрите демонстрацию, которая показывает все возможности, которые идут с ним. 

### TBGRAKnob

[![BC-Bgraknob.png](https://wiki.freepascal.org/images/3/39/BC-Bgraknob.png)](</File:BC-Bgraknob.png>)

Рукоятка настройки, которая может быть стилизована через свойства. 

### TBGRAResizeSpeedButton

[![TBGRAResizeSpeedButton.png](https://wiki.freepascal.org/images/2/29/TBGRAResizeSpeedButton.png)](</File:TBGRAResizeSpeedButton.png>)

Speed button, которая может изменить размер глифа, чтобы он вписался [полностью] в элемент управления. 

### TBGRAShape

[![samplebgrashape.png](https://wiki.freepascal.org/images/b/b8/samplebgrashape.png)](</File:samplebgrashape.png>)

Элемент управления с настраиваемыми формами, такими как многоугольник и эллипс, которые могут быть заполнены градиентами и могут иметь пользовательские границы и многие другие визуальные параметры. 

### TBGRASpeedButton

[![BGRASpeedButton.png](https://wiki.freepascal.org/images/e/e9/BGRASpeedButton.png)](</File:BGRASpeedButton.png>)

Speed button, которая в GTK и GTK2 обеспечивает прозрачность на основе BGRABitmap для глифа. 

### TBGRASpriteAnimation

[![bgraspriteanimation.png](https://wiki.freepascal.org/images/e/ee/bgraspriteanimation.png)](</File:bgraspriteanimation.png>)

Компонент, который можно использовать как средство просмотра изображений или средство просмотра анимации, поддерживает загрузку файлов GIF. 

### TBGRAVirtualScreen

[![TBGRAVirtualScreen.gif](https://wiki.freepascal.org/images/e/e3/TBGRAVirtualScreen.gif)](</File:TBGRAVirtualScreen.gif>)

Это как панель. Вы можете нарисовать этот элемент управления, используя событие OnRedraw. 

### TDTAnalogClock

[![TDTThemedClock.gif](https://wiki.freepascal.org/images/5/59/TDTThemedClock.gif)](</File:TDTThemedClock.gif>)

Часы. 

### TDTAnalogGaugue

[![TDTAnalogGaugue.gif](https://wiki.freepascal.org/images/3/33/TDTAnalogGaugue.gif)](</File:TDTAnalogGaugue.gif>)

Датчик. 

### TDTThemedClock

[![TDTAnalogClock.gif](https://wiki.freepascal.org/images/3/3d/TDTAnalogClock.gif)](</File:TDTAnalogClock.gif>)

Еще одни часы. 

### TDTThemedGauge

Еще один датчик. 

### TPSImport_BGRAPascalScript

Компонент для загрузки утилит паскалевского скрипта BGRABitmap. 

# BGRA Custom Drawn

[![TBCDButton.png](https://wiki.freepascal.org/images/c/c7/TBCDButton.png)](</File:TBCDButton.png>)

BGRA Custom Drawn - это набор элементов управления, унаследованных от Custom Drawn. Они идут с темным стилем по умолчанию, который похож на Photoshop. 

### TBCDButton

Элемент управления - кнопка, стилизованный под TBGRADrawer. 

### TBCDEdit

Элемент редактирования, стилизованный с помощью TBGRADrawer. 

### TBCDStaticText

Элемент управления - label, стилизованный с помощью TBGRADrawer. 

### TBCDProgressBar

Элемент управления - индикатора выполнения, стилизованный с помощью TBGRADrawer. 

### TBCDSpinEdit

Элемент управления - spin edit, стилизованный с помощью TBGRADrawer. 

### TBCDCheckBox

Элемент управления - checkbox, стилизованный с помощью TBGRADrawer. 

### TBCRadioButton

Элемент управления - radiobutton, стилизованный с помощью TBGRADrawer. 

# Образцы кода

BGRA Controls поставляется с хорошими демками, показывающими, как использовать материал и дополнительные вещи, которые вы можете использовать в своих собственных проектах. 

### Библиотека паскалевского скрипта

Помещение методов BGRABitmap внутрь .dll с заголовками c #, java и pascal. 

### Пользовательские BGRA Ribbon компоненты

Как создать полностью тематическое окно, используя элементы управления для создания Ribbon-подобного приложения. 

### [Каталог] Tests

[В поставке с демками] есть тестовые [проекты] для аналоговых элементов управления (часы и датчик), элементы управления с префиксом BC, элементы управления с префиксом BGRA, элементы управления BGRA Custom Drawn, как использовать Pascal Script и BGRABitmap, bgrascript или как создать собственное решение для сценариев с BGRABitmap. 

### [Каталог] Tests Extra

[![game maze.png](https://wiki.freepascal.org/images/0/01/game_maze.png)](</File:game_maze.png>) [![game puzzle.png](https://wiki.freepascal.org/images/1/1f/game_puzzle.png)](</File:game_puzzle.png>) [![customdrawnwindows7.png](https://wiki.freepascal.org/images/7/78/customdrawnwindows7.png)](</File:customdrawnwindows7.png>) [![slicescaledtachart.png](https://wiki.freepascal.org/images/7/70/slicescaledtachart.png)](</File:slicescaledtachart.png>)

Это дополнительные тесты, например, как использовать эффект затухания, тему fpGUI, игры, такие как лабиринт и головоломки, как мы создали material design animation, pix2svg или как преобразовать маленькое изображение в SVG с использованием шестиугольников, прямоугольников и эллипсов, плагинов или как загрузить .dll и использовать в TBGRAVirtualScreen, эффект дождя, эффект тени, 9-фрагментное масштабирование с помощью Custom Drawn или как создавать темы с растровыми изображениями приложения, чтобы они выглядели как темы Windows, и 9-фрагментное масштабирование с помощью диаграмм. 

# Другие модули

Эти модули поставляются с элементами управления BGRA и содержат еще больше функций, которые иногда используются с элементами управления, иногда нет, но в некотором роде полезны. Некоторые из них перечислены здесь, другие, которые вы можете видеть, связаны напрямую с любым элементом управления, таким как bcrtti, bcstylesform, bctools и bctypes. 

### BCEffect

Эффект затухания [для использования] с BGRABitmap. 

### BCFilters

Набор пиксельных фильтров для использования с BGRABitmap. 

### BGRAScript

Создание сценариев с помощью BGRABitmap, см. тестовый проект. 

# Статьи по теме

[BGRASpriteAnimation](<../en/BGRASpriteAnimation.md> "BGRASpriteAnimation") \- Использование компонента анимации спрайтов. 

[uE_Controls](<../en/uE_Controls.md> "uE Controls") \- Другие элементы управления, разработанные с помощью BGRABitmap. 

[BGRABitmap](<../en/BGRABitmap.md> "BGRABitmap") \- Библиотека, используемая для создания этих элементов управления. 

[LazPaint](<../en/LazPaint.md> "LazPaint") \- Программа рисования, разработанная с помощью Lazarus и BGRABitmap.

---

_Source: [https://wiki.freepascal.org/BGRAControls/ru](https://web.archive.org/web/20250418103436/https://wiki.freepascal.org/BGRAControls/ru)_
