# Finally

│ **English (en)** │

  
Back to [Reserved words](<Reserved_words.md> "Reserved words"). 

  
The reserved word `finally` identifies a [block](<Block.md> "Block") of code that should always be processed, regardless of whether the preceding `try` block was fully executed or has been exited early (in case of an [Exception](</index.php?title=Exception&action=edit&redlink=1> "Exception \(page does not exist\)"), [Exit](<Exit.md> "Exit"), [Break](<Break.md> "Break") or [Continue](<Continue.md> "Continue")). 

## Contents

  * 1 Examples
  * 2 Non-Exception Code
  * 3 Caveats
  * 4 See also



## Examples

Simple example: 
    
    
    begin
      ...
      try
        ...    // code to check
      finally
        ...    // code which should always be executed even if try exits early
      end;
      ...
    end;
    

Example with error handling: 
    
    
    begin
      ...
      try
        try
          ...  // code to check
        except 
          ...  // error handling
        end;
      finally  
        ...    // code which should always be executed even if try exits early
      end;
      ...
    end;
    

## Non-Exception Code

Despite the name, Try-Finally is not strictly tied to the usage of exceptions, and also provides benefits in code where no exceptions can be raised. The reason for this is that the Finally block will always be executed, even if the try block exits early. 

This can be very useful e.g. for the [Break Fast Break Even](</index.php?title=Break_Fast_Break_Even&action=edit&redlink=1> "Break Fast Break Even \(page does not exist\)") paradigm, where [Exit](<Exit.md> "Exit") or other jumps (such as [Break](<Break.md> "Break") or [Continue](<Continue.md> "Continue")) are used: 
    
    
    function CheckFormat(const CSVString: String): Boolean;
    var
      sl: TStringList;
    begin
      Result := False;
      sl := TStringList.Create;
      try
        sl.Delimiter := ';';
        sl.StrictDelimiter := True;
        sl.DelimitedText := CSVString;
    
        if sl.Count <> RowCount then
          Exit; // Will automatically execute the finally block before exiting
        // Check field data, e.g.
        if not isNumber(sl[1]) then
          Exit; // Will automatically execute the finally block before exiting
        // ...
      finally
        sl.Free;
      end;
      // All checks passed
      Result := True;
    end;
    

Because of the try-finally, the code can simply call exit, and the finally block ensures that sl will be freed afterwards. Without the try-finally, each of these blocks would require a seperate sl.Free call: 
    
    
    function CheckFormat(const CSVString: String): Boolean;
    var
      sl: TStringList;
    begin
      Result := False;
      sl := TStringList.Create;
      sl.Delimiter := ';';
      sl.StrictDelimiter := True;
      sl.DelimitedText := CSVString;
    
      if sl.Count <> RowCount then
      begin
        sl.Free;
        Exit;
      end;
    
      if not isNumber(sl[1]) then
      begin
        sl.Free;
        Exit;
      end;
    
      // ...
    
      sl.Free;
    end;
    

Therefore try-finally can even in code without exceptions, help to ensure that the cleanup is performed, without having to consider each exit point individually 

## Caveats

The `finally` part is _not_ executed if a [`halt`](<https://www.freepascal.org/docs-html/rtl/system/halt.html>) occurred. 

## See also

  * [Try](<Try.md> "Try")
  * [Except](<Except.md> "Except")
  * [Avoiding implicit try finally section](<Avoiding_implicit_try_finally_section.md> "Avoiding implicit try finally section")

---

_Source: [https://wiki.freepascal.org/Finally](https://web.archive.org/web/20250321111734/https://wiki.freepascal.org/Finally)_
