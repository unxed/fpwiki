# JVCL Components

│ **[English (en)](<../en/JVCL_Components.md> "JVCL Components")** │  **русский (ru)** │    
****

  


* * *

\--[Zoltanleo](</User:Zoltanleo> "User:Zoltanleo") 00:42, 7 October 2018 (CEST) Ввиду сложности дословного перевода текста с английского на русский слова, требующиеся по смыслу, но отсутствующие в английской версии, указаны в квадратных скобках. 

* * *

  


## Contents

  * 1 О компонентах
  * 2 Пакеты
    * 2.1 JvCore
    * 2.2 JvStdCtrls
    * 2.3 JvCtrls
    * 2.4 JvCustom
    * 2.5 JvCmp
    * 2.6 JvDB
    * 2.7 JvPageComps
    * 2.8 JvValidators
    * 2.9 JvXPCtrls
    * 2.10 JvRuntimeDesign
    * 2.11 JvHMI
    * 2.12 JvJans
    * 2.13 JvMM
  * 3 Загрузка и установка
    * 3.1 Загрузка
    * 3.2 Установка



## О компонентах

JVCL - это библиотека из более, чем 600 визуальных и невизуальных компонентов Delphi [github-jvcl](<https://github.com/project-jedi/jvcl>). Несмотря на то, что они сильно ориентированы на Windows, некоторые из них были перенесены на Lazarus. Эта страница намеревается дать краткое описание компонентов JVCL, доступных для Lazarus. Некоторые фрагменты информации были взяты из [JVCL_Component_Overview](<http://wiki.delphi-jedi.org/wiki/JVCL_Component_Overview>). 

## Пакеты

Из-за большого количества компонентов исходная версия Delphi разделяется на множество пакетов. Порт Lazarus следует этому соглашению, поэтому пользователь может установить только необходимые функции. 

Для каждого пакета доступна версия времени исполнения и времени разработки. Пакеты времени выполнения (Runtime packages) могут быть идентифицированы по добавленному [символу] R, пакеты времени разработки (designtime packages) по добавленному [символу] D. В требованиях к проекту всегда используется пакет времени выполнения. 

### JvCore

Обеспечивает базовую функциональность, необходимую всем остальным пакетам. 

### JvStdCtrls

Стандартные элементы управления 

  * **TJvCheckbox** : имеет свойство `LinkedControls`. Связанные элементы управления могут быть включены/отключены в соответствии с состоянием `Checked` Checkbox'а.
  * **JvCalcEdit** : числовое управление редактированием, которое открывает диалоговое окно калькулятора.



### JvCtrls

Расширенные элементы управления 

  * **TJvRollOut** : расширяемая панель с заголовком.
  * **TJvMovableBevel** : скошенная плоскость[(фаска)], которую можно перетаскивать и изменять размер во время выполнения [(runtime)].
  * **TJvMovablePanel** : то же, но теперь для панели. Обратите внимание, что этот компонент не принадлежит JVCL, это простая адаптация TJvMovableBevel применительно к панели.
  * **TJvRuler** : линейка для указания расстояний в сантиметрах, дюймах или пикселях.
  * **TJvGroupHeader** : заголовок с прикрепленной скошенной линией.
  * **TJvHtControls** и связанные с ним: набор элементов управления, которые могут отображать HTML-текст.



[![JvHTCcontrols.png](https://wiki.freepascal.org/images/2/26/JvHTCcontrols.png)](</File:JvHTCcontrols.png>)

### JvCustom

Пользовательские элементы управления 

  * **TJvValidateEdit** : Общее управления элементами редактирования для текста и чисел (целое число, float, валюта). MaxValue, MinValue, HasMaxValue, HasMinValue - Значения корректируются, когда Edit теряет фокус. См. демо в _JvValidateEdit_.
  * **TJvTimeLine** : одномерный календарь ("временная шкала"), который может отображать различные события и задачи.
  * **TJvTMTimeLine** : другой компонент временной шкалы.
  * **TJvThumbView** : отображает список картинок в виде эскизов.
  * **TJvOutlookBar** : полоса-гармошка контейнеров для других элементов управления, используемых в Outlook 97.
  * **TJvImageViewer** : отображает список картинок в виде эскизов, похожих на TJvThumbView. См. демо _JvItemViewer_.
  * **TJvImagesListViewer** : отображает все каритнки TImageList. См. демо _JvItemViewer_.
  * **TJvOwnerDrawViewer** : представляет произвольную информацию в виде сетки. См. демо _JvItemViewer_.  




[![tjvownerdrawviewer.png](https://wiki.freepascal.org/images/3/3a/tjvownerdrawviewer.png)](</File:tjvownerdrawviewer.png>)

### JvCmp

Специальные элементы управления 

  * **TJvSpellChecker** : класс проверки орфографии с примерными словарями для английского и голландского языков. См. демо в папке _examples/JvSpellChecker_ установки JVCL.



[![JvSpellChecker.png](https://wiki.freepascal.org/images/7/7e/JvSpellChecker.png)](</File:JvSpellChecker.png>)

### JvDB

Управление данными 

  * **TJvSearchEdit** : TEdit с инкрементными возможностями поиска в поле набора данных.
  * **TJvDBTreeView** :дерево, построенное из записей базы данных. Свяжите уникальное поле ID с "MasterField", поле ID родительского узла с "DetailField", а поле с текстом узла - "ItemField". При желании поле "IconField" предоставляет индекс изображения для ImageList.
  * **TJvDBCalcEdit** : числовое окно редактирования, которое открывает диалоговое окно калькулятора.



![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Примечание:** Пакеты _JvStdCtrlsLazR_ и _JvCtrlsLazR_ должны быть скомпилированы до того, как этот пакет можно будет установить.

### JvPageComps

  * **TJvNavigationPane** : панель навигации похожа на MS Outlook. См. демо в папке _examples/JvNavigationPane_ установки Lazarus JVCL.



[![JvNavigationPane demo.png](https://wiki.freepascal.org/images/6/60/JvNavigationPane_demo.png)](</File:JvNavigationPane_demo.png>)

### JvValidators

Различные классы валидатора для проверки ввода. См. демо в папке _examples/JvValidators_ установки Lazarus JVCL 

[![JvValidationDemo.png](https://wiki.freepascal.org/images/9/9e/JvValidationDemo.png)](</File:JvValidationDemo.png>)

  * **TJvErrorIndicator** : Показывает индикаторы ошибок рядом с полями редактирования и т.д., аналогичны индикаторам "недостающие поля" в веб-форумах.
  * **TJvValidators** : Чтобы проверить ввод [данных] пользователем. Щелкните правой кнопкой мыши на редакторе полей, чтобы определить правила проверки. Проверки выполняются при вызове метода "Validate".



### JvXPCtrls

Несколько элементов управления, имитирующих внешний вид Windows XP. 

  * **JvXPButton** и **JvXPCheckbox** : кнопка и checkbox в стиле XP
  * **TJvXPProgressBar** : Рисует индикатор прогресса в стиле XP. Цвет фона и блоков настраивается. Не поддерживает стиль Marquee.
  * **[JvXPBar](<../en/JvXPBar.md> "JvXPBar")** : боковая панель-контейнер со схлопывающимися панелями. Подобна TJRollOut-Panel, но с строковыми элементами вместо элементов управления. Схлопывание/развертывание происходит гладко и приятно анимировано.



[![JvXPBarDemo.png](https://wiki.freepascal.org/images/d/dd/JvXPBarDemo.png)](</File:JvXPBarDemo.png>)

### JvRuntimeDesign

  * **Дизайнер формы** времени выполнения. См демо в папке _example/JvDesigner_ установки JVCL.



[![JvDesignerDemo.png](https://wiki.freepascal.org/images/a/a4/JvDesignerDemo.png)](</File:JvDesignerDemo.png>)

### JvHMI

Пакеты JvHDMILazR (время выполнения) и JvHDMILazD (время разработки) предоставляют некоторые компоненты для разработки "человеко-машинного интерфейса" (HMI): 

  * **JvLED** : светодиодный индикатор. См. демо _examples/JvLED_.
  * **JvDialButton** : вращающаяся кнопка(варньера). См. демо _examples/JvDialButton_.



### JvJans

Назван в честь оригинального автора Яна Верховена(Jan Verhoeven), который пожертвовал эти компоненты проекту JEDI. Портированы следующие компоненты: 

  * [**TJvSIM components**](<../en/JvSIM_components.md> "JvSIM components"): набор компонентов для моделирования цифровой и аналоговой электроники.
  * **JvJanLED** : компонент светодиодного индикатора (не входит в оригинальный JVCL, портирован из собственного хранилища Яна Верховена).
  * **JvJanToggle** : кнопка переключения с областью включения и выключения (не содержащаяся в оригинальной JVCL, портирован из собственного хранилища Яна Верховена).
  * **JvMarkupViewer** и **JvMarkupLabel** : отображает простой HTML-форматированный текст (только теги, связанные с шрифтом). Наподобие JvHTControls.
  * **JvYearGrid** : календарь на год



[![TJvYearGrid.png](https://wiki.freepascal.org/images/8/8b/TJvYearGrid.png)](</File:TJvYearGrid.png>)

### JvMM

Оригинальный пакет JVCL для Delphi содержит набор мультимедийных компонентов, а также некоторые компоненты общего назначения. 

  * **JvID3v1** : невизуальный компонент для отображения и редактирования тегов ID3v1 mp3-файлов. См. демо _examples/JvID3v1_.
  * **JvID3v2** : невизуальный компонент для отображения и редактирования тегов ID3v2 mp3-файлов. См. демо _examples/JvID3v2_.
  * **JvBmpAnimator** : анимирует серию изображений, взятых из TImagelist. В отличие от предложенного названия, он работает с другими типами изображений, а также в Lazarus. См. демо _examples/JvBmpAnimator_.
  * **JvSpecialProgress** : продвинутый индикатор выполнения с градиентом и текстовым дисплеем. См. демо _examples/JvSpecialProgress_.
  * **JvGradient** : рисует несколько видов градиентов (горизонтальный, вертикальный, эллиптический, пирамидальный).
  * **JvGradientHeaderPanel** : панель заголовка с градиентным фоном (рисуется JvGradient'ом).



  


## Загрузка и установка

### Загрузка

Портированная версия разработчика Lazarus доступна из svn: 
    
    
     svn co https://sourceforge.net/p/lazarus-ccr/svn/HEAD/tree/components/jvcllaz/
    

Текущую версию можно загрузить [отсюда](<https://sourceforge.net/projects/lazarus-ccr/files/jvcllaz/>); его также предлагает Online Package Manager Lazarus'а. 

### Установка

Если вы хотите использовать все портированные компоненты JVCL, скомпилируйте все пакеты времени исполнения (runtime packages с добавленным "R") и все пакеты времени разработки (designtime packages с добавленным "D"). Затем установите все пакеты времени разработки (за исключением JvCoreLazD, который устанавливать не обязательно). 

Если вы хотите использовать только несколько компонентов, узнайте, в каком пакете они реализованы. Всегда компилируйте пакеты JvCoreLazR и JvCoreLazD. Затем скомпилируйте пакет(ы) времени исполнения и времени разработки, которые является для вас интересным. Наконец, установите пакет(ы) времени разработки. 

Эта процедура значительно упрощается, если вы устанавливаете версию выпуска через диспетчер Online Package Manager. Здесь вам нужно только проверить запрошенный пакет. OPM автоматически обнаружит, какие другие пакеты необходимы.

---

_Source: [https://wiki.freepascal.org/JVCL_Components/ru](https://web.archive.org/web/20250121222751/https://wiki.freepascal.org/JVCL_Components/ru)_
