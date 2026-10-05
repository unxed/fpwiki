# Turbopower Visual PlanIt

│ **English (en)** │  **[русский (ru)](<../ru/Turbopower_Visual_PlanIt.md>)** │

## Contents

  * 1 About
    * 1.1 Screenshot
    * 1.2 Author
    * 1.3 License
    * 1.4 Download
    * 1.5 SVN
    * 1.6 Change Log: New features
    * 1.7 Incompatible changes
    * 1.8 Bug reporting / Feature request
    * 1.9 Dependencies / System Requirements
    * 1.10 Installation
    * 1.11 Documentation
  * 2 Using the planner components
    * 2.1 Getting started
      * 2.1.1 A simple project
      * 2.1.2 Event categories
      * 2.1.3 Playing an event sound
    * 2.2 Datastores
      * 2.2.1 TVpIniDatastore
      * 2.2.2 TVpXmlDatastore
      * 2.2.3 TVpJsonDatastore
      * 2.2.4 TVpBufDSDatastore
      * 2.2.5 TVpSqlite3Datastore
      * 2.2.6 TVpFirebirdDatastore
      * 2.2.7 TVpZeosDatastore
      * 2.2.8 TVpFlexDatastore
      * 2.2.9 Other datastores
    * 2.3 Holidays
    * 2.4 Overlaying events
    * 2.5 Printing and print preview
    * 2.6 Localization
    * 2.7 TVpDayView, TVpWeekView, and TVpMonthView reference
      * 2.7.1 Common
        * 2.7.1.1 Properties
        * 2.7.1.2 Events
      * 2.7.2 TVpDayView only
        * 2.7.2.1 Properties
        * 2.7.2.2 Events
      * 2.7.3 TVpWeekView only
        * 2.7.3.1 Properties
      * 2.7.4 TVpMonthView only
        * 2.7.4.1 Properties
        * 2.7.4.2 Events
  * 3 Using the navigation bar
  * 4 Gadgets
    * 4.1 TVpLEDLabel
    * 4.2 TVpClock



## About

Visual PlanIt is a set of synchronized, data-aware components for adding time, task, & contact management capabilities to applications. Get that Outlook look & feel without the hassle. 

This component was designed for cross-platform applications. 

### Screenshot

