# FPReport Lazarus support

│ **English (en)** │

## Contents

  * 1 Package
  * 2 TFPReport and TFPJSONReport component editor
  * 3 TFPReportDesigner component editor
  * 4 Export components editors
  * 5 Basic usage
  * 6 TFPReport vs. TFPJSONReport
  * 7 Navigation to other pages



## Package

The lazidefpreport package contains the support for fpreport. It can be installed in the IDE, and then the following components appear on the component palette: 

  


TFPReport
    The basic report. Only use this component if you intend to do everything (creating report, adding data) manually.
    It does not write any properties to the .lfm file. You can use the designer to design a report, but you must save the design to file and load it manually at runtime, as desrcibed in [FPReport_Usage](<FPReport_Usage.md> "FPReport Usage").
TFPJSONReport
    This is the basic report which you should normally be using. The report design is saved in the .lfm form file. You can of course use the LoadFromFile/SaveToFile commands to load from file.
TFPReportDesigner
    Drop this on a form if you want to add designing capabilities to your executable. This allows the end-user to manipulate the design of the report. The possibilities offered to the end-user can be specified in the options.
TFPReportUserData
    This is a simple, event-driven, report loop. You can set the field definitions, and use the events to provide the data to the report. Note that you cannot preview the report in the designer, since the event handlers cannot be executed in the designer.
TFPReportDatasetData
    This class offers data to the report from a dataset. The dataset will be opened to get the list of field names.
TFPReportJSONData
    This class offers data from a JSON structure to the dataset.
TFPReportCollectionData,
    This class offers data from a collection to the dataset. The collection must be specified at runtime. When using this component, you cannot preview the report in the designer, since the data is not available.
TFPReportObjectListData,
    This class offers data from a list to the dataset. The collection must be specified at runtime. When using this component, you cannot preview the report in the designer, since the data is not available.
TFPReportExportHTML,
    Drop this component on a form to export the data from the report to HTML files.
TFPReportExportfpImage,
    Drop this component on a form to export the data from the report to image files.
TFPReportExportPDF,
    Drop this component on a form to export the data from the report to PDF files.
TFPreportPreviewExport,
    Drop this component on a form to export the data from the report to screen, i.e. preview the report.
TFPreportPrinterExport
    Drop this component on a form to export the data from the report to printer.

## TFPReport and TFPJSONReport component editor

The component editor for the TFPReport and TFPJSONReport components has the following entries: 

Design Report
    This will show the report designer. From within the designer, the report data cannot be managed.
    Note that if you design a TFPReport instance, you must save the design to file.
    Only the TFPJSONReport component will save the definition to the .lfm file.
Assign local data
    This will check the form for TFPReportData instances, and allows you to select the instances that should be added to the report data sources.
    You can also do this manually, by editing the ReportData collection.
Preview Report
    This will preview the report, preparing it if necessary.
    Note that this will only work with dataset based data, since none of the event handlers for user/collection/list data can be executed in the IDE.
Run Report
    This will prepare the report.

## TFPReportDesigner component editor

The TFPReportDesigner has a component menu editor which allows you to show the editor. 

## Export components editors

The various export components share a component menu editor which allows you to configure the export, and to actually execute the export. For this you must have prepared the report. 

## Basic usage

To create a report for which the design is stored in the .lfm file, the following steps are necessary: 

  1. Drop a TFPJSONReport instance on a form or data module
  2. Drop one or more TFPReportData descendents on the form or data module and configure them correctly.
  3. Connect the data to the report (you can use the 'Assign local data' component editor menu entry for this)
  4. Design the report. Make sure you save the design.
  5. Drop an export component and/or a preview component on the form/datamodule.



When this is done, the report is ready for use 

  1. At runtime, run the report, and export it using one of the export components.



## TFPReport vs. TFPJSONReport

In general, you should use TFPJSONReport: it has the ability to store the design in the form file, or can easily load/save report designs from file. Unless you want to do everything in code, the use of TFPReport is not really recommended. 

## Navigation to other pages

  1. [FPReport](<FPReport.md> "FPReport")
  2. [FPReport_Usage](<FPReport_Usage.md> "FPReport Usage")
  3. [FPReport_Designer](<FPReport_Designer.md> "FPReport Designer")
  4. [FPReport_Custom_elements](<FPReport_Custom_elements.md> "FPReport Custom elements")

---

_Source: [https://wiki.freepascal.org/FPReport_Lazarus_support](https://web.archive.org/web/20230610224127/https://wiki.freepascal.org/FPReport_Lazarus_support)_
