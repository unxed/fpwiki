# ATFileNotif

## Contents

  * 1 About
  * 2 Example
  * 3 Download
  * 4 License



# About

ATFileNotif is simple component which detects file change. It uses timer, timer checks are file props (file exists; file size; file age) changed or not. It calls OnChanged. 

[![atfilenotif.png](https://wiki.freepascal.org/images/f/f0/atfilenotif.png)](</File:atfilenotif.png>)

# Example

Example of function which starts file watch: 
    
    
    procedure TfmMain.NotifyFile;
    begin
      with ATFileNotif1 do
      begin
        Timer.Enabled:= False;
        Timer.Interval:= 1000;
        FileName:= edFileName.Text;
        Timer.Enabled:= True;
      end;
    end;
    

# Download

Github: <https://github.com/Alexey-T/ATFileNotif-Lazarus>

# License

MPL 2.0 or LGPL.

---

_Source: [https://wiki.freepascal.org/ATFileNotif](https://web.archive.org/web/20250418094419/https://wiki.freepascal.org/ATFileNotif)_
