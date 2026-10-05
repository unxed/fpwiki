# DateTimeCtrls Package

## Contents

  * 1 About
  * 2 Author
  * 3 License
  * 4 Getting the package
  * 5 TDateTimePicker
    * 5.1 Properties
      * 5.1.1 DateTime: TDateTime (public)
      * 5.1.2 Date: TDate
      * 5.1.3 Time: TTime
      * 5.1.4 MinDate: TDate
      * 5.1.5 MaxDate: TDate
      * 5.1.6 NullInputAllowed: Boolean
      * 5.1.7 CenturyFrom: Word
      * 5.1.8 Kind: TDateTimeKind
      * 5.1.9 DateMode: TDTDateMode
      * 5.1.10 ShowCheckBox: Boolean
      * 5.1.11 Checked: Boolean
      * 5.1.12 DateDisplayOrder: TDateDisplayOrder
      * 5.1.13 DateSeparator: String
      * 5.1.14 TimeSeparator: String
      * 5.1.15 UseDefaultSeparators: Boolean
      * 5.1.16 TrailingSeparator: Boolean
      * 5.1.17 LeadingZeros: Boolean
      * 5.1.18 TimeDisplay: TTimeDisplay
      * 5.1.19 TimeFormat: TTimeFormat
      * 5.1.20 TextForNullDate: String
      * 5.1.21 AutoAdvance: Boolean
      * 5.1.22 Cascade: Boolean
      * 5.1.23 AutoButtonSize: Boolean
      * 5.1.24 HideDateTimeParts: TDateTimeParts
      * 5.1.25 CalendarWrapperClass: TCalendarControlWrapperClass (public)
      * 5.1.26 ShowMonthNames: Boolean
      * 5.1.27 MonthNames: String
      * 5.1.28 Options: TDateTimePickerOptions
    * 5.2 DateTimePicker Editor
    * 5.3 User interaction in runtime
    * 5.4 Using some custom calendar control for drop-down
      * 5.4.1 Defining the wrapper class
        * 5.4.1.1 GetCalendarControlClass
        * 5.4.1.2 SetDate
        * 5.4.1.3 GetDate
        * 5.4.1.4 AreCoordinatesOnDate
      * 5.4.2 Telling to the DateTimePicker to use new wrapper
  * 6 TDBDateTimePicker
    * 6.1 Handling of null values
      * 6.1.1 Displaying null values
      * 6.1.2 Setting the field value to null



## About

The DateTimeCtrls package contains two controls: 

