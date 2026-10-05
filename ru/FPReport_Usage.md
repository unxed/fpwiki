# FPReport Usage

│ **[English (en)](<../en/FPReport_Usage.md> "FPReport Usage")** │  **русский (ru)** │    
****

На этой странице мы рассмотрим, как создать простой отчет в коде. В качестве отправной точки мы возьмем демонстрационный отчет dataset из демонстрационного приложения Free Pascal. 

## Contents

  * 1 Владельцы и родительско-дочерняя структура
  * 2 Создание отчета
    * 2.1 Предоставление данных для отчета
    * 2.2 Добавление страницы
    * 2.3 Добавление бэндов на страницу
    * 2.4 Данные цикла
    * 2.5 Выражения
      * 2.5.1 Встроенные переменные
      * 2.5.2 Встроенные выражения
    * 2.6 Поддержка изображений
  * 3 Запуск отчета
  * 4 Визуализация (или экспорт) отчета
  * 5 Полный пример кода
    * 5.1 Требования
    * 5.2 Исходный код
  * 6 Загрузка и сохранение отчета из/в файла
    * 6.1 Загрузка и сохранение вручную
    * 6.2 Самый простой способ: использовать TFPJSONReport
    * 6.3 Управление данными
  * 7 Вернуться на главную страницу FP Report



## Владельцы и родительско-дочерняя структура

Все элементы отчета (страницы, бэнды, элементы, доступные для печати) являются наследниками TComponent. Они организованы по принципу "родитель-потомок": 

  * Страницы являются дочерними элементами отчета
  * Бэнды являются дочерними элементами страницы
  * Элементы, доступные для печати, являются дочерними элементами бэнда.



Владелец (как в TComponent) различных элементов не важен для структуры отчета, но если владелец является естественным родительским элементом вновь созданного компонента, то родительский элемент будет установлен автоматически. 

## Создание отчета

Большинство классов, связанных с fpReport, определены в одном модуле, **fpreport**. Поэтому обязательно добавьте это в раздел uses. 
    
    
    uses
      SysUtils
      ,fpreport           // для большинства классов отчетов FPReport
      ,dbf                // для доступа к DBF (базе данных)
      ,fpreportdb         // для класса TFPReportDatasetData
      ,fpttf              // для доступа к gTTFontCache singleton
      {$IFDEF ExportPDF}
      ,fpreportpdfexport  // для доступа к классу экспортера отчетов в формате PDF
      {$ENDIF}
      {$IFDEF ExportAggPas}
      ,fpreport_export_aggpas // для доступа к классу экспортера fpReport AggPas в fpGUI
      {$ENDIF}
      {$IFDEF ExportFPImage}
      ,fpreportfpimageexport  // для доступа к классу FP Image exporter
      {$ENDIF}
      ;
    

Все начинается с компонента отчета: 
    
    
    PaperManager.RegisterStandardSizes; // регистрация типов бумаги для fpReport
    rpt := TFPReport.Create(nil); // если ваш объект Application является потомком TComponent, используйте Self вместо nil.
    rpt.Author := 'Graeme Geldenhuys';
    rpt.Title := 'FPReport Demo 8 - Datasets';
    

Свойства **Author** и **Title** могут использоваться в выражениях отчета. 

### Предоставление данных для отчета

Для отображения каждого отчета необходим цикл обработки данных. Цикл обработки данных является производным от **TFPReportData** , существует несколько предопределенных циклов обработки данных. 

Один из возможных циклов обработки данных основан на наборе данных, **TFPReportDatasetData** \- определен в модуле **fpreportdb** , поэтому не забудьте добавить его в раздел uses. У него есть свойство **Dataset** , которое должно быть присвоено набору данных, предоставляющему данные для отчета. Цикл будет выполняться по всем записям в наборе данных, и все поля в наборе данных будут доступны для использования в выражениях, используемых в отчете. 

