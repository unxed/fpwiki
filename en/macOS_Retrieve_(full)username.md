# macOS Retrieve (full)username

## Retrieve username

The code snippet below will retrieve the current user's username (eg joe). In the example code below, the function to retrieve the username is called from a menu item. 
    
    
    ...
    
    Uses
      CocoaAll,
    ...
    
    function NSUserName: CFStringRef
       external name '_NSUserName';
    ...
    
    procedure TForm1.MenuItem21Click(Sender: TObject);
    var
      usernameStr: ShortString;
      status: Boolean = false;
    begin
      status := CFStringGetPascalString(CFStringRef(NSusername),@usernameStr,255,CFStringGetSystemEncoding);
      if(status = true) then
         ShowMessage(usernameStr)
      else
         ShowMessage('Error retrieving username');
    end;
    

## Retrieve full username

The code snippet below will retrieve the current user's full username (eg Joe Bloggs). In the example code below, the function to retrieve the full username is called from a menu item. 
    
    
    ...
    
    Uses
      CocoaAll,
    ...
    
    function NSFullUserName: CFStringRef
       external name '_NSFullUserName';
    ...
    
    procedure TForm1.MenuItem22Click(Sender: TObject);
    var
      fullUsernameStr: ShortString;
      status: Boolean = false;
    begin
      status := CFStringGetPascalString(CFStringRef(NSFullUserName),@fullUsernameStr,255,CFStringGetSystemEncoding);
      if(status = true) then
         ShowMessage(fullUsernameStr)
      else
         ShowMessage('Error retrieving full username');
    end;
    

## See also

  * [NSProcessInfo](<Accessing_macOS_System_Information.md> "Accessing macOS System Information")

---

_Source: [https://wiki.freepascal.org/macOS_Retrieve_(full)username](https://web.archive.org/web/20240913182828/https://wiki.freepascal.org/macOS_Retrieve_(full)username)_
