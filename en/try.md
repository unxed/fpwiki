# try

│ **English (en)** │

**try** is part of either a try..[finally](</index.php?title=finally&action=edit&redlink=1> "finally \(page does not exist\)") block or a try..[except](</index.php?title=except&action=edit&redlink=1> "except \(page does not exist\)") block. 

If an [exception](</index.php?title=exception&action=edit&redlink=1> "exception \(page does not exist\)") occurs while executing the code between try and finally, excution resumes at finally. If no exception occurs the code between finally and end will and end will be executed also. 
    
    
    try
      // code that might generate an exception
    finally 
      // will always be executed as last statements
    end;

Whenever an [exception](</index.php?title=exception&action=edit&redlink=1> "exception \(page does not exist\)") occurs, the code between except and end will be executed. 
    
    
    try
      // code that might generate an exception
    except
      // will only be executed in case of an exception
      on E: EDatabaseError do
        ShowMessage( 'Database error: '+ E.ClassName + #13#10 + E.Message );
      on E: Exception do
        ShowMessage( 'Error: '+ E.ClassName + #13#10 + E.Message );
    end;

## See also

  * [raise](</index.php?title=raise&action=edit&redlink=1> "raise \(page does not exist\)")
  * [Exception handling](</index.php?title=Exception_handling&action=edit&redlink=1> "Exception handling \(page does not exist\)")

---

_Source: [https://wiki.freepascal.org/try](https://web.archive.org/web/20170628050719/https://wiki.freepascal.org/try)_