Следующий код создает компонент цикла обработки данных и назначает ему набор данных **TDBF**. Обратите внимание, что класс DBF определен в модуле **dbf**. 
    
    
    var
      lDataSet: TDBF;
      lReportData: TFPReportDatasetData;
    begin
      lReportData := TFPReportDatasetData.Create(nil);  // То же, что и в предыдущем примере кода. Используйте Self
      lDataSet := TDBF.Create(nil);                     // или Nil, в зависимости от объекта Application
      lDataSet.TableName := 'test.dbf';
      lReportData.DataSet := lDataSet;
    

Затем компонент **lReportData** может быть использован в структуре отчета. 

### Добавление страницы

Для каждого отчета требуется как минимум одна страница. 

Следующая страница принадлежит отчету, поэтому она будет автоматически добавлена к страницам отчета. После создания необходимо задать размер страницы (доступен набор стандартных размеров), это делается с помощью свойства `PageSize.PaperName`. Если название известно движку global paper manager, то размеры будут установлены автоматически на основе названия. 
    
    
    p := TFPReportPage.Create(rpt);
    p.Orientation := poPortrait;
    p.PageSize.PaperName := 'A4';
    

После того, как было задано название страницы, можно задать поля. Размеры указаны в миллиметрах: 
    
    
    { поля страницы }
    p.Margins.Left := 30;
    p.Margins.Top := 20;
    p.Margins.Right := 30;
    p.Margins.Bottom := 20;
    

Страница может содержать шрифт. Этот шрифт затем используется по умолчанию для всех полос на странице. Аналогично, все элементы на полосе будут использовать шрифт полосы по умолчанию. Обратите внимание, что название шрифта - это PostScript-название файла шрифтов TTF. 
    
    
    p.Font.Name := 'LiberationSans';
    

Когда отчет должен быть отрисован, модуль компоновки запустит цикл обработки данных страницы и будет повторять просмотр страницы столько раз, сколько потребуется для соответствия данным. 

Следовательно, со страницей должен быть связан цикл обработки данных. 
    
    
    p.Data := lReportData;
    

### Добавление бэндов на страницу

Теперь страница должна быть заполнена каким-либо контентом. Это означает добавление нескольких полос на страницу отчета. 

Заголовок отчета будет напечатан один раз, в начале отчета: 
    
    
    TitleBand := TFPReportTitleBand.Create(p);
    TitleBand.Layout.Height := 40;
    

Ширину полосы задавать не нужно, она рассчитывается автоматически исходя из ширины страницы, количества колонок и полей страницы. 

Сама по себе полоса не очень полезна, она служит только в качестве заполнителя для некоторых элементов, которые можно распечатать. 

Для титульной страницы мы добавим простой статический текст в качестве заголовка страницы. Весь текст (динамический или статический) должен быть добавлен с помощью компонента **TFPReportMemo** : 
    
    
      Memo := TFPReportMemo.Create(TitleBand);
      Memo.Layout.Left := 5;
      Memo.Layout.Top := 0;
      Memo.Layout.Width := 140;
      Memo.Layout.Height := 15;
    

Расположение memo относительно полосы и определяет, где она будет размещена. 

Текст memo и внутреннее форматирование можно задать с помощью свойств **Text** и **TextAlignment** : 
    
    
      Memo.Text := 'Dataset Demo';
      Memo.TextAlignment.Vertical := tlCenter;
      Memo.TextAlignment.Horizontal := taCentered;
    

Наконец, можно настроить шрифт и его размер: 
    
    
      Memo.UseParentFont := False;
      Memo.Font.Color := TFPReportColor($000080);
      Memo.Font.Size := 24;
    

Цвет имеет значение RRGGBB (красный/зеленый/синий). Поддержка альфа-канала пока недоступна. В модуле FP Report задано несколько предопределенных значений. 

### Данные цикла

Заголовок отчета печатается только один раз, но обычно отчет содержит бэнд, который печатается для каждой записи в цикле отчета. Для этого в отчет необходимо добавить бэнд данных (**TFPReportDataBand**): 
    
    
    DataBand := TFPReportDataBand.Create(p);
    DataBand.Layout.Height := 30;
    DataBand.Data:= lReportData;
    

Этот бэнд будет повторяться для каждой записи в цикле обработки данных. 