[![TDateTimePicker.png](https://wiki.freepascal.org/images/6/63/TDateTimePicker.png)](</File:TDateTimePicker.png>) TDateTimePicker and [![TDBDateTimePicker.png](https://wiki.freepascal.org/images/3/34/TDBDateTimePicker.png)](</File:TDBDateTimePicker.png>)TDBDateTimePicker

Delphi's VCL has a control named [TDateTimePicker](<http://docwiki.embarcadero.com/Libraries/en/Vcl.ComCtrls.TDateTimePicker>), which I find very useful for editing dates. LCL, however, does not have this control. 

Therefore, I tried to create a cross-platform Lazarus control which would resemble VCL's TDateTimePicker as much as possible. 

The TDateTimePicker does not use [native Win control](<http://msdn.microsoft.com/en-us/library/system.windows.forms.datetimepicker.aspx>). It descends from LCL-s TCustomControl to be cross-platform. It has been tested on Windows with Win32/64 and qt widgetsets and on Linux with gtk2 and qt widgetsets. 

Note that the TDateTimePicker control does not descend from TEdit, so it does not have unnecessary caret. The VCL's control doesn't have caret either. 

## Author

    [ Zoran Vučenović](</User:Zoran> "User:Zoran")

## License

Modified LGPL (same as the FPC RTL and the Lazarus LCL). 

## Getting the package

This package is part of Lazarus distribution (since Lazarus 1.4). You can find it in <_your_lazarus_folder_ >/components/datetimectrls. 

The components are installed on Component palette by default (if you build the IDE yourself, they are installed with "make bigide"). The components TDateTimePicker and TDBDateTimePicker are located in [Common Controls](<Common_Controls_tab.md> "Common Controls tab") and [Data Controls](<Data_Controls_tab.md> "Data Controls tab") palette pages, respectively. 

Since Lazarus 1.8 the designtime code is separated to another package DateTimeCtrlsDsgn. Normally, you should not care about this, but only if you are installing the controls in the IDE manually, you should know that it is actually the designtime package DateTimeCtrlsDsgn which is installed in the IDE. 

This separation of designtime code to different package is done to prevent the code which is needed only in design time to be linked in final executable. [See the 1.8 release notes.](<Lazarus_1.8.md> "Lazarus 1.8.0 release notes")

## TDateTimePicker [![TDateTimePicker.png](https://wiki.freepascal.org/images/6/63/TDateTimePicker.png)](</File:TDateTimePicker.png>)

### Properties

I'll explain some properties of TDateTimePicker control: 

#### DateTime: TDateTime (public)

    The DateTime value displayed on the control. This property is not published in object inspector, but its value is actually the same as Date and Time properties composed in one value. This property is provided to allow setting or reading of both date and time value at once in program code. In design time, Date and Time can be set in object inspector. There is also component editor which provides easy way of setting this property in design time.

#### Date: TDate

    The date displayed on the control which the user can edit.

#### Time: TTime

    The time displayed on the control which the user can edit.

#### MinDate: TDate

    The minimal date user can enter.

#### MaxDate: TDate

    The maximal date user can enter.

#### NullInputAllowed: Boolean

    When True, the user can set the date to NullDate constant by pressing N key.

#### CenturyFrom: Word

    When user enters the year in two-digit format, then the CenturyFrom property is used to determine which century the year belongs to. The default is 1941, which means that when two digit year is entered, it falls in interval 1941 – 2040. Note that MinDate and MaxDate properties can also have influence on the decision – for example, if the CenturyFrom is set to 1941 and MaxDate to 31. 12. 2010, if user enters year 23, it will be set to 1923, because it can’t be 2033, due to MaxDate limit.

#### Kind: TDateTimeKind

    **type** TDateTimeKind = (dtkDate, dtkTime, dtkDateTime);

    The control displays only date, only time or both.
    [![DateControls03.PNG](https://wiki.freepascal.org/images/e/e3/DateControls03.PNG)](</File:DateControls03.PNG>)

#### DateMode: TDTDateMode

    **type** TDTDateMode = (dmComboBox, dmUpDown, dmNone)

    When DateMode is set to dmComboBox, there is a button on the right side of the control. When user clicks the button, [the calendar control](<http://lazarus-ccr.sourceforge.net/docs/lcl/calendar/tcalendar.html>) is shown, allowing the user to pick the date. When set to dmUpDown, then UpDown buttons are shown.

    In my opinion the UpDown buttons aren't really useful in this control, they are provided for compatibility with Delphi's TDateTimePicker. Up and down keys can always serve for same purpose, so can mouse wheel.

    In the picture the first control's DateMode is set to dmComboBox and the second control's to dmUpDown:
    [![DateControls01.PNG](https://wiki.freepascal.org/images/3/3d/DateControls01.PNG)](</File:DateControls01.PNG>)

#### ShowCheckBox: Boolean

    When set, there is a check box on the left side of the control.
    By default, when unchecked, the display appears grayed and user interaction with the date is not possible. (The control is still enabled, though, only in sense that the check box remains enabled). This behaviour may be changed if dtpoEnabledIfUnchecked is added to Options property \-- then this check box is shown, but does nothing by itself (doesn't disable the control) and the programmer can decide how it will be used (by using OnCheckBoxChange event).
    [![DateControls02.PNG](https://wiki.freepascal.org/images/2/2e/DateControls02.PNG)](</File:DateControls02.PNG>)

#### Checked: Boolean

    If ShowCheckBox is set to True, this property determines whether the check box is checked or not. If ShowCheckBox is False, this property has no purpose and is automatically set to True.

#### DateDisplayOrder: TDateDisplayOrder

    **type** TDateDisplayOrder = (ddoDMY, ddoMDY, ddoYMD, ddoTryDefault);

    Defines the order for displaying day, month and year part of the date. When ddoTryDefault is set, then the controls tries to determine the order from [ShortDateFormat global variable](<http://lazarus-ccr.sourceforge.net/docs/rtl/sysutils/shortdateformat.html>).
    This is similar to DateEdit's [DateOrder](<http://lazarus-ccr.sourceforge.net/docs/lcl/editbtn/tdateedit.dateorder.html>) property.

#### DateSeparator: String

    Defines the string used to separate date, month and year date parts. Setting this property automatically sets the UseDefaultSeparators property to False. To ensure that date and time separators are set to user's system defaults, set UseDefaultSeparators property to True.

#### TimeSeparator: String

    Defines the string used to separate hour, minute, second and millisecond time parts. Setting this property automatically sets the UseDefaultSeparators property to False. To ensure that date and time separators are set to user's system defaults, set UseDefaultSeparators property to True.

#### UseDefaultSeparators: Boolean

    When this property is set to True, then the DateSeparator and TimeSeparator properties will be set to [DateSeparator](<http://lazarus-ccr.sourceforge.net/docs/rtl/sysutils/dateseparator.html>) and [TimeSeparator](<http://lazarus-ccr.sourceforge.net/docs/rtl/sysutils/timeseparator.html>) global variables, which are set to user system defaults when application initializes.

#### TrailingSeparator: Boolean

    When set to True, then the DateSeparator is shown once more, after the last date part. This property exists because in some languages the correct format is 31. 1. 2010. including the last point, after the year.

#### LeadingZeros: Boolean

    Determines whether the date parts are displayed with or without leading zeros (this actually affects day, month and hour parts of date and time display).

#### TimeDisplay: TTimeDisplay

    **type** TTimeDisplay = (tdHM, tdHMS, tdHMSMs);

    If Kind is dtkTime or dtkDateTime, then TimeDisplay value of tdHM means that only hours and minutes are displayed, tdHMS adds displaying of seconds and value of tdHMSMs means that milliseconds are displayed too.

#### TimeFormat: TTimeFormat

    **type** TTimeFormat = (tf12, tf24);

    The value of tf12 sets the display of time to 12 hours format, with AM/PM string and tf24 sets to 24 hours format.

#### TextForNullDate: String

    Text which appears when the null date is set and control does not have focus. When control is focused, the text changes to defined format, but displaying zeros, which is appropriate to user input. User can set the date to NullDate by pressing N key, provided NullInputAllowed property is True.
    When TextForNullDate is set to empty string, zeros/nines format is displayed even when control does not have focus. If you want empty display, this can be achieved by setting TextForNullDate to one or more space characters.

#### AutoAdvance: Boolean

    When true, then when user is entering valid text, the selection automatically advances to next part of date/time. The default is True, because it makes user interaction easier and this behaviour is what a user should expect.

#### Cascade: Boolean

    When true, then when user is increasing or decreasing one date/time part (using up-down keys or mouse wheel), it can increase or decrease by one another date/time part. For example, when date is 31.08.2013. and user increases the day, the day becomes 1 and month increases by one and becomes 9, so the date becomes 01.09.2013. If Cascade were set to False, the month would not change and the date would become 01.08.2013.

#### AutoButtonSize: Boolean

    When true, the width of the arrow button (or up-down control, if it is shown instead) is automatically adjusted proportionally to the height.

#### HideDateTimeParts: TDateTimeParts

    **type**

    

    TDateTimePart = (dtpDay, dtpMonth, dtpYear, dtpHour, dtpMinute, dtpSecond, dtpMiliSec, dtpAMPM);

    

    TDateTimeParts = set of dtpDay..dtpMiliSec;

    With HideDateTimeParts property, you can chose which date/time parts will not be shown. Most of the time you do not need to use this property and you can get the format you want by using other properties (see Kind, TimeDisplay). However, if you need more control (for example, you might want to let user edit only days, months and hours), you can additionally hide any date/time parts with this control. Keep in mind that, with this property, you cannot show any date/time part which is hidden by another property (for example, if TimeDisplay is tdHM, the second part is not shown, regardless of this property).

#### CalendarWrapperClass: TCalendarControlWrapperClass (public)

    When assigned, this property determines the type of the calendar control used for drop-down calendar. When set to nil, which is the default, the value of global variable DefaultCalendarWrapperClass is used. More details here.

#### ShowMonthNames: Boolean

    When this property is set to True, month names, set in MonthNames property, will be displayed instead of numbers.

#### MonthNames: String

    When ShowMonthNames is set to True, this property determines which month names should appear.
    If MonthNames is set to 'Short' or 'Long', then month names are set to [ShortMonthNames](<http://www.freepascal.org/docs-html/rtl/sysutils/shortmonthnames.html>) or [LongMonthNames](<http://www.freepascal.org/docs-html/rtl/sysutils/longmonthnames.html>) respectively.
    To set the month names explicitly, this property should be set to string which starts with a character which will be used to separate months and then twelve names separated by this character. For example ',I,II,III,IV,V,VI,VII,VIII,IX,X,XI,XII' — roman numbers are used for months — the first character is comma, which means that comma is used to separate the months. Another valid example ';jan;feb;mar;apr;maj;jun;jul;avg;sep;okt;nov;dec'.
    So, the separator should be the first character, before the first month name, and then only if there are twelve month names separated by that separator, the format is valid.
    The default value of this property is 'Long'.

#### Options: TDateTimePickerOptions

  * dtpoDoChangeOnSetDateTime: the OnChange handler will be called also when DateTime is programatically changed.
  * dtpoEnabledIfUnchecked: enable the date time picker if the checkbox is unchecked.
  * dtpoAutoCheck: auto-check an unchecked checkbox when DateTime is changed (makes sense only if dtpoEnabledIfUnchecked is set).
  * dtpoFlatButton: use flat button for calendar picker.
  * dtpoResetSelection: when the control receives focus, the selection is always in the first part (the control does not remember which part was selected).



### DateTimePicker Editor

[![datetimepickereditor.png](https://wiki.freepascal.org/images/2/2b/datetimepickereditor.png)](</File:datetimepickereditor.png>)

    DateTimePicker Editor is a dialog which provides easy way to edit Date, Time, MinDate and MaxDate properties in design time. It is invoked when DateTimePicker control is double-clicked in form designer. It is also shown when the ellipsis (…) button shown in Date, Time, MinDate and MaxDate properties in Object inspector gets clicked.

  


### User interaction in runtime

    The goal was to provide same user interaction as native DateTimePicker from Windows. The keyboard input is restricted to valid date/time values only. Up and down buttons can be used too. The controls behave just like VCL's control.

    After this behaviour is achieved it is further improved with mouse wheel interaction. Date and time parts can be edited using mouse wheel, which is very fast and comes handy.

  


### Using some custom calendar control for drop-down

Instead of LCL's TCalendar, some other calendar control can be used for drop-down. This calendar control has to provide a way to determine if the coordinates (within the calendar control) are on the date or not (see AreCoordinatesOnDate function). DateTimePicker needs it to decide if it should close the drop-down calendar and take its date when the calendar gets clicked (see AreCoordinatesOnDate function). The calendar only needs to be derived from TControl. The mouse interaction with calendar will work and, if it is also TWinControl's descendant, the keyboard interaction will also work. 

Now, let's see how to use some calendar control for drop-down control. First, we need to define the "wrapper" class, then we will have to tell the DateTimePicker control to use this wrapper instead of default one. 

#### Defining the wrapper class

    In unit calendarcontrolwrapper.pas, there is an abstract class TCalendarControlWrapper. We need to derive a new class from this abstract class. There are four abstract methods which new class has to override - GetCalendarControlClass, SetDate, GetDate and AreCoordinatesOnDate.
    This should be pretty simple, please see the file lclcalendarwrapper.pas, where TLCLCalendarWrapper (the default wrapper) is defined.

##### GetCalendarControlClass

    
    
    
    class function GetCalendarControlClass: TControlClass; virtual abstract;
    

    This class function should return the class of the wrapped calendar control.

##### SetDate

    
    
    
    procedure SetDate(Date: TDate); virtual abstract;
    

    Should be overridden to set the date in the calendar control.

##### GetDate

    
    
    
    function GetDate: TDate; virtual abstract;
    

    Should be overridden to get the date from the calendar control.

##### AreCoordinatesOnDate

    
    
    
    function AreCoordinatesOnDate(X, Y: Integer): Boolean; virtual abstract;
    

    This function should return True if coordinates (X, Y) are on the date in the calendar control (DateTimePicker calls this function when the calendar is clicked, to determine whether the drop-down calendar should return the date or not). The calendar control must at least provide a way to determine whether the coordinates are on the date (when this control gets clicked, we must decide if the date has just been chosen — then we should respond by closing the drop-down form and setting the date from calendar to DateTimePicker — for example in LCL's TCalendar we will respond when the calendar is clicked on date, but not when the user clicks in title area changing months or years, then we let the user keep browsing the calendar).

#### Telling to the DateTimePicker to use new wrapper

    Now, when we have the wrapper class defined, the easiest way to make all DateTimePickers use the new control is setting global variable DefaultCalendarWrapperClass (declared in unit datetimepicker.pas) and all DateTimePickers in the project will use the new control.

    There is public property CalendarWrapperClass in DateTimePicker. We can set it and DateTimePicker will use it.

    This pseudo code shows how a DateTimePicker decides which calendar control to use:
    
    
    if property DateTimePicker.CalendarWrapperClass is not nil
      the property is used
    else if global variable DefaultCalendarWrapperClass is not nil
      the global variable is used
    else
      LCL's TCalendar (implemented through TLCLCalendarWrapper class) is used
    

## TDBDateTimePicker [![TDBDateTimePicker.png](https://wiki.freepascal.org/images/3/34/TDBDateTimePicker.png)](</File:TDBDateTimePicker.png>)

    TDBDateTimePicker is a data-aware version of TDateTimePicker, with nice way of handling null database values.

### Handling of null values

#### Displaying null values

    When the underlying DB field has null value, then:

    

    If the control is not focused, then it displays the text defined in TextForNullDate property. The default is "NULL".

    

    When the control gets focus, the text changes to defined format, but displaying zeros for date parts and nines for time parts (for example "00/00/0000 99:99:99"), which is appropriate to user input.

#### Setting the field value to null

    

    If NullInputAllowed property is True, the user can set the date and time to null, by pressing N key.

---

_Source: [https://wiki.freepascal.org/DateTimeCtrls_Package](https://web.archive.org/web/20240907082355/https://wiki.freepascal.org/DateTimeCtrls_Package)_
