# MSEide MSEgui Howto

│ **English (en)** │  **[русский (ru)](<../ru/MSEide_MSEgui_Howto.md>)** │

## Win32 Application Icon and Properties

1) Prepare the resource source files in the directory of your program: 

File "version_data.rc": 
    
    
    1 VERSIONINFO
    FILEVERSION 4,0,3,17
    PRODUCTVERSION 3,0,0,0
    FILEFLAGSMASK 0
    FILEOS 0x40000
    FILETYPE 1
    {
     BLOCK ”StringFileInfo”
     {
      BLOCK ”040904E4”
      {
      VALUE ”CompanyName”,”JSC TashkentP”
      VALUE ”FileDescription”,”The main program file”
      VALUE ”FileVersion”,”0.9a”
      VALUE ”InternalName”,”ARM Podpiska”
      VALUE ”LegalCopyright”,”JSC TashkentP’s property”
      VALUE ”OriginalFilename”,”podpiska.pas”
      VALUE ”ProductName”,”The program for ARM Podpiska”
      VALUE ”ProductVersion”,”0.9a”
      }
     }
    }
    

File ”icon_data.rc”: 
    
    
     AppIcon ICON ”app_icon.ico”
    

where ”app_icon.ico” is 128x128 24bit ICO image. You may use any size at your favour ( usually depends on preferrable destop resilution/size ) commonly greater or equal to 24x24 pixels. 

My favorite editor to prepare such ( and any kind of image ) files is ”Embellish”. 

2) From within Win-32 command shell, complile the prepared files with the supplied FPC resource compiler: 
    
    
     # windres -O res -i version_data.rc -o version_data.res
     # windres -O res -i icon_data.rc -o icon_data.res
    

3) use the compiled resources in your application : 
    
    
    program super_puper;
    
    {$ifdef FPC}{$mode objfpc}{$h+}{$INTERFACES CORBA}{$endif}
    {$ifdef FPC}
     {$ifdef mswindows}
      {$apptype console}
     {$endif}
    {$endif}
    uses
     {$ifdef FPC}{$ifdef linux}cthreads,{$endif}{$endif}msegui,mseforms,
     main,dmmain,dmprint, dmacnt1, dmf18,dmrefs,
     connsetupform,mseconsts,mseconsts_ru,mseconsts_uzcyr;
    
    // importing the compiled resources
    
    {$ifdef mswindows}
     {$R version_data.res}
     {$R icon_data.res}
    {$endif}
    
    begin
     setlangconsts('ru');
     application.createdatamodule(tdmmainmo,dmmainmo);
     application.createdatamodule(tdmprintmo, dmprintmo);
     application.createdatamodule(tdmacnt1mo, dmacnt1mo);
     application.createdatamodule(tdmf18mo, dmf18mo);
     application.createdatamodule(tdmrefsmo, dmrefsmo);
     application.createform(tmainfo,mainfo);
     application.run;
    end.
    

That's all at the moment.

---

_Source: [https://wiki.freepascal.org/MSEide_MSEgui_Howto](https://web.archive.org/web/20200919114645/https://wiki.freepascal.org/MSEide_MSEgui_Howto)_
