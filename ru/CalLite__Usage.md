# CalLite: Usage

│ **[English (en)](<../en/CalLite__Usage.md>)** │  **русский (ru)** │

## Contents

  * 1 Использование
    * 1.1 Изменения цвета
    * 1.2 Использование свойства TCalOption календаря
    * 1.3 Смена языка
    * 1.4 События
    * 1.5 Праздники
      * 1.5.1 Подсказки для праздников
      * 1.5.2 Вписанные имена праздников
      * 1.5.3 Праздники в popup-меню
    * 1.6 Рисование иконок для определенных дней



# Использование

[![CalLite](https://wiki.freepascal.org/images/5/59/callite.png)](</File:callite.png> "CalLite")

Просто поместите компонент [TCalendarLite](<CalLite.md> "CalLite/ru") на форму и получите функциональный календарь на месяц. 

## Изменения цвета

Свойство `Colors` календаря содержит все настройки, влияющие на цвета, используемые для различных элементов: 

  * **ArrowBorderColor** : цвет границы стрелки над календарем (по умолчанию: clSilver  )
  * **ArrowColor** : цвет заливки стрелок над календарем (по умолчанию: clSilver  )
  * **BackgroundColor** : цвет фона всего календаря (по умолчанию: clWhite  )
  * **BorderColor** : цвет необязательной границы вокруг календаря (по умолчанию: clSilver  ). Добавьте `coShowBorder` в `Options` календаря, чтобы отобразить границу.
  * **DayLineColor** : цвет необязательной разделительной линии между верхней панелью навигации и областью дня календаря (по умолчанию: clSilver  ). Добавьте `coDayLine` в `Options` календаря, чтобы отобразить строку.
  * **HolidayColor** : цвет текста, используемый для отображения праздников (по умолчанию: clRed  ). Добавьте обработчик событий в `OnGetHolidays`, чтобы задать, какие дни являются выходными.
  * **PastMonthColor** : цвет текста, используемый для отображения дней предыдущего или следующего месяца (по умолчанию: clSilver  )
  * **SelectedDateColor** : цвет заливки текущего выбранного дня (по умолчанию: clMoneyGreen  )
  * **TextColor** : цвет текста, используемый для отрисовки обычных календарных дней (по умолчанию: clBlack  )
  * **TodayFrameColor** : цвет, используемый для отрисовки прямоугольной линии вокруг ячейки "сегодня" (по умолчанию: clLime  )
  * **TopRowColor** : цвет фона, используемый для верхней строке навигации (по умолчанию: clHighlight)
  * **TopRowTextColor** : цвет текста, используемый в верхней строке навигации (по умолчанию: clHighlightText)
  * **WeekendColor** : цвет текста, используемый для отрисовки выходных дней (по умолчанию: clRed  )



## Использование свойства TCalOption календаря

Отображение календаря можно изменить, модифицируя свойство `TCalOption` календаря, представленного как перечисление флагов: 
    
    
    type
      TCalOption = (coBoldDayNames, coBoldHolidays, coBoldToday, coBoldTopRow,
                    coBoldWeekend, coDayLine, coShowBorder, coShowHolidays,
                    coShowTodayFrame, coShowTodayName, coShowTodayRow,
                    coShowWeekend, coUseTopRowColors);
    

  * **coBoldDayNames** : Рисует строку с названиями дней, используя символы с жирным начертанием.
  * **coBoldHolidays** : Рисует число дней праздников, используя символы с жирным начертанием. Обычно праздничные дни отключены, но вы добавляете обработчик событий в `OnGetHolidays` для определения выходных.
  * **coBoldToday** : Рисует сегодняшнюю ячейку, используя символы с жирным начертанием.
  * **coBoldTopRow** : Рисует название месяца и номер года в верхнем ряду, используя символы с жирным начертанием.
  * **coBoldWeekend** : Рисует ячейки выходных дней, используя символы с жирным начертанием.
  * **coDayLine** : Добавляет разделительную линию между верхней навигационной линией и областью дней календаря.
  * **coShowBorder** : Рисует тонкий прямоугольник вокруг календаря.
  * **coShowHolidays** : Активирует подсветку праздников. Обратите внимание, что для `OnGetHolidays` также должен быть задан обработчик событий, который определяет праздники.
  * **coShowTodayFrame** : Рисует прямоугольник вокруг ячейки «сегодня».
  * **coShowTodayName** : Добавляет название дня в строку, отображающую сегодняшнюю дату (см. также: `coShowTodayRow`).
  * **coShowTodayRow** : Показывает строку внизу календаря для отображения сегодняшней даты (см. также: `coShowTodayName`).
  * **coShowWeekend** : Подсветка выходных дней (см. также: `Colors.WeekendColor`).
  * **coUseTopRowColors** : Рисует фон и текст верхней строки навигации, используя цвета, заданные в `Colors.TopRowColor` и `Colors.TopRowTextColor`.



## Смена языка

Строки, используемые компонентом, могут быть легко переведены на множество языков путем изменения значения свойства `Languages`: 
    
    
    type
      TLanguage = (lgEnglish, lgFrench, lgGerman, lgHebrew, lgSpanish, lgItalian,
                   lgPolish, lgFinnish);
    

## События

Помимо стандартных событий TCalendarLite запускает следующие события. Некоторые события получают номер года, месяца и дня окрашенной ячейки в качестве параметра. Набор флагов состояния указывает, является ли день отмеченным, текущим или принадлежит предыдущему или следующему месяцу: 
    
    
    type
      TCalCellState = (csSelectedDay, csToday, csOtherMonth);
      TCalCellStates = set of TCalCellState;
    

  * **OnDateChange** : Срабатывает всякий раз, когда в календаре выбрана другая дата.
  * **OnDrawCell** : Может использоваться для полного или частичного переопределения процесса окраски ячейки дня. Если для логического параметра `AContinueDrawing` установлено значение `true`, нормальный процесс рисования продолжается после выхода из обработчика события; если оно `false`, то отрисовка по умолчанию этого дня не выполняется. Это событие можно использовать, например, для добавления значков дня рождения или праздника в ячейки дня.
  * **OnGetDayText** : Может использоваться для изменения отображения текста на указанный день. По умолчанию используется строка, показывающая значение дня. Используя это событие, например, можно добавить названия праздников в календарь.
  * **OnGetHolidays** : Задает, какие дни текущего месяца являются выходными. Номер дня выходных кодируется в виде установленных битов в 32-разрядном целом числе.
  * **OnHint** : Определяет подсказку, которая может отображаться во всплывающем окне подсказок для каждого дня. Полезно, например, для отображения названия праздников, если указатель мыши находится над праздником.
  * **OnMonthChange** : Срабатывает всякий раз, когда в календаре отображается другой месяц. Может использоваться для более эффективного расчета отпуска.
  * **OnPrepareCanvas** : Вызывается непосредственно перед окрашиванием ячейки. Может использоваться для переопределения свойств кисти и пера, например, для рисования различных фонов для встреч и важных событий.



## Праздники

Календарь не «знает» ни одного праздника. Вы должны написать обработчик событий, чтобы определить, какие дни в текущем месяце являются выходными. Значения месяца и года соответствующего месяца передаются в качестве параметров. Информация о том, что день является выходным или нет, кодируется как установленные биты в 32-разрядном целом числе. Модуль _callite_ упрощает это, предоставляя следующие универсальные процедуры: 

  * procedure **AddHoliday**(ADay: Integer; var AHolidays: THolidays): Добавляет указанный день в список бит праздников в параметре `AHolidays`.
  * function **IsHoliday**(ADay: Integer; AHolidays: THolidays): Boolean: Проверяет, указан ли указанный день в `AHolidays`.
  * procedure **ClearHolidays**(var AHolidays: THolidays): Очищает список праздников.



Вот простой пример определения праздников Нового Года, Рождества, Пасхи и Троицы: 
    
    
    function Easter(year:integer): TDateTime; 
    var
      Day, Month    : integer;
      a,b,c,d,e,m,n : integer;
    begin
      case Year div 100 of
        17    : begin m := 23; n := 3; end;
        18    : begin m := 23; n := 4; end;
        19,20 : begin m := 24; n := 5; end;
        21    : begin m := 24; n := 6; end;
        else    raise Exception.Create('Поддерживается дата только после 1700 года.');
      end;
      a := Year mod 19;
      b := Year mod 4;
      c := Year mod 7;
      d := (19*a + m) mod 30;
      e := (2*b + 4*c + 6*d + n) mod 7;
      day := 22 + d + e;
      Month := 3;
      if Day>31 then begin
        Day := d + e - 9;
        Month := 4;
        if (d=28) and (e=6) and (a>10) then begin
          if day=26 then day := 19;
          if day=25 then day := 18;
        end;
      end;
      result := EncodeDate(year, month, day);
    end;
    
    procedure TForm1.CalendarLite1GetHolidays(Sender: TObject; AMonth, AYear: Integer;
      var Holidays: THolidays);
    var
      d, m, y: Word;
      e: TDate;
    begin
      ClearHolidays(Holidays);
      if not FNoHolidays then
      begin
        // Фиксированные праздники
        case AMonth of
          1: AddHoliday(1, Holidays);          // Новый Год
         12: AddHoliday(25, Holidays);         // Рождество
        end;
        // Пасха
        e := Easter(AYear);
        DecodeDate(e, y,m,d);
        if m = AMonth then
          AddHoliday(d, Holidays);
        //Троица --> 49 дней после Пасхи
        DecodeDate(e+49, y,m,d);
        if m = AMonth then
          AddHoliday(d, Holidays);
      end;
    end;
    

Убедитесь, что опция календаря `coShowHolidays` установлена, чтобы выделять праздники в календаре. 

### Подсказки для праздников

Если вы хотите отобразить название праздника как всплывающее окно с подсказкой при наведении мыши, установите для `ShowHint` в календаре значение `true` и добавьте этот обработчик события для `OnHint`: 
    
    
    procedure TForm1.CalendarLite1Hint(Sender: TObject; AYear, AMonth, ADay: Word;
      var AText: String);
    var
      dt, e: TDate;
    begin
      AText := '';
      case AMonth of
        1: if ADay = 1 then AText := 'Новый Год';
       12: if ADay = 25 then AText := 'Рождество';
       else
           e := Easter(AYear);
           dt := EncodeDate(AYear, AMonth, ADay);
           if (dt = e) then
             AText := 'Пасха'
           else if (dt = e + 49) then
             AText := 'Троица';
      end;
    end;
    

### Вписанные имена праздников

Кроме того, вы также можете напрямую добавить название праздника в сетку календаря. Добавьте следующий обработчик события для `OnGetDayText`. Обратите внимание, что размер календаря должен быть достаточно большим, чтобы обеспечить место для дополнительного текста: 
    
    
    procedure TForm1.CalendarLite1GetDayText(Sender: TObject; AYear, AMonth, ADay: Word;
      var AText: String);
    var
      s: String;
    begin
      GetHintText(Sender, AYear, AMonth, ADay, s);
      if s <> '' then
        AText := IntToStr(ADay) + LineEnding + s;
    end;
    

### Праздники в popup-меню

Если свойство календаря `PopupMenu` пустое, предоставляется встроенное всплывающее меню, в котором отображаются все праздники текущего выбранного года. Как обычно, это всплывающее окно открывается щелчком правой кнопкой мыши на компоненте. 

## Рисование иконок для определенных дней

Следующий пример рисует значок дня рождения в ячейке 11 ноября каждого года. Значок хранится в списке изображений с индексом 0: 
    
    
    procedure TForm1.CalendarLite1DrawCell(Sender: TObject; ACanvas: TCanvas;
      AYear,AMonth,ADay: Word; AState: TCalCellStates; var ARect: TRect;
      var AContinueDrawing: Boolean);
    var
      bmp: TBitmap;
    begin
      if (AMonth = 11) and (ADay = 11) and not (csOtherMonth in AState) then begin
        bmp := TBitmap.Create;
        try
          ImageList1.GetBitmap(0, bmp);
          ACanvas.Draw(ARect.Left, (ARect.Top + ARect.Bottom - bmp.Height) div 2, bmp);
          inc(ARect.Left, bmp.Width + 2);
          // свойство AContinueDrawing, оставленное со значением по умолчанию (true), означает, 
          // что текст дня рисуется встроенной процедурой в уменьшенный прямоугольник
        finally
          bmp.Free;
        end;
      end;
    end;

---

_Source: [https://wiki.freepascal.org/CalLite%3A_Usage/ru](https://web.archive.org/web/20250117053445/https://wiki.freepascal.org/CalLite%3A_Usage/ru)_
