# MouseAndKeyInput

│ **English (en)** │

## Contents

  * 1 About
  * 2 Change Log
  * 3 Restrictions
    * 3.1 Carbon
    * 3.2 Gtk1/2
  * 4 Usage



## About

MouseAndKeyInput package is a tool for cross-platform manipulation with mouse and key input. You can move mouse cursor to specified location, send clicks and do key presses. It is suitable for GUI testing or program control demonstration. 

Package is preinstalled in Lazarus in folder "components/mouseandkeyinput". 

Author: [Tom Gregorovic](</User:Tombo> "User:Tombo")

License: GPL 

## Change Log

  * Version 0.1



## Restrictions

  * It is not recommended to call mouse and key input directly from events like OnClick, use [Application.QueueAsyncCall](<Asynchronous_Calls.md> "Asynchronous Calls") instead.
  * Do not forget to reset the mouse button and key state after Down method with Up method.



#### Carbon

  * Pressing alpha chars is not supported.



#### Gtk1/2

  * Needs Xtst library.
  * ALT key pressing is not supported.



Under Debian and Ubuntu the library libxtst-dev contains Xtst: sudo apt install libxtst-dev 

## Usage

After opening your project: 

  * Go to the Lazarus install directory -> components -> mouseandkeyinput.
  * There you will find: lazmouseandkeyinput.lpk.
  * Open and compile the .lpk.



In your unit.pas add in the Uses clause: 
    
    
    Uses
      ..., MouseAndKeyInput, LCLType;
    

To simulate press of F1 from a button: 
    
    
    procedure TForm1.HelpButtonClick(Sender: TObject);
    begin
      KeyInput.Apply([ssCtrl]);
      KeyInput.Press(VK_F1);                // This will simulate press of F1 function key.
      KeyInput.Unapply([ssCtrl]); 
    end;
    

VK- definitions are found here: <http://lazarus-ccr.sourceforge.net/docs/lcl/lcltype/index-2.html>

Mouse control: 
    
    
      MouseInput.Click(mbLeft,[],300,300);   // Left click on X:=300 , Y:=300
      MouseInput.Click(mbRight,[],1365,2);   // Right click on X:=1365 , Y:=2

---

_Source: [https://wiki.freepascal.org/MouseAndKeyInput](https://web.archive.org/web/20240308084926/https://wiki.freepascal.org/MouseAndKeyInput)_
