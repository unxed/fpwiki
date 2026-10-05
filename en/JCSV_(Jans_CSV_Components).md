# JCSV (Jans CSV Components)

## Contents

  * 1 About
  * 2 Author
  * 3 License
  * 4 Download
  * 5 Example Program
  * 6 Change Log
  * 7 Dependencies / System Requirements
  * 8 Installation
  * 9 See Also



### About

_JCSV (Jans Freeware CSV Database Components_ is a set of components for using a CSV Database 

The download contains the component, an installation package. 

### Author

Jan Verhoeven Email jan1.verhoeven at wxs.nl Modified by David Stewart Email davesimplewear at yahoo dot com 

### License

[modified](<http://svn.freepascal.org/svn/lazarus/trunk/COPYING.modifiedLGPL>) [LGPL](<http://svn.freepascal.org/svn/lazarus/trunk/COPYING.LGPL>) (same as the FPC RTL and the Lazarus LCL). You can contact the author if the modified LGPL doesn't work with your project licensing. 

### Download

The latest stable release can be found on [the lazarus-ccr sf download location](<http://sourceforge.net/projects/lazarus-ccr/files/JCSV/>). Also from [David's Freeware](<http://www.users.on.net/~dave.stewart/index.html>). 

### Example Program

To create the example first create your csv database, using your text editor type the field names you wish to use, as in the image below. 
    
    
    [![Example.png](https://wiki.freepascal.org/images/7/70/Example.png)](</File:Example.png>)
    

You can search on any field by using the search button in the navigator. as shown in the images below. 

[![Csv.png](https://wiki.freepascal.org/images/f/f3/Csv.png)](</File:Csv.png>)

[![Csv1.png](https://wiki.freepascal.org/images/6/67/Csv1.png)](</File:Csv1.png>)

[![Csv2.png](https://wiki.freepascal.org/images/0/07/Csv2.png)](</File:Csv2.png>)

The example code is listed below also. 
    
    
    uses
      Classes, SysUtils, FileUtil, LResources, Forms, Controls, Graphics, Dialogs,
      StdCtrls, jvCSVBase;
     
    type
     
      { TForm1 }
     
      TForm1 = class(TForm)
        Button1: TButton;
        CSVBase: TjvCSVBase;
        jvCSVCheckBox1: TjvCSVCheckBox;
        jvCSVComboBox1: TjvCSVComboBox;
        jvCSVEdit1: TjvCSVEdit;
        jvCSVEdit2: TjvCSVEdit;
        jvCSVEdit3: TjvCSVEdit;
        jvCSVEdit4: TjvCSVEdit;
        jvCSVEdit5: TjvCSVEdit;
        jvCSVEdit6: TjvCSVEdit;
        jvCSVEdit7: TjvCSVEdit;
        jvCSVEdit8: TjvCSVEdit;
        jvCSVEdit9: TjvCSVEdit;
        jvCSVLabel1: TjvCSVLabel;
        jvCSVNavigator1: TjvCSVNavigator;
        Label1: TLabel;
        Label2: TLabel;
        Label3: TLabel;
        Label4: TLabel;
        procedure Button1Click(Sender: TObject);
        procedure FormShow(Sender: TObject);
      private
        { private declarations }
      public
        { public declarations }
      end; 
     
    var
      Form1: TForm1; 
     
    implementation
     
    { TForm1 }
     
    procedure TForm1.FormShow(Sender: TObject);
    begin
      CSVbase.DataBaseOpen(ExtractFilePath(ParamStr(0))+'example.csv');
    end;
     
    procedure TForm1.Button1Click(Sender: TObject);
    begin
      jvCSVEdit8.Caption:= FormatDateTime('dd/mm/yy',now);
    end;
     
    initialization
      {$I Unit1.lrs}
     
    end.

### Change Log

  * Version 1.0 _date_



### Dependencies / System Requirements

  * None 



Status: _Stable / Alpha_

Issues: 

  


### Installation

  * Use *.lpk file to install 



### See Also

  * [CsvDocument](<CsvDocument.md> "CsvDocument")
  * [CSV](<CSV.md> "CSV")
  * [ZMSQL](<ZMSQL.md> "ZMSQL")

---

_Source: [https://wiki.freepascal.org/JCSV_(Jans_CSV_Components)](https://web.archive.org/web/20160605203719/https://wiki.freepascal.org/JCSV_(Jans_CSV_Components))_
