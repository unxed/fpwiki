# Remember form position and size

## Contents

  * 1 Using TForm.SessionProperties and TXMLPropStorage
  * 2 General method
    * 2.1 Simple example
    * 2.2 Support for more forms
    * 2.3 Minimized and Fullscreen states
    * 2.4 Ensure that restored form is visible
  * 3 See also



# Using TForm.SessionProperties and TXMLPropStorage

In some applications, it is nice if the user modified settings for the position and size of the form are saved at program end and reused the next time the program starts. Lazarus has a fairly easy way to do this using [TXMLPropStorage](<TXMLPropStorage.md> "TXMLPropStorage") or [TINIPropStorage](<TINIPropStorage.md> "TINIPropStorage") available. 

A simple example will illustrate these functions: 

  * create new application (main menu Project -> New Project -> Application)
  * pass an TXMLPropStorage component on the form (find it on the Misc tab of the component palette)



[![KomponentenpaletteTXMLPropStorage.png](https://wiki.freepascal.org/images/0/00/KomponentenpaletteTXMLPropStorage.png)](</File:KomponentenpaletteTXMLPropStorage.png>)

  * in the Object Inspector: XMLPropStorage1 -> FileName eg set "session.xml"
  * in the Object Inspector: Form1 -> Edit sessionProperties (the [...] button simply click)
  * now select each of the Form1 properties Left, Top, Width, Height and add it
  * confirm with OK and restart project
  * change and move the form size
  * end the program and restart it and it will return to the size and position it ended with last time.



Further information under [TXMLPropStorage](<TXMLPropStorage.md> "TXMLPropStorage")

# General method

## Simple example

A basic approach to keep form position and size between application restart is to store these state values to some persistent storage like XML file, INI file or Windows registry. You need at least store TForm properties Left, Top, Width and Height. But if form is maximized state you need also store values of TForm RestoredLeft, RestoredTop, RestoredWidth and RestoredHeight so form will be correctly sized if user click on maximize botton again to switch back to normal state. 

For simplification TXMLConfig component is used as persistent storage. Place this component on your form and set its filename property to Config.xml. 
    
    
    procedure TForm1.StoreFormState;
    begin
      with XMLConfig1 do begin
        SetValue('NormalLeft', Left);
        SetValue('NormalTop', Top);
        SetValue('NormalWidth', Width);
        SetValue('NormalHeight', Height);
    
        SetValue('RestoredLeft', RestoredLeft);
        SetValue('RestoredTop', RestoredTop);
        SetValue('RestoredWidth', RestoredWidth);
        SetValue('RestoredHeight', RestoredHeight);
    
        SetValue('WindowState', Integer(WindowState));
      end;
    end;
    

Restoration of stored form properties is a little bit complicated as properties have to be set in right order to prevent form resize animation or values to be overwritten by internal form mechanism. There is a rule that if normal and restored bounds are equal then form was in normal state. If normal and restored bounds are different then form is probably in maximized state and restored bounds are set to original position and size before maximization. As we have stored WindowState then we can use it to decide if form is in maximized state. To restore maximized state we have to first set form to normal state then set TForm.BoundsRect and after that set wsMaximized state. This is because Restored properties are read only and we can set them directly. 
    
    
    procedure TForm1.RestoreFormState;
    var
      LastWindowState: TWindowState;
    begin
      with XMLConfig1 do begin
        LastWindowState := TWindowState(GetValue('WindowState', Integer(WindowState)));
    
        if LastWindowState = wsMaximized then begin
          WindowState := wsNormal;
          BoundsRect := Bounds(
            GetValue('RestoredLeft', RestoredLeft),
            GetValue('RestoredTop', RestoredTop),
            GetValue('RestoredWidth', RestoredWidth),
            GetValue('RestoredHeight', RestoredHeight));
          WindowState := wsMaximized;
        end else begin
          WindowState := wsNormal;
          BoundsRect := Bounds(
            GetValue('NormalLeft', Left),
            GetValue('NormalTop', Top),
            GetValue('NormalWidth', Width),
            GetValue('NormalHeight', Height));
        end;
      end;
    end;
    

Functions should be executed from OnShow and OnClose handlers. 
    
    
    procedure TForm1.FormClose(Sender: TObject; var CloseAction: TCloseAction);
    begin
      StoreFormState;
    end;
    
    procedure TForm1.FormShow(Sender: TObject);
    begin
      RestoreFormState;
    end;
    

  


## Support for more forms

To support storing state for multiple forms we need to extend our functions to support Form parameter and allow store their values to different XMLConfig paths. 
    
    
    procedure TForm1.StoreFormState(Form: TForm);
    begin
      with XMLConfig1 do begin
        SetValue('Forms/' + Form.Name + '/NormalLeft', Form.Left);
        SetValue('Forms/' + Form.Name + '/NormalTop', Form.Top);
        SetValue('Forms/' + Form.Name + '/NormalWidth', Form.Width);
        SetValue('Forms/' + Form.Name + '/NormalHeight', Form.Height);
    
        SetValue('Forms/' + Form.Name + '/RestoredLeft', Form.RestoredLeft);
        SetValue('Forms/' + Form.Name + '/RestoredTop', Form.RestoredTop);
        SetValue('Forms/' + Form.Name + '/RestoredWidth', Form.RestoredWidth);
        SetValue('Forms/' + Form.Name + '/RestoredHeight', Form.RestoredHeight);
    
        SetValue('Forms/' + Form.Name + '/WindowState', Integer(Form.WindowState));
      end;
    end;
    
    procedure TForm1.RestoreFormState(Form: TForm);
    var
      LastWindowState: TWindowState;
    begin
      with XMLConfig1 do begin
        LastWindowState := TWindowState(GetValue('Forms/' + Form.Name + '/WindowState', Integer(Form.WindowState)));
    
        if LastWindowState = wsMaximized then begin
          Form.WindowState := wsNormal;
          Form.BoundsRect := Bounds(
            GetValue('Forms/' + Form.Name + '/RestoredLeft', Form.RestoredLeft),
            GetValue('Forms/' + Form.Name + '/RestoredTop', Form.RestoredTop),
            GetValue('Forms/' + Form.Name + '/RestoredWidth', Form.RestoredWidth),
            GetValue('Forms/' + Form.Name + '/RestoredHeight', Form.RestoredHeight));
          Form.WindowState := wsMaximized;
        end else begin
          Form.WindowState := wsNormal;
          Form.BoundsRect := Bounds(
            GetValue('Forms/' + Form.Name + '/NormalLeft', Form.Left),
            GetValue('Forms/' + Form.Name + '/NormalTop', Form.Top),
            GetValue('Forms/' + Form.Name + '/NormalWidth', Form.Width),
            GetValue('Forms/' + Form.Name + '/NormalHeight', Form.Height));
        end;
      end;
    end;
    
    procedure TForm1.FormClose(Sender: TObject; var CloseAction: TCloseAction);
    begin
      StoreFormState(Self);
    end;
    
    procedure TForm1.FormShow(Sender: TObject);
    begin
      RestoreFormState(Self);
    end;
    

## Minimized and Fullscreen states

TForm.WindowState have two additional states wsMinimized and wsFullscreen. These special states need special care if they should be supported. 

## Ensure that restored form is visible

As size of screen can change in time we need to check restored form position and size to ensure that form is visible to user and so your can manipulate with them. 
    
    
    function TForm1.CheckFormIsEntireVisible(Rect: TRect): TRect;
    var
      Width: Integer;
      Height: Integer;
    begin
      Result := Rect;
      Width := Rect.Right - Rect.Left;
      Height := Rect.Bottom - Rect.Top;
      if Result.Left < (Screen.DesktopLeft) then begin
        Result.Left := Screen.DesktopLeft;
        Result.Right := Screen.DesktopLeft + Width;
      end;
      if Result.Right > (Screen.DesktopLeft + Screen.DesktopWidth) then begin
        Result.Left := Screen.DesktopLeft + Screen.DesktopWidth - Width;
        Result.Right := Screen.DesktopLeft + Screen.DesktopWidth;
      end;
      if Result.Top < Screen.DesktopTop then begin
        Result.Top := Screen.DesktopTop;
        Result.Bottom := Screen.DesktopTop + Height;
      end;
      if Result.Bottom > (Screen.DesktopTop + Screen.DesktopHeight) then begin
        Result.Top := Screen.DesktopTop + Screen.DesktopHeight - Height;
        Result.Bottom := Screen.DesktopTop + Screen.DesktopHeight;
      end;
    end;
    

If requirement for entire form surface to be visible is too strong then we could check that at least part of form is visible. Here Part is size in pixels which should be at least visible for both horizontal and vertical position and size. 
    
    
    function TForm1.CheckFormIsPartialVisible(Rect: TRect; Part: Integer): TRect;
    var
      Width: Integer;
      Height: Integer;
    begin
      Result := Rect;
      Width := Rect.Right - Rect.Left;
      Height := Rect.Bottom - Rect.Top;
      if Result.Right < (Screen.DesktopLeft + Part) then begin
        Result.Left := Screen.DesktopLeft + Part - Width;
        Result.Right := Screen.DesktopLeft + Part;
      end;
      if Result.Left > (Screen.DesktopLeft + Screen.DesktopWidth - Part) then begin
        Result.Left := Screen.DesktopLeft + Screen.DesktopWidth - Part;
        Result.Right := Screen.DesktopLeft + Screen.DesktopWidth - Part + Width;
      end;
      if Result.Bottom < (Screen.DesktopTop + Part) then begin
        Result.Top := Screen.DesktopTop + Part - Height;
        Result.Bottom := Screen.DesktopTop + Part;
      end;
      if Result.Top > (Screen.DesktopTop + Screen.DesktopHeight - Part) then begin
        Result.Top := Screen.DesktopTop + Screen.DesktopHeight - Part;
        Result.Bottom := Screen.DesktopTop + Screen.DesktopHeight - Part + Height;
      end;
    end;
    

# See also

  * [Form Tutorial](<Form_Tutorial.md> "Form Tutorial")

---

_Source: [https://wiki.freepascal.org/Remember_form_position_and_size](https://web.archive.org/web/20250120182427/https://wiki.freepascal.org/Remember_form_position_and_size)_
