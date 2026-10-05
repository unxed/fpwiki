# ServiceManager

[![Windows logo - 2012.svg](https://upload.wikimedia.org/wikipedia/commons/thumb/5/5f/Windows_logo_-_2012.svg/50px-Windows_logo_-_2012.svg.png)](</File:Windows_logo_-_2012.svg>)

This article applies to [Windows](</Category:Windows> "Category:Windows") only.

See also: [Multiplatform Programming Guide](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

│ **English (en)** │

## Contents

  * 1 ServiceManager
    * 1.1 Service Name
    * 1.2 Service management using console
  * 2 Code example
  * 3 See also



## ServiceManager

For Windows, the fcl-extra package contains a unit called ServiceManager. 

The example below checks whether the SamSs (Security Accounts Manager; should always be running) service is running. (Command line equivalent `sc query samss`). 

### Service Name

Please note that if you develop your own service using the Lazdaemon package, the service name you must use is important. While the commands NET START and NET STOP use the service display name, the command SC and the code below use the service internal name. The display name is the one given to the property "DisplayName" of the item created in the DaemonDefs items. The service name is the property "Name" of the same item. The display name is the one currently displayed in the **Name** column by the MS services manager applet (services.msc) Let's use an example to clarify this. Assume you have written a service application called **myService.exe** in which there is one item in the DaemonDefs. The item has a name of "**MyServ** " and the associated "DisplayName" property has the value of "**Internal build service** ". Using the command line as administrator you can: 

### Service management using console

You will need: Microsoft .NET Framework 4.5.2 (Web Installer) for Windows Vista SP2, Windows 7 SP1, Windows 8, Windows 8.1, Windows Server 2008 SP2 Windows Server 2008 R2 SP1, Windows Server 2012 and Windows Server 2012 R2 

You have to get the support of .NET SDK Command prompt for install the service into your local machine. 1\. Call to SDK Command prompt, follow Start>All Programs>Microsoft .NET Framework SDK v2.0>SDK Command prompt 

Install the service: 
    
    
    \your\path\to\application\myService.exe --install 
    rem .... or /install, -install works too
    

Activate the service: In these commands, do not forget **double quotes** when multiple words are needed! 
    
    
    net start "Internal build service"
    rem .... or alternatively:
    sc start "MyServ"
    

To stop the service: 
    
    
    net stop '''"Internal build service"'''
    rem ... or alternatively 
    sc stop "'''MyServ'''"
    

## Code example

Given the above, the code snippet to call the function below would be in our case: 
    
    
    if IsServiceRunning('MyServ') then
    

  
Example program: 
    
    
    program ServiceTest;
    // Check if a certain process is running.
    {$mode objfpc}{$H+}
    uses
      Classes,
      SysUtils,
      ServiceManager,
      JwaWinSvc {for services declarations};
    
    function IsServiceRunning(ServiceName: string): boolean;
      {description Checks if a Windows service is running}
    var
      Services: TServiceManager;
      ServiceStatus: TServiceStatus;
    begin
      //Check for existing services
      //equivalent to sc query <servicename>
      Services := TServiceManager.Create(nil);
      try
        try
          Services.Acces := SC_MANAGER_CONNECT; //Note typo in property.
          //We don't need more access permissions than this; by default
          //the servicemanager is trying to get all access
          Services.Connect; //Now connect with requested access level
          Services.GetServiceStatus(ServiceName, ServiceStatus);
          Result := (ServiceStatus.dwCurrentState = SERVICE_RUNNING);
          Services.Disconnect;
        except
          on E: EServiceManager do
          begin
            // A missing service might throw a missing handle exception? No?
            {LogOutput('Error getting service information for ' + ServiceName +
                       '. Technical details: ' + E.ClassName + '/' + E.Message); }
            Result := False;
            raise; //rethrow original exception
          end;
          on E: Exception do
          begin
           {LogOutput('Error getting service information for ' + ServiceName +
                      '. Technical details: ' + E.ClassName + '/' + E.Message); }
            Result := False;
            raise; //rethrow original exception
          end;
        end;
      finally
        Services.Free;
      end;
    end;
    
    const
      ServiceToTest = 'SamSs';
    
    //Security Accounts Manager, should be running, at least on Vista
    begin
      WriteLn('Starting test for ' + ServiceToTest + ' service.');
      if IsServiceRunning(ServiceToTest) then
        WriteLn('The ' + ServiceToTest + ' service is running')
      else
        WriteLn('The ' + ServiceToTest + ' service is not running');
    end.
    

## See also

  * [Daemons and Services](<Daemons_and_Services.md> "Daemons and Services")
  * [TServiceManager](<TServiceManager.md> "TServiceManager")

---

_Source: [https://wiki.freepascal.org/ServiceManager](https://web.archive.org/web/20250417151201/https://wiki.freepascal.org/ServiceManager)_
