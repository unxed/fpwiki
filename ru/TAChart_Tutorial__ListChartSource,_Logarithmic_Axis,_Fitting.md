# TAChart Tutorial: ListChartSource, Logarithmic Axis, Fitting

│ **[English (en)](<../en/TAChart_Tutorial__ListChartSource,_Logarithmic_Axis,_Fitting.md>)** │  **русский (ru)** │

## Contents

  * 1 Введение
  * 2 Подготовка
    * 2.1 Настройка диаграммы
    * 2.2 Создание серии точек
  * 3 Ввод данных
    * 3.1 Список источников данных (ListChartSource)
    * 3.2 Редактор данных в ListChartSource
    * 3.3 Отображение меток точек
  * 4 Настройка логарифмической оси
    * 4.1 TChartTransformations и LogarithmicAxisTransform
    * 4.2 Автоматическая расстановка осевых меток
    * 4.3 Расстановка осевых меток вручную
    * 4.4 Мелкие деления
  * 5 Аппроксимация
    * 5.1 TFitSeries
    * 5.2 Результаты аппроксимации
  * 6 Исходный код
    * 6.1 Файл проекта
    * 6.2 Unit1.pas
    * 6.3 Unit1.lfm



## Введение

[![TAChart LogAx Tutorial15.png](https://wiki.freepascal.org/images/3/3d/TAChart_LogAx_Tutorial15.png)](</File:TAChart_LogAx_Tutorial15.png>)

Вы знаете [Закон Мура](<https://ru.wikipedia.org/wiki/Закон_Мура>)? Он перед вами... 

Возможно, читая это, вы сидите перед своим десктопом или держите на коленях ноутбук, или смартфон в руках. В последние десятилетия произошёл огромный прогресс микроэлектроники и он всё продолжается. Это происходит благодаря минитюаризации электронных устройств, позволяющей упаковывать всё больше функций на один кремниевый чип. И закон Мура именно об этом: количество транзисторов на чипе удваивается примерно каждые два года. 

В этом руководстве мы возьмем опубликованные данные крупного производителя микропроцессоров и воспользуемся TAChart для построения графика количества транзисторов в зависимости от года выпуска продуктов на рынок. Этими данными мы попытаемся подтвердить закон Мура. 

Вы можете найти эти данные на сайте [www.intel.com/pressroom/kits/quickreffam.htm](<http://www.intel.com/pressroom/kits/quickreffam.htm>), который содержит список микропроцессоров, дату их выхода на рынок и количество транзисторов на чип. 

При изучении этого примера вы научитесь: 

  * как **вводить данные в фазе разработки формы (designtime)**
  * как **создать серию точек с метками сверху каждой точки**
  * как **создать логарифмическую ось**
  * как **аппроксимировать данные**.



Вам нужно иметь базовые знания о том, как работать с TAChart; иначе необходимо просмотреть [Руководство по началу работы](<TAChart_Tutorial__Getting_started.md> "TAChart Tutorial: Getting started/ru") TAChart. Конечно, вы должны быть знакомы с [Lazarus](<../en/Lazarus.md> "Lazarus") и [Object Pascal](<../en/Object_Pascal.md> "Object Pascal"). И не надо слишком бояться математики, вам понадобятся логарифмы и экспоненциальная функция. 

## Подготовка

### Настройка диаграммы

  * Создадим новый проект.
  * Поскольку получается несколько меток по длинной оси, увеличим размер формы примерно до 540 x 320 пикселей.
  * Добавим компонент `TChart`, выровняем его в режиме `alClient`, установим `BackColor` в `clWhite` и `Grid.Color` каждой оси в `clSilver`.
  * Добавим имя оси **X** («Year of market introduction» — Год выхода на рынок) и оси **Y** («Number of transistors» — Количество транзисторов).
  * Используем текст «Progress in Microelectronics» (Прогресс в микроэлектронике) как заголовок диаграммы.
  * Зададим стиль шрифта `fsBold`.
  * Дадим ссылку на наши данные в нижнем колонтитуле. Используем для этого свойство диаграммы [`Foot`](<../en/TAChart_documentation.md> "TAChart documentation"). Заметьте, что редактор свойства `Foot.Text` (а также `Title.Text`) позволяет вводить многострочные заголовки.



[![TAChart LogAx Tutorial1.png](https://wiki.freepascal.org/images/1/17/TAChart_LogAx_Tutorial1.png)](</File:TAChart_LogAx_Tutorial1.png>)

### Создание серии точек

Мы хотим нарисовать каждую запись TDataRecord как единую точку данных — в других программах для построения диаграмм это называется «серией точек». Редактор серии TAChart не имеет такого типа серии потому что это можно сделать с помощью [`TLineSeries`](<TAChart_documentation.md> "TAChart documentation/ru"). Нам нужно только установить его свойство `ShowPoints` в `true` и отключить соединительные линии (`LineType` = `ltNone`).Эти символы определены свойством `Pointer`. 

Итак, добавим LineSeries на форму и установим свойства для создания серии точек. Дополнительно установим `Pointer.Brush.Color` в `clRed`, и `Pointer.Style` в `psCircle`, чтобы нарисовать красный круг в каждой точке. 

## Ввод данных

### Список источников данных (ListChartSource)

Есть много способов для ввода данных в диаграмму. Мы будем использовать [ListChartSource](<TAChart_documentation.md> "TAChart documentation/ru"). Обычно данные загружаются в источник динамически во время выполнения программы, например, после чтения файла. Однако в нашем проекте мы хотим ввести их во время разработки программы с помощью редактора точек ListChartSource. Главное преимущество этого в том, что мы немедленно увидим результат наших действий, не компилируя проект. Однако у этого подхода также есть и свои недостатки. 

ListChartSource хранит данные для рисования в списке — отсюда и его название. Элементы списка это так называемые `TChartDataItem` объекты, содержащие для каждой точки следующее: 

  * координаты `x` и `y`,
  * `Text` для метки точки,
  * `Color`, который перекрывает SeriesColor,
  * а также `YList`, содержащий дополнительные значения `y`, которые нужны в некоторых специальных типах серий.



ListChartSource — это тот же самый тип источника данных, который используется внутри многих классов серий. Однако наличие его в качестве отдельного компонента имеет большое преимущество, заключающееся в том, что тот же источник диаграммы можно снова использовать для других серий — эта функция понадобится нам позже при применении FitSeries. 

[![TAChart LogAx Tutorial2.png](https://wiki.freepascal.org/images/3/3b/TAChart_LogAx_Tutorial2.png)](</File:TAChart_LogAx_Tutorial2.png>)

Начнем с добавления TListChartSource в нашу форму. Это вторая иконка в палитре компонентов диаграммы. Привяжем к этой серии наш источник, указав в её свойстве `Source` новый `ListChartSource1`. 

### Редактор данных в ListChartSource

[![TAChart LogAx Tutorial16.png](https://wiki.freepascal.org/images/a/af/TAChart_LogAx_Tutorial16.png)](</File:TAChart_LogAx_Tutorial16.png>)

При нажатии на кнопку с многоточием рядом со свойством `DataPoints` открывается редактор данных источника диаграммы. Это таблица, в которую мы можем вводить данные. Введём данные с предыдущего изображения. Они взяты с веб-сайта, упомянутого во введении. На этом сайте указано время выхода на рынок по годам и месяцам — для простоты я пропустил месяц и округлил до ближайшего календарного года. Введите год**а** в столбец **X**. Столбец **Y** получит количество транзисторов на чип. Название каждого микропроцессора войдёт в столбец **Text**. Поскольку мы не хотим присваивать каждой точке отдельный цвет, оставляем столбец **Color** в покое, но, конечно, можно поэкспериментировать и с этой функцией. 

После закрытия редактора данных, серия автоматически обновляется и отображает текущий набор данных. Вау! 

### Отображение меток точек

Почему мы не видим имена процессоров, введённых в таблицу? Потому, что [метки точек](<TAChart_documentation.md> "TAChart documentation/ru") по умолчанию отключены. Выберем серию и перейдём к `Marks`. Посмотрим на свойство `Style`, оно содержит `smsNone`, что значит «выключено». Откроем раскрывающийся список. Увидим много вариантов маркировки точек. Поиграемся с этими установками чтобы выучить, что они значат. Здесь мы хотим использовать `smsLabel`, отображающий поле `Text` свойства `TChartDataItem`. Чтобы назначить **Marks.Style** через исходный код, нужно добавить `uses ... TAChartUtils`. 

Между метками и точками существуют также соединительные линии . У них по умолчанию белый цвет, поэтому на белом фоне мы его не видим. Имя свойства для этой линии `LinkPen`. Можно установить `LinkPen.Color` в `clGray`. 

Позиция метки нас устраивает, но надо знать, что серия имеет свойство [`MarkPositions`](<../en/TAChart_documentation.md> "TAChart documentation"), чтобы контролировать её положение. 

[![TAChart LogAx Tutorial3.png](https://wiki.freepascal.org/images/a/a4/TAChart_LogAx_Tutorial3.png)](</File:TAChart_LogAx_Tutorial3.png>)

В этот момент можно скомпилировать проект. 

Ничего нового — мы видели всё уже в режиме разработки. Это большое преимущество работы с редактором точек в ListChartSource. 

Есть две вещи, которые можно улучшить на этом этапе: 

  * Метки точек обрезаются по краям диаграммы. Можно исправить это увеличением `Margin.Left` и `Margin.Right` диаграммы до 24. [`Margin`](<TAChart_documentation.md> "TAChart documentation/ru") определяет пространство, окружающее внутреннюю область графика, чтобы оно не содержало точек. Также есть свойство [`MarginExternal`](<TAChart_documentation.md> "TAChart documentation/ru"), которое определяет окружение внешней границы диаграммы и может использоваться для изменения расстояния до соседних элементов управления.
  * Поскольку почти все точки сосредоточены в нижней части диаграммы, график не очень удобен. Именно поэтому нам нужна **логарифмическая ось**.



## Настройка логарифмической оси

Отображение данных на логарифмической оси означает, что на график наносятся логарифмы значений данных, а не сами значения. Например, значения **y** на нашей диаграмме находятся в диапазоне от 2300 до 731 миллиона. Когда мы вычисляем [десятичные](<https://ru.wikipedia.org/wiki/Десятичный_логарифм>) (log10) логарифмы, диапазон составляет примерно от 3,3 до 7,1 — на такой диаграмме данные различить намного проще. 

  


### TChartTransformations и LogarithmicAxisTransform

[![TAChart LogAx Tutorial4.png](https://wiki.freepascal.org/images/3/36/TAChart_LogAx_Tutorial4.png)](</File:TAChart_LogAx_Tutorial4.png>)

Вычисление логарифма может быть выполнено TAChart автоматически. Основное преимущество этого заключается в том, что исходные единицы данных отображаются на оси, а логарифмы используются для построения графика. 

Фактически есть целая группа компонентов для трансформаций осей: [`TChartAxisTransformations`](<TAChart_documentation.md> "TAChart documentation/ru"). Преобразование оси это функция, которая сопоставляет данные «реального мира» в единицах, указанных на оси (_«осевые координаты»_) во внутренние единицы, общие для всех серий в той же диаграмме (_«координаты графика»_). Осевая координата количества транзисторов процессора 4004, например, 2300, координата на графике — логарифм этого числа, т.е. log10(2300) = 3.36. 

TAChart предлагает много преобразований. В добавок к логарифмическому преобразованию есть [линейное преобразование](<../en/TAChart_documentation.md> "TAChart documentation"), которое позволяет умножать данные на коэффициент и добавлять смещение. [Автомасштабирующее преобразование](<../en/TAChart_documentation.md> "TAChart documentation") полезно когда несколько серий надо нарисовать на одной оси. [Пользовательское преобразование](<../en/TAChart_documentation.md> "TAChart documentation") позволяет применить любое произвольное преобразование. 

[![TAChart LogAx Tutorial5.png](https://wiki.freepascal.org/images/5/55/TAChart_LogAx_Tutorial5.png)](</File:TAChart_LogAx_Tutorial5.png>)

Добавим на форму компонент `TAChartTransformations`, дважды кликнем по нему (или кликнем правой клавишей на нём в дереве объектов и выберем «Edit axis transformations»), кликнем на «Add» и выберем «Logarithmic». Создастся компонент `ChartAxisTransformations1LogarithmAxisTransform1` — вот ведь имечко! Хорошо хоть, что скорее всего вам не придётся вводить его руками. 

В инспекторе объектов мы увидим несколько свойств — самое важное — `Base`. Это основание вычисляемого логарифма. Сменим его на 10, т.к. мы хотим считать [десятичные логарифмы](<http://www.wikipedia.org/wiki/Common_logarithm> "wikipedia:Common logarithm") — это подходит к 99% всех логарифмических диаграмм. 

Примечание: очень важно **в этом месте сохранить проект**. Почему? Увидите через минуту. 

Теперь мы должны определить ось, которая должна быть преобразована. Для этого каждая ось имеет свойство `Transformations`. В нашем случае огромные числа отложены по оси **Y**. Итак, переходим к левой оси и устанавливаем ее свойство `Transformations` в `ChartAxisTransformations1`. 

[![TAChart LogAx Tutorial17.png](https://wiki.freepascal.org/images/0/0d/TAChart_LogAx_Tutorial17.png)](</File:TAChart_LogAx_Tutorial17.png>)

Чёрт, что это? Выскакивает сообщение об ошибке проверки диапазона. Надеюсь, вы сохранили проект. Если вы нажмете «Отмена», Lazarus выключится, все будет потеряно. Если вы нажмете «ОК», диаграмма внезапно исчезнет, ​​​​она снова появится, когда вы кликните где-нибудь, но неисправная ось будет скрыта. 

Вы в отчаянии? 

Это основной недостаток работы в фазе конструирования. Если что-то пойдет не так, то нет отладчика, нет индикации того, что вызвало проблему. Поскольку компоненты компилируются в Lazarus, вам придется отлаживать IDE. Звучит сложно. 

Давайте сядем и подумаем, что мы сделали. Мы присвоили логарифмическое преобразование оси **Y**. Преобразование пока не имеет никакой связи с данными, поэтому данные пока в «реальных» единицах, их максимум 731 миллион. Но преобразование «думает», что данные уже представлены в графических единицах (логарифмах). Когда оно вычисляет метки осей в осевых единицах, это занимает сотни миллионов в степени 10! Вот что вызывает ошибку проверки диапазона. Однако ситуация не всегда столь драматична; самое меньшее, что может случиться, это то, что данные не трансформируются, а ось трансформируется. 

Что мы можем с этим сделать? Каждая серия имеет свойства `AxisIndexX` и `AxisIndexY`. Преобразование может использовать эту информацию чтобы вычислить логарифмы правильных координат перед обновлением меток осей. Это решает нашу проблему. 

Итак, нажмём «Отмена», чтобы выключить Lazarus. Перезапустим и перезагрузим проект. В сохраненном состоянии трансформация еще не связана с осью. 

Теперь взглянем на дерево объектов над инспектором объектов, и увидим, что левая ось имеет индекс 0. Поэтому установим `AxisIndexY` серии в это значение. Хотя это и не обязательно, может быть хорошей идеей обезвредить также ось **X** , назначив индекс 1 свойству `AxisIndexX` серии — кто может гарантировать, что мы не будем трансформировать ось x в будущем? 

После этого мы можем установить для `LeftAxis.Transformation` значение `LeftAxis.Transformation` без ошибки проверки диапазона. 

С активированным логарифмическим преобразованием точки теперь хорошо распределяются по диапазону оси **Y**. Но что не так с метками оси **Y**? А годы на оси абсцисс слишком близки и частично перекрываются. 

[![TAChart LogAx Tutorial6.png](https://wiki.freepascal.org/images/d/d0/TAChart_LogAx_Tutorial6.png)](</File:TAChart_LogAx_Tutorial6.png>)

Поиск меток осей — нетривиальная задача, особенно когда активны преобразования, сильно искажающие интервалы осей. К сожалению, логарифмические оси принадлежат к этой группе. По сути, есть два способа управления позиционированием метки: **автоматический** и **ручной**. 

### Автоматическая расстановка осевых меток

Для автоматической расстановки меток ось имеет свойство [`Intervals`](<TAChart_documentation.md> "TAChart documentation/ru"), которое дает доступ к нескольким, частично взаимоисключающим параметрам — см. пояснение в [документации TAChart](<TAChart_documentation.md> "TAChart documentation/ru"). В случае логарифмической оси проблема обычно возникает из-за того, что не установлена опция `aipGraphCoordinates`. Этот параметр, если он установлен, принудительно вычисляет интервалы делений для преобразованных данных («координаты графика»), а не для данных «реального мира» («координаты оси»). Итак, установим `aipGraphCoordinates` в `LeftAxis.Intervals.Options`. Метки разместятся более равномерно. 

[![TAChart LogAx Tutorial7.png](https://wiki.freepascal.org/images/f/f1/TAChart_LogAx_Tutorial7.png)](</File:TAChart_LogAx_Tutorial7.png>)

В зависимости от размера формы могут получиться очень хорошие или не очень хорошие метки. Изменив размер формы, увидим несколько «кривых» меток. 

Улучшить внешний вид метки можно следующим образом: 

  * Увеличить `Intervals.Tolerance`. Это позволяет изменять расстояние между делениями.
  * Отрегулировать диапазон в пикселях, в котором может варьироваться расстояние между метками. Он определяется свойствами `Intervals.MaxLength` и `Intervals.MinLength`. Оптимальное значение зависит от размера диаграммы и диапазона данных. В нашем учебном проекте хорошие метки получаются при установке этих свойств в 100 и 50 соответственно. Обычно `Intervals.MaxLength` обеспечивает лучшие результаты.



Таким же образом можно поступить с перекрывающимися метками года оси **X**. Просто увеличим `BottomAxis.Intervals.MaxLength` до 70. 

Теперь осталась «1», которая появляется на оси **Y** между «10000000» и «1E009». Это связано с ошибкой в некоторых версиях FPC. Если у вас это тоже есть, просто измените свойство `LeftAxis.Marks.Format`. Эта строка передается функции `Format` для преобразования чисел в строки. Спецификатор формата «%0.0n», например, позволяет избежать этой ошибки преобразования и, кроме того, добавляет к меткам разделители тысяч, что делает их намного более читабельными. 

[![TAChart LogAx Tutorial8.png](https://wiki.freepascal.org/images/7/7e/TAChart_LogAx_Tutorial8.png)](</File:TAChart_LogAx_Tutorial8.png>)

### Расстановка осевых меток вручную

Это лучшее, что мы смогли сделать с автоматическим позиционированием меток. Это не идеально, потому что, когда мы увеличиваем высоту окна или увеличиваем масштаб, могут появиться значения полудекады, или интервал метки может составлять две декады, как на рисунке выше. 

Если нас это не устраивает, мы должны использовать **ручной выбор осевых меток**. Для этого у каждой оси есть свойство `Source`, которое можно связать с ListChartSource, содержащим только разрешенные метки осей. Поэтому, когда этот источник диаграммы содержит только метки полных декад, нет риска меток полудекад или пропуска всех остальных меток. С другой стороны, когда мы увеличиваем диаграмму, мы можем наткнуться на точку, у которой метки больше не видны. 

Добавим в форму второй источник ListChartSource. Можно снова использовать редактор DataPoints, чтобы ввести числа полных декад. Это имеет то преимущество, что мы можем выполнить большую часть этого проекта, не написав ни одной строки кода! 

Но можно также легко заполнить источник списка и в событии `FormCreate`: 
    
    
    procedure TForm1.FormCreate(Sender: TObject);
    const
      MIN = 0;
      MAX = 12;
    var
      i: Integer;
      value: double;
    begin
      for i:=MIN to MAX do begin
        value := Power(10, i);
        ListChartSource2.Add(value, value);
      end;
    end;
    

Эта процедура добавляет степени 10 в достаточно широком диапазоне в ListChartSource с помощью метода `Add`. 

Подключим `ListChartSource1` к `LeftAxis.Marks.Source`, чтобы активировать ручные метки ListChartSource. Нам нужно также удалить все флаги из свойства `Options`. В противном случае автоматический поиск делений будет в какой-то степени активен. Если мы не использовали редактор DataPoints, мы должны скомпилировать проект, чтобы увидеть эффект. 

[![TAChart LogAx Tutorial9.png](https://wiki.freepascal.org/images/5/51/TAChart_LogAx_Tutorial9.png)](</File:TAChart_LogAx_Tutorial9.png>)

### Мелкие деления

Очень часто между крупными делениями располагаются мелкие деления. TAChart позволяет добавить несколько наборов мелких делений на каждую ось. Нам здесь нужен только один. Ищем свойство `LeftAxis` и кликаем кнопку с многоточием рядом со свойством `Minors`. Открывается редактор для `Chart1.AxisList[0].Minors`. Нажимаем «Add» и «М» в списке ниже. Теперь можем настроить параметры в инспекторе объектов, чтобы получить «хорошие» мелкие деления. Если большие деления на логарифмической оси соответствуют полным декадам, то мелкие деления обычно соответствуют 2, 3, 4,..., 8, 9 и, конечно же, десятым степеням. Этого можно легко добиться, отключив все `Intervals.Options`, кроме `aipUseCount`, и установив `Intervals.Count` = 9. Конечно, это имеет смысл только в том случае, если большие метки показывают полные декады, как в описанном выше ручном подходе. 

Обычно график становится слишком переполненным мелкой сеткой, как сейчас, и нам надо установить для мелкой сетки `Grid.Visible` значение `false`. 

[![TAChart LogAx Tutorial10.png](https://wiki.freepascal.org/images/5/5c/TAChart_LogAx_Tutorial10.png)](</File:TAChart_LogAx_Tutorial10.png>)

## Аппроксимация

Теперь поищем связь между данными, т. е. мы хотим найти математическую формулу, описывающую зависимость количества транзисторов от года выхода на рынок. Это называется «аппроксимация»: мы выбираем формулу с параметрами и подбираем эти параметры так, чтобы отклонение от данных было как можно меньше. 

TAChart не содержит полноценного механизма аппроксимации. Он использует процедуры аппроксимации из библиотеки FPC [numlib](<../en/numlib.md> "numlib"). Поэтому TAChart не может охватить все варианты аппроксимации, но охватывает самый важный случай — приближение с помощью [полиномов](<http://www.wikipedia.org/wiki/Polynomial> "wikipedia:Polynomial"), используя [метод наименьших квадратов](<http://www.wikipedia.org/wiki/Least_squares> "wikipedia:Least squares"). Это уровень, доступный пользователям Excel, когда они добавляют «линию тренда» на свой график. 

### TFitSeries

Для аппроксимации TAChart предоставляет специалиальный объект типа [`TFitSeries`](<../en/TAChart_documentation.md> "TAChart documentation"). Такая серия имеет свойство `FitEquation`, которое определяет используемую формулу: 

  * `fePolynomial`: _y_ = _b_ 0 \+ _b_ 1 _x_ \+ _b_ 2 _x_ 2 \+ … + _b nx_ _n_. Указывает число аппроксимирующих параметров _a i_ в свойстве `ParamCount` = _n_ \+ 1.
  * `feLinear`: _y_ = _a_ \+ _bx_ — это частный случай полинома с _n_ = 1 и параметрами аппроксимации _a_ and _b_. Он доступен как отдельный элемент, поскольку прямые линии определяют наиболее важные условия аппроксимации.
  * `feExp`: _y_ = _a_ * _e bx_ — Эта функция также может быть к полиномиальной, хотя это не так просто заметить. Но взяв натуральный логарифм этой функции, получим ln(_y_) = _a_ \+ _bx_. Теперь, когда мы аппроксимируем ln(y) вместо y, мы снова имеем линейный случай.
  * `fePower`: _y_ = _a_ * _x b_. Снова это может быть сведено к линейной функции путём логарифмических преобразований обеих осей.



[![TAChart LogAx Tutorial11.png](https://wiki.freepascal.org/images/b/b5/TAChart_LogAx_Tutorial11.png)](</File:TAChart_LogAx_Tutorial11.png>)

Довольно теории. Давайте добавим FitSeries на диаграмму: дважды кликнем по диаграмме и в редакторе серий нажмём «Add» и выберем «Least squares fit series»(аппроксимация методом наименьших квадратов) из выпадающего меню. 

Во-первых, нам нужно указать серии, где она найдёт свои данные. Для этого мы свяжем свойство серии `Source` с `ListChartSource1`, как мы уже делали с линейной серией. Заметим, что один и тот же источник данных может использоваться несколькими сериями. 

Надеюсь, вы помните описанную выше катастрофу с AxisIndex. Так что установим в `AxisIndexY` индекс левой оси, как мы делали в линейной серии. 

Какую из четырёх возможностей `FitEquation` выбрать? Ну, данные выглядят как лежащие на прямой. Так что выберем `feLinear`. 

[![TAChart LogAx Tutorial12.png](https://wiki.freepascal.org/images/0/06/TAChart_LogAx_Tutorial12.png)](</File:TAChart_LogAx_Tutorial12.png>)

Блин! Мы видим черную аппроксимирующую кривую, но она совсем не аппроксимирует. Мы хотели прямую линию, а получили изогнутую кривую. Как такое может быть? 

Причина в логарифмическом преобразовании, которое мы применили к данным оси **Y**. Поэтому на нашем графике показаны логарифмы, но для аппроксимации используются «сырые» данные. Мы эффективно аппроксимируем прямой данные на скриншоте в разделе Отображение меток точек, где логарифмическое преобразование ещё не было введено — понятно, что прямая линия там не подойдёт. И когда аппроксимирующая функция нарисована, логарифмическое преобразование искажает прямую линию в искривленную кривую, которую мы и видим. 

С другой стороны, если **логарифмизированные данные** находятся на прямой линии, то наша аппроксимирующая функция не линейна, а экспоненциальна. Установим `FitEquation` в `feExp` и попробуем снова. 

[![TAChart LogAx Tutorial13.png](https://wiki.freepascal.org/images/a/ac/TAChart_LogAx_Tutorial13.png)](</File:TAChart_LogAx_Tutorial13.png>)

Вот, намного лучше! 

Теперь мы знаем, что экспоненциальный закон _y_ = _a_ * _x b_ — хорошее приближение наших данных. Но как нам получить параметры _a_ и _b_? 

### Результаты аппроксимации

Аппроксимирующая серия имеет свойство типа массив `Param`, содержащий параметры аппроксимации. `a` это `Params[0]`, и `b` лежит в `Params[1]`. Конечно, эти значения имеют смысл только тогда, когда была произведена удачная аппроксимация. Как мы это узнаем? Ну, аппроксимирующая серия генерирует событие `OnFitComplete`, когда аппроксимация удачно завершается. Вот где мы можем узнать полученные параметры аппроксимации. Например, отобразим результаты аппроксимации в сообщении вместе с аппроксимирующей функцией: 
    
    
    procedure TForm1.Chart1FitSeries1FitComplete(Sender: TObject);
    begin
      with Chart1FitSeries1 do
        ShowMessage(Format(
          'Fit result: a = %g, b = %g', [
          Param[0], Param[1]
        ]));
    end;
    

И вот что мы получим: 

[![TAChart LogAx Tutorial14.png](https://wiki.freepascal.org/images/d/d1/TAChart_LogAx_Tutorial14.png)](</File:TAChart_LogAx_Tutorial14.png>)

Теперь мы хотим вычислить время T за которое число транзисторов на пластине удваивается. В качестве упражнения попробуйте показать, что 
    
    
    T = ln(2) / b
    

Было бы неплохо указать время удвоения в виде дополнительной строки в заголовке графика. Для этого модифицируем обработчик события `OnFitComplete` следующим образом: 
    
    
    procedure TForm1.Chart1FitSeries1FitComplete(Sender: TObject);
    begin
      Chart1.Title.Text.Add(Format(
        'The number of transistors doubles every %.0f years',
        [ln(2) / Chart1FitSeries1.Param[1]]
      ));
    end;
    

[![TAChart LogAx Tutorial15.png](https://wiki.freepascal.org/images/3/3d/TAChart_LogAx_Tutorial15.png)](</File:TAChart_LogAx_Tutorial15.png>)

Вау! Это закон Мура: «Количество транзисторов на пластине удваивается каждые два года». 

## Исходный код

### Файл проекта
    
    
    program project1;
    
    {$mode objfpc}{$H+}
    
    uses
      {$IFDEF UNIX}{$IFDEF UseCThreads}
      cthreads,
      {$ENDIF}{$ENDIF}
      Interfaces, // this includes the LCL widgetset
      Forms, Unit1, tachartlazaruspkg
      { you can add units after this };
    
    {$R *.res}
    
    begin
      //RequireDerivedFormResource := True;
      Application.Initialize;
      Application.CreateForm(TForm1, Form1);
      Application.Run;
    end.
    

### Unit1.pas
    
    
    unit Unit1;
    
    {$mode objfpc}{$H+}
    
    interface
    
    uses
      Classes, SysUtils, FileUtil, TAGraph, TASeries, TASources, Forms, Controls,
      Graphics, Dialogs, TACustomSource, TATransformations, TAFuncSeries;
    
    type
    
      { TForm1 }
    
      TForm1 = class(TForm)
        Chart1: TChart;
        Chart1FitSeries1: TFitSeries;
        Chart1LineSeries1: TLineSeries;
        ChartAxisTransformations1: TChartAxisTransformations;
        ChartAxisTransformations1LogarithmAxisTransform1: TLogarithmAxisTransform;
        ListChartSource1: TListChartSource;
        ListChartSource2: TListChartSource;
        procedure Chart1FitSeries1FitComplete(Sender: TObject);
        procedure FormCreate(Sender: TObject);
      private
        { private declarations }
      public
        { public declarations }
      end;
    
    var
      Form1: TForm1;
    
    implementation
    
    {$R *.lfm}
    
    uses
      math;
    
    { TForm1 }
    
    procedure TForm1.FormCreate(Sender: TObject);
    const
      MIN = 0;
      MAX = 12;
    var
      i: Integer;
      value: double;
    begin
      for i:=MIN to MAX do begin
        value := IntPower(10, i);
        ListChartSource2.Add(value, value);
      end;
      Chart1FitSeries1.ExecFit;
    end;
    
    procedure TForm1.Chart1FitSeries1FitComplete(Sender: TObject);
    begin
      {
      with Chart1FitSeries1 do
        ShowMessage(Format(
          'Fit result: a = %g, b = %g', [
          Param[0], Param[1]
        ]));
      }
      Chart1.Title.Text.Add(Format(
        'The number of transistors doubles every %.0f years',
        [ln(2) / Chart1FitSeries1.Param[1]]
      ));
    end;
    
    end.
    

### Unit1.lfm
    
    
    object Form1: TForm1
      Left = 244
      Height = 356
      Top = 193
      Width = 552
      Caption = 'Form1'
      ClientHeight = 356
      ClientWidth = 552
      OnCreate = FormCreate
      LCLVersion = '1.1'
      object Chart1: TChart
        Left = 0
        Height = 356
        Top = 0
        Width = 552
        AxisList = <    
          item
            Grid.Color = clSilver
            Marks.Format = '%0:.0n'
            Marks.Source = ListChartSource2
            Marks.Style = smsCustom
            Minors = <        
              item
                Grid.Visible = False
                Intervals.Count = 9
                Intervals.MinLength = 5
                Intervals.Options = [aipUseCount]
              end>
            Title.LabelFont.Orientation = 900
            Title.LabelFont.Style = [fsBold]
            Title.Visible = True
            Title.Caption = 'Number of transistors'
            Transformations = ChartAxisTransformations1
          end    
          item
            Grid.Color = clSilver
            Intervals.MaxLength = 60
            Alignment = calBottom
            Minors = <>
            Title.LabelFont.Style = [fsBold]
            Title.Visible = True
            Title.Caption = 'Year of market introduction'
          end>
        BackColor = clWhite
        Foot.Alignment = taLeftJustify
        Foot.Brush.Color = clBtnFace
        Foot.Font.Color = clBlue
        Foot.Text.Strings = (
          'Source:'
          'http://www.intel.com/pressroom/kits/quickreffam.htm'
        )
        Foot.Visible = True
        Margins.Left = 24
        Margins.Right = 24
        Title.Brush.Color = clBtnFace
        Title.Font.Color = clBlue
        Title.Font.Style = [fsBold]
        Title.Text.Strings = (
          'Progress in Microelectronics'
        )
        Title.Visible = True
        Align = alClient
        ParentColor = False
        object Chart1LineSeries1: TLineSeries
          Marks.Format = '%2:s'
          Marks.LinkPen.Color = clGray
          Marks.Style = smsLabel
          AxisIndexY = 0
          LineType = ltNone
          Pointer.Brush.Color = clRed
          Pointer.Style = psCircle
          ShowPoints = True
          Source = ListChartSource1
        end
        object Chart1FitSeries1: TFitSeries
          AxisIndexX = 1
          AxisIndexY = 0
          FitEquation = feExp
          OnFitComplete = Chart1FitSeries1FitComplete
          ParamCount = 2
          Source = ListChartSource1
        end
      end
      object ListChartSource1: TListChartSource
        DataPoints.Strings = (
          '1972|2300|?|4004'
          '1974|6000|?|8080'
          '1978|29000|?|8086'
          '1982|134000|?|80286'
          '1986|275000|?|80386'
          '1989|1200000|?|80486'
          '1993|3100000|?|Pentium'
          '1997|7500000|?|Pentium II'
          '2001|42000000|?|Xeon'
          '2006|152000000|?|Core Duo'
          '2009|731000000|?|Core i7'
        )
        left = 240
        top = 40
      end
      object ChartAxisTransformations1: TChartAxisTransformations
        left = 243
        top = 96
        object ChartAxisTransformations1LogarithmAxisTransform1: TLogarithmAxisTransform
          Base = 10
        end
      end
      object ListChartSource2: TListChartSource
        left = 243
        top = 176
      end
    end

---

_Source: [https://wiki.freepascal.org/TAChart_Tutorial%3A_ListChartSource%2C_Logarithmic_Axis%2C_Fitting/ru](https://web.archive.org/web/20240619103034/https://wiki.freepascal.org/TAChart_Tutorial%3A_ListChartSource%2C_Logarithmic_Axis%2C_Fitting/ru)_
