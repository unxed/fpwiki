# LazAutoUpdater

│ **[English (en)](<../en/LazAutoUpdater.md> "LazAutoUpdater")** │  **[polski (pl)](</LazAutoUpdater/pl> "LazAutoUpdater/pl")** │  **русский (ru)** │    
****

## Lazarus Auto-Updater

## Contents

  * 1 Lazarus Auto-Updater
    * 1.1 Резюме
    * 1.2 Загрузка
    * 1.3 Установка
    * 1.4 Пример приложения
    * 1.5 Интернационализация(локализация)
    * 1.6 Использование
      * 1.6.1 Больший контроль
      * 1.6.2 Отладка
    * 1.7 Поддержка
    * 1.8 Лицензия
    * 1.9 Развертывание вашего приложения с помощью LazAutoUpdate
    * 1.10 Использование [свойств] auOtherSourceFilename и auOtherSourceURL
    * 1.11 LazAutoUpdate [в виде приложения] в системном трее
    * 1.12 Создание универсального онлайн-установщика/программы обновления
    * 1.13 Список Public-методов
    * 1.14 Список неопубликованных свойств
    * 1.15 Развертывание нескольких обновлений в одном проекте
    * 1.16 Если загрузка занимает много времени
    * 1.17 Versions.ini
    * 1.18 Исходный код и последние версии
    * 1.19 Безопасность
      * 1.19.1 Windows 10
      * 1.19.2 Linux
    * 1.20 Рабочий процесс



### Резюме

  * LazAutoUpdate - это визуальный раскрывающийся компонент для IDE Lazarus / FPC, который делает обновление приложения из онлайн-источника более простым и интеллектуальным.
  * При создании приложения для распространения, написание процесса обновления для пользователей может быть проблематичным, однако пользователи ценят это средство (если оно не навязано им!), и поддержка проще, если вы знаете, что пользователь всегда имеет самую актуальную версию вашего приложения. LazAutoUpdate предназначен для хорошей интеграции с системой контроля версий SourceForge или GitHub, обеспечивая беспроблемную работу для вас и ваших пользователей.
  * LazAutoUpdate загружает через фоновый поток, чтобы пользователь мог продолжать использовать ваше приложение без перерыва 
    * Это означает, например, что ваше приложение может «проверять наличие обновлений» при запуске без "зависаний".
  * Компонент предназначен для разработчиков Lazarus для Windows и Linux, которые размещают свои проекты в SourceForge или GitHub.
  * Вставьте компонент, установите пару свойств (имя вашего проекта SourceForge или некоторые свойства GitHub) и вызовите один простой метод: 
    * **LazAutoUpdate.AutoUpdate**
  * Вы (разработчик) имеете достаточный контроль над поведением компонента, и он достаточно прост в использовании.
  * Конечные пользователи видят процесс обновления простым и прозрачным
  * Протестировано и разработано в Windows 10 64/32-битной и Linux 64/32-битной



  
