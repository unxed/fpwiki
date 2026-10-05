# Locating macOS significant directories

[![macOSlogo.png](https://wiki.freepascal.org/images/1/15/macOSlogo.png)](</File:macOSlogo.png>)

Эта статья относится только к [macOS](</Category:macOS> "Category:macOS").

См. также: [Multiplatform Programming Guide](<../en/Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

│ **[English (en)](<../en/Locating_macOS_significant_directories.md> "Locating macOS significant directories")** │  **русский (ru)** │ 

## Contents

  * 1 Обзор
  * 2 Определение места для хранения файлов
  * 3 Функция получения важных каталогов
  * 4 Таблица расположения каталогов
  * 5 Таблица доменов путей
  * 6 Примеры кода
    * 6.1 Каталог пользовательских кэшей
    * 6.2 Пользовательский каталог корзины
    * 6.3 Каталог пользователей
    * 6.4 Все каталоги приложений
    * 6.5 Все каталоги пользовательских приложений
    * 6.6 Все системные каталоги приложений
  * 7 Замена GetAppConfigDir и GetAppConfigFile в FPC
  * 8 см.также



## Обзор

Чтобы найти различные важные каталоги в macOS, вы можете использовать встроенную функцию macOS Foundation 
    
    
    NSSearchPathForDirectoriesInDomains(NSSearchPathDirectory directory, NSSearchPathDomainMask domainMask, BOOL expandTilde);
    

, которая создает список строк путей для указанных каталогов в указанных доменах. Список приведен в том порядке, в котором вы должны выполнять поиск по каталогам. 

Примечание: Каталог, возвращаемый этим методом, может не существовать. Этот метод просто указывает вам подходящее местоположение для запрашиваемого каталога. В зависимости от потребностей приложения, разработчик может создать соответствующий каталог и любой промежуточный каталог. 

## Определение места для хранения файлов

Прежде чем мы продолжим, давайте вспомним, где, согласно рекомендациям Apple, ваше приложение должно хранить свои файлы: 

  * Используйте каталог `/Applications` или `/Applications/Utilities` для [application bundle](<Application_Bundle.md> "Application Bundle/ru")(пакета приложения). Пакет приложений должен содержать все: библиотеки, зависимости, справку, все файлы, необходимые приложению для запуска, за исключением тех, которые создаются самим приложением. Если пакет приложений скопирован в каталог `/Applications` или `/Applications/Utilities` на другом компьютере, он должен быть запущен. Для установки в эти папки требуются права администратора. Резервное копирование данных в этих папках выполняется Time Machine.


  * Используйте каталог `~/Applications`, если права администратора недоступны. Это стандартное расположение для однопользовательского приложения. Не следует ожидать, что этот каталог будет существовать. Пакет приложений должен содержать все: библиотеки, зависимости, справку, все файлы, необходимые приложению для запуска, за исключением тех, которые создаются самим приложением. Если пакет приложений скопирован в каталог `/Applications` или `/Applications/Utilities` на другом компьютере, он должен быть запущен. Резервная копия этих данных создается Time Machine.


  * Используйте [каталог Application Support](<../en/Locating_macOS_app_support,_preferences_folders.md> "Locating macOS app support, preferences folders") (эти данные хранятся в резервной копии Time Machine), добавив свой <bundle_ID>(Идентификатор пакета), для: 
    * Файлов ресурсов и данных, которые ваше приложение создает для пользователя и управляет ими. Вы можете использовать этот каталог для хранения информации о состоянии приложения, вычисленных или загруженных данных или даже созданных пользователем данных, которыми вы управляете от имени пользователя.
    * Автосохранения файлов.


  * Используйте каталог [Caches](<Locating_macOS_significant_directories.md> "Locating macOS significant directories/ru") (эти данные **не** хранятся в резервной копии Time Machine), добавив свой <bundle_ID> для кэшированных файлов данных или любых файлов, которые ваше приложение может легко воссоздать.


  * Используйте каталог [CFPreferences](<../en/Mac_Preferences_Read_and_Write.md> "Mac Preferences Read and Write") для чтения и записи настроек вашего приложения. Это автоматически запишет настройки в соответствующее расположение и прочитает их из соответствующего местоположения. Резервное копирование этих данных выполняется Time Machine.


  * Используйте каталог [Resources](<../en/Locating_the_macOS_application_resources_directory.md> "Locating the macOS application resources directory") (это резервная копия Time Machine) для файлов изображений, звуковых файлов, файлов значков и других неизменяемых файлов данных, необходимых для работы вашего приложения.


  * Используйте функцию [NSTemporaryDirectory](<../en/Locating_the_macOS_tmp_directory.md> "Locating the macOS tmp directory"), которая возвращает путь к временной директории (эти данные **не** хранятся в резервной копии Time Machine) для хранения временных файлов, которые вы собираетесь немедленно использовать для какой-либо текущей операции, но планируете удалить позже. Удалите временные файлы, как только вы закончите с ними.



## Функция получения важных каталогов
    
    
    {$mode objfpc}{H+}
    {$modeswitch objectivec1} 
    
    interface
    
    Uses
      ...
      CocoaUtils,   // for NStringToString
      CocoaAll;     // for NSArray      
    ...
    
    function GetSignificantDir(DirLocation: qword; DomainMask: qword; count: byte): string;
    var
      paths : NSArray;
    begin
      paths := NSSearchPathForDirectoriesInDomains(DirLocation, DomainMask, True);
      if(count < paths.count) then
        Result := NSString(paths.objectAtIndex(0)).UTF8String
      else
        Result := '';  
    end;
    

## Таблица расположения каталогов

Directory Locations  Имя каталога | Описание каталога   
---|---  
NSApplicationDirectory | Поддерживаемые приложения (/Applications)   
NSDemoApplicationDirectory | Неподдерживаемые приложения и демонстрационные версии   
NSDeveloperApplicationDirectory | Приложения для разработчиков (/Developer/Applications)   
NSAdminApplicationDirectory | Приложения для системного и сетевого администрирования   
NSLibraryDirectory | Различная доступная пользователю документация, поддержка и конфигурационные файлы (/Library)   
NSDeveloperDirectory | Ресурсы разработчика (/Developer)   
NSUserDirectory | Домашние каталоги пользователей (/Users)   
NSDocumentationDirectory | Документация   
NSDocumentDirectory | Каталог документов   
NSCoreServiceDirectory | Службы ядра (System/Library/CoreServices)   
NSAutosavedInformationDirectory | Автоматически сохраненные документы пользователя (Library/Autosave Information)   
NSDesktopDirectory | Каталог рабочего стола пользователя   
NSCachesDirectory | Удаляемые файлы кэша (Library/Caches)   
NSApplicationSupportDirectory | Файлы поддержки приложений (Library/Application Support)   
NSDownloadsDirectory | Каталог загрузок пользователя   
NSInputMethodsDirectory | Методы ввода (Library/Input Methods)   
NSMoviesDirectory | Каталог фильмов пользователя (~/Movies)   
NSMusicDirectory | Каталог музыки пользователя (~/Music)   
NSPicturesDirectory | Каталог изображений пользователя (~/Pictures)   
NSPrinterDescriptionDirectory | Системный каталог PostScript Printer Description (Library/Printers/PPDs)   
NSSharedPublicDirectory | Общедоступный каталог общего доступа пользователя (~/Public)   
NSPreferencePanesDirectory | Каталог панели настроек (preference panes) для системных настроек (Library/PreferencePanes)   
NSApplicationScriptsDirectory | Папка пользовательских сценариев для вызывающего приложения (~/Library/Application Scripts/<code-signing-id>  
NSItemReplacementDirectory | Константа, используемая для создания временного каталога   
NSAllApplicationsDirectory | Все каталоги, в которых могут храниться приложения   
NSAllLibrariesDirectory | Все каталоги, в которых могут храниться ресурсы   
NSTrashDirectory | Каталог корзины пользователя   
  
## Таблица доменов путей

Домены Путей  Доменное имя | Описание домена   
---|---  
NSUserDomainMask | Домашний каталог пользователя — место для установки личных настроек пользователя (~).   
NSLocalDomainMask | Место для установки элементов, доступное каждому на этом компьютере   
NSNetworkDomainMask | Место для установки элементов, доступных в сети (/Network)   
NSSystemDomainMask | Каталог для системных файлов, предоставленный компанией Apple (/System)   
NSAllDomainsMask | Все домены   
  
## Примеры кода

Теперь вы можете находить различные важные каталоги и отображать их, как показано в примерах кода ниже. 

### Каталог пользовательских кэшей
    
    
    procedure TForm1.MenuItem5Click(Sender: TObject);
    begin
      ShowMessage('User caches dir: ' + GetSignificantDir(NSCachesDirectory,NSUserDomainMask,0));
    end;
    

### Пользовательский каталог корзины
    
    
    procedure TForm1.MenuItem16Click(Sender: TObject);
    begin
      ShowMessage('User trash dir: '
              + GetSignificantDir(NSTrashDirectory,NSUserDomainMask,0));
    end;
    

### Каталог пользователей

Возможно, это не то, что вы ожидаете... это каталог "/Users" :-) 
    
    
    procedure TForm1.MenuItem10Click(Sender: TObject);
    begin
      ShowMessage('Users dir: ' +  GetSignificantDir(NSUserDirectory,NSLocalDomainMask,0));
    end;
    

Чтобы получить домашний каталог текущего пользователя, вам нужно вместо этого использовать функцию _NSHomeDirectory()_ : 
    
    
    procedure TForm1.MenuItem11Click(Sender: TObject);
    begin
      ShowMessage('User''s dir: ' + NSStringToString(NSHomeDirectory));
    end;
    

**НО** обратите внимание, что если приложение находится в изолированной среде, это вернет каталог приложения [sandbox](<../en/Sandboxing_for_macOS.md> "Sandboxing for macOS"), а не домашний каталог текущего пользователя. 

Альтернативный способ получить домашний каталог текущего пользователя: 
    
    
    function NSUserName: CFStringRef external name '_NSUserName';
    ...
    
    procedure TForm1.MenuItem22Click(Sender: TObject);
    var
      usernameStr: ShortString;
      status: Boolean = false;
    begin
       status := CFStringGetPascalString(CFStringRef(NSusername),@usernameStr,255,CFStringGetSystemEncoding);
      if(status = true) then
         ShowMessage('Current user''s dir: ' + NSStringToString(NShomeDirectoryForUser(NSStr(usernameStr))))
      else
         ShowMessage('Error retrieving username'); 
    end;
    

### Все каталоги приложений
    
    
    procedure TForm1.MenuItem7Click(Sender: TObject);
    var
      count: byte;
    begin
      for count := 0 to 12 do
       if GetSignificantDir(NSAllApplicationsDirectory, NSAllDomainsMask, count) <> '' then
          ShowMessage('Application dir ' + IntToStr(count) + ': '
               + GetSignificantDir(NSAllApplicationsDirectory,NSAllDomainsMask,count))
       else
          exit; 
    end;
    

### Все каталоги пользовательских приложений
    
    
    procedure TForm1.MenuItem8Click(Sender: TObject);
    var
      count: byte;
    begin
      for count := 0 to 12 do
       if GetSignificantDir(NSAllApplicationsDirectory, NSUserDomainMask, count) <> '' then
          ShowMessage('User application dir ' + IntToStr(count) + ': '
               + GetSignificantDir(NSAllApplicationsDirectory,NSUserDomainMask,count))
       else
          exit; 
    end;
    

### Все системные каталоги приложений
    
    
    procedure TForm1.MenuItem9Click(Sender: TObject);
    var
      count: byte;
    begin
      for count := 0 to 12 do
       if GetSignificantDir(NSAllApplicationsDirectory, NSSystemDomainMask, count) <> '' then
          ShowMessage('System application dir ' + IntToStr(count) + ': '
               + GetSignificantDir(NSAllApplicationsDirectory,NSSystemDomainMask,count))
       else
          exit; 
    end;
    

## Замена GetAppConfigDir и GetAppConfigFile в FPC

Существующие функции FPC `GetAppConfigDir` и `GetAppConfigFile` не подходят для Mac OS, поскольку они используют соглашение UNIX и **не** соответствуют соглашению Apple macOS. Например, выходные данные этих функций: 
    
    
    writeLn(GetAppConfigDir(true));
    writeLn(GetAppConfigDir(false));
    
    writeLn(GetAppConfigFile(true));
    writeLn(GetAppConfigFile(false));
    
    writeLn(GetAppConfigFile(true,true));
    writeLn(GetAppConfigFile(true,false));
    
    writeLn(GetAppConfigFile(false,false));
    writeLn(GetAppConfigFile(false,true));
    

это: 
    
    
    /etc/
    /Users/<username>/.config/
    
    /etc/project1.cfg
    /Users/<username>/.config/project1.cfg
    
    /etc/project1/project1.cfg
    /etc/project1.cfg
    
    /Users/<username>/.config/project1.cfg
    /Users/<username>/.config/project1/project1.cfg
    

принимая во внимание, что правильный вывод для приложения командной строки macOS (т.е. без [Пакета приложений](<Application_Bundle.md> "Application Bundle/ru")) выглядит следующим образом: 
    
    
    /Library/Application Support/project1/
    /Users/<username>/Library/Application Support/project1/
    
    /Library/Application Support/project1/project1.plist
    /Users/<username>/Library/Application Support/project1/project1.plist
    
    /Library/Application Support/project1/project1.plist
    /Library/Application Support/project1/project1.plist
    
    /Users/<username>/Library/Application Support/project1/project1.plist
    /Users/<username>/Library/Application Support/project1/project1.plist
    

и правильный вывод для приложения с графическим интерфейсом Mac OS (например, с [пакетом приложений](<Application_Bundle.md> "Application Bundle/ru")): 
    
    
    /Library/Application Support/com.company.project1/
    /Users/<username>/Library/Application Support/com.company.project1/
    
    /Library/Application Support/com.company.project1/project1.plist
    /Users/<username>/Library/Application Support/com.company.project1/project1.plist
    
    /Library/Application Support/com.company.project1/project1.plist
    /Library/Application Support/com.company.project1/project1.plist
    
    /Users/<username>/Library/Application Support/com.company.project1/project1.plist
    /Users/<username>/Library/Application Support/com.company.project1/project1.plist
    

Вот заменяющие функции, названные с добавлением 2 к существующим именам функций, и с ${IFDEF}s, чтобы их можно было использовать в мультиплатформенном коде: 
    
    
    {$mode objfpc}{H+}
    {$modeswitch objectivec1}
    
    ...
    
    Uses
      ...
      SysUtils,
      CocoaAll,
      Cocoautils;
    ...
    
    //
    // Замена GetAppConfigDir
    //
    
    function GetAppConfigDir2(Global: Boolean): String;
    
    {$IFDEF DARWIN}
    var
      bundleIdStr: String;
      pathsArr : NSArray;
    {$ENDIF}
    begin
      {$IFDEF DARWIN}
      bundleIdStr := NSStringToString(NSBundle.mainBundle.bundleIdentifier);
      if(bundleIdStr = '') then
         bundleIdStr := ExtractFileName(paramstr(0)); // Работа с приложениями, не входящими в пакет приложения, например, с командной строкой
    
      if(Global) then
        pathsArr := NSSearchPathForDirectoriesInDomains(NSApplicationSupportDirectory, NSLocalDomainMask, True)
      else
        pathsArr := NSSearchPathForDirectoriesInDomains(NSApplicationSupportDirectory, NSUserDomainMask, True);
    
      Result := NSString(pathsArr.objectAtIndex(0)).Utf8String + '/' + bundleIdStr + '/';
      {$ELSE}
        GetAppConfigDir(global);
      {$ENDIF}
    end;  
    
    //    
    // Замена GetAppConfigFile
    //
    
    Function GetAppConfigFile2(Global : Boolean; SubDir : Boolean) : String;
    
    {$IFDEF DARWIN}
    var
      bundleIdStr: ShortString;
      appNameStr: ShortString;
      pathBufferStr: String;
      isAppBundle : Boolean = True;
    {$ENDIF}
    begin
      {$IFDEF DARWIN}
      bundleIdStr := NSStringToString(NSBundle.mainBundle.bundleIdentifier);
      if(bundleIdStr = '') then
        begin
         bundleIdStr := ExtractFileName(paramstr(0)); // Работа с приложениями, не входящими в пакет приложения, например, с командной строкой
         isAppBundle := False;
        end;
    
      pathBufferStr := GetAppConfigDir2(Global);
    
      if(isAppBundle) then
        appNameStr := StringReplace(ExtractFileExt(bundleIdStr), '.', '', [rfReplaceAll])
      else
        appNameStr := extractFileName(ExcludeTrailingPathDelimiter(pathBufferStr));
    
      if(SubDir) then
        // Игнорируем, потому что Apple уже задает subdir как часть GetAppConfigDir()
        ;
    
      Result := pathBufferStr + appNameStr + '.plist';
    
      {$ELSE}
      Result := GetAppConfigFile(Global, SubDir);
      {$ENDIF}
    end;
    

## см.также

  * [Locating macOS app support, preferences folders](<../en/Locating_macOS_app_support,_preferences_folders.md> "Locating macOS app support, preferences folders")
  * [Locating the macOS application resources directory](<../en/Locating_the_macOS_application_resources_directory.md> "Locating the macOS application resources directory")
  * [Locating the macOS tmp directory](<../en/Locating_the_macOS_tmp_directory.md> "Locating the macOS tmp directory")

---

_Source: [https://wiki.freepascal.org/Locating_macOS_significant_directories/ru](https://web.archive.org/web/20250426232905/https://wiki.freepascal.org/Locating_macOS_significant_directories/ru)_
