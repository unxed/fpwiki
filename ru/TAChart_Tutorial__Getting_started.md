# TAChart Tutorial: Getting started

│ **[English (en)](<../en/TAChart_Tutorial__Getting_started.md>)** │  **русский (ru)** │

## Contents

  * 1 Введение
  * 2 Что нужно?
  * 3 Новый проект
  * 4 Добавление серий
  * 5 Добавление данных
  * 6 Форматирование серий
  * 7 Добавление легенды
  * 8 Тонкая настройка
  * 9 Готово!
  * 10 Исходный код
    * 10.1 Файл проекта (Project file)
    * 10.2 Unit1.pas
    * 10.3 Файл формы Unit.lfm



## Введение

[![tachart getting started step6.png](https://wiki.freepascal.org/images/b/b0/tachart_getting_started_step6.png)](</File:tachart_getting_started_step6.png>)

Если вам хочется добавить график или диаграмму на форму, вам надо ознакомиться с [TAChart](<../en/TAChart.md> "TAChart"). Это стандартный пакет Lazarus для рисования графиков и диаграмм. 

В этом руководстве мы нарисуем с помощью TAChart несколько элементарных математических функций. Мы создадим диаграмму с тремя рядами данных (line series) для функций 

  * y=cos(x),
  * y=sin(x), и
  * y=cos(x)*sin(x).



Возможно здесь стоит пояснить слова "series" и "chart" (диаграмма): вся диаграмма в целом — это "chart", а каждая из функций будет отображаться в виде кривой, называемой "series". Мы соединим точки линейными отрезками и поэтому будем использовать название "line series". TAChart, конечно, позволяет использовать много других типов данных, например, столбики (bar series), площади (area series), круговые диаграммы (pie series), или даже такие продвинутые типы как сплайны или fit series. 

## Что нужно?

Это руководство будет очень элементарным. Конечно, вам необходимы базовые знания языка [Object Pascal](<../en/Object_Pascal.md> "Object Pascal") и Lazarus [IDE](<../en/IDE.md> "IDE"). 

TAChart включён в Lazarus, так что нет необходимости искать этот пакет где-то в Интернете. Однако не используйте слишком старые версии Lazarus, так как TAChart активно развивается и некоторые особенности могут меняться от версии к версии. 

Лучше всего изучите это руководство шаг за шагом. Но если хотите, можете использовать [исходный код](<../en/Source_code.md> "Source code") завершённого проекта в конце этой статьи. 

## Новый проект

[![TAChart component palette.png](https://wiki.freepascal.org/images/9/95/TAChart_component_palette.png)](</File:TAChart_component_palette.png>)

Для начала создадим в Lazarus новый проект и выберем в [палитре компонентов](<../en/Component_Palette.md> "Component Palette") страницу ["Chart"](<../en/Chart_tab.md> "Chart tab"). Кликнем по самой левой иконке, компоненту [TChart](<../en/TChart.md> "TChart"), и добавим его на форму. Увидите пустую диаграмму со стандартными осями X и Y. 

[![TAChart GettingStarted Step1.png](https://wiki.freepascal.org/images/6/63/TAChart_GettingStarted_Step1.png)](</File:TAChart_GettingStarted_Step1.png>)

## Добавление серий

Теперь давайте добавим серию. Для этого дважды кликнем по нашей диаграмме (или кликнем правой клавишей и выберем "Редактор диаграмм" в контекстном меню). Появится редактор серий TAChart. Он ещё пуст, но когда нажмём "Добавить", то увидим меню со всеми типами серий доступных в пакете TAChart. Выберем "График(line series)". Повторим дважды, чтобы создать три графика, приданных этой диаграмме. Они ещё не отображаются на диаграмме, потому что ещё не содержат данных. Нет данных — нет отображения. Мы поправим это за минуту. 

[![TAChart GettingStarted Step1a.png](https://wiki.freepascal.org/images/6/6f/TAChart_GettingStarted_Step1a.png)](</File:TAChart_GettingStarted_Step1a.png>)

Перед тем, как мы это сделаем, посмотрим на дерево инспектора объектов. TAChart использует довольно сложную архитектуру классов и контейнеров, поэтому важно понимать дерево объектов. В нашем проекте мы видим компонент TChart и его потомков: три графика (line series) и AxisList с левой(Left) и нижней (Bottom) осями тоже в качестве потомков. Имена графиков (series) созданы в соответствии с шаблоном `Chart<number><SeriesType><Number>`. 

[![tachart getting started object tree.png](https://wiki.freepascal.org/images/4/42/tachart_getting_started_object_tree.png)](</File:tachart_getting_started_object_tree.png>)

Почему бы нам не дать им более понятные имена, например, `SinSeries`, `CosSeries`, and `SinCosSeries`? Для этого кликните на каждой серии и поменяйте `Name` в соответствующем поле инспектора объектов. 

## Добавление данных

Теперь пора добавить данные. Простейший способ — создать данные вместе с формой, т.е. мы пишем обработчик для события формы `OnCreate`. Конечно, в «реальном» проекте, вы добавите данные по-другому, например, после нажатия кнопки, которое может запустить вычисления или чтение данных из файла. 
    
    
    procedure TForm1.FormCreate(Sender: TObject);
    const
      N = 100;
      MIN = -10;
      MAX = 10;
    var
      i: Integer;
      x: Double;
    begin
      for i:=0 to N-1 do begin
        x := MIN + (MAX - MIN) * i /(N - 1);
        SinSeries.AddXY(x, sin(x));
        CosSeries.AddXY(x, cos(x));
        SinCosSeries.AddXY(x, sin(x)*cos(x));
      end;
    end;
    

Эта [процедура](<../en/Procedure.md> "Procedure") создаёт 100 точек с данными на каждую серию. Значения x вычисляются эквидистантными между `MIN` и `MAX`, т.е. -10 and 10. Важными строками являются вызовы [метода](<../en/Method.md> "Method")`AddXY` по всем сериям. Этот метод берёт координаты x и y каждой точки и добавляет их во внутренний список. Существуют также перегруженные версии этих методов где можно передать текстовую метку для каждой точки, а также индивидуальный цвет, но нам здесь не нужна эта функция. 

Вышеупомянутый список — так называемый исходник диаграммы (chart source) — это класс [класс](<../en/Class.md> "Class"), который обеспечивает данные для построения диаграммы. Вы можете использовать встроенный источник данных (chart source), или связать серии с самостоятельным источником. На палитре компонентов есть много источников данных, например 

  * `TListChartSource` запоминает данные в списке (этот тип используется нашими сериями по умолчанию)
  * `TDBChartSource` обращается к записям и полям в базе данных
  * `TUserDefinedChartSource` даёт очень общий доступ к данным, хранимым, например, в массиве записей, определённых пользователем
  * `TCalculatedChartSource` позволяет анализировать одну серию и отображать результаты в другой серии.



Но достаточно об источниках диаграммы — встроенных источников достаточно для этого ознакомительного проекта. 

[![tachart getting started step2.png](https://wiki.freepascal.org/images/3/34/tachart_getting_started_step2.png)](</File:tachart_getting_started_step2.png>)

Скомпилируем программу. Мы видим три кривых, и ось x автоматически покрывает наш диапазон данных между -10 и 10. Ну не слишком плохо, но далеко от совершенства: диаграмма очень запутанная, нельзя различить индивидуальные линии, и трудно сказать какие серии какой функции принадлежат. 

## Форматирование серий

Почему бы не изменить цвет каждой линии? Для этого нужно найти серию в дереве объектов и в инспекторе объектов изменить [свойство](</Property> "Property") `SeriesColor` на желаемый вами — `SeriesColor` это цвет линии соединяющей точки. 

После перекомпиляции увидите окрашенные линии. Но пока мы не можем различить разные функции. Нам нужна легенда. 

## Добавление легенды

Выделите диаграмму и в инспекторе объектов прокрутите до пункта `Legend`, затем откройте вложенные свойства. В конце есть свойство `Visible`, которое по умолчанию `false` — установите его `true` и перекомпилируйте. 

[![tachart getting started step3.png](https://wiki.freepascal.org/images/6/6e/tachart_getting_started_step3.png)](</File:tachart_getting_started_step3.png>)

Вот это уже лучше. Не хватает только пояснительного текста к сериям, такого как `y = sin(x)`. Для этой цели каждая серия имеет свойство `Title` — текст, который отобразится в легенде вместе с символом соответствующей серии. Поэтому придётся заново пройтись по всем сериям и установить свойство `Title` в `y=sin(x)`, `y=cos(x)`, and `y=sin(x)*cos(x)` соответственно. 

[![tachart getting started step4.png](https://wiki.freepascal.org/images/6/67/tachart_getting_started_step4.png)](</File:tachart_getting_started_step4.png>) [![tachart getting started step4a.png](https://wiki.freepascal.org/images/c/c1/tachart_getting_started_step4a.png)](</File:tachart_getting_started_step4a.png>)

Теперь после компиляции диаграмма почти идеальна. Почти — потому что легенда довольно велика и диаграмма оказалась втиснутой в оставшееся пространство. Конечно мы могли бы увеличить размер формы. А не можем ли мы поместить легенду под диаграммой? Без проблем: возвращаемся к свойству диаграммы `Legend` и устанавливаем `Alignment` в `laBottomCenter`. Также надо установить `ColumnCount` значение `3`, иначе элементы легенды будут расположены не в линию, а столбцом. 

[![tachart getting started step5.png](https://wiki.freepascal.org/images/2/20/tachart_getting_started_step5.png)](</File:tachart_getting_started_step5.png>)

Ой! — третий элемент легенды не влез из-за недостаточной ширины окна. Выравняйте диаграмму по `alClient` и немного увеличьте ширину окна. 

## Тонкая настройка

Надо улучшить ещё несколько вещей: Оси должны иметь названия, типа "ось x" и "ось y", соответственно, а также должен быть заголовок сверху диаграммы. 

Для поименования оси x выберем в дереве объектов пункт `Bottom` дочернего компонента TChart — AxisList, или перейдём к `BottomAxis` в инспекторе объектов. Во вложенных свойствах найдём `Title`, который снова имеет вложенные свойства. Текст в поле `Caption` будет заголовком вверху диаграммы. Не забудьте установить свойство `Visible` в `true`. Можете также установить стиль шрифта заголовка жирным — это свойство находится под `LabelFont`. То же самое повторим с `LeftAxis`. 

Та же самая процедура может быть использована для установки заголовка диаграммы: прокрутим до свойства диаграммы `Title`, введём заголовок в поле `Text` (здесь можете даже использовать многострочный текст), и снова установим свойство `Visible` в `true`. 

И наконец, если вы предпочитаете использовать белый фон области диаграммы, он изменяется свойством `BackColor` компонента TChart. А если сетка должна быть менее заметной, то она лучше будет выглядеть цветом `clSilver`. Используем свойства LeftAxis и BottomAxis `Grid.Color` для их изменения. 

## Готово!

В итоге наша диаграмма выглядит вот так. Мило, да? И это было совсем не трудно. 

Если хотите узнать побольше о TAChart, посмотрите официальную [ документацию по TAChart](<TAChart_documentation.md> "TAChart documentation/ru"), и изучите некоторые из многих демо-примеров, которые идут вместе с TAChart. Они находятся в папке `components\tachart\demo` дистрибутива Lazarus. 

[![tachart getting started step6.png](https://wiki.freepascal.org/images/b/b0/tachart_getting_started_step6.png)](</File:tachart_getting_started_step6.png>)

## Исходный код

Исходный код этого учебного проекта находится в папке _tutorials/getting_started_ дистрибутива TAChart. 

### Файл проекта (Project file)
    
    
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
      RequireDerivedFormResource := True;
      Application.Initialize;
      Application.CreateForm(TForm1, Form1);
      Application.Run;
    end.
    

### Unit1.pas
    
    
    unit Unit1;
    
    {$mode objfpc}{$H+}
    
    interface
    
    uses
      Classes, SysUtils, FileUtil, TAGraph, TASeries, Forms, Controls, Graphics,
      Dialogs;
    
    type
    
      { TForm1 }
    
      TForm1 = class(TForm)
        Chart1: TChart;
        SinSeries: TLineSeries;
        CosSeries: TLineSeries;
        SinCosSeries: TLineSeries;
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
    
    { TForm1 }
    
    procedure TForm1.FormCreate(Sender: TObject);
    const
      N = 100;
      MIN = -10;
      MAX = 10;
    var
      i: Integer;
      x: Double;
    begin
      for i:=0 to N - 1 do begin
        x := MIN + (MAX - MIN) * i / (N - 1);
        SinSeries.AddXY(x, sin(x));
        CosSeries.AddXY(x, cos(x));
        SinCosSeries.AddXY(x, sin(x)*cos(x));
      end;
    
    end;
    
    end.
    

### Файл формы Unit.lfm
    
    
    object Form1: TForm1
      Left = 14
      Height = 244
      Top = 137
      Width = 347
      Caption = 'Form1'
      ClientHeight = 244
      ClientWidth = 347
      OnCreate = FormCreate
      LCLVersion = '1.1'
      object Chart1: TChart
        Left = 0
        Height = 244
        Top = 0
        Width = 347
        AxisList = <    
          item
            Grid.Color = clSilver
            Minors = <>
            Title.LabelFont.Orientation = 900
            Title.LabelFont.Style = [fsBold]
            Title.Visible = True
            Title.Caption = 'y axis'
          end    
          item
            Grid.Color = clSilver
            Alignment = calBottom
            Minors = <>
            Title.LabelFont.Style = [fsBold]
            Title.Visible = True
            Title.Caption = 'x axis'
          end>
        BackColor = clWhite
        Foot.Brush.Color = clBtnFace
        Foot.Font.Color = clBlue
        Legend.Alignment = laBottomCenter
        Legend.ColumnCount = 3
        Legend.Visible = True
        Title.Brush.Color = clBtnFace
        Title.Font.Color = clBlue
        Title.Font.Style = [fsBold]
        Title.Text.Strings = (
          'My first chart'
        )
        Title.Visible = True
        Align = alClient
        ParentColor = False
        object SinSeries: TLineSeries
          Title = 'y=sin(x)'
          LinePen.Color = clRed
        end
        object CosSeries: TLineSeries
          Title = 'y=cos(x)'
          LinePen.Color = clBlue
        end
        object SinCosSeries: TLineSeries
          Title = 'y=sin(x)*cos(x)'
          LinePen.Color = clGreen
        end
      end
    end

---

_Source: [https://wiki.freepascal.org/TAChart_Tutorial%3A_Getting_started/ru](https://web.archive.org/web/20231203205616/https://wiki.freepascal.org/TAChart_Tutorial%3A_Getting_started/ru)_
