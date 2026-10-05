# FPReport

│ **English (en)** │  **[русский (ru)](<../ru/FPReport.md>)** │

## Contents

  * 1 Architecture
  * 2 units
  * 3 Report structure
  * 4 Types of bands
    * 4.1 DataBand
    * 4.2 Report title and Summary
    * 4.3 Page Header and Footer
    * 4.4 Data Header and Footer
    * 4.5 Group Header and Footer
    * 4.6 Column Header and Footer
    * 4.7 Child band
  * 5 Printable elements
  * 6 Data loops
    * 6.1 Data loop structure
    * 6.2 User data loop
    * 6.3 DB Data loop
    * 6.4 Collection Data loop
    * 6.5 ObjectList Data loop
    * 6.6 JSON Data loop
  * 7 Loading and Saving to file
  * 8 Data management
  * 9 More Info
    * 9.1 FAQ



## Architecture

FPReport is a banded reporting tool. 

It was designed from zero, to be able to run on a headless webserver, with minimal dependencies. An important use-case was a Linux server running in a container without X libraries installed. Creating a report is just placing squares with contents on a page, usually based on a data loop. This can be done entirely in-memory, and does not need a display or GUI. 

Reports can be created entirely in code, or they can be read from a definition file. The default shipped file uses [JSON](<JSON.md> "JSON") as the format to store the report definition. The reports can also be designed visually, using a designer. A stand-alone designer exists, as well as the possibility to design a report within the Lazarus IDE. 

Once the report is created, it can be saved to disk, or rendered. Rendering is the act of creating visual output. 

Various renderers have been implemented: 

  * To [PDF](<PDF.md> "PDF"). The PDF renderer is the renderer of choice
  * To a series of images. The FPImage renderer creates a bitmap per page, and the bitmaps can be saved in a directory in any of the supported FPImage formats.
  * To HTML: each page is rendered as a HTML page using appropriate CSS markup.



    Parts that cannot be rendered as HTML and CSS will be rendered as an image and placed in the HTML page.

  * To the LCL (a canvas).



    This is in fact the basis of the preview, the designer and the printing support.

  * To an AggPas canvas. AggPas is a powerful graphics library, and this renderer is similar in scope to the FPImage renderer
  * To fpGUI - this can be used to preview the report in a FPGUI report



## units

fpreport
    To create a report in code, this unit is all that is necessary.
    It contains the basic report, page, bands and elements.
fpreportstreamer
    This unit contains the streamer, which reads/writes a report definition from/to file.
fpjsonreport
    This unit contains a report class that can be dropped on a Lazarus form/Datamodule:
    the report definition will be written to the form file.
fpreportdb
    This unit contains the data loop component **TFPReportDatasetData** , which bases the loop on data from a dataset.
fpreportpdfexport
    This unit contains the PDF exporter **TFPReportExportPDF**.
    An instance of this class can be used to export a report to PDF.
fpreporthtmlexport
    This unit contains the HTML exporter **TFPReportExportHTML**.
    An instance of this class can be used to export a report to HTML pages.
fpreportfpimageexport
    This unit contains the image exporter **TFPReportExportfpImage**.
    An instance of this class can be used to export a report to any supported image format.

## Report structure

A report is an instance of the **TFPReport** class. It consists of one or multiple pages, each page is an instance of **TFPReportCustomPage**. The instances are available in the **Pages** array property of the report class. The number of pages is reported in the **PageCount** property. 

When the report is rendered, first the first page will be rendered (according to its data loop), then the second page will be rendered, and so on. 

Each page can consist of one or more bands. Some bands will appear only once in the output, others on regular places, but most bands will appear an arbitrary number of times, depending on the data which is given to the report. Typically there will be a data band which is printed once for each record in the data loop. 

Every band contains one or more elements (a descendent of **TFPReportElement**). 

Every time when the band is printed, all the elements on the band are printed. There are several types of element. The basic ones are 

  * A Memo. This is a text element which prints a text.



    The text can contain placeholders, which will be calculated every time the memo is printed.

  * A checkbox. This is a graphical element, which prints a checkbox (checked or not)
  * A image. This is a graphical element, which prints an image.
  * A shape. This is an element which can print one of a series of geometrical shapes.



The elements share some common features, such as the ability to draw a frame around it, and to have a background color. 

## Types of bands

FPReport has various types of bands. Each band has its own purpose. Some bands can appear only once in the definition of a report, other bands can appear multiple times. 

### DataBand

