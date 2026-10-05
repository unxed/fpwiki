# Mac Buttons

[![macOSlogo.png](https://wiki.freepascal.org/images/1/15/macOSlogo.png)](</File:macOSlogo.png>)

This article applies to [macOS](</Category:macOS> "Category:macOS") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

│ **English (en)** │  **[русский (ru)](<../ru/Mac_Buttons.md>)** │

By default, the button placed onto a form by Lazarus is 75 width by 25 height, which is not the standard for Mac Applications. 

In order for buttons to appear oval, make the button height 22 maximum. 

This can be done by setting the button height directly or through code. 

  


## CODE FOR A SINGLE BUTTON:
    
    
    procedure TForm1.FormCreate(Sender: TObject);
    begin
      Button1.Height := 22;
    end;
    

## CODE FOR ALL BUTTONS:
    
    
    procedure TForm1.FormCreate(Sender: TObject);
    var
      I: Integer;
    begin
      for I := 0 to Form1.ControlCount - 1 do begin
        if (Form1.Controls[I].ClassType = TButton) then Form1.Controls[I].Height := 22;
      end;
    end;
    

## See also

  * [Introduction to platform-sensitive development](<Introduction_to_platform-sensitive_development.md> "Introduction to platform-sensitive development")
  * [Mac Show Application Title, Version, and Company](<Mac_Show_Application_Title,_Version,_and_Company.md> "Mac Show Application Title, Version, and Company")
  * [Add an Apple Help Book to your macOS app](<Add_an_Apple_Help_Book_to_your_macOS_app.md> "Add an Apple Help Book to your macOS app")
  * [macOS Programming Tips](<macOS_Programming_Tips.md> "macOS Programming Tips")

---

_Source: [https://wiki.freepascal.org/Mac_Buttons](https://web.archive.org/web/20240907044910/https://wiki.freepascal.org/Mac_Buttons)_
