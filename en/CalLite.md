# CalLite

│ **English (en)** │  **[suomi (fi)](</CalLite/fi> "CalLite/fi")** │  **[русский (ru)](<../ru/CalLite.md> "CalLite/ru")** │    
****

## Contents

  * 1 About
  * 2 Authors
  * 3 License
  * 4 Download and Installation
    * 4.1 Release version
    * 4.2 Development version
    * 4.3 Version Notes
    * 4.4 Installation
  * 5 Navigation
  * 6 Multi-selection
  * 7 Documentation
  * 8 See also



## About

[![CalLite](https://wiki.freepascal.org/images/5/59/callite.png)](</File:callite.png> "CalLite")

TCalendarLite is a lightweight calendar component, a TCustomControl descendant which consequently is not dependent on any widgetset. It is not a fixed-size component, as are most calendars, but will align and resize as needed. Various properties give access to almost every aspect of its appearance. 

## Authors

Howard Page-Clark, Ariel Rodriguez and Werner Pamler 

## License

Modified LGPL (with linking exception, like Lazarus LCL) 

## Download and Installation

#### Release version

A zip file with the most recent release version can be found at [Lazarus CCR at SourceForge](<https://sourceforge.net/projects/lazarus-ccr/files/CalLite/>). Unzip the file into any folder. 

The current release version is 0.3.1 

#### Development version

Use an svn client to download the current trunk version from <svn://svn.code.sf.net/p/lazarus-ccr/svn/components/callite/>

#### Version Notes

  * **v0.3.10** breaks existing code relying on `TDayOfWeek` starting at value 1. This had to be changed to start at 0 to fix compilation with FPC 3.3.1.



#### Installation

[![tcalendarlite 150.png](https://wiki.freepascal.org/images/7/78/tcalendarlite_150.png)](</File:tcalendarlite_150.png>)

In Lazarus, go to _"Package"_ > _"Open Package File .lpk"_. Navigate to the folder with the callite sources, and select **callight_pkg.lkp**. Click _"Compile"_ , then _"Use"_ > _"Install"_. This will rebuild the IDE (it may take some time). When the process is finished the IDE will restart, and you'll find TCalendarLite in the component palette **[Misc](<Misc_tab.md> "Misc tab")**. 

## Navigation

  * Click on any date
  * Use the arrow keys on the keyboard
  * Click on the arrow keys above the calendar; the single arrow advances by one month, the double arrow advances by one year
  * Click on the month name to open a popdown menu with month names, or click on the year number to open a popdown menu with the last and next ten years.



## Multi-selection

If the property `MultiSelect` is set to `true` then several days can be selected in the calendar. All selected days are drawn with a highlighted background. Multi-selection is controlled by holding special keys down while selecting a day either by a mouse click or a key press: 

  * **CTRL** : If the `CTRL` key is pressed while selecting another day then this day is added to the selection. This way a non-contiguous array of dates can be selected.
  * **SHIFT** : If the `⇧ Shift` key is held down during day selection then all day between the prevsiously and the currently selected day are added to the selection.
  * **Double-click** : A double click on a workday selects all workdays of the same week. Holding the `CTRL` or `⇧ Shift` key down extends the selection by the workdays of one or more weeks.
  * The selection can be extended into neighboring months if the arrow keys of the keyboard are pressed with the `CTRL` key held down.
  * Selection is cleared if any date is selected without pressed any of these keys, or if the arrows or dropdown menus in the top bar are used.
  * If previously selected days are added for a second time then they are unselected.



At **run-time** , the selected data can be controlled or queried by these methods of the calendar: 

  * `procedure AddSelectedDate(ADate: TDate)` \- adds the specified date to the list of selected dates
  * `procedure ClearSelectedDates` \- clears all selected dates
  * `function IsSelected(ADate: TDate): Boolean` \- returns `true` when the given date is selected, `false` otherwise.
  * `function SelectedDates: TCalDateArray` \- returns an array of TDate elements which contains the selected dates.



## Documentation

  * [CalLite: Usage](<CalLite__Usage.md> "CalLite: Usage")
  * [CalLite: Flag days in Finland](<CalLite__Flag_days_in_Finland.md> "CalLite: Flag days in Finland")



## See also

  * [TurboPower Visual PlanIt](<Turbopower_Visual_PlanIt.md> "Turbopower Visual PlanIt")
  * [DateTimeCtrls Package](<DateTimeCtrls_Package.md> "DateTimeCtrls Package")

---

_Source: [https://wiki.freepascal.org/CalLite](https://web.archive.org/web/20250601000000/https://wiki.freepascal.org/CalLite)_
