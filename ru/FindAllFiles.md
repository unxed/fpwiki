# FindAllFiles

│ **[English (en)](<../en/FindAllFiles.md>)** │  **русский (ru)** │

[Unit](<../en/Unit.md> "Unit"): Lazarus [fileutil](<../en/fileutil.md> "fileutil")

Чтобы подключить FileUtil в вашем проекте, добавьте LazUtils в необходимые пакеты. Проделайте следующее: 

  * Перейдите к _Lazarus IDE Menu_ > _Project_(Проект) > _Project Inspector_(Инспектор проекта)
  * В диалоговом окне _Project Inspector_(Инспектор проекта) нажмите _Add_(Добавить) > _New Requirement_(Новая зависимость)
  * В диалоговом окне _New Requirement_(Новая зависимость) найдите пакет _LazUtils_ и нажмите OK.



* * *

[Прим. перев.](</User:Zoltanleo> "User:Zoltanleo"): можно поступить по старинке, добавив данный модуль в секцию uses. 

* * *

См.также: 

  * <https://lazarus-ccr.sourceforge.io/docs/lazutils/fileutil/findallfiles.html>
  * <https://lazarus-ccr.sourceforge.io/docs/lazutils/fileutil/searchfileinpath.html>
  * <https://lazarus-ccr.sourceforge.io/docs/lazutils/fileutil/searchallfilesinpath.html>



  

    
    
    procedure FindAllFiles(AList: TStrings; const SearchPath: String;
      SearchMask: String = ''; SearchSubDirs: Boolean = True; DirAttr: Word = faDirectory); 
    
    function FindAllFiles(const SearchPath: String; SearchMask: String = '';
      SearchSubDirs: Boolean = True): TStringList;
    

**FindAllFiles** ищет файлы, соответствующие маске поиска, в каталоге SearchPath и, если указано, в его вложенных папках, и заполняет [stringlist](<../en/TStrings.md> "TStrings") результирующими именами файлов. 

Маска может быть единственной маской, которую вы можете использовать с функциями FindFirst/FindNext, или она может состоять из списка масок, разделенных [точкой с запятой(;)](<../en/Semicolon.md> "Semicolon").  
Пробелы в маске рассматриваются как литералы. 

Есть две перегруженные версии этой процедуры. Первая из них представляет собой **[процедуру](<Procedure.md> "Procedure/ru")** и предполагает, что получающий список строк уже создан. Вторая - это **[функция](<Function.md> "Function/ru")** , которая создает список строк внутри себя и возвращает его как результат функции. В обоих случаях список строк должен быть уничтожен вызывающей процедурой. 

**Пример:**
    
    
    uses 
      ..., FileUtil, ...
    var
      PascalFiles: TStringList;
    begin
      PascalFiles := TStringList.Create;
      try
        FindAllFiles(PascalFiles, LazarusDirectory, '*.pas;*.pp;*.p;*.inc', true); //находим, например, все исходные файлы паскаля
        ShowMessage(Format('Found %d Pascal source files', [PascalFiles.Count]));
      finally
        PascalFiles.Free;
      end;
    
    //или
    
    begin
      //Нет необходимости создавать список строк; функция делает это для вас
      PascalFiles := FindAllFiles(LazarusDirectory, '*.pas;*.pp;*.p;*.inc', true); //находим, например, все исходные файлы паскаля
      try
        ShowMessage(Format('Found %d Pascal source files', [PascalFiles.Count]));
      finally
        PascalFiles.Free;
      end;
    

**ВАЖНОЕ ЗАМЕЧАНИЕ:** Функция _FindAllFiles_ создает внутренний список строк. На первый взгляд это может показаться очень удобным, но создать **утечки памяти** очень просто: 
    
    
      // НИКОГДА ТАК НЕ ДЕЛАЙТЕ !!!! - Нет способа уничтожить список строк, созданный [функцией] FindAllFiles.
      Listbox1.Items.Assign(FindAllFiles(LazarusDirectory, '*.pas;*.pp;*.p;*.inc', true);
    

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Примечание:** Если вы хотите использовать эту функцию в программах командной строки, добавьте в требования проекта _LCLBase_ , которое не будет тянуть весь LCL.

---

_Source: [https://wiki.freepascal.org/FindAllFiles/ru](https://web.archive.org/web/20240711002531/https://wiki.freepascal.org/FindAllFiles/ru)_
