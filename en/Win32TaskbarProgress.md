# Win32TaskbarProgress

[![Windows logo - 2012.svg](https://upload.wikimedia.org/wikipedia/commons/thumb/5/5f/Windows_logo_-_2012.svg/50px-Windows_logo_-_2012.svg.png)](</File:Windows_logo_-_2012.svg>)

This article applies to [Windows](</Category:Windows> "Category:Windows") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

## Contents

  * 1 About
  * 2 Author
  * 3 License
  * 4 Usage
  * 5 Download



## About

This is unit which contains the class to control the progressbar over the Windows 7+ taskbar button. The demo looks like this: 

[![Win32TaskbarProgressDemo.png](https://wiki.freepascal.org/images/0/00/Win32TaskbarProgressDemo.png)](</File:Win32TaskbarProgressDemo.png>)

Taskbar button progress can have several styles: 

  * none (inactive)
  * green progress
  * yellow progress (looks like paused state)
  * red progress (looks like error state)
  * marquee floating animation (progress value is ignored, it's constantly changing animation from min to max)



  


## Author

Alexey Torgashin 

## License

MIT 

## Usage

In the form's OnShow (or maybe OnCreate) create the object like this: 
    
    
    uses
      win32taskbarprogress;
    
    procedure TForm1.FormShow(Sender: TObject);
    begin
      GlobalTaskbarProgress:= TWin7TaskProgressBar.Create;
    end;
    

And then call properties of this object like this: 
    
    
      //to change state: none, green, yellow, red, floating
      GlobalTaskbarProgress.Style:= TTaskBarProgressStyle(ComboBoxStyle.ItemIndex);
     
      //to change progress value 0 to 100
      GlobalTaskbarProgress.Progress:= Edit1.Value;
    

Author tried to initialise the object in the "initialization" part of win32taskbarprogress, but failed, maybe because the Application.Handle is not initialised so early. 

## Download

Unit file and demo project: <https://github.com/Alexey-T/Win32TaskbarProgress>

---

_Source: [https://wiki.freepascal.org/Win32TaskbarProgress](https://web.archive.org/web/20250219043748/https://wiki.freepascal.org/Win32TaskbarProgress)_
