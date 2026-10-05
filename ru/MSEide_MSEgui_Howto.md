# MSEide MSEgui Howto

│ **[English (en)](<../en/MSEide_MSEgui_Howto.md>)** │  **русский (ru)** │

## Значок и свойства приложения для Win32

1) Подготовьте исходный файл ресурсов в директории программы: 

File "version_data.rc": 
    
    
    1 VERSIONINFO
    FILEVERSION 4,0,3,17
    PRODUCTVERSION 3,0,0,0
    FILEFLAGSMASK 0
    FILEOS 0x40000
    FILETYPE 1
    {
     BLOCK "StringFileInfo"
     {
      BLOCK "040904E4"
      {
      VALUE "CompanyName","JSC TashkentP"
      VALUE "FileDescription","The main program file"
      VALUE "FileVersion","0.9a"
      VALUE "InternalName","ARM Podpiska"
      VALUE "LegalCopyright","JSC TashkentP’s property"
      VALUE "OriginalFilename","podpiska.pas"
      VALUE "ProductName","The program for ARM Podpiska"
      VALUE "ProductVersion","0.9a"
      }
     }
    }
    

Файл "icon_data.rc": 
    
    
     AppIcon ICON "app_icon.ico"
    

где "app_icon.ico" — это рисунок в формате ICO размерами 128x128, 24 бита. Можно использовать любой другой размер (это зависит от предпочитаемого разрешения экрана), как правило используются размеры больше 24x24 пикселей. 

Удобный редактор для подготовки таких (и любых других) изображений — "Embellish". 

2) Из командной строки Win32, скомпилируйтие подготовленный файл компилятором ресурсов, поставляемым с FreePascal'ем: 
    
    
     # windres -O res -i version_data.rc -o version_data.res
     # windres -O res -i icon_data.rc -o icon_data.res
    

3) Используйте скомпилированные ресурсы в своём приложении: 
    
    
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
    
    // импорт скомпилированных ресурсов
    
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
    

На этом всё. 

## Динамическое создание элементов управления

Чтобы динамически создать элемент управления (не через IDE), нужно выполнить следующие шаги: 

  * Создать экземпляр класса конструктором `Create` (передавая ему владельца как аргумент).
  * Создать рамку (`frame`) для компонента и настроить её свойства.
  * Если нужно, также создать фасад (`face`) для компонента и настроить его свойства.
  * Установить размер компонента в свойствах `bounds_cx` и `bounds_cy`.
  * Добавить компонент в окно или другой контейнер, вызвав метод контейнера `insertwidget`.
  * Показать компонент, вызвав его метод `show` или установив свойству `visible` значение `true`.



Вот пример динамического создания компонента `tedit` (чтобы этот код работать, нужно добавить объявление свойства `dynedit: tedit;`): 
    
    
    procedure tmainfo.mainformcreated(const sender: TObject);
    begin
      dynedit := tedit.Create(self);
      dynedit.bounds_cx := 100;
      dynedit.bounds_cy := 21;
      dynedit.createframe;
      insertwidget(dynedit, makepoint(300, 10));
      dynedit.show;
    end;
    

If you need custom value for the `face` property, you can set it up as follows: 
    
    
      dynedit.createface;
      with dynedit.face do begin
        fade_color.count := 2;
        fade_color[0] := cl_ltgreen;
        fade_color[1] := cl_dkgreen;
        fade_direction := gd_down;
      end;

---

_Source: [https://wiki.freepascal.org/MSEide_MSEgui_Howto/ru](https://web.archive.org/web/20221007004714/https://wiki.freepascal.org/MSEide_MSEgui_Howto/ru)_