Как отмечалось выше, любой текст должен быть напечатан с memo. Это также относится к данным из цикла обработки данных. В следующем memo будет напечатано имя поля из набора данных, дополненное буквальным текстом **"Name: "**
    
    
      Memo := TFPReportMemo.Create(DataBand);
      Memo.Layout.Left := 30;
      Memo.Layout.Top := 0;
      Memo.Layout.Width := 50;
      Memo.Layout.Height := 5;
      Memo.Text := 'Name: [name]';
    

### Выражения

Динамический текст получается путем смешивания статического текста и выражений. 

Выражения - это все, что заключено в квадратные скобки **[]**. 

  
Для вычисления данных используется механизм анализа выражений **TFPExpressionParser**. 

Это означает, что выражения во многом похожи на выражения Pascal. Выражение: 

  * Типизировано (string, integer, float, datetime)
  * может содержать вычисления для этих типов.
  * может содержать переменные отчета
  * может использовать поля данных из данных отчета
  * Может использовать любую из предопределенных функций, доступных в fpexprpars.



#### Встроенные переменные

Эти переменные создаются во фрипорте и всегда определены 

  * **[TODAY]**
  * **[AUTHOR]**
  * **[TITLE]**



#### Встроенные выражения

Это выражение создается во фрипорте и всегда предопредено 

  * **[RecNo]**
  * **[PageNo]** Фактический номер страницы
  * **[PageCount]** Максимальное количество страниц (подсказка: для работы необходимо активировать два прохода)
  * **[ColNo]** Фактический столбец
  * **[PageNoPerDesignerPage]**
  * **[InRepeatedGroupHeader]**
  * **[InIntermediateGroupFooter]**
  * **[IsOverflowed]**
  * **[IsGroupDetailPrinted]**
  * **[FieldIsNull]**



### Поддержка изображений

Если набор данных содержит большое двоичное поле с изображением, то его также можно распечатать: 
    
    
      Image := TFPReportImage.Create(DataBand);
      Image.Layout.Top := 0;
      Image.Layout.Left := 10;
      Image.Layout.Height := 20;
      Image.Layout.Width := 14.8;
      Image.FieldName := 'Photo';
      Image.Stretched := True;
    

Для этого достаточно присвоить свойству FieldName значение имени поля, содержащего данные изображения. 

При желании можно также добавить фиксированное изображение: 
    
    
      Image := TFPReportImage.Create(TitleBand);
      Image.Layout.Left := 0;
      Image.Layout.Top := 0;
      Image.Layout.Width := 40;
      Image.Layout.Height := 30;
      Image.LoadFromFile('company-logo.png');
      Image.Stretched := True;
    

## Запуск отчета

Для компоновки отчета можно использовать метод RunReport: 
    
    
    { укажите, какие каталоги следует использовать для поиска шрифтов TrueType }
    gTTFontCache.SearchPath.Add(cFCLReportDemosLocation + '/fonts/');
    gTTFontCache.BuildFontCache;
    
    rpt.RunReport;
    

В результате отчет будет создан в памяти. 

Он не отображается на экране и не сохраняется в файл. 

## Визуализация (или экспорт) отчета

TO DO [поправить корректность внешней ссылки] 

Для реального просмотра отчета его необходимо отрисовать или экспортировать. Предварительный просмотр отчета также является экспортом. 

