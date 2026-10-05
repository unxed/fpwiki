# TAChart

│ **[English (en)](<../en/TAChart.md>)** │  **русский (ru)** │

## Contents

  * 1 О компоненте
  * 2 Документация
  * 3 Возможности
  * 4 Скриншот
  * 5 Авторы
  * 6 Загрузить
  * 7 См. также



### О компоненте

TAChart является основным компонентом для построения графиков и диаграмм в [Lazarus](<Lazarus_Faq.md> "Lazarus Faq/ru"), подобным фреймворку TeeChart, и распространяется под лицензией LGPL. TeeChart - это компонент для построения графиков и диаграмм, устанавливаемый в последних версиях Delphi. 

Данный компонент содержит функции, разработанные Филиппом Мартинолем ([Philippe Martinole](</User:Marty> "User:Marty")) для его проекта TeleAuto, которые были тщательно проверены Луисом Родригесом ([Luís Rodrigues](</User:Lfrodrigues> "User:Lfrodrigues")) при портировании приложения Epanet с Delphi на Lazarus. Александр Кленин ([Alexander Klenin](</User:Ask> "User:Ask")) переписал значительную часть кода и расширил функциональность. На данный момент он является главным сопровождающим проекта. 

Если у вас есть вопросы или предложения, а также если вы хотите сообщить о проблеме, пожалуйста, напишите об этом в [Список рассылки Lazarus](<http://lists.lazarus.freepascal.org/mailman/listinfo>) или в [Раздел по TAChart на форуме Lazarus](<http://www.lazarus.freepascal.org/index.php/board,55.0.html>). 

### Документация

Многие возможности TAChart продемонстрированы в примерах, расположенных в директории (Lazarus Install Dir)/components/tachart/demo/. См. [Демо-примеры TAChart](<../en/TAChart_Demos.md> "TAChart Demos") с краткими описаниями и скриншотами. 

Некоторые классы и свойства TAChart представлены в документации FPDoc (доступно из среды разработки Lazarus через клавишу [F1]). 

Обзор большинства понятий и возможностей TAChart можно найти в [документации по TAChart](<../en/TAChart_documentation.md> "TAChart documentation"). 

Для новичков могут быть полезны следующие руководства: 

  * [Руководство по TAChart: Getting started](<../en/TAChart_Tutorial__Getting_started.md> "TAChart Tutorial: Getting started");
  * [Руководство по TAChart: ListChartSource, Logarithmic Axis, Fitting](<../en/TAChart_Tutorial__ListChartSource,_Logarithmic_Axis,_Fitting.md> "TAChart Tutorial: ListChartSource, Logarithmic Axis, Fitting").
  * [Руководство по TAChart: Userdefined ChartSource](<../en/TAChart_Tutorial__Userdefined_ChartSource.md> "TAChart Tutorial: Userdefined ChartSource")
  * [Руководство по TAChart: BarSeries](<../en/TAChart_Tutorial__BarSeries.md> "TAChart Tutorial: BarSeries")
  * [Руководство по TAChart: Stacked BarSeries](<../en/TAChart_Tutorial__Stacked_BarSeries.md> "TAChart Tutorial: Stacked BarSeries")
  * [Руководство по TAChart: Dual y axis, Legend](<../en/TAChart_Tutorial__Dual_y_axis,_Legend.md> "TAChart Tutorial: Dual y axis, Legend")
  * [Руководство по TAChart: Multiple Panes in one Chart](<../en/TAChart_Tutorial__Multiple_Panes_in_one_Chart.md> "TAChart Tutorial: Multiple Panes in one Chart")
  * [Руководство по TAChart: Function Series](<../en/TAChart_Tutorial__Function_Series.md> "TAChart Tutorial: Function Series")
  * [Руководство по TAChart: ColorMapSeries, Zooming](<../en/TAChart_Tutorial__ColorMapSeries,_Zooming.md> "TAChart Tutorial: ColorMapSeries, Zooming")
  * [Руководство по TAChart: Chart Tools](<../en/TAChart_Tutorial__Chart_Tools.md> "TAChart Tutorial: Chart Tools")
  * [Руководство по TAChart: Background design](<../en/TAChart_Tutorial__Background_design.md> "TAChart Tutorial: Background design")
  * [Часто задаваемые вопросы по работе с TAChart в режиме выполнения программы](<../en/TAChart_Runtime_FAQ.md> "TAChart Runtime FAQ")



Отображение может быть исправлено с помощью сторонней библиотеки: 

  * [Отображение с помощью BGRABitmap](<../en/BGRABitmap_tutorial_TAChart.md> "BGRABitmap tutorial TAChart").



### Возможности

  * Более 15 различных графиков, включая круговые диаграммы, гистограммы, диаграммы с областями, линейные и точечные графики
  * Функциональные ряды с поддержкой домена
  * Нет ограничений на количество точек, осей и самих графиков
  * Flexible chart sources, including design-time editing, and random, dynamic and database-aware sources.
  * Легенда к графикам, заголовки и подписи
  * Подписи к осям или маркерам могут быть установлены вручную или сгенерированы автоматически
  * Инвертирование осей, независимое масштабирование и смещение, логарифмический масштаб
  * Интерактивные утилиты, включая зуммирование и панорамирование
  * Автоматическое или ручное ограничение графиков
  * Умная отрисовка маркеров
  * Легко расширяется с помощью новых типов графиков
  * Вывод диаграмм в SVG, OpenGL, printer, WMF, [AggPas](<http://www.crossgl.com/aggpas/>), [BGRABitmap](<../en/BGRABitmap.md> "BGRABitmap"), [fpvectorial](<../en/fpvectorial.md> "fpvectorial")
  * Может использоваться в неграфических приложениях, таких как веб-сервисы
  * Находится в активной разработке (см. [roadmap](<../en/TAChart_Roadmap.md> "TAChart Roadmap"))



### Скриншот

На данном скриншоте представлен пример работы компонента TAChart с отображением линейного графика, гистограммы и круговой диаграммы 

[![tachart.png](https://wiki.freepascal.org/images/7/7b/tachart.png)](</File:tachart.png>)

### Авторы

[Luís Rodrigues](</User:Lfrodrigues> "User:Lfrodrigues"), [Philippe Martinole](</User:Marty> "User:Marty"), [Alexander Klenin](</User:Ask> "User:Ask")

### Загрузить

Последнюю актуальную версию можно найти в SVN-репозитории Lazarus (сами компоненты находятся на вкладке [Chart](<Chart_tab.md> "Chart tab/ru") в Lazarus). 

### См. также

  * [RingChart and AnalogWatch](</index.php?title=RingChart_and_AnalogWatch/ru&action=edit&redlink=1> "RingChart and AnalogWatch/ru \(page does not exist\)")
  * [PlotPanel](</index.php?title=PlotPanel/ru&action=edit&redlink=1> "PlotPanel/ru \(page does not exist\)")
  * [Сравнение TAChart и TeeChart Standard в Delphi](<../en/Comparing_TAChart_with_Delphi's_TeeChart_Standard.md> "Comparing TAChart with Delphi's TeeChart Standard")

---

_Source: [https://wiki.freepascal.org/TAChart/ru](https://web.archive.org/web/20230529211428/https://wiki.freepascal.org/TAChart/ru)_
