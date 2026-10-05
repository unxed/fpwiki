# CalLite: Flag days in Finland

│ **English (en)** │  **[suomi (fi)](</CalLite:_Flag_days_in_Finland/fi> "CalLite: Flag days in Finland/fi")** │    
****

  


[![Flag days in Finland.png](https://wiki.freepascal.org/images/e/ed/Flag_days_in_Finland.png)](</File:Flag_days_in_Finland.png>)

[TCalendarLite](<CalLite.md> "CalLite") is a lightweight calendar component. This demo shows how CalLite can be used to display flag days in Finland in the calendar as small icons 

  


## Contents

  * 1 unit uFinlandFlagDays
  * 2 unit unitFinlandFlagDays
  * 3 unitFinlandFlagDays.lfm
  * 4 program flagdays_project



## unit uFinlandFlagDays
    
    
    unit uFinlandFlagDays;
    
    {$mode objfpc}{$H+}
    
    interface
    
    uses
      Classes, SysUtils, CalendarLite, DateUtils;
    
    procedure CalendarGetFlagDaysInFinland( AMonth, AYear: Integer;
      var Holidays: THolidays);
    
    implementation
    const
      SATURDAY = 6;
      SUNDAY   = 7;
      FEBRUARY =  2;
      MARCH    =  3;
      APRIL    =  4;
      MAY      =  5;
      JUNE     =  6;
      JULY     =  7;
      OCTOBER  = 10;
      NOVEMBER = 11;
      DECEMBER = 12;
    
    function MothersDay(year:integer):integer;
    var
      day  : integer;
      a_date : TDateTime;
    begin
      day := 7;
      repeat
        inc(day);
        a_date := EncodeDate(year, MAY, day);
      until  DayOfTheWeek( a_date ) = SUNDAY;
      result := day;
    end;
    
    function FathersDay(year:integer):integer;
    var
      day  : integer;
      a_date : TDateTime;
    begin
      day := 7;
      repeat
        inc(day);
        a_date := EncodeDate(year, NOVEMBER, day);
      until  DayOfTheWeek( a_date ) = SUNDAY;
      result := day;
    end;
    
    function MidsummerDay(year:integer):integer;
    var
      day  : integer;
      a_date : TDateTime;
    begin
      day := 20-1;
      repeat
        inc(day);
        a_date := EncodeDate(year, JUNE, day);
      until  DayOfTheWeek( a_date ) = SATURDAY;
      result := day;
    end;
    
    function MemorialDayWarDead(year:integer):integer; //memorial day for the war dead
    var
      day  : integer;
      a_date : TDateTime;
    begin
      day := MothersDay(year);
      repeat
        inc(day);
        a_date := EncodeDate(year, MAY, day);
      until  DayOfTheWeek( a_date ) = SUNDAY;
      result := day;
    end;
    
    procedure CalendarGetFlagDaysInFinland( AMonth, AYear: Integer;
      var Holidays: THolidays);
    begin
      ClearHolidays(Holidays);
      begin
        case AMonth of
          FEBRUARY: begin
                AddHoliday( 5, Holidays);//Birthday of the national poet JL Runeberg
                AddHoliday(28, Holidays);//Day of Kalevala
              end;
          MARCH: AddHoliday(19, Holidays);// Birthday of novelist and playwright
                                          // Minna Canth, Day of Equality
          APRIL: begin
                AddHoliday( 9, Holidays);// The day Mikael Agricola, also celebrated
                                         // as the Day of the Finnish language
                AddHoliday(27, Holidays);// National War Veterans' Day
             end;
          MAY: begin
                AddHoliday( 1, Holidays);// 1 May, Vappu, the Day of Finnish Labour
                AddHoliday( 9, Holidays);// Europe Day
                AddHoliday(12, Holidays);// Day of the Finnish Identity, birthday
                                         // of the statesman J V Snellman
                AddHoliday(MothersDay(AYear), Holidays); // Mother's Day
                AddHoliday(MemorialDayWarDead(AYear), Holidays);//Memorial day for
                                                                //the war dead
              end;
          JUNE: begin
                AddHoliday( 4, Holidays);// Flag Day of the Finnish Defence Forces
                AddHoliday(MidsummerDay(AYear), Holidays);// Midsummer Day
              end;
          JULY: AddHoliday( 6, Holidays);// Birthday of the poet Eino Leino
          OCTOBER: begin
                AddHoliday(10, Holidays);// Birthday of the National writer
                                         // Aleksis Kivi, also celebrated as the
                                         // Day of Finnish literature
                AddHoliday(24, Holidays);// Day of the United Nations
              end;
          NOVEMBER: begin
                AddHoliday( 6, Holidays);// Finnish Swedish Heritage Day
                AddHoliday(FathersDay(AYear), Holidays);//Father's Day
              end;
          DECEMBER: begin
                AddHoliday( 6, Holidays);// Independence Day
                AddHoliday( 8, Holidays);// Birthday of the composer Jean Sibelius
                                         // also celebrated as the Day of Finnish
                                         // music
              end;
        end;
      end;
    end;
    
    end.
    

## unit unitFinlandFlagDays
    
    
    unit unitFinlandFlagDays;
    
    {$mode objfpc}{$H+}
    
    interface
    
    uses
      Classes, SysUtils, FileUtil, Forms, Controls, Graphics, Dialogs, CalendarLite,
      Types,  lcltype, uFinlandFlagDays;
    
    type
    
      { TForm1 }
    
      TForm1 = class(TForm)
        CalendarLite1: TCalendarLite;
        procedure CalendarLite1DrawCell(Sender: TObject; ACanvas: TCanvas; AYear,
          AMonth, ADay: Word; AState: TCalCellStates; var ARect: TRect;
          var AContinueDrawing: Boolean);
        procedure FormCreate(Sender: TObject);
      private
    
      public
    
      end;
    
    var
      Form1: TForm1;
    
    implementation
    
    procedure FlagOfFinland(var Bitmap: TBitmap);
    const
      FFH   = 11; // FinlandFlagHeight
      FFCH1 =  4; // FinlandFlagCrossH1
      FFCH2 =  7; // FinlandFlagCrossH2
      FFW   = 18; // FinlandFlagWidth
      FFCW1 =  5; // FinlandFlagCrossW1
      FFCW2 =  8; // FinlandFlagCrossW2
    begin
      Bitmap.Canvas.Pen.Color := clWhite;
      Bitmap.Canvas.Brush.Style:=bsSolid;
      Bitmap.Canvas.Brush.Color:= clWhite;
      Bitmap.Canvas.FillRect(0, 0, Bitmap.Width, Bitmap.Height);
      Bitmap.Canvas.Brush.Color:=RGBToColor(0, 47, 108) ;
      Bitmap.Canvas.FillRect(0,MulDiv(Bitmap.Height,FFCH1,FFH),
        Bitmap.Width,MulDiv(Bitmap.Height,FFCH2,FFH));
      Bitmap.Canvas.FillRect(MulDiv(Bitmap.Width,FFCW1,FFW),
        0,MulDiv(Bitmap.Width,FFCW2,FFW),Bitmap.Height);
    end;
    
    
    {$R *.lfm}
    
    { TForm1 }
    
    procedure TForm1.CalendarLite1DrawCell(Sender: TObject; ACanvas: TCanvas;
      AYear, AMonth, ADay: Word; AState: TCalCellStates; var ARect: TRect;
      var AContinueDrawing: Boolean);
    
    var
      Bitmap: TBitmap;
      Flagdays: THolidays;
    begin
      CalendarGetFlagDaysInFinland(AMonth,AYear,Flagdays);
      if  IsHoliday(aDay,Flagdays) and not (csOtherMonth in AState) then begin
        Bitmap := TBitmap.Create;
        try
          // Initializes the Bitmap Size
          Bitmap.Height := 11;
          Bitmap.Width := 18;
          FlagOfFinland(Bitmap);
          ACanvas.Draw(ARect.Left, (ARect.Top + ARect.Bottom - Bitmap.Height) div 2,
            Bitmap);
          inc(ARect.Left, Bitmap.Width + 2);
          // Not changing AContinueDrawing from its default value (true) means
          // that the day text is drawn by the built-in procedure into
          // the reduced rectangle
        finally
          Bitmap.Free;
        end;
      end;
    end;
    
    procedure TForm1.FormCreate(Sender: TObject);
    begin
      CalendarLite1.Languages := lgFinnish;
      CalendarLite1.StartingDayOfWeek := dowMonday;
      CalendarLite1.Date:=now;
    end;
    
    end.
    

## unitFinlandFlagDays.lfm
    
    
    object Form1: TForm1
      Left = 543
      Height = 278
      Top = 159
      Width = 426
      Caption = 'Flag days in Finland'
      ClientHeight = 278
      ClientWidth = 426
      OnCreate = FormCreate
      LCLVersion = '1.8.0.5'
      object CalendarLite1: TCalendarLite
        Left = 72
        Height = 200
        Top = 56
        Width = 274
        Constraints.MinHeight = 120
        Constraints.MinWidth = 120
        ParentColor = False
        TabOrder = 0
        TabStop = True
        Colors.TodayFrameColor = clGray
        Date = 42993
        DayNames = 'Su,Ma,Ti,ke,To,Pe,La'
        DisplayTexts = '"Tänään on",dd.mm.yyyy,Lomapäivät,"Lomapäiviä ei ole asetettu"'
        MonthNames = 'Tammikuu,Helmikuu,Maaliskuu,Huhtikuu,Toukokuu,Kesäkuu,Heinäkuu,Elokuu,Syyskuu,Lokakuu,Marraskuu,Joulukuu'
        MultiSelect = False
        StartingDayOfWeek = dowMonday
        WeekendDays = [dowSunday, dowSaturday]
        Languages = lgFinnish
        OnDrawCell = CalendarLite1DrawCell
      end
    end
    

## program flagdays_project
    
    
    program flagdays_project;
    
    {$mode objfpc}{$H+}
    
    uses
      {$IFDEF UNIX}{$IFDEF UseCThreads}
      cthreads,
      {$ENDIF}{$ENDIF}
      Interfaces, // this includes the LCL widgetset
      Forms, unitFinlandFlagDays, uFinlandFlagDays
      { you can add units after this };
    
    {$R *.res}
    
    begin
      RequireDerivedFormResource:=True;
      Application.Initialize;
      Application.CreateForm(TForm1, Form1);
      Application.Run;
    end.

---

_Source: [https://wiki.freepascal.org/CalLite%3A_Flag_days_in_Finland](https://web.archive.org/web/20250322054220/https://wiki.freepascal.org/CalLite%3A_Flag_days_in_Finland)_
