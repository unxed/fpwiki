# CalLite: Usage

│ **English (en)** │  **[suomi (fi)](</CalLite:_Usage/fi> "CalLite: Usage/fi")** │  **[русский (ru)](<../ru/CalLite__Usage.md> "CalLite: Usage/ru")** │    
****

## Contents

  * 1 Usage
    * 1.1 Changing colors
    * 1.2 Using the calendar's Options
    * 1.3 Changing the language
    * 1.4 Events
    * 1.5 Holidays
      * 1.5.1 Hints for holidays
      * 1.5.2 Embedded holiday names
      * 1.5.3 Holidays in popup menu
    * 1.6 Drawing icons for specific days



# Usage

[![CalLite](https://wiki.freepascal.org/images/5/59/callite.png)](</File:callite.png> "CalLite")

Simply drop a [TCalendarLite](<CalLite.md> "CalLite") component on the form and get a functional month-calendar. 

## Changing colors

The property `Colors` of the calendar collects all settings affecting the colors used for the various items: 

  * **ArrowBorderColor** : the color of the border of the arrows above the calendar (default: clSilver  )
  * **ArrowColor** : the fill color of the arrows above the calendar (default: clSilver  )
  * **BackgroundColor** : the background color of the entire calendar (default: clWhite  )
  * **BorderColor** : the color of an optional border around the calenard (default: clSilver  ). Add `coShowBorder` to the calendar's `Options` to display the border.
  * **DayLineColor** : the color of an optional separating line between the top navigation pane and the calendar's day area (default: clSilver  ). Add `coDayLine` to the calendar's `Options` to display the line.
  * **HolidayColor** : the text color used to display holidays (default: clRed  ). Add an event handler to `OnGetHolidays` to define which days are holidays.
  * **PastMonthColor** : the text color used to display the days of the previous or next month (default: clSilver  )
  * **SelectedDateColor** : fill color of the currently selected day (default: clMoneyGreen  )
  * **TextColor** : text color used for paint the regular calendar days (default: clBlack  )
  * **TodayFrameColor** : color used to draw a rectangular line around the "today" cell (default: clLime  )
  * **TopRowColor** : background color used for the top navigation line (default: clHighlight)
  * **TopRowTextColor** : text color used in the top navigation line (default: clHighlightText)
  * **WeekendColor** : text color used for painting weekend days (default: clRed  )



## Using the calendar's Options

The display of the calendar can be modified by changing the calendar's Options, an enumeration of flags: 
    
    
    type
      TCalOption = (coBoldDayNames, coBoldHolidays, coBoldToday, coBoldTopRow,
                    coBoldWeekend, coDayLine, coShowBorder, coShowHolidays,
                    coShowTodayFrame, coShowTodayName, coShowTodayRow,
                    coShowWeekend, coUseTopRowColors);
    

  * **coBoldDayNames** : Draws the line with the day names using bold characters
  * **coBoldHolidays** : Draws the day number of holidays using bold characters. Holidays are normally off, but you an add an event handler to `OnGetHolidays` in order to define holidays.
  * **coBoldToday** : Draws today's cell using bold characters
  * **coBoldTopRow** : Draws the month name and year number in the top row using bold characters
  * **coBoldWeekend** : Draws the cells of weekend days using bold characters
  * **coDayLine** : Adds a separating line between the top navigation line and the calendar's day area.
  * **coShowBorder** : Paints a thin border rectangle around the calendar
  * **coShowHolidays** : Activate highlighting holidays. Note that there must be also an event handler for `OnGetHolidays` which defines the holidays.
  * **coShowTodayFrame** : Draws a rectangle around the "today" cell.
  * **coShowTodayName** : Adds the day name to the line displaying today's date (see also: `coShowTodayRow`)
  * **coShowTodayRow** : Shows a line at the bottom of the calendar to display today's date (see also: `coShowTodayName`).
  * **coShowWeekend** : Highlights weekend days (see also: `Colors.WeekendColor`).
  * **coUseTopRowColors** : Paints background and text of the top navigation row using the colors defined by `Colors.TopRowColor` and `Colors.TopRowTextColor`



## Changing the language

The strings used by the component can be easily translated to a variety of languages by changing the value of the property `Languages`: 
    
    
    type
      TLanguage = (lgEnglish, lgFrench, lgGerman, lgHebrew, lgSpanish, lgItalian,
                   lgPolish, lgFinnish);
    

## Events

In addition to the standard events TCalendarLite fires the following events. Some events get year, month and day numbers of the currently painted cell as a parameter. A set of state flags indicates whether this day is selected, today or belongs to the previous or next month: 
    
    
    type
      TCalCellState = (csSelectedDay, csToday, csOtherMonth);
      TCalCellStates = set of TCalCellState;
    

  * **OnDateChange** : Fires whenever another date is selected in the calendar.
  * **OnDrawCell** : Can be used to override the painting process of a day cell completely or partially. If the boolean parameter `AContinueDrawing` is set to true the normal painting process continues after leaving the event handler; if it is false then no default painting occurs for this day. The event can be used, for example, to add birthday or holiday icons to the day cells.
  * **OnGetDayText** : Can be used to modify the text display for the specified day. The default is a string showing the day value. Using this event, the names of holidays can be added to calendar, for example.
  * **OnGetHolidays** : Defines which days of current month are holidays. The day number of the holidays are encoded as set bits in a 32-bit integer.
  * **OnHint** : Defines a hint which can be displayed in a popup hint window for each day. Useful, for example, to show the name of holidays if the mouse hovers over a holiday.
  * **OnMonthChange** : Fires whenever another month is displayed in the calendar. Can be used for more efficient holiday calculation.
  * **OnPrepareCanvas** : Is called immediately before a cell is painted. Can be used to override Brush and Pen properties, for example, in order to draw different backgrounds for appointments and important events.



## Holidays

The calendar does not "know" any holidays. You must write an event handler to define which days in the current month are holidays. Month and year values of the respective month are passed as parameters. The information that a day is a holiday or not is encoded as set bits in a 32-bit integer. The unit _callite_ simplifies this by providing these general-purpose procedures: 

  * procedure **AddHoliday**(ADay: Integer; var AHolidays: THolidays): Adds the specified day to the holiday bit list in parameter `AHolidays`.
  * function **IsHoliday**(ADay: Integer; AHolidays: THolidays): Boolean: Checks whether the specified day is listed in `AHolidays`
  * **ClearHolidays**(var AHolidays: THolidays): Clears the holiday list



Here is a simple example defining the holidays New Year, Christmas, Easter and Whit Sunday: 
    
    
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
        else    raise Exception.Create('Only years after 1700 supported.');
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
        // Fixed holidays
        case AMonth of
          1: AddHoliday(1, Holidays);          // New Year
         12: AddHoliday(25, Holidays);         // Christmas
        end;
        // Easter
        e := Easter(AYear);
        DecodeDate(e, y,m,d);
        if m = AMonth then
          AddHoliday(d, Holidays);
        // Whit Sunday --> 49 days after easter
        DecodeDate(e+49, y,m,d);
        if m = AMonth then
          AddHoliday(d, Holidays);
      end;
    end;
    

Make sure to have the calendar Option `coShowHolidays` set in order to highlight the holidays in the calendar. 

### Hints for holidays

If you want to display the holiday name as a mouse popup hint window then you should set the calendar's `ShowHint` to `true` and add this event handler for `OnHint`: 
    
    
    procedure TForm1.CalendarLite1Hint(Sender: TObject; AYear, AMonth, ADay: Word;
      var AText: String);
    var
      dt, e: TDate;
    begin
      AText := '';
      case AMonth of
        1: if ADay = 1 then AText := 'New Year';
       12: if ADay = 25 then AText := 'Christmas';
       else
           e := Easter(AYear);
           dt := EncodeDate(AYear, AMonth, ADay);
           if (dt = e) then
             AText := 'Easter'
           else if (dt = e + 49) then
             AText := 'Whit Sunday';
      end;
    end;
    

### Embedded holiday names

In addition, you can also add the holiday name to the calendar grid directly. Add the next event handler for `OnGetDayText`. Note that the size of the calendar must be large enough to provide space for the additional text: 
    
    
    procedure TForm1.CalendarLite1GetDayText(Sender: TObject; AYear, AMonth, ADay: Word;
      var AText: String);
    var
      s: String;
    begin
      GetHintText(Sender, AYear, AMonth, ADay, s);
      if s <> '' then
        AText := IntToStr(ADay) + LineEnding + s;
    end;
    

### Holidays in popup menu

If the calendar's `PopupMenu` property is empty a built-in popup menu is provided which displays all the holidays of the currently selected year. As usual, this popup opens with a right-click onto the component. 

## Drawing icons for specific days

The next example paints a birthday icon into the Nov 11 cell of each year. The icon is stored in an image list at index 0: 
    
    
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
          // Not changing AContinueDrawing from its default value (true) means
          // that the day text is drawn by the built-in procedure into the reduced rectangle
        finally
          bmp.Free;
        end;
      end;
    end;

---

_Source: [https://wiki.freepascal.org/CalLite%3A_Usage](https://web.archive.org/web/20250206165147/https://wiki.freepascal.org/CalLite%3A_Usage)_