[![Tvplanit.PNG](https://wiki.freepascal.org/images/f/fb/Tvplanit.PNG)](</File:Tvplanit.PNG>)

### Author

Author: Turbo Power Software 

LCL Port: [Christian Ulrich](</User:Christian> "User:Christian") and [Werner Pamler](</index.php?title=User:Wp&action=edit&redlink=1> "User:Wp \(page does not exist\)")

### License

[Mozilla Public Licence 1.1](<http://opensource.org/licenses/mozilla1.1.php>)

### Download

  * Version 1.010: <https://sourceforge.net/projects/lazarus-ccr/files/tvplanit/tvplanit-1.0.10.zip/download>
  * Version 1.08: <https://sourceforge.net/projects/lazarus-ccr/files/tvplanit/tvplanit-1.08.zip/download>
  * Version 1.06: <https://sourceforge.net/projects/lazarus-ccr/files/tvplanit/tvplanit-1.06.zip/download>
  * Version 1.04: <https://sourceforge.net/projects/lazarus-ccr/files/tvplanit/tvplanit-1.04.zip/download>



### SVN

You can checkout the actual source from <https://lazarus-ccr.svn.sourceforge.net/svnroot/lazarus-ccr/components/tvplanit>

The commandline to do this is 
    
    
    svn co <https://lazarus-ccr.svn.sourceforge.net/svnroot/lazarus-ccr/components/tvplanit>
    

### Change Log: New features

  * Feb 02, 2008 - Initial port (version 1.03)
  * Aug 01, 2016 - (Almost) complete port (version 1.04)
  * Nov 28, 2016 - Drag and drop, mORMot datastore, new fields, holiday support, designtime and runtime packages (version 1.06)
  * May 2018 - Adapt to Laz1.8+ high DPI support, json datastore.
  * May 04, 2018 - NavBar component editor (version 1.0.10)
  * June 21, 2018 - New properties for VpDayView and VpContactButtons, import from vCard and iCal files (version 1.2.0)
  * Jan 18, 2023 - Use category colors in week and month views. Simplify unlimited recurring events. Add context menu item to go to any date. Preview form for vCard and iCal import. Add new control: TVpGanttView. New icons by Roland Hahn (version 1.8.0)
  * Feb 10, 2023 - Bugfix for VpCalendar (version 1.8.1)


  * Current



### Incompatible changes

  * Version 1.0.4 does not compile with Delphi any more.
  * Starting with v1.0.11, the VpDayView events `OnDVBeforeDrawEvent` and `OnDVAfterDrawEvent` have an additional parameter `GutterRect`. This was done in order to allow custom painting of the event/gutter background.
  * Starting with v1.0.11, the events `OnOwnerEditEvent` of `TVpDayView`/`TVpWeekView`/`TVpMonthView`, `OnOwnerEditContact` of `TVpContactGrid`, and `OnOwnerEditTask` of `TVpTaskList` have a new parameter `IsNewEvent`, `IsNewContact`, and `IsNewTask` to indicate the custom editor whether a new event/contact/task is to be edited.



### Bug reporting / Feature request

Bug reports and feature requests should be sent to the Lazarus/Free Pascal Bug Tracker; please specify the "Lazarus-CCR" project: <http://bugs.freepascal.org/>

### Dependencies / System Requirements

  * FPC 2.6.4 or newer
  * Lazarus 1.4.4 or newer
  * These versions were available at the time of writing this update. It is not excluded that the package can be used with older versions as well.



### Installation

  * Create a directory for the components, such as _lazarus\components\tvplanit_
  * In this directory, unzip the files from the zip file, or execute the svn commandline from above.
  * Open lazarus
  * In the release verison 1.04 the library has designtime and runtime code in the same package. Open the package _laz_visualplanit.lpk_ from the folder _source_ with _Package/Open file (-lpk)_ and click **Compile**. Click **Use > Install** if you want to install the components into the IDE.
  * Beginning with version 1.06 the library will be split into designtime and runtime packages 
    * Open the runtime package _laz_visualplanit.lpk_ from the library root folder with _Package/Open package file (.lpk)_ and click **Compile** to compile it.
    * Open the designtime package _laz_visualplanit_design.lpk_ from the same folder, click **Use > Install**.
  * If you need the Zeos datastore repeat with package _laz_visualplanit_zeos.lpk_ (and _laz_visualplanit_zeos_design.lpk_) from folder _source/addons/zeos_. Make sure that the [Zeos components](<Zeos_tutorial.md> "Zeos tutorial") are installed.



### Documentation

If you need a full documentation of the the VisualPlanIt suite get TurboPower's original 300-page pdf file from the official SourceForge site of the Delphi version (<https://sourceforge.net/projects/tpvplanit/files/tpvplanit_docs/>). 

## Using the planner components

### Getting started

[![tvplanit tutorial.png](https://wiki.freepascal.org/images/6/60/tvplanit_tutorial.png)](</File:tvplanit_tutorial.png>)

#### A simple project

Here is a quick tutorial for your first steps with the VisualPlanIt components. 

  * Create a new application. Save it into its own folder.
  * Add a TPageControl with three sheets. Name them _"Events"_ , _"Tasks"_ , and _"Contacts"_.
  * In the component palette seek for the tab _Visual PlanIt_.
  * Pick `TVpDayView` and add it to the _"Events"_ sheet of PageControl. This is a day calendar and displays events (appointments, meetings, etc) for the entire day with a resolution as defined by its so-called `Granularity`. 
    * If you are used to times in the 24-hour format switch `TimeFormat` to `tf24Hours`.
    * If the calendar is sufficiently wide you may want to change `NumDays` to - say - 2 to see two days at once.
    * Give the components a more modern look by setting `DrawingStyle` to `dsFlat`.
    * Similarly you can add a `TVpWeekView` (a week calendar) and/or a `TVpMonthView` (a month calendar) to the "Events" sheet. They give an overview of the same events, but with less resolution. Text ellipsis ("...") are shown if the event text cannot be fully displayed.
  * Now add a `TVpTaskList` to the _"Tasks"_ sheet of the PageControl. It is a listing of to-do items along with their due dates. Overdue tasks are written in red. Completed tasks are striked out. Note that `DisplayOptions.ShowCompleted` or `.ShowAll` must be checked in order to see completed tasks.
  * Finally add a `TVpContactGrid` to sheet _"Contacts"_ of the PageControl.
  * Again, you may want to use `DrawingStyle` `dsFlat` for a more modern look.
  * Play with the properties to setup all these visual controls according to your preferences. The `TVpWeekView`, for example, has a property to define whether the week starts on Monday or Sunday (`WeekStartsOn`).


  * Two ingredient are missing: the **DataStore** and the **ControlLink**. 
    * The **Datastore** is a component which stores the data to be displayed in the visual controls. The Visual PlanIt package contains a series of predefined datastores - see below. We select here a TVpIniDatastore or TVpXmlDatastore which store the data in an ini or xml file - these stores do not require a database like the other datastores.
    * In the property `Filename`, enter a name for the datastore file, e.g. _data.ini_ or _data.xml_ , respectively. Using a relative filename here means that the path will be relative to the location of the exe file of the program which is convenient for this tutorial. In a "real" application, however, an absolute path may be preferrable. And it may also be advantageous to enter the filename at runtime, e.g. in the FormCreate event handler.
    * If the filename is known at designtime you can set `AutoConnect` to true in order to open the datastore automatically when the program starts. If the filename is specified at runtime leave `AutoConnect` at `false` and set the datastore's `Connected` property to true.
    * The **ControlLink** automatically establishes a link between the datastore and the visual controls and makes sure that the controls get their data if needed and are synchronized.


  * Now it's time to run the program
  * How to **add data**? This is done by double-clicking on any of the controls that we added to open a specific built-in editor for convenient entering of the data needed.
  * At the first time, however, a dialog pops up saying that "no resources have been defined". A **resource** is everything which can be administrated by the VisualPlanIt tools, such as an employee, a meeting room, a library of books, etc. Enter the name of the resource. If your program will deal with various resources you can add a TVpResourceCombobox to select the resource needed for the program run. Make sure to use unique resource names because VisualPlanIt will throw a duplicate-resource exception otherwise.
  * When you restart the program the next time you will notice that the data are no longer visible. No reason to be alarmed - the data are still there, but the program just does not know which resource to open. Enter this code in the FormCreate event, after connecting to the datastore:


    
    
    procedure TForm1.FormCreate(Sender: TObject);
    begin
      // Use this code if the filename is not set in the Object Inspector...
      VpXmlDatastore1.FileName := name_of_the_datastore_file;
      // ... and the datastore is not autoconnected.
      VpXmlDatastore1.Connected := true;
      // This code selects the first resource (index 0) found in the datastore.
      if VpXmlDatastore1.Resources.Count > 0 then
        VpXmlDatastore1.Resource := VpXmlDatastore1.Resources.Items[0];  
    end;
    

#### Event categories

[![tvplanit eventeditor.png](https://wiki.freepascal.org/images/e/e7/tvplanit_eventeditor.png)](</File:tvplanit_eventeditor.png>)

The event editor is used to enter or edit event details. You'll find here a combobox labeled "Category". The combobox items can be used to classify the events, like "Business", "Private", "Birthday" etc. The items, however, only have generic names, such as "Category 1", "Category 2", etc. You can select more descriptive names by visiting the datastore property `CategoryColorMap`. The field "Descriptions" defines the text to be displayed in the combobox. In order to be able to distinguish the categories in the DayView control you can also set up a background color and a bitmap here. The background colors can also be applied to the WeekView and MonthView controls when their `ApplyCategoryInfos` property is set to `true` (requires v1.7 at least). 

#### Playing an event sound

The planner components are prepared to play a sound some pre-defined time ahead of the occurence of an event. For this to happen the "Reminder" checkbox must be checked in the event editor. 

There are several ways to set up the sound: 

  * The datastore exposes a property `DefaultSound`. The sound file specified here will be played whenever an event with checked "Reminder" box happens.
  * Additionally, an individual sound file can be selected for each particular event by clicking at the loudspeaker icon next to the reminder interval.



In Windows, the sounds are played automatically without further code because there universal sound utilities are embedded. In Linux, however, this is less standardized. But the datastore provides an event `OnPlaySound` where the correct sound playing command can be executed. The _fulldemo_ project in the _examples_ folder contains a unit which checks for the sound engines typically available in various systems; this unit is an adapted version of the **playsound component** by Gordon Bamber in the _Lazarus components and code repository_ (<https://sourceforge.net/p/lazarus-ccr/svn/HEAD/tree/components/playsoundpackage/>). 

### Datastores

Datastores are responsible for persistent storage of event, task and contact data records. 

The ancestor of this hierachy of classes is `TVpCustomDatastore`. It implements the basic functionality and provides fundamental abstract methods, such as `LoadEvents`, `LoadTasks`, `LoadContacts`, `PostResources`, `PostEvents`, `PostTasks`, `PostContacts`. Its member class `Resources` (type: `TVpResources`) stores all resources in a list as `TVpResource` items. Such a resource item, in turn, contains other lists: `Schedule`, `Tasks`, and `Contacts` for all event, task and contact records. The currently selected resource can be accessed by the datastore's `Resource` or `ResourceID` properties (the latter is a unique numerical ID to identify the resource). 

`TVpCustomDBDatastore` inherited directly from `TVpCustomDatastore`. It is another (pseudo-) abstract class which provides the basic database functionality by means of four datasets, `ResourceTable`, `EventsTable`, `TasksTable`, and `ContactsTable`. Since datebases have very efficient methods of searching and filterung these datastores provide a property `DayBuffer` which defines a date range before and after the currently selected date for which records are loaded. 

#### TVpIniDatastore

This is a simple file datastore which inherits from `TVpCustomDatastore`. It stores the data records in a simple **ini** file structure. The name of the ini file must be specified in the corresponding property. The file is loaded after the datastore has been created if the design-time property `AutoConnect` is true, or when the run-tim property `Connected` is switched to true. The file is saved automatically when `Connected` goes back to `false` \- this happens before the datastore is destroyed. 

The demo project _examples/datastores/ini_ is an example how the ini datastore can be used. 

#### TVpXmlDatastore

This is another simple file datastore. It works similarly as the ini datastore except that the file is an **xml** file now. The datastore's tree of xml nodes can be inserted into any existing xml file, e.g. files written by `TXMLDataStorage`. For this purpose, the node must be specified to which the datastore's nodes will be attached (property `ParentNode`). 

Have a look at the demo project in _examples/datastores/xml_ to see the xml datastore at work. 

#### TVpJsonDatastore

Yet another simple file datastore inherited from `TVpCustomDatastore`. It takes the **json** file format to store the data records. Usage is demonstrated in the sample code in folder _examples/datastores/json_. 

#### TVpBufDSDatastore

The following datastores inherit from `TVpCustomDBDatastore`. The first one, `TBufDSDataset`, uses simple `TBufDataset` tables for storage of the planner data. The four required files are named "Resources.db", "Events.db", "Contacts.db" and "Tasks.db", and are stored in the folder specified by the property `Directory`. If `AutoCreate` is true then the tables are created automatically when the program runs for the first time; otherwise the datastore method `CreateTables` must be called. 

Application of this datastore is demonstrated in _examples/datastores/bufdataset_ and in _examples/fulldemo_. 

#### TVpSqlite3Datastore

This datastore is a specialized storage for SQLite3 tables. It uses SQLDB components for this purpose. You must add a TSQLite3Connection and a TSQLTransaction to the form, link them together using the corresponding properties. Link the SQLite3Connection to the `Connection` of the datastore. The SQLQueries required are created automatically if the datastore's `AutoCreate` is true or after `CreateTables` is called. The database is stored in the file specified by the connection's `DatabaseName`. 

The project in _examples/datastores/sqlite3_ demonstrates how the sqlite3 datastore can be used. 

#### TVpFirebirdDatastore

A Firebird database provides the storage for `TVpFirebirdDatastore`. Again, SQLDB components are used here. To establish a connection to the database, a `TIBConnection` must be linked to the datastore's `Connection`. It must provide all information to connect to the database, such as database name, user name and password. In addition, a `TSQlTransaction` component is needed. The database should be created by the tools provided by Firebird or by third-party database administration tools. It is possible to do this also from the datastore, but this results in an exception. If the datastore's `AutoCreate` the required tables are created automatically when not found; alternatively call `CreateTables` for the datastore. 

Usage of the datastore is demonstrated in _examples/datastores/fb_. 

#### TVpZeosDatastore

[![TvPlanit FieldMapping.png](https://wiki.freepascal.org/images/6/65/TvPlanit_FieldMapping.png)](</File:TvPlanit_FieldMapping.png>)

The `TVpZeosDatastore` works with the [ZEOS database components](<Zeos_tutorial.md> "Zeos tutorial"). Since they are not contained in the standard Lazarus distribution it is contained in a separate package, _laz_visualplanit_zeos_ , which is found in the _source/addons/zeos_ folder and must be installed after _laz_visualplanit_. 

A `TZConnection` is required to establishe a connection to the database. The tables are created internally, either automatically if `AutoCreate` is true, or manually by calling `CreateTable`. 

The demo project can be found in _examples/datastores/zeos_. 

#### TVpFlexDatastore

In the database-aware datastores mentioned so far, the tables are created by the datastore. This makes sure that the fields are named in the way as expected by the datastore. If, on the other hand, tables are already existing or are provided by other applications it is very likely that field names will not be reckognized. `TVpFlexDatastore` can be a solution. Its name indicates that this is a very _flexible_ datastore. In particular, it is possible to link to any database system if a TDataset-descendent is available. 

You must add four TDataset components to the form, each one corresponding to one of the Resources, Events, Contacts, and Tasks data, and link them to the `DataSsource` of the datastore via TDataSource components. Then double-click on the `TvVpFlexDatastore` to open the FieldMapper. This is a tool to assign the database fields to the internal fields of the datastore. For each database field that you want to include in the planner select the corresponding datastore field in the right list box. After clicking "Add" the pair of fields is shown in the "Field Mappings" listbox. 

There are two sample projects: 

  * _examples/datastores/flex/access_ demonstrates how an MS Access database can serve for storage using an ODBCConnection and other SQLDB components (there is also a _readme.txt_ with step-by-step instructions).
  * _examples/datastores/flex/sqlite3_ repeats the same with a SQLite3 database.



#### Other datastores

The following datastores from the original Delphi distribution are included in the source folder, but not added to the package because they require the BDE which is not available for Lazarus: 

  * **TVpBDEDatastore** for database access by means of the Borland Database Engine (BDE) (unit _vpbdeds_)
  * **TVpAdvDatastore** for Advantage Database (unit _vpadvds_)
  * **TVpFF2Datastore** for FlashFiler 2 databases (unit _vpff2ds_)
  * **TVpDBISAMDatastore** for DBISAM database access (unit _vpdbisamds_)
  * **TVpSQLDatastore** (unit _vpsqlds_): This is a very general datastore for any sql database. An interfaced class to the TDataset-descendent must be provided, and there must be a class specifying the particular sql dialect. For application in Lazarus, some restructuring of BDE-related properties will be required. The package contains an example for BDE access.



### Holidays

TvPlanIt has no ready-to-use support of holidays because this would involve a huge database covering all countries. Rather than that each datastore provides an event `OnHoliday` which queries the holiday name for the specified date; the holiday name is left empty when the day is not a holiday. The best way to apply this model is to calculate a list of the dates and names of all holidays for each year: 
    
    
    uses
      fgl, DateUtils;
    
    type
      THolidayList = specialize TFPGMap<TDateTime, string>;  
    
    var 
      FHolidays: THolidayList = nil;  // must be created and destroyed by the main form.
    
    { Calculates the date of some U.S. holidays for the specified year. }
    procedure TMainForm.CalcHolidays(AYear: Integer);
    
      // If a federal holiday falls on a Saturday the preceding Friday is a holiday.
      // If a federal holiday falls on a Sunday the following Monday is a holiday.
      // https://www.officeholidays.com/countries/usa/2022
      function InLieuHoliday(ADate: TDateTime): TDateTime;
      var
        wd: Integer;
      begin
        wd := DayOfTheWeek(ADate);
        if wd = DaySaturday then 
          Result := ADate - 1
        else
        if wd = DaySunday then
          Result := ADate + 1
        else
          Result := ADate;
      end;
        
    var
      d, d1: TDate;
    begin
      FHolidays.Clear;
      
      // New Year
      d := EncodeDate(AYear, 1, 1);
      FHolidays.Add(d, 'New Year');
      d1 := InLieuHoliday(d);
      if d <> d1 then
        FHolidays.Add(d1, 'New Year (in lieu)');
      
      // Easter & associated
      d := Easter(AYear);  // Calculation of the Easter date is shown in the "Easter" example in the Lazarus installation.
      FHolidays.Add(d-2, 'Good Friday');
      FHolidays.Add(d, 'Easter Sunday');
      FHolidays.Add(d+49, 'Whitsunday');
      
      // Independence day
      d := EncodeDate(AYear, 7, 4);
      FHolidays.Add(d, 'Independence Day');
      d1 := InLieuHoliday(d);
      if d1 <> d then
        FHolidays.Add(d1, 'Independence Day (in lieu)');
      
      // Labor Day (1st Monday in September)
      d := EncodeDayOfWeekInMonth(AYear, 9, 1, DayMonday);  
      FHolidays.Add(d, 'Labor Day (U.S.)');
      
      // Thanksgiving (4th Thursday in November)
      d := EncodeDayOfWeekInMonth(AYear, 11, 4, DayThursday); 
      FHolidays.Add(d, 'Thanksgiving (U.S.)');
      
      // Christmas
      d := EncodeDate(AYear, 12, 25);
      FHolidays.Add(d, 'Christmas Day');
      d1 := InLieuHoliday(d);
      if d1 <> d then
        FHolidays.Add(d1, 'Christmas Day (in lieu)');
      
      // Due to the off-days in the calendar, add also Christmas of the preceding
      // and New Year of the following years
      d := EncodeDate(AYear-1, 12, 25);   // -1 --> previous year
      FHolidays.Add(d, 'Christmas Day');
      d1 := InLieuHoliday(d);
      if d1 <> d then
        FHolidays.Add(d1, 'Christmas Day (in lieu)');
      
      // New Year
      d := EncodeDate(AYear+1, 1, 1);   // +1 --> following year
      FHolidays.Add(d, 'New Year');
      d1 := InLieuHoliday(d);
      if d <> d1 then
        FHolidays.Add(d1, 'New Year (in lieu)');
    end;
    

Since this code calculates the holidays for one year only the list must be updated when the year changes. This can be done in the OnDateChanged handler of the datastore; perform the calculation only when the year of the new date is different from the previously calculated year (`FHolidayYear: Integer`): 
    
    
    procedure TMainForm.DateChangedHandler(Sender: TObject; ADate: TDateTime);
    var
      year: Integer;
    begin
      year := YearOf(ADate);
      if year <> FHolidayYear then
      begin
        CalcHolidays(year);
        FHolidayYear := year;
      end;
    end;
    

In the `OnHoliday` event, finally, the holiday name then can be found easily: 
    
    
    procedure TMainForm.VpHoliday(Sender: TObject; ADate: TDateTime;
      var AHolidayName: String);
    var
      idx: Integer;
    begin
      if FHolidays.Find(ADate, idx) then
        AHolidayName := FHolidays.Data[idx];;
    end;
    

### Overlaying events

The DayView and WeekView components primarily display events of a single resource. It is possible, however, to switch into an **overlay mode** where other resources can be displayed within the same controls. This can be achieved by assigning a `TVResourceGroup` to the resource (property `Group`): 
    
    
    type
      TVpOverlayPattern = (opSolid, opClear, opHorizontal, opVertical, opFDiagonal, opBDiagonal, opCross, opDiagCross);
    
      TVpOverlayDetail = (odResource, odEventDescription, odEventCategory);
      TVpOverlayDetails = set of TVpOverlayDetail;  
    
      TVpResourceGroup = class
        //...
        property Caption: String;
        property Count: Integer;
        property Items[AIndex: Integer]: TVpResource;
        property Pattern: TVpOverlayPattern;
        property ResourceID: Integer;
        property ReadOnly: boolean read FReadOnly;
        property ShowDetails: TVpOverlayDetails;
      end;
    

The most convenient way to create a resource group is the `OverlayResources` method of a resource. It takes an array of the resource instances to be grouped as a parameter. When the array is empty an existing group is ungrouped. An optional `ACaption` parameter defines the name of the resource group. The following code snippet (taken from the _FullDemo_ sample project) assumes that all resources which can be grouped with the currently active resource are listed in a `TCheckListBox` (named `lbOtherResources`) and is executed when one of the listbox entries is checked or unchecked: 
    
    
    procedure TMainForm.lbOtherResourcesClickCheck(Sender: TObject);
    var
      i, n: Integer;
      resArray: TVpResourceArray;
    begin
      // Collect resources checked for overlaying in an array
      SetLength(resArray, lbOtherResources.Items.Count);
      n := 0;
      for i := 0 to lbOtherResources.Items.Count-1 do
        if lbOtherResources.Checked[i] then
        begin
          resArray[n] := TVpResource(lbOtherResources.Items.Objects[i]);
          inc(n);
        end;
      SetLength(resArray, n);
    
      // Overlay the checked resources to the currently active resource
      VpControlLink1.DataStore.Resource.OverlayResources(resArray);
    end;
    

Another way is to call the method `AddResourceGroup` of the datastore's `Resources` list. Here the IDs of the overlayed resources must be specified as an array in the first parameter of the procedure. The first id plays a special role: it refers to the resource to which the group is attached. If this resource is displayed in a Dayview or WeekView component then the events of the other specified ID(s) are overlayed within the same control. 
    
    
    procedure TMainForm.CreateResourceGroup;
    const
      NAME_OF_GROUP = '';  // empty --> use resource descriptions instead
      PARENT_ID = 1;       // ID of the resource which will display the entire group
      OVERLAYED_ID = 2;    // ID of the resource to be overlayed in the view of the PARENT_ID resource.
    var
      datastore: TVpCustomDatastore;
      grp: TVpResourceGroup;
    begin
      datastore := VpControlLink1.Datastore;
      grp := datastore.Resources.AddResourceGroup([PARENT_ID, OVERLAYED_ID], NAME_OF_GROUP);
      grp.ReadOnly := true;
      grp.Pattern := opDiagCross;
      if datastore.Resource <> nil then
        datastore.Resource.Group := grp 
      else
        datastore.Resource.Group := nil;
      datastore.RefreshEvents; 
    end;
    

The level of detail visible to the user seeing overlayed events is controlled by the property `ShowDetails` of the resource group. By default, only the name of the overlayed resource is visible. If more details are to be exposed you can add the other elements of `TVpOverlayDetails`. The background of overlayed events in the dayview is hatched as specified by the `Pattern` property of the resource group if the event category is not excluded from the visible details; otherwise the event is drawn as specified by the property `HiddenCategories` of the Datastore. 

Editing, deleting, drag-and-drop of overlayed events is off by default, but can be allowed by setting the `ReadOnly` property of their resource group to false. 

In order to turn resource groups on and off the context menu of DayView and WeekView components is extended by a corresponding submenu. The submenu contains an item "none" meaning "no overlayed events" as well as an item for each resource group in which the currently selected resource is the parent. The caption of the menu items is defined by the (optional) second parameter of the `OverlayResources` and `AddResourceGroup` calls; if empty the group name is derived from the descriptions of the overlayed resources. 

### Printing and print preview

[![tvplanit printformatdesigner.png](https://wiki.freepascal.org/images/d/db/tvplanit_printformatdesigner.png)](</File:tvplanit_printformatdesigner.png>)

The VisualPlanIt package contains routines for printing the planner controls. Even a print preview is implemented. The printing process is defined by means of templates which are called "print formats" here. They can be created at designtime or by means of an xml file loaded at runtime, contain a list of the components to be printed. 

The heart of printing support is the TVpPrinter class - it is accessible by the `Printer` property of the `TVpControlLink`. It contains extra information needed for the printing process and contains a list of the available `PrintFormats`. 

In order to quickly access the print format designer double-click on the `TVpControlLink` component. Click on the upper "New" button to open the basic format editor. Here you can assign a reasonable name to the print format. Clicking on the lower "Edit" opens the "Element" editor - this is the tool to define the contents on the print print format, similar to a report designer, however less comfortable. In addition to the planner controls, DayView, WeekView, MonthView, TaskList and ContactGrid, the editor allows also to add additional text and shape objects to the format. Multiple elements can be contained within the same format. Position the elements by using the Top/Left/Width/Height specifications at the right. In addition, the entire format page can be rotated in 90° steps. 

A **print preview** can be generated by means of a `TVpPrintPreview` component. Assign the ControlLink of the form to the `ControlLink` and the `Printer` of the _Printers_ units to the `Printer` property of the preview component. For printing to work correctly, you must have the package _Printer4Lazarus_ in the requirements of the project; this can be done most easily by adding a standard `TPrintDialog` to the form which you will probably need anyway. Open the print preview by calling its `Execute` method. It should be mentioned that the print preview itself does not issue any printing commands. Therefore, extra printing code must be written if the user decides to initiate a print-out from the preview, something like this: 
    
    
    if VpPrintPreviewDialog1.Execute then
      if PrintDialog1.Execute then begin
        Printer.BeginDoc;
        try
          VpPrintPreviewDialog1.ControlLink.Printer.Print(
            Printer, 
            VpPrintPreviewDialog1.StartDate, 
            VpPrintPreviewDialog1.EndDate
          );
        finally
          Printer.EndDoc;
         end;
      end;
    

See the _fulldemo_ project in the _examples_ folder for a worked-out example. 

### Localization

All internal strings used by the VisualPlanIt package can be localized by means of the translation utilities available within Lazarus. The **translated po files** can be found in the folder _languages_ of the distribution. Copy them to the language folder of your application, and translate them by calling 
    
    
    uses
      translations;
    var
      langdir: String;  // language folder of your application 
      lang: String;     // language abbreviation, e.g. "de" for German
    //...
      TranslateUnitResourceStrings('vpsr', langdir + 'vpsr.' + lang + 'po');
    

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** Users are cordially invited to submit translations of the resource strings to their native language

In addition to providing translated strings a variety of **date/time related properties** of the planner controls should be adapted: 

  * Set `VpDateView.DateLabelFormat` to `DefaultFormatSettings.LongDateFormat`, dto. with `VpWeekView.DayHeadAttributes.DateFormat` and `VpWeekView.DateLabelFormat`.
  * Set `VpMonthView.DatelabelFormat` to the string `'mmmm yyyy'` which means "month in words + year".
  * Set `VpTaskView.DisplayOptions.DueDateFormat` to `DefaultFormatSettings.ShortDateformat`.
  * Determine the `TVpTimeFormat` of your system by calling `GetTimeFormat` (in Unit _vpmisc_). The result is either `tf12Hour` or `tf24Hour` and should be assigned to the `TimeFormat` properties of VpDayView, VpWeekView and VpMonthView.
  * Set the properties `WeekStartsOn` of VpMonthView and VpWeekView to the day code of the first day of a week (1 = Sunday, ..., 7 = Saturday).



Finally, the `TVpControlLink` implements procedures for localizing **addresses** in the Contact Editor Dialog. For this purpose, the file _vplocalize.xml_ accompanies the VisualPlanIt package. It contains rules for construction of addresses in the correct format varying from country to country. If this filename is assigned to the property `LocalizationFile` of the ControlLink then these rule are effective when the Contact Editor Dialog is created. Please note that this file is not translated by the po files. 

### TVpDayView, TVpWeekView, and TVpMonthView reference

These are the main visual components of the TvPlanIt library, they display the same information, but group it on different time-scales. 

#### Common

##### Properties

  * **`AllowDragAndDrop: boolean`** : Activates the feature that TvPlanIt events can be dragged to other time slots or other viewing controls.
  * **`AllowInplaceEditing: boolean`** : If enabled the `Description` of the selected TvPlanit event can be edited in-place by pressing the `F2` button.
  * **`ApplyCategoryInfos: boolean`** \- when `true` the planner event category colors are applied when drawing the event. Note that this is property is not available in `TVpDayView` since this behaviour is activated automatically.
  * **`DateLabelFormat: String`** : Defines the format how dates are displayed by the control. Use the conventions established for the standard `FormatDateTime()` function.
  * **`DragDropTransparent: Boolean`** : Activates a semitransparent copy of the TvPlanIt event's area during a drag-and-drop operation.
  * **`DrawingStyle: TVpDrawingStyle = (dsFlat, ds3d, dsNoBorder)`** : Defines how the border of the control is drawn
  * **`HeadAttributes: TVpHeadAttributs`** : Contains `Color` and `Font` elements which determine how the headers of these table-like controls are painted. `TVpDayView` has also a **`RowHeadAttributes`** property for the row headers, with separate fonts for display of hours and minutes.
  * **`HintMode: TVpHintMode = (hmPlannerHint, hmComponentHint)`** : Defines the information displayed in the component hint window. If `hmPlannerHint` is selected the hint summarizes the information of the TvPlanIt event over which the mouse hovers. If `hmComponentHint` is selected the hint displays the string assigned to the `Hint` property of the control.
  * **`LineColor: TColor`** : This is the color of the grid lines.
  * **`PopupMenu: TPopupMenu`** : By default, returns the built-in popup menu of the component (allowing to access the individual menu items). An external popupmenu can be attached to replace the default menu. Attach an empty TPopupMenu if you want to turn off the built-in menu.
  * **`ShowEventTime(s): Boolean`** : If `true` the start and end times of the TvPlanIt events are added the the event description in the control (Note that this property is named in plural for `TVpDayView`, but in singular for the others).
  * **`TimeFormat: TVpTimeFormat = (tf24Hour, tf12Hour)`** : Determines whether event times are displayed in the 24-hour or 12-hour format.
  * **`WeekStartsOn: TVpDayType = (dtSunday, dtMonday, dtTuesday, dtWednesday, dtThursday, dtFriday, dtSaturday)`** : Identifies the first day of a week. Not available for `TVpDayView`.



##### Events

Please be aware that the word "event" has two meanings in the following section: a Lazarus event or a TvPlanit event... 

  * **`AfterEnter`** : Fires when the control is focused
  * **`AfterExit`** : Fires when the control loses focus
  * **`AfterEdit`** : Fires when editing of the TvPlanIt event description in the inplace editor is completed. Not available in `TVpMonthView`.
  * **`BeforeEdit`** : Fires when the inplace editor for editing of the TvPlanIt event description is invoked by pressing `F2`. Not available in `TVpMonthView`.
  * **`OnAddEvent`** : Fires when a new TvPlanIt event has been added by means of the event editor (opened after double-clicking on the event position in the control).
  * **`OnDeletingEvent`** : Fires before a TvPlanIt event is deleted and by-passes the default confirmation prompt. You can provide your own confirmation message. Note that you must set the argument `DoIt` to false if you want to inhibit deletion.
  * **`OnHoliday`** : The painting routine queries here whether the currently drawn day is a holiday. If yes, it is painted in a special color.
  * **`OnModifyEvent`** : Fires when an existing TvPlanIt event has been changed by means of the event editor (opened after double-clicking on the event position in the control).
  * **`OnOwnerEditEvent`** : Allows to by-pass the built-in event editor for definiting the specification of new or existing TvPlanIt events.



#### TVpDayView only

##### Properties

  * **`AllDayEventAttributes: TVpAllDayEventAttributes`** : Defines how a TvPlanIt event is displayed which extends over the entire day. All-day events are displayed above the DayView's time-table. 
    * `BackgroundColor: TColor`: Is the background color of the all-day event area
    * `EventBackgroundColor: TColor`: The background color of the event itself
    * `EventBorderColor: TColor`: The color of the event border
    * `Font: TFont`: The font used to display the event description.
  * **`DefaultTopHour: TVpHours`** : This enumeration of hours follows the schema `h_00`, `h_01` ... `h_23` and defines the hour at which the DayView's time-table starts by default.
  * **`DotDotDotColor: TColor`** : When a TvPlanIt event description is longer than the space reserved for it on the time-table, three dots are displayed in the color defined by this property.
  * **`FixedDate: Boolean`** : If `true` the VpDayView cannot be scrolled to another date by user interaction.
  * **`Granularity: TVpGranularity = (gr05Min, gr06Min, gr10Min, gr15Min, gr20Min, gr30Min, gr60Min)`** : Defines the time range covered by a time-table cell.
  * **`GutterWidth: Integer`** : The width of the highlighting bar at the left of the each TvPlanIt event cell. The gutter is drawn in the `Color` of the event's category.
  * **`IconAttributes: TVpDayViewIconAttributes`** : Defines which icons should be displayed in each event cell: 
    * `ShowAlarmBitmap: boolean`: Displays a bitmap for those events for which an alarm rings. The bitmap can be selected either by the `AlarmBitmap` property or by the `AlarmImageIndex` in the datastore's imagelist, `Images`.
    * `ShowCategoryBitmap: boolean`: Displays the icon of the category assigned to the event in this cell
    * `ShowInPrint: boolean`: Main "switch" to turn icon display off (badly named...). Further control on icon display is possible by means of the `OnDrawIcons` event.
    * `ShowRecurringBitmap: boolean`: Displays a bitmap for recurring events. The bitmap can be selected either by the `RecurringBitmap` property or by the `RecurringImageIndex` in the datastore's imagelist, `Images`.
  * **`IncludeWeekends: boolean`** : Skips displaying weekend days in the control.
  * **`NumDays: Integer`** : Number of days displayed as columns in the control.
  * **`RowHeight: Integer`** : Height of the time table rows; automatic adjustment depending on `RowHeadAttributes.MinuteFont.Size` if `0` (default).
  * **`RowLinesStep: Integer`** : Determines the number of horizontal grid lines omitted. Normally (when `RowLinesStep` is 1) grid lines are drawn as set up by the `Granularity`.
  * **`ShowNavButtons: Boolean`** : Displays date navigation buttons in the upper left corner of the control
  * **`ShowResourceName: Boolean`** : Displays the name of the resource to which the current schedule belongs at the top of the time table.
  * **`WrapStyle: TVpDVWrapStyle`** : Determines how text longer than the line in the dayview is wrapped: 
    * `wsNone`: no wrapping, the text is displayed in a single line and ends with an ellipsis (...) if it is too long.
    * `wsIconFlow`: the text is word-wrapped and flows around the icons (if any) displayed in the line. An ellipsis (...) is displayed if the text still is too long.
    * `wsNoFlow`: the text is word-wrapped and indented by the width of the icons (if any) displayed in the line. An ellipsis (...) is displayed if the text still is too long.



##### Events

  * **`OnAfterDrawEvent`** : Fires after each TvPlanIt event is painted.
  * **`OnBeforeDrawEvent`** : Fires before each TvPlanIt event is painted.
  * **`OnDrawIcons`** : Fires when a TvPlanIt event is drawn on the time-table and allows to hide specific icons. The icon types are classified by the enumeration `TVpDVIconTypes = (itAlarm, itRecurring, itCategory, itCustom)`. Icons to be drawn must be listed in the `Icons` set which is given as a parameter in addition to the event being painted.
  * **`OwnerDrawCells`** : Allows to override the built-in painting procedure of event cells.
  * **`OwnerDrawColHeader`** : Allows to override the built-in painting routine of the header of a time-table day column
  * **`OwnerDrawRowHeader`** : Allows to override the built-in painting routine of the header of a time slot row.



#### TVpWeekView only

##### Properties

  * **`AllDayEventAttributes: TVpAllDayEventAttributes`** : Defines how a TvPlanIt event is displayed which extends over the entire day. 
    * `BackgroundColor: TColor`: Is the background color of the all-day event area
    * `EventBackgroundColor: TColor`: The background color of the event itself
    * `EventBorderColor: TColor`: The color of the event border
    * `Font: TFont`: The font used to display the event description.
  * **`EventFont: TFont`** : Font used for painting the TvPlanIt event information in the weekly time-table.
  * **`Layout: TVpWeekViewLayout = (wvlVertical, wvlHorizontal)`** : Defines whether the VpWeekView puts the week days into its grid first horizontally or first vertically.
  * **`WeekStartsOn: TVpDayType = (dtSunday, dtMonday, dtTuesday, dtWednesday, dtThursday, dtFriday, dtSaturday)`** : Identifies the first day of a week.



#### TVpMonthView only

##### Properties

  * **`DayNameStyle: TVpMVDayNameStyle = (dsLong, dsShort, dsLetter)`** : Determines which kind of day names are displayed in the header row of the month grid: full name ('Monday'), abbreviation ('Mo') or only first letter ('M').
  * **`DayNumberFont: TFont`** : Font used to draw the day numbers.
  * **`EventDayStyle: TFontStyles`** : Font style used to highlight days with assigned TvPlanIt events.
  * **`EventFont: TFont`** : Font used when painting the TvPlanIt event information in the month grid.
  * **`HolidayAttributes:TVpMvHolidayAttr`** : Background color and font of calendar cells which correspond to holidays.
  * **`KBNavigation: Boolean`** : Allows to navigate the calendar using the arrow keys on the keyboard.
  * **`OffDayColor: TColor`** : Background color for the overflow days from the previous and next months
  * **`OffDayFontColor: TColor`** : Font color for the overflow days from the previous and next months
  * **`RightClickChangeDate: Boolean`** : Allows a right-click to change the selected date to the clicked cell.
  * **`SelectedDayColor: TColor`** : Color of the selected day number.
  * **`ShowEvents: Boolean`** : Allow to show/hide TvPlanIt event information in the calendar grid.
  * **`TodayAttributes: TVpMvTodayAttr`** : Background color and font of today's cell.
  * **`WeekendAttributes: TVpMvWeekendAttr`** : Background color and font of weekend cells.



##### Events

  * **`OnEventClick`** : Fires when a TvPlanIt event is clicked
  * **`OnEventDblClick`** : Fires when a TvPlanIt event is double-clicked.



## Using the navigation bar

[![vpnavbar.png](https://wiki.freepascal.org/images/7/77/vpnavbar.png)](</File:vpnavbar.png>)

`TVpNavBar`, seen in the left of this screenshot of the _navbar_ demo, is a navigation tool smiliar to the one known from Outlook. It consists of several **folders** represented by button-like controls and several **items** \- these are the labelled icons in the large space between two buttons. The folder button - which can be drawn in several button- or tab-like styles - is always shown above the associated item area. Clicking on a folder divides the stack of folders in an upper and lower part, and displays the items assigned to this folder between the corresponding folder at the top and the next folder at the bottom. The index of the currently expanded folder is given by the property `ActiveFolder`, and the index of the clicked items within this folder is given by property `ActiveItem`. An event `OnItemClick` fires when an item is clicked, it has the index of the clicked item as a parameter. 

An **icon** can be assigned to each item. These images are collected in the image list assigned to the property `Images` of the navigation bar. The images normally should be 32x32 pixels. The property `IconSize` of each folder determines whether these images are displayed directly as "large images" or reduced by a factor 2 (16x16 pixels) as "small images". The down-scaling is done by the component.   


[![vpnavbareditor.png](https://wiki.freepascal.org/images/d/d1/vpnavbareditor.png)](</File:vpnavbareditor.png>)

**Populating a TVpNavBar at designtime** is easy if the version of VisualPlanIt is at least 1.0.10 in which the component editor of the NavBar is functional. Just double-click on the component, or select "Layout Tool..." from the context menu of the component. This opens the component editor displayed here as a screenshot. It contains three listboxes: 

  * The **left listbox** shows the **folders** found in the navigation bar. Click on the "+" button to add a new folder, "-" to delete the selected folder, or the "up"/"down" arrows to rearrange the folders. The properties of the selected folder are displayed in the object inspector: 
    * Specify the `Caption` of the folder - this is the text to appear on the button/tab of the folder.
    * Leave `IconSize` at `isLarge` if you want to use large icons for this folder's items. In this case, icons and their captions will be centered, the caption will be below the icon and word-wrapped if the text is longer than the width of the NavBar. If you select `isSmall`, on the other hand, icons and captions are left aligned, and the (non-wrapped) captions are drawn at the right of the icons. `IconSize` can change from folder to folder.
    * Do not set the `FolderType` to `ftContainer` \- this feature for adding arbitrary controls to the folder is not working in Lazarus at the time of this writing.
  * The **center listbox** shows the **items** assigned to the folder highlighted in the left list. Caption and icon of this item are displayed in each line. Again click on the "+", "-", or "up"/"down" buttons to add, delete or rearrange the items. And again, the properties of the selected item can be changed in the object inspector. Besides the `Caption` already mentioned for the folders, there is also a property `ImageIndex` which specifies the icon displayed for the item. The image index refers to the ImageList assigned to the property `Images` of the NavBar.
  * The **right listbox** contains all the **images** found in the ImageList. It is intended to simplify the assignment of icons to items: Select the item for which an icon is to be selected. pick the icon in the right image list and click on the arrow between the item and images listboxes.



If the VisualPlanIt version is older than version 1.0.10, then getting folders and items into the NavBar is a bit more complicated because the component editor had not yet been ported/adapted to Lazarus. In this case, it is best to learn this task by means of the object inspector: 

  * Add a TVpNavBar component to the form.
  * Add an imagelist with, ideally 32x32 images, and assign it to the `Images` property of the NavBar.
  * Click on the "..." button next to `FolderCollection` to open the collection editor for the folders.
  * Click "Add" to add a new folder.
  * Select the new folder in the collection editor, its properties appear in the object inspector and can be changed there.
  * In order to add items to the folder click on `ItemCollection`. This opens the collection editor for the items belonging to the current folder.
  * Again click "Add" to create a new item, and select it in the collection editor to enter its properties in the object inspector.



## Gadgets

[![vpgadgets.png](https://wiki.freepascal.org/images/5/5e/vpgadgets.png)](</File:vpgadgets.png>)

In addition to the planner components the Visual PlanIt package contains also two stand-alone gadgets: 

### TVpLEDLabel

This is a 14-segment LED display which can display any ASCII string. The string is defined by means of the property `Caption`, but the count of LED characters must be defined explicitly using the property `Columns`

### TVpClock

This is a digital or analog clock - depending on the property `DisplayMode`. A variety of settings can be adjusted for each mode separately by using the properties in `AnalogOptions` and `DigitalOptions`. The properties are pretty much self-explanatory, except maybe the `DigitalOptions.MilitaryTime` which activates the 24-hour display if `true`, or the 12-hour am/pm mode if `false`. The `AnalogOptions.ClockFace` replaces the default-drawn clock face by a bitmap - see screenshot. 

With the property `ClockMode` the operation of the clock can be changed from standard clock to count-up or count-down timer, `cmClock`, `cmTimer`, `cmCountdownTimer`, respectively. 

The property `Active` starts the clock when set to `true`, or stops it when set to `false`. There is also a property `Hold` which temporarily pauses the clock when set to `true` and resumes it when set to `false`. Convenience methods `Start`, `Stop`, `Pause` and `Resume` have the same purpose. 

Property `TimeResolution` determines how often the display is updated (in milliseconds).

---

_Source: [https://wiki.freepascal.org/Turbopower_Visual_PlanIt](https://web.archive.org/web/20240409061932/https://wiki.freepascal.org/Turbopower_Visual_PlanIt)_