[![botton](https://wiki.freepascal.org/images/e/e7/lauobjinsp1.jpg)](</File:lauobjinsp1.jpg> "botton") [![lauobjinsp2.jpg](https://wiki.freepascal.org/images/7/70/lauobjinsp2.jpg)](</File:lauobjinsp2.jpg>)   


* * *

### Загрузка

  * Установщик пакета LazAutoUpdate (Windows) или zip (Linux) можно загрузить с [сайта проекта SourceForge](<http://sourceforge.net/projects/lazautoupdate/>)
  * OnlinePackageManager Lazarus также содержит LazAutoUpdate



  


* * *

### Установка

  * Загрузите установочный файл Windows или zip-файл linuxbinaries
  * Windows: установить, Linux: распаковать в запасную папку
  * Используйте UpdatePack для создания и распространения ваших файлов обновлений.
  * Используйте компонент LazAutoUpdate, чтобы добавить функциональность обновления в ваши Lazarus'овские приложения. 
    * **Вам нужно будет распространять updatehm <os>(.exe) вместе с вашим приложением, чтобы LazAutoUpdate работал правильно**
    * **Не забудьте устанавливать номера VersionInfo в опциях вашего проекта Lazarus**



  


* * *

### Пример приложения

  * В загруженном пакете находится проект «TestApp»
  * Скомпилируйте и запустите TestApp (не забудьте скомпилировать + скопировать соответствующее средство обновления консоли (updatehm_xxx) в исполняемую папку) 
    * Это должно обновить себя "из коробки"
  * Проверьте исходный код TestApp, чтобы получить представление о том, как использовать LazAutoUpdate в полной мере



  


* * *

### Интернационализация(локализация)

  * i8n: LazAutoUpdate имеет папку 'locale' со всеми файлами .po, необходимыми для перевода. Базовый язык английский



  


* * *

### Использование

  * Установите номер версии для вашего приложения в Project(Проект)/Options(Настройки)/Versioninfo(Информация о версии)
  * Перетащите компонент в основную форму вашего приложения
  * Задайте для свойства SFProjectName имя проекта на SourceForge (тот же текст, который отображается в URL-адресе страницы вашего проекта).
  * или .. Установите свойства GitHub: 
    * GitHubProjectname: Имя пользователя или Организация
    * GitHubRepositoryName: Ваш project(проект)/repository(хранилище)
    * GitHubBranchOrTag: 'master' для root или имя ветки GitHub **или** имя тэга
  * В обоих случаях укажите в UpdatesFolder расположение ваших файлов обновлений (вы можете оставить его пустым)
  * Используйте UpdatePack для загрузки вашего приложения в Интернет (SourceForge, GitHub и т.д.)
  * **Затем позвольте компоненту позаботиться о деталях. Просто используйте его (возможно, как пункт меню справки «Проверка обновлений»)**


    
    
    LazAutoUpdate1.AutoUpdate;
    

  * Вы не хотите, чтобы пользователь закрывал ваше приложение в середине обновления? Используйте этот код в обработчике Form.CloseQuery:


    
    
    If LazAutoUpdate1.DownloadInProgress Then
      Begin
      CanClose := False;
      ShowMessage('Пожалуйста, подождите. Загрузка все еще продолжается.');
      End;
    

  * После обновления ваше приложение может автоматически показывать «Что нового». Поместите эту строку в событие OnShow вашей формы:


    
    
    LazAutoUpdate1.ShowWhatsNewIfAvailable;
    

  * Это дает вам «каркасность»(бареюон-основу) кода обновления для вашего приложения. Как видите, все довольно просто и автоматизировано.



#### Больший контроль

  * Вы хотите, чтобы ваше приложение проверяло наличие обновлений при запуске? В этом случае, в вашем обработчике form.activate:


    
    
    If LazAutoUpdate1.NewVersionAvailable Then
          MessageDlg(Application.Title, 'Новая версия ' + Application.Title +
            ' - доступна.' + LineEnding +
            'Нажмите «Проверить наличие новой версии» в меню «Справка», чтобы обновиться.', mtConfirmation,
            [mbOK], 0);
    

  * Если вы не используете метод автоматического обновления, вы можете контролировать процесс обновления поэтапно с помощью функций:


    
    
    If LazAutoUpdate1.NewVersionAvailable then ..... else ...
    
    
    
    If LazAutoUpdate1.DownloadNewVersion then ..... else ...
    
    
    
    If LazAutoUpdate1.UpdateToNewVersion then ..... else ...
    

#### Отладка

  * Если вы установите


    
    
    LazAutoUpdate1.DebugMode:=TRUE;
    

тогда вы можете использовать встроенный метод OnDebugEvent для отслеживания активности: 
    
    
    procedure TForm1.LazAutoUpdate1DebugEvent(Sender: TObject; lauMethodName,lauMessage: string);
    begin
    // используется TEventLog
       Logger.Info('('+lauMethodName+') - ' + lauMessage);
    end;
    

Также возникают события: OnDownloaded и OnNewVersionAvailable, а также свойство LastError с информацией, которую вы можете использовать для отладки.   


* * *

### Поддержка

  * LazAutoUpdate поддерживается через форум Lazarus [здесь](<http://forum.lazarus.freepascal.org/index.php/topic,25444.0.html>).



  


* * *

### Лицензия

  * LazaAutoUpdate: LGPLv2 (так же, как компоненты Lazarus)
  * Update Pack: GPLv2



  


* * *

### Развертывание вашего приложения с помощью LazAutoUpdate

  * Используйте UpdatePack для управления процессом. 
    * UpdatePack - это приложение с графическим интерфейсом, которое делает файлы обновления точными и быстрыми и даже предоставляет вам соответствующий код LazAutoUpdate для вашего приложения.
    * Оно использует 'profiles'(профили), которые позволяют вам развертывать будущие обновления, просто обновляя номер версии и нажимая «Создать пакет обновления».
    * Пользователям вашего приложения всегда будет полезна последняя версия программного обеспечения.
  * При тестировании помните, что ваше тестовое приложение должно иметь меньший номер версии, чем ваше приложение 'для обновления', хранящееся в сети :)
  * Если вы хотите создать (или добавить) обновление zip самостоятельно, оно должно содержать, как минимум, приложение **и** текстовый файл «whatsnew.txt»(что нового?).



  
[![updatepack1.jpg](https://wiki.freepascal.org/images/b/b0/updatepack1.jpg)](</File:updatepack1.jpg>) [![updatepack2.jpg](https://wiki.freepascal.org/images/b/b8/updatepack2.jpg)](</File:updatepack2.jpg>)

* * *

### Использование [свойств] auOtherSourceFilename и auOtherSourceURL

  * Когда для ProjectType установлено значение auOther, LazAutoUpdate будет использовать эти свойства для всех методов. Смотрите пример приложения в архиве SVN.



  


* * *

### LazAutoUpdate [в виде приложения] в системном трее

  * Пример можно найти в папке /trayicon исходника.
  * Когда он запускается, он находится в области панели задач и молча проверяет наличие обновлений по расписанию (настраивается разработчиком и пользователем) 
    * Если он находит новую версию, появляется всплывающее уведомление для обновления: 
      * Если приложение не запущено, обновление выполняется в фоновом режиме
      * Если приложение работает, оно закрывает его, а затем выполняет обновление. [Затем] приложение перезапускается.
  * Updater в системном трее имеет контекстное меню: 
    * _Configure_ (Настройка): 
      * Пользователь может редактировать расписание - у каждого приложения есть 'profile'(профиль)
    * _Runs at startup_ (Автозапуск после старта ОС (для Windows)): 
      * Приложение молча запускается в системный трей при каждой загрузке Windows
  * Приложение в системном трее (lautraynotify) автоматически выбирает точки входа из приложений, использующих LazAutoupdate, поэтому его не нужно настраивать - его просто устанавливают один раз и забывают.
  * График [обновления] гибкий, от одного раза в день до одного раза в месяц 
    * UpdatePack заботится о создании файла конфигурации systray для вашего приложения. По умолчанию проверка [осуществляется] каждый понедельник в 9 утра.
  * Приложение systray работает как в Windows, так и в Linux - все, что вам нужно сделать, это привести его в действие.



  


* * *

### Создание универсального онлайн-установщика/программы обновления

  * После того, как компонент LazAutoUpdate был добавлен в форму, его можно использовать для нескольких обновлений, загрузок и т.д. (См. '/testinstaller' в исходном коде пакета)
  * Для универсального установщика/программы обновления вам необходимо установить: 
    * VersionsINIFilename
    * ZipFilename
    * AppFileWithPath
    * AppVersion
  * Динамически устанавливая эти свойства (возможно, через меню), вы можете разрешить пользователю обновлять несколько приложений, размещенных на сайте проекта SourceForge/GitHub (или на любом сайте, использующем параметры auOther - см. выше).


  * Вот пример кода для использования LazAutoUpdate для загрузки и запуска приложения.
  * Заархивированные файлы для приложения (включая 'whatsnew.txt') и файл 'mywizzyapp.ini' находятся в подкаталоге /updates проекта SourceForge 'mywizzy'.
  * Исполняемый файл 'updatehm(ostype)(.exe)' находится в том же каталоге, что и ваше приложение для обновления 
    * ostype=[win32|win64|linux32|linux64]
    * Windows ext=(.exe)
  * Пользователь загружает небольшой установщик InnoSetup только с онлайн-установщиком/программой обновления, который, в свою очередь, загружает приложения и вспомогательные файлы. 
    * Конечно, "installer"(установщик) может **быть** основным приложением, а родственные приложения можно установить через меню или что-то подобное.


    
    
    procedure TForm1.Button1Click(Sender: TObject);
    // Uses SysUtils,LazFileUtils,ulazautoupdate и т.д.
    Var sDirectoryToInstallTo: string;
    begin
    sDirectoryToInstallTo := ProgramDirectory + 'installed';
    Application.Title:='My whizzy app'
    LazAutoUpdate1.WorkingMode := lauInstall;
    LazAutoUpdate1.ProjectType := auSourceForge;
    LazAutoUpdate1.SFProjectname:='mywizzy';
    LazAutoUpdate1.VersionsININame:='mywizzyapp.ini';
    LazAutoUpdate1.ZipfileName:='mywizzyapp.zip';
    LazAutoUpdate1.AppFileWithPath := sDirectoryToInstallTo + DirectorySeparator + 'mywizzyapp.exe';
    // Наша обязанность создать папку
    if not DirectoryExistsUTF8(sDirectoryToInstallTo) then ForceDirectoriesUTF8(sDirectoryToInstallTo);
    LazAutoUpdate1.Appversion:='0.0.0.0';
    If LazAutoUpdate1.DownloadNewVersion then
      begin
        If LazAutoUpdate1.UpdateToNewVersion then
        begin
          LazAutoUpdate1.ShortCut.Category := scUtility;
          LazAutoUpdate1.ShortCut.Target := LazAutoUpdate1.AppFileWithPath;
          LazAutoUpdate1.ShortCut.ShortcutName := Application.Title;
          LazAutoUpdate1.MakeShortCut; //создает ярлык на рабочем столе и пункт меню Пуск в Windows
        end;
      end;
    end;
    

  * Затем вы можете легко запустить установленное приложение программно:


    
    
    // Uses asyncprocess
    procedure TForm1.RunInstalledApp;
    var
      AProcess: TAsyncProcess;
    begin
      if not FileExistsUTF8(LazAutoUpdate1.AppFileWithPath) then
      begin
        ShowMessageFmt('%s не существует! Сначала установите его.',
          [LazAutoUpdate1.AppFileWithPath]);
        Exit;
      end;
      AProcess := TAsyncProcess.Create(nil);
      try
        AProcess.Executable := LazAutoUpdate1.AppFileWithPath;
        AProcess.Execute;
      finally
        Aprocess.Free;
      end;
    end;
    

  


* * *

### Список Public-методов

  * Function NewVersionAvailable: Boolean;
  * Function DownloadNewVersion: Boolean;
  * Function UpdateToNewVersion: Boolean;
  * Procedure ShowWhatsNewIfAvailable;
  * Procedure AutoUpdate;
  * function AppIsActive
  * procedure ResetAppVersion;



  


* * *

### Список неопубликованных свойств

  * GUIOnlineVersion: String
  * ReturnCode: Integer
  * DownloadInprogress: Boolean
  * AppFileWithPath: String
  * AppVersion: String
  * LastError: String;
  * DebugMode: boolean
  * LCLVersion: string
  * WidgetSet: string
  * FPCVersion: string
  * LastCompiled: string
  * TargetOS: string
  * WindowsAdminCheck: boolean



  


* * *

### Развертывание нескольких обновлений в одном проекте

  * По умолчанию ZipFilename совпадает с вашим приложением, но вы можете установить для него уникальное значение и развернуть его в своем каталоге Online Files/updates
  * Каждое обновляемое приложение должно иметь уникальное имя для файла versions.ini. Установите свойство VersionsINIFilename.



  


* * *

### Если загрузка занимает много времени

  * Свойство `VersionCountLimit` определяет, как долго LazAutoUpdate будет проверять наличие новой версии до истечения времени ожидания.
  * Свойство `DownloadCountLimit` определяет, как долго LazAutoUpdate будет пытаться загрузить новую версию до истечения времени ожидания.



  


* * *

### Versions.ini

Формат выглядит следующим образом: 
    
    
    ;файл версии LazAutoUpdate 
    [versions]
    GUI=0.0.2
    

  * Файл небольшой, поэтому его очень быстро скачать



  


* * *

### Исходный код и последние версии

  * [Страница проекта](<https://sourceforge.net/projects/lazautoupdate/>)
  * [SVN HTTP Checkout](<https://svn.code.sf.net/p/lazarus-ccr/svn/components/lazautoupdate/latest_stable>)



  


* * *

### Безопасность

#### Windows 10

  * Безопасность Windows 10 не позволит пользователю без прав администратора обновлять приложения из Интернета (если только это не приложение Магазина Windows). На мой взгляд, это разумная политика, и LazAutoUpdate придерживается ее. 
    * Если «обычный пользователь» попытается обновить приложение с помощью LazAutoUpdate, все пройдет гладко до финальной фазы «Обновление приложения». В этот момент у LazAutoUpdate появится диалоговое окно, информирующее пользователя о том, что для фактического обновления необходима учетная запись администратора.
    * Для пользователя с правами администратора LazAutoUpdate откроет диалоговое окно UAC (так же, как и любой установщик) и продолжит работу после нажатия «Да», чтобы выполнить обновление.
    * Если вы хотите обойти это (возможно, с помощью манифеста), LazAutoUpdate имеет public-свойство 'WindowsAdminCheck', которое вы можете установить в FALSE, чтобы избежать [появление] диалога.



#### Linux

  * Linux будет выполнять обновление, даже не показывая консоль обновления, и пользователь получает беспроблемный опыт.



  


* * *

### Рабочий процесс

(Техническое объяснение) 

Рабочий процесс LazAutoUpdater для обновления запущенного приложения выглядит следующим образом: 

  * Приложение загружает небольшой файл 'version.ini' из sourceforge или github с информацией о версии (это можно сделать при запуске)
  * Приложение сравнивается с собственной внутренней версией
  * Если доступна новая версия 
    * Приложение удаляет любое содержимое локальной папки/папки обновлений
    * Приложение загружает, а затем распаковывает его из Интернета в локальную папку/папку обновлений.
  * Updater.exe копирует загруженный файл «whatsnew.txt» в папку с приложением и на несколько секунд переходит в режим сна.
  * Между тем приложение производит цикл проверки того, был ли скопирован файл «whatsnew.txt» в его каталог
  * Приложение обнаруживает «whatsnew.txt» и закрывает (другими словами, TProcess успешно запускается)
  * Updater копирует/обновляет/ обновляет приложение в каталоге приложения.
  * Updater использует TProcess для запуска обновленного приложения
  * В Form.Show приложение отображает «whatsnew.txt», а затем удаляет его.



Пользователь видит: 

  * Диалоговое окно: "Доступна новая версия ххх. Хотите скачать?" Да/Нет
  * Если Да, нажмите: 
    * Загрузка происходит в фоновом режиме (через фоновый поток). Необязательный «счетчик загрузки» отображается для обозначения прогресса [загрузки].
    * Пользователю запрещено закрывать приложение во время загрузки (в Form.CloseQuery)
    * Диалоговое окно: "Обновление загружено. Нажмите OK, чтобы установить его и перезапустить xxx сейчас" OK
    * Пользователь нажимает ОК
    * Консоль (окно DOS в Windows) открывается автоматически, а приложение закрывается. Консоль сообщает: «Пожалуйста, подождите, обновляется ххх»
    * Через пару секунд консоль исчезает, и запускается новая версия приложения.
    * Как только отображается главное окно, отображается информационное окно «Что нового» с кнопкой «ОК»
    * Пользователь нажимает кнопку ОК и больше никогда не видит [эту] информацию [вновь]



  


* * *

[Minesadorada](</User:Minesadorada> "User:Minesadorada")

---

_Source: [https://wiki.freepascal.org/LazAutoUpdater/ru](https://web.archive.org/web/20250601000000/https://wiki.freepascal.org/LazAutoUpdater/ru)_
