# macOS Retrieve MAC address

[![macOSlogo.png](https://wiki.freepascal.org/images/1/15/macOSlogo.png)](</File:macOSlogo.png>)

This article applies to [macOS](</Category:macOS> "Category:macOS") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

Simple Free Pascal command line program to retrieve the MAC address for the specified network interface or, if no network interface is specified, retrieve the MAC addresses for all the network interfaces. 
    
    
    Program mac;
    
    {$mode objfpc}{$H+}
    
    Uses 
      SysUtils,
      StrUtils,
      Process,
      Classes;
    
    function GetmacOSMacAddress : string;
    var
       theProcess		 : TProcess;
       theOutput		 : TStringList;
       i, j, theLine, thePos : integer;
    
    begin
       theProcess := TProcess.Create(nil);
       theProcess.Executable := 'ifconfig';
    
       // If interface specified, then retrieve its MAC address
       // Otherwise, retrieve MAC addresses for all interfaces
       if(ParamStr(1) <> '') then
          theProcess.Parameters.Add(ParamStr(1));
    
       theProcess.Options := theProcess.Options + [poWaitOnExit, poUsePipes];
       theProcess.Execute;
       theOutput := TStringList.Create;
       theOutput.LoadFromStream(theProcess.Output);
    
       theLine := -1;
       for i := 0 to theOutput.Count - 1 do
         begin
           j := pos('ether', theOutput.Strings[i]);
           if (j > 0) then
             begin
               theLine := i;
               thePos := j + length('ether') + 1;
    
               if(theLine > -1) then
                  begin
                    Result := UpperCase(ExtractSubStr(theOutput.Strings[theLine], thePos, [' ']));
                    WriteLn(Result);
                  end;
             end;
         end;
    
       // Always free the memory used
       theOutput.Free;
       theProcess.Free;
    end;
    
    begin
       GetmacOSMacAddress;
    end.

---

_Source: [https://wiki.freepascal.org/macOS_Retrieve_MAC_address](https://web.archive.org/web/20210117131828/https://wiki.freepascal.org/macOS_Retrieve_MAC_address)_
