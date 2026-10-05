# DateControls Package for Lazarus

[![Note-icon.png](https://wiki.freepascal.org/images/b/be/Note-icon.png)](</File:Note-icon.png>)

**Note:** These controls are now obsolete. The controls in [DateTimeCtrls Package](<DateTimeCtrls_Package.md> "DateTimeCtrls Package") have improved functionality and completely cover the functionality of these!

## Contents

  * 1 Author
  * 2 Introduction
  * 3 Downloading and installing
  * 4 TDatePicker
    * 4.1 properties
      * 4.1.1 TheDate: TDateTime
      * 4.1.2 MinDate: TDateTime
      * 4.1.3 MaxDate: TDateTime
      * 4.1.4 NullInputAllowed: Boolean
      * 4.1.5 CenturyFrom: Word
      * 4.1.6 ShowCalendar: Boolean
      * 4.1.7 ShowCheckBox: Boolean
      * 4.1.8 Checked: Boolean
      * 4.1.9 DateDisplayOrder: TDateDisplayOrder
      * 4.1.10 TheDateSeparator: UTF8String
      * 4.1.11 UseDefaultDateSeparator: Boolean
      * 4.1.12 TrailingSeparator: Boolean
      * 4.1.13 LeadingZeros: Boolean
      * 4.1.14 TextForNullDate: UTF8String
  * 5 TDBDatePicker
    * 5.1 Handling of null values
      * 5.1.1 Displaying null values
      * 5.1.2 Setting the field value to null
  * 6 See also



## Author

    [ Zoran Vučenović](</User:Zoran> "User:Zoran")

## Introduction

DateControls package contains two controls: 

[![TDatePicker.PNG](https://wiki.freepascal.org/images/1/18/TDatePicker.PNG)](</File:TDatePicker.PNG>) TDatePicker and [![TDBDatePicker.PNG](https://wiki.freepascal.org/images/e/e8/TDBDatePicker.PNG)](</File:TDBDatePicker.PNG>)TDBDatePicker

**These controls are now obsolete. The controls in[DateTimeCtrls Package](<DateTimeCtrls_Package.md> "DateTimeCtrls Package") have improved functionality and completely cover the functionality of these!**

Delphi's VCL has a control named [TDateTimePicker](<http://docwiki.embarcadero.com/VCL/en/ComCtrls.TDateTimePicker>), which I find very useful for editing dates. LCL, however, does not have this control. Instead, for editing dates LCL has a control named [TDateEdit](<http://lazarus-ccr.sourceforge.net/docs/lcl/editbtn/tdateedit.html>), but I prefer the VCL's TDateTimePicker. 

  
Therefore, I tried to create a cross-platform Lazarus control which would resemble VCL's TDateTimePicker as much as possible. 

  
The TDatePicker control does not use [native Win control](<http://msdn.microsoft.com/en-us/library/system.windows.forms.datetimepicker.aspx>). It descends from LCL-s TCustomControl to be cross-platform. It has been written and initially tested on Windows XP with Win widgetset, but then tested on Ubuntu Linux 9.10 with gtk2 widgetset, where additional adjustments had been made. 

Thanks to [ Željko Rikalo](</User:Zeljan> "User:Zeljan"), its tested on qt and adjusted to be fully functional to qt users. 

  
Note that the TDatePicker control does not descend from TEdit, so it does not have unnecessary caret. The VCL's control doesn't have caret either. 

  
Unlike VCL's control, this control has no time editing feature. It can be used for date editing only. That's why it is named TDatePicker, not TDateTimePicker. 

## Downloading and installing

Lazarus package DateControls.lpk can be downloaded [from here](<http://datepicker.000space.com>), packed in zip format. 

After downloaded, unzip the package. 

To install the package in Lazarus IDE follow these steps: 

  1. Open the package in Package Editor (in Lazarus' main menu click _Package_ , then _Open package file..._ locate the file _datecontrols.lpk_ and click _Open_).
  2. Compile the package (click _Compile_ in Package Editor's tool bar).
  3. Install the package in the IDE (click _Install_ – you will be asked if you want to rebuild Lazarus, click _Yes_. Wait until Lazarus rebuilds and restarts itself. The new tab _DateControls_ appears on the component palette with TDatePicker and TDBDatePicker controls.



## TDatePicker [![TDatePicker.PNG](https://wiki.freepascal.org/images/1/18/TDatePicker.PNG)](</File:TDatePicker.PNG>)

### properties

I'll explain some properties of TDatePicker control: 

#### TheDate: TDateTime

    The date displayed on the control. This property is named TheDate, not Date, to avoid name conflict with [Date function](<http://lazarus-ccr.sourceforge.net/docs/rtl/sysutils/date.html>).

#### MinDate: TDateTime

    The minimal date user can enter.

#### MaxDate: TDateTime

    The maximal date user can enter.

#### NullInputAllowed: Boolean

    When True, the user can set the date to NullDate constant by pressing N key.

#### CenturyFrom: Word

    When user enters the year in two-digit format, then the CenturyFrom property is used to determine which century the year belongs to. The default is 1941, which means that when two digit years is entered, it falls in interval 1941 – 2040. Note that MinDate and MaxDate properties can also have influence on the decision – for example, if the CenturyFrom is set to 1941 and MaxDate to 31. 12. 2010, if user enters year 23, it will be set to 1923, because it can’t be 2033, due to MaxDate limit.

#### ShowCalendar: Boolean

    When set to True, there is a button on the right side of the control. When user clicks the button, [the calendar control](<http://lazarus-ccr.sourceforge.net/docs/lcl/calendar/tcalendar.html>) is shown, allowing the user to pick the date.
    [![ShowCalendar.PNG](https://wiki.freepascal.org/images/2/27/ShowCalendar.PNG)](</File:ShowCalendar.PNG>)

#### ShowCheckBox: Boolean

    When set, there is a check box on the left side of the control. When unchecked, the display appears grayed and user interaction with the date is not possible. (The control is still enabled, though, only in sense that the check box remains enabled).
    [![ShowCheckBox.PNG](https://wiki.freepascal.org/images/6/63/ShowCheckBox.PNG)](</File:ShowCheckBox.PNG>)

#### Checked: Boolean

    If ShowCheckBox is set to True, this property determines whether the check box is checked or not. If ShowCheckBox is False, this property has no purpose and is automatically set to True.

#### DateDisplayOrder: TDateDisplayOrder

    **type** TDateDisplayOrder = (ddoDMY, ddoMDY, ddoYMD, ddoTryDefault);

    Defines the order for displaying day, month and year part of the date. When ddoTryDefault is set, then the controls tries to determine the order from [ShortDateFormat global variable](<http://lazarus-ccr.sourceforge.net/docs/rtl/sysutils/shortdateformat.html>).
    This is similar to DateEdit's [DateOrder](<http://lazarus-ccr.sourceforge.net/docs/lcl/editbtn/tdateedit.dateorder.html>) property.

#### TheDateSeparator: UTF8String

    Defines the string used to separate date, month and year date parts. Setting this property automatically sets the UseDefaultDateSeparator property to False. This property is named TheDateSeparator, not DateSeparator, to avoid name conflict with [DateSeparator global variable](<http://lazarus-ccr.sourceforge.net/docs/rtl/sysutils/dateseparator.html>).

#### UseDefaultDateSeparator: Boolean

    When UseDefaultDateSeparator is set to True, TheDateSeparator property is set to [DateSeparator global variable](<http://lazarus-ccr.sourceforge.net/docs/rtl/sysutils/dateseparator.html>).

#### TrailingSeparator: Boolean

    When set to True, then TheDateSeparator is shown once more, after the last date part. This property exists because in some languages the correct format is 31. 1. 2010. including the last point, after the year.

#### LeadingZeros: Boolean

    Determines whether the date parts are displayed with or without leading zeros.

#### TextForNullDate: UTF8String

    Text which appears when the null date is set and control does not have focus. When control is focused, the text changes to defined format, but displaying zeros, which is appropriate to user input. User can set the date to NullDate by pressing N key, provided NullInputAllowed property is True.

  


## TDBDatePicker [![TDatePicker.PNG](https://wiki.freepascal.org/images/1/18/TDatePicker.PNG)](</File:TDatePicker.PNG>)

    TDBDatePicker is a data-aware version of TDatePicker, with nice way of handling null database values.

### Handling of null values

#### Displaying null values

    When the underlying DB field has null value, then:

    

    If the control is not focused, then it displays the text defined in TextForNullDate property. The default is "NULL".

    

    When the control gets focus, the text changes to defined format, but displaying zeros (for example "00/00/0000"), which is appropriate to user input.

#### Setting the field value to null

    

    If NullInputAllowed property is True, the user can set the date to null, by pressing N key.

## See also

  * [DateTimeCtrls Package](<DateTimeCtrls_Package.md> "DateTimeCtrls Package") Please use this package instead as DateControls is obsolete.

---

_Source: [https://wiki.freepascal.org/DateControls_Package_for_Lazarus](https://web.archive.org/web/20220228203333/https://wiki.freepascal.org/DateControls_Package_for_Lazarus)_
