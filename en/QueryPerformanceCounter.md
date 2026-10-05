# QueryPerformanceCounter

[![Windows logo - 2012.svg](https://upload.wikimedia.org/wikipedia/commons/thumb/5/5f/Windows_logo_-_2012.svg/50px-Windows_logo_-_2012.svg.png)](</File:Windows_logo_-_2012.svg>)

This article applies to [Windows](</Category:Windows> "Category:Windows") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

│ **English (en)** │

Maybe the one microsecond timestamp is too long for your program. You can access the Windows API Performance Counter using code such as shown below. Note that this API may not be as reliable as you might hope. For more information see <http://www.virtualdub.org/blog/pivot/entry.php?id=106>
    
    
    unit Unit1; 
    {$mode objfpc}{$H+}
    interface
    
    uses
      Classes, SysUtils, FileUtil, LResources, Forms, Controls, Graphics, Dialogs,
      StdCtrls, Windows;
    
    type
      { TForm1 }
      TForm1 = class(TForm)
        procedure FormClick(Sender: TObject);
      private
        { private declarations }
      public
        { public declarations }
      end; 
     var
      Form1: TForm1; 
    implementation
    
    { TForm1 }
    
    procedure TForm1.FormClick(Sender: TObject);
    var
      PerformanceCounter: int64;
      PerformanceFrequency: int64;
    begin
      if QueryPerformanceFrequency(PerformanceFrequency)
        and QueryPerformanceCounter(PerformanceCounter)
        then ShowMessage('Freq:' + IntToStr(PerformanceFrequency)
           + ', Counter:' + IntToStr(PerformanceCounter))
        else ShowMessage('Sorry, performance counters not supported.');
    end;
    
    initialization
    
    {$I unit1.lrs}
    
    end.

---

_Source: [https://wiki.freepascal.org/QueryPerformanceCounter](https://web.archive.org/web/20240909161514/https://wiki.freepascal.org/QueryPerformanceCounter)_