This is the band (class **TFPReportDataBand**) that will be printed once for each record in the report data loop. Multiple data bands can be put in a report, if they are linked in a master-detail relation. 

### Report title and Summary

A report title (**TFPReportTitleBand**) and summary (**TFPReportSummaryBand**) can appear once on each page of the report. The are printed once: when the report rendering is started, and when the rendering ends. 

### Page Header and Footer

A Page Header (**TFPReportPageHeaderBand**) and footer (**TFPReportPageFooterBand**) are printed at the top and bottom of each page. This kind of band can be added once per page. There are options to skip printing the header on the first page, and the footer on the last page. 

### Data Header and Footer

The data header (**TFPReportDataHeaderBand**) and footer (**TFPReportDataFooterBand**) are printed when a data loop starts, and ends. If a report contains multiple pages and uses multiple loops, then the report summary and title are printed only once, but the data header and summary are printed for every loop. This band can be added once for each data loop on the report. 

### Group Header and Footer

Data can be grouped based on an expression. When the value of the expression changes, a new group is started. A group is defined by placing a group header band (**TFPReportGroupHeaderBand**) on a page. Totals of a group can be displayed at the end of the group using a group footer (**TFPReportGroupFooterBand**) 

This band can be added once for each data loop on the report. 

### Column Header and Footer

A report can be on multiple columns. At the head of each column (on every page), a header can be printed using the Column Header band (**TFPReportColumnHeaderBand**). At the bottom of a column (again, on every page), a column footer band (**TFPReportColumnFooterBand**) can be printed. 

This band can be added once for each page in the report. 

### Child band

With the exception of Page footer and column footer bands, every band can have a child band (**TFPReportChildBand**) attached to it. This can be useful for aligning purposes, for example when a band contains memo that grows, and the band is configured to stretch, so the content of the memo is accomodated. 

This band can be placed an arbitrary number of times on the report. 

## Printable elements

On a band, you can drop one or more printable elements. A printable element is one of the following: 

  * a Memo. This is a text element. The text can contain formulas, and a limited set of HTML tags, such as bold, italic, underline and font or anchor tags.



The formulas will be evaluated, and the result will be substituted in the text. 

  * A Shape: this is one of several shapes such as a circle, square, triangle or straight lines in various orientations.
  * An image: You can embed any image in your report. All image formats as supported by FPC fcl-image can be used.
  * A checkbox. This is a special case of the image component: it uses 2 images, one for true, the other for false values.



Which one is used depends on the value of an expression which is evaluated every time the checkbox is printed. 

  * A Barcode element. This element is in a separate unit (fpreportbarcode) and can be used to draw one of many types of barcode.
  * a QRcode element. This element is in a separate unit (fpreportqrcode) and can be used to draw a QR code. It has some properties to determine the kind of barcode and how to position it.



## Data loops

The data loop is a basic concept in the report. It can be thought of as an abstraction of an array of records: the data band will be printed for each "record" in the "array". The "fields" of each record are available to be used in an expression, and when an expression is evaluated which contains a field, then the value of the field for the current record will be used in the expression. 

Clearly, the data loop determines the number of pages in a rendered report. 

Currently, 5 kinds of data loop are available: 

  * A user data loop.



    This loop is entirely event driven, and the various events are used to fetch the data.

  * A DB data loop.



    This loop is driven by a data set: the record is 1 record in the data set, the 'fields' in the record are the fields in the dataset.

  * A TFPObjectList based loop.



    This uses RTTI of the objects to expose the data in the loop.

  * A TCollection based loop.



    Similar to the TFPObjectList loop, this uses RTTI of the objects to expose the data in the loop.

  * A JSON array/Object based loop.



    This takes a JSON array (or object) and will loop over the elements in the array.

### Data loop structure

Data loops appear on several levels, they determine the structure of the report. 

  * The page can have a data loop.
  * The main (master) data band. Only one such band can exist on the page.



    This is the main data loop for the page.

  * Detail data bands.



    The loop of these bands are run every time the master data band of the detail data bands is printed.
    Detail data bands can be nested, a detail data band can be the master of another detail data band.

Per page in the report, the engine will determine the main data loop for that page. 

This is the first data band's data loop, if the data band has no master. This will be used to start rendering the bands on that page. 

Usually, Page.Data will be Nil. 

But, if it is set, and it is different from the main data loop on the page, the complete page (and any loops on it) is rendered for each record in the page.data loop. 

If the main data loop equals the page data loop, the logic is as if the Page.Data is Nil (for backwards compatibility). 

For each band that has sub-bands, the engine will run a a similar loop over sub-bands. 