Существуют различные экспортеры: 

  * [PDF](<../en/PDF.md> "PDF") экспорт
  * FPImage экспорт (без субпиксельного рендеринга)
  * Экспортер AggPas (субпиксельный рендеринг и сглаживание). Рекомендуется использовать код [AggPas](<https://github.com/graemeg/fpGUI/tree/develop/src/corelib/render/software>), содержащийся в репозитории кода [fpGUI](<../en/fpGUI.md> "fpGUI").
  * HTML экспорт
  * LCL canvas экспорт, используемый в качестве основы для предварительного просмотра LCL
  * [fpGUI](<../en/fpGUI.md> "fpGUI") canvas экспорт, используется в качестве основы для экспортера предварительного просмотра fpGUI



Таким образом, предварительный просмотр или сохранение макета отчета - это всего лишь вопрос создания правильного экспорта и вызова метода отчета **RenderReport** : 
    
    
    RptExporter := TFPReportExportPDF.Create(nil); // как и прежде, используйте Self или Nil в зависимости от класса приложения
    rpt.RenderReport(RptExporter);
    

## Полный пример кода

### Требования

Этот пример может быть скомпилирован с помощью FPC 2.6.4 или более поздней версии. 

Для компиляции этого кода вам необходимо указать следующие пути поиска модулей: 

  * /data/devel/fpc-3.1.1/src/packages/fcl-report/src/
  * ../../fpgui/src/fpreport
  * ../../fpgui/src/corelib/render/software



Первый путь поиска - это каталог **fcl-report** из транка FPC. 

Второй путь поиска (необязательный) - это путь к экспортеру AggPas, как определено в репозитории кода [fpGUI](<../en/fpGUI.md> "fpGUI"). 

Третий путь поиска (необязательный) - к исходному коду библиотеки AggPas, который находится в репозитории кода fpGUI. 

Вам также необходимо убедиться, что константа **cFCLReportDemosLocation** , определенная в исходном коде, указывает на правильный каталог fcl-report/demo/, который находится в вашей собственной системе. 

### Исходный код
    
    
    program project1;
    
    {$mode objfpc}{$H+}
    
    {  Какой экспортер вы хотите использовать. Выберите любой из них или все. Если вы включили AggPas
      экспортер, не забудьте добавить дополнительные пути поиска объектов в настройки вашего проекта. }
    {$define ExportPDF}
    {$define ExportFPImage}
    {$define ExportAggPas}
    
    uses
      SysUtils
      ,fpreport           // для большинства классов  FPReport
      ,dbf                // для доступа к DBF (базе данных)
      ,fpreportdb         // для класса TFPReportDatasetData 
      ,fpttf              // для доступа к gTTFontCache singleton
      {$IFDEF ExportPDF}
      ,fpreportpdfexport  // для доступа к классу экспорта отчетов в формате PDF
      {$ENDIF}
      {$IFDEF ExportAggPas}
      ,fpreport_export_aggpas // для доступа к классу экспорта fpReport AggPas в fpGUI
      {$ENDIF}
      {$IFDEF ExportFPImage}
      ,fpreportfpimageexport  // для доступа к классу FP Image exporter
      {$ENDIF}
      ;
    
    const
      cFCLReportDemosLocation = '/data/devel/fpc-3.1.1/src/packages/fcl-report/demos/';
    
    var
      rpt: TFPReport;
      lDataSet: TDBF;
      lReportData: TFPReportDatasetData;
      p: TFPReportPage;
      TitleBand: TFPReportTitleBand;
      Memo: TFPReportMemo;
      DataBand: TFPReportDataBand;
      Image: TFPReportImage;
      RptExporter: TFPReportExporter;
    begin
      RptExporter := nil;
      if not FileExists(cFCLReportDemosLocation + 'test.dbf') then
      begin
        writeln('Не удается найти файл базы данных <' + cFCLReportDemosLocation + 'test.dbf>');
        writeln('Пожалуйста, запустите демо набора данных fcl-report хотя бы один раз.');
        writeln('');
        exit;
      end;
    
      // ***** Создание отчета *****
      PaperManager.RegisterStandardSizes;
      rpt:=TFPReport.Create(nil);
      rpt.Author := 'Graeme Geldenhuys';
      rpt.Title := 'FPReport Demo 8 - Datasets';
      try
        // ***** Предоставление данных для отчета *****
        lReportData := TFPReportDatasetData.Create(nil);
        lDataSet := TDBF.Create(nil);
        lDataSet.TableName := cFCLReportDemosLocation + 'test.dbf';
        lReportData.DataSet := lDataSet;
    
        // ***** Добавление страницы *****
        p := TFPReportPage.Create(rpt);
        p.Orientation := poPortrait;
        p.PageSize.PaperName := 'A4';
    
        { поля страницы }
        p.Margins.Left := 30;
        p.Margins.Top := 20;
        p.Margins.Right := 30;
        p.Margins.Bottom := 20;
    
        { шрифт страницы по умолчанию }
        p.Font.Name := 'LiberationSans';  // это название шрифта TTF в формате PostScript
    
        { назначение данных для цикла обработки данных страницы }
        p.Data := lReportData;
    
        // ***** Добавление бэндов на страницу *****
        TitleBand := TFPReportTitleBand.Create(p);
        TitleBand.Layout.Height := 40;
    //    TitleBand.Frame.Shape := fsRectangle;
    //    TitleBand.Frame.BackgroundColor := clYellow;
    
        Memo := TFPReportMemo.Create(TitleBand);
        Memo.Layout.Left := 5;
        Memo.Layout.Top := 0;
        Memo.Layout.Width := 140;
        Memo.Layout.Height := 15;
    
        Memo.Text := 'Dataset Demo';
        Memo.TextAlignment.Vertical := tlCenter;
        Memo.TextAlignment.Horizontal := taCentered;
    
        Memo.UseParentFont := False;
        Memo.Font.Color := TFPReportColor($000080);
        Memo.Font.Size := 24;
    
        // ***** The loop data *****
        DataBand := TFPReportDataBand.Create(p);
        DataBand.Layout.Height := 30;
        DataBand.Data:= lReportData;
    //    DataBand.Frame.Shape := fsRectangle;
    //    DataBand.Frame.BackgroundColor := clCream;
    
        Memo := TFPReportMemo.Create(DataBand);
        Memo.Layout.Left := 30;
        Memo.Layout.Top := 0;
        Memo.Layout.Width := 50;
        Memo.Layout.Height := 5;
        Memo.Text := 'Name: [name]';
    
        // ***** Поддержка изображений *****
        Image := TFPReportImage.Create(DataBand);
        Image.Layout.Top := 0;
        Image.Layout.Left := 10;
        Image.Layout.Height := 20;
        Image.Layout.Width := 14.8;
        Image.FieldName := 'Photo';
        Image.Stretched := True;
    
        Image := TFPReportImage.Create(TitleBand); // обратите внимание, что это изображение размещено на титульной полосе
        Image.Layout.Left := 0;
        Image.Layout.Top := 0;
        Image.Layout.Width := 40;
        Image.Layout.Height := 30;
        Image.LoadFromFile(cFCLReportDemosLocation + 'pictures/woman01.png');
        Image.Stretched := True;
    
        // ***** Запуск отчета *****
        { укажите, какие каталоги следует использовать для поиска шрифтов TrueType }
        gTTFontCache.SearchPath.Add(cFCLReportDemosLocation + '/fonts/');
        gTTFontCache.BuildFontCache;
    
        rpt.RunReport;
    
        // ***** Визуализация (или экспорт) отчета *****
        {$IFDEF ExportPDF}
        if Assigned(RptExporter) then
          FreeAndNil(RptExporter);
        RptExporter := TFPReportExportPDF.Create(nil);
        rpt.RenderReport(RptExporter);
        {$ENDIF}
        {$IFDEF ExportAggPas}
        if Assigned(RptExporter) then
          FreeAndNil(RptExporter);
        RptExporter := TFPReportExportAggPas.Create(nil);
        TFPReportExportAggPas(RptExporter).BaseFileName := ApplicationName + '-aggpas-%.2d.png';
        rpt.RenderReport(RptExporter);
        {$ENDIF}
        {$IFDEF ExportFPImage}
        if Assigned(RptExporter) then
          FreeAndNil(RptExporter);
        RptExporter := TFPReportExportfpImage.Create(nil);
        TFPReportExportfpImage(RptExporter).BaseFileName := ApplicationName + '-fpimage-.png';
        rpt.RenderReport(RptExporter);
        {$ENDIF}
    
      finally
        // Freeing all objects we used
        FreeAndNil(RptExporter);
        FreeAndNil(rpt);
        FreeAndNil(lReportData);
        FreeAndNil(lDataset);
      end;
    end.
    

## Загрузка и сохранение отчета из/в файла

Как объяснено на вводной странице [FPReport](<../en/FPReport.md> "FPReport"), класс **TFPReport** сам по себе не имеет понятия о файлах. Он знает только стример (**TFPReportStreamer**). FPC предоставляет JSON-стример **TFPReportJSONStreamer** , есть также более старый XML-стример, который в настоящее время не тестировался. 

### Загрузка и сохранение вручную

В следующем коде показано, как использовать стример отчетов TFP в формате JSON для сохранения отчета в файл: 
    
    
    Var
      J : TFPReportJSONStreamer;
      F : TFileStream;
      S : TJSONStringType;
    begin
      F:=Nil;
      J:=TFPReportJSONStreamer.Create(Self);
      try
        FReport.WriteElement(J);
        F:=TFileStream.Create('txt2pdf.fpr',fmCreate);
        S:=J.JSON.FormatJSON();
        F.WriteBuffer(S[1],Length(S));
      finally  
        F.Free;
        J.Free;
      end;
    end;
    

И следующий код загружает его снова из файла: 
    
    
    procedure TPrintApplication.LoadReportDesign;
    
    Var
      J : TFPReportJSONStreamer;
      F : TFileStream;
      O : TJSONObject;
    
    begin
      J:=Nil;
      F:=TFileStream.Create('txt2pdf.fpr',fmOpenRead);
      try
        O:=GetJSON(F) as TJSONObject;
        J:=TFPReportJSONStreamer.Create(Self);
        J.JSON:=O;
        J.OwnsJSON:=True;
        FReport.ReadElement(J);
      finally
        F.Free;
        J.Free;
      end;
    end;
    

### Самый простой способ: использовать TFPJSONReport

Модуль _fpjsonreport_ содержит класс **TFPJSONReport**. Он объединяет стример JSON и отчет TFP и упрощает его загрузку/сохранение. Модуль _fpjsonreport_ содержит потомка **TFPJSONReport** , который выполняет все вышеперечисленное с помощью нескольких простых методов: 
    
    
    TFPJSONReport = class(TFPReport)
      procedure LoadFromStream(const aStream: TStream);
      procedure SaveToStream(const aStream: TStream);
      Procedure LoadFromJSON(aJSON : TJSONObject); virtual;
      Procedure SavetoJSON(aJSON : TJSONObject); virtual;
      Procedure LoadFromFile(const aFileName : String);
      Procedure SaveToFile(const aFileName : String);
      Property DataManager : TFPCustomReportDataManager;
    end;
    

Он также может сохранять дизайн в формате JSON в файле lazarus.lfm, поэтому, если вы хотите использовать отчет, в котором информация о дизайне хранится в файле .lfm, используйте **TJSONFPReport** в lazarus. 

### Управление данными

Класс **TFPReportDataManager** может использоваться для создания наборов данных и управления ими на основе определений, найденных в файле JSON, созданном автономным разработчиком. 

(на самом деле автономный разработчик использует **TFPReportDataManager** для управления своими данными). 

Когда вы используете **TFPJSONReport** , вы можете присвоить его свойству **DataManager** значение экземпляра **TFPReportDataManager**. 

При загрузке/сохранении данных он будет использовать диспетчер данных для добавления и/или интерпретации дополнительной информации в JSON-файле отчета для создания наборов данных. 

Если набор данных не открывается по какой-либо причине (нет подключения к базе данных или по другим причинам), свойство **LoadErrors** (типа **TStrings**) будет содержать сообщения об ошибках. 

Менеджер данных использует движок для создания необходимых наборов данных. Для этого в исполняемом файле должны быть зарегистрированы обработчики. Доступны следующие модули с обработчиками данных: 

  * **fpreportdatacsv** для поддержки CSV.
  * **fpreportdatadbf** для поддержки dbf.
  * **fpreportdatajson** для поддержки файла данных JSON.
  * **fpreportdatasqldb** для поддержки SQLDB.



You must make sure to include the units for the data formats that you need in your project file. If you fail to do so, you will get errors saying that an unknown data type is used. 

Вы должны убедиться, что в вашем файле проекта указаны модули для нужных вам форматов данных. Если вы этого не сделаете, вы получите сообщение об ошибке, указывающее на то, что используется неизвестный тип данных. 

## Вернуться на главную страницу FP Report

  * [FPReport](<FPReport.md> "FPReport/ru")

---

_Source: [https://wiki.freepascal.org/FPReport_Usage/ru](https://web.archive.org/web/20250429010203/https://wiki.freepascal.org/FPReport_Usage/ru)_