### User data loop

The user data loop is entirely event driven, and should be usable for any kind of data. The user is expected to implement several event handlers for the dataloop to implement its functionality. 

OnOpen
    Called when the data loop is 'opened'.
    This can be used to set up the actual data.
OnFirst
    Called when the reporting engine needs to position the data loop on the first record.
OnGetValue
    Called when the reporting engine needs to get a value of a field.
OnGetNames
    Called before opening the data loop, it is used to retrieve the list of fields in the data loop
OnNext
    Called to signal that the data loop should go to the next record
OnGetEOF
    Called to see if there are any more records in the data loop.
OnClose
    Called when the report is finished, and the data loop is no longer needed.

### DB Data loop

The dataset data loop (**TFPReportDatasetData** , unit _fpreportdb_) has some of the events of the user data loop, but in difference with the user data loop, the events are purely for the user to get feedback. The dataset data loop manages all data by itself. It will open the dataset or navigate through the dataset as the reporting engine calls the various methods. 

### Collection Data loop

Similar to the DB Data loop, the collection data loop (**TFPReportCollectionData** , unit _fpreportcontnr_) has some of the events of the user data loop, but in difference with the user data loop, the events are purely for the user to get feedback. The collection data loop loops over the items in a collection. It exposes the published properties of the collection items as fields. 

### ObjectList Data loop

Similar to the collection data loop, the objectlist data loop (**TFPReportObjectListData** , unit _fpreportcontnr_) has some of the events of the user data loop, but in difference with the user data loop, the events are purely for the user to get feedback. 

The object list data loop loops over the items in an object list. 

It exposes the published properties of the list items as fields. It assumes all the objects in the list are if the same class, and uses the first object in the list to determine the fields. 

### JSON Data loop

Similar to the collection data loop, the JSON data loop (**TFPReportJSONData** , unit _fpreportjson_) has some of the events of the user data loop, but in difference with the user data loop, the events are purely for the user to get feedback. 

The JSON data loop loops over the items in a JSON Array or object. 

It exposes the properties of the first item in the array (or object) as fields. If the first item in the array is again an array, then Column1 to ColumnN are used as field names 

It assumes all the objects/arrays in the list are identical in structure. 

The array can be nested inside a complex object: the Path property can be used to point the JSON Data loop to the correct location of the array (or object) 

## Loading and Saving to file

By itself, TFPReport has no concept of file formats. 

It only knows a streamer class. For this reason, it has no LoadFromFile/SaveToFile methods. The only currently supported streamer class is **TFPReportJSONStreamer** , which can be used to save and load a report design to/from file. 

The [FPReport_Usage](<FPReport_Usage.md> "FPReport Usage") page explains how to load a report from file. 

For the same reason, a TFPReport cannot be used to store information in a lazarus .lfm file. 

The **TFPJSONReport** class (unit **fpjsonreport**) descends from **TFPReport** , and combines it with the JSON streamer. If you want to be able to automatically load a design from file, or store it in the lazarus .dfm file, you are better off using this class. 

  


## Data management

By itself the TFPReport component does not have knowledge of datasets. It only knows an abstract data loop. 

If you design a report using the report designer, the stand-alone designer writes data definitions to the JSON it creates. However, the report itself ignores this information, and no datasets or other data loops will be created. 

In order to load and use the information about data in the report design, you can use a **TFPReportDataManager** component. 

Just point the **FPJSONReport** instance's _DataManager_ property to the instance of the **TFPReportDataManager** component, and it will instantiate the necessary datasets, and couple them to the report. 

The [FPReport_Usage](<FPReport_Usage.md> "FPReport Usage") page explains how to do this. 

## More Info

  * The [FPReport_Usage](<FPReport_Usage.md> "FPReport Usage") page explains how to create a basic report.
  * The [FPReport_Designer](<FPReport_Designer.md> "FPReport Designer") page explains how to use the stand-alone report designer.
  * The [FPReport_Custom_elements](<FPReport_Custom_elements.md> "FPReport Custom elements") page explains how to create custom elements for the reporting engine
  * The [FPReport_Lazarus_support](<FPReport_Lazarus_support.md> "FPReport Lazarus support") page gives an overview of how FPReport can be used in Lazarus.



### FAQ

See the FAQ -> [FPReport_FAQ](<FPReport_FAQ.md> "FPReport FAQ")

---

_Source: [https://wiki.freepascal.org/FPReport](https://web.archive.org/web/20250517161833/https://wiki.freepascal.org/FPReport)_
