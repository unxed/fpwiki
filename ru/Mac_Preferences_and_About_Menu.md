# Mac Preferences and About Menu

[![macOSlogo.png](https://wiki.freepascal.org/images/1/15/macOSlogo.png)](</File:macOSlogo.png>)

Эта статья относится только к [macOS](</Category:macOS> "Category:macOS").

См. также: [Multiplatform Programming Guide](<../en/Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

│ **[English (en)](<../en/Mac_Preferences_and_About_Menu.md> "Mac Preferences and About Menu")** │  **русский (ru)** │    
****

Некоторые элементы должны отображаться в меню приложения по умолчанию в строке меню macOS вашего приложения. Хотя Lazarus автоматически добавляет: 

  * Services (Службы)
  * Hide MyApp (Скрыть MyApp)
  * Hide Others (Скрыть остальное)
  * Show All (Показать все)
  * Quit MyApp (Выйти из MyApp)



Lazarus не добавляет пункт меню «О моем приложении» или «Настройки ...». Чтобы добавить эти два элемента в то же меню приложения, что и «Выход», вам нужно проделать небольшой трюк. 

Добавьте компонент MainMenu со вкладки Standard палитры компонентов в основную форму. Создайте пункт меню и измените его свойство `Caption` в инспекторе объектов на имя вашего приложения только для справки. Затем в этом меню добавьте три пункта подменю со следующими заголовками: "About MyApp"(О моем приложении), «-» (разделитель) и "Preferences..."(Настройки). 

[![MenuItems3new.jpg](https://wiki.freepascal.org/images/3/32/MenuItems3new.jpg)](</File:MenuItems3new.jpg>)

Назначьте ярлык "Command" пункту меню "Preferences..."(Настройки) редактора ярлыков (выберите свойство `ShortCut` пункта меню в Инспекторе объектов и щелкните точки с многоточием рядом с ним). После отметки мета-клавиши смените надпись в комбобоксе "Unknown" на запятую, нажав сначала клавишу "Grab key", а затем запятую на клавиатуре, в результате ваш ярлык будет выглядеть так: 

[![MenuItemsGrab.png](https://wiki.freepascal.org/images/5/50/MenuItemsGrab.png)](</File:MenuItemsGrab.png>)

Наконец, чтобы получить эти элементы в меню приложения MyApp, мы должны изменить заголовок первого элемента меню в обработчике событий OnCreate главной формы следующим образом: 
    
    
         procedure TForm1.FormCreate(Sender: TObject);
         var
              AppMenu : TMenuItem;
         begin
              AppMenu := TMenuItem.Create(Self); // Меню приложения
              AppMenu.Caption := #$EF#$A3#$BF;   // Юникодный символ логотипа Apple
              MainMenu.Items.Insert(0, AppMenu);
         end;
    

После перекомпиляции вашего приложения и его запуска вы заметите, что "About MyApp" и "Preferences..." были добавлены в меню MyApp, где они и должны быть. 

[![MenuItems4new.png](https://wiki.freepascal.org/images/0/05/MenuItems4new.png)](</File:MenuItems4new.png>)

Вы заметили проблему? Вы действительно хотите назвать свое приложение «MyApp», а не «My App»? 

Нет, вы не можете просто так изменить файл проекта «program MyApp» на «Program My App», иначе вам сообщат "MyApp.lpr(1,11) Fatal: Syntax error, ";" expected but "identifier APP" found"(ошибка синтаксиса: ожидается ";", но найден идентификатор APP). 

Решение состоит в том, чтобы изменить файл Info.plist вашего приложения, открыв пакет вашего приложения, перейдя к Contents, дважды щелкнув Info.plist и используя редактор Xcode plist, чтобы изменить значение ключа «Bundle name» на строку "My App" и сохранить его. Теперь, когда вы запустите"MyApp", оно отобразится как "My App". 

Of course you would also now need to change the "About MyApp" caption too :-) Конечно, теперь вам также нужно будет изменить заголовок "About MyApp" :-) 

Все? Нет. Пункты меню, созданные Lazarus, по-прежнему показывают "Hide MyApp"(Скрыть MyApp) и "Quit MyApp"(Выйти из MyApp). Чтобы изменить их на "My App", вам необходимо изменить обработчик событий `OnCreate` основной формы, как показано ниже: 
    
    
         procedure TForm1.FormCreate(Sender: TObject);
         var
              AppMenu : TMenuItem;
         begin
              Application.Title := 'My App';
              AppMenu := TMenuItem.Create(Self); // Меню приложения
              AppMenu.Caption := #$EF#$A3#$BF;   // Юникодный символ логотипа Apple
              MainMenu.Items.Insert(0, AppMenu);
         end;
    

И вот окончательный результат: 

[![macOSMenuFinal.png](https://wiki.freepascal.org/images/1/18/macOSMenuFinal.png)](</File:macOSMenuFinal.png>)

Чтобы связать параметры меню с действием, добавьте TActionList в основную форму, добавьте действия для About и Preferences (отредактируйте свойства, а затем создайте соответствующие обработчики событий `OnExecute` для каждого действия), а затем измените обработчик событий `OnCreate` основной формы как ниже: 
    
    
    procedure TForm1.FormCreate(Sender: TObject);
         var
              AppMenu     : TMenuItem;
              AppAboutCmd : TMenuItem;
              AppSepCmd   : TMenuItem;
              AppPrefCmd  : TMenuItem;
         begin
              Application.Title := 'My App';
    
              AppMenu := TMenuItem.Create(Self); // Меню приложения
              AppMenu.Caption := #$EF#$A3#$BF;   // Юникодный символ логотипа Apple
              MainMenu.Items.Insert(0, AppMenu);
    
              AppAboutCmd := TMenuItem.Create(Self);
              AppAboutCmd.Action:= ActionList1.Actions[1];
              AppMenu.Add(AppAboutCmd);  // Добавляем About как пункт в меню приложения
    
              AppSepCmd := TMenuItem.Create(Self);
              AppSepCmd.Caption := '-';
              AppMenu.Add(AppSepCmd);   // добавляем разделитель меню
    
              AppPrefCmd := TMenuItem.Create(Self);
              AppPrefCmd.Action:= ActionList1.Actions[2];
              AppMenu.Add(AppPrefCmd);   // Добавляем Preferences как пункт в меню приложения
    
              AppSepCmd := TMenuItem.Create(Self);
              AppSepCmd.Caption := '-';
              AppMenu.Add(AppSepCmd);   // добавляем разделитель меню
         end;
    

  


## См. также

  * [Introduction to platform-sensitive development](<../en/Introduction_to_platform-sensitive_development.md> "Introduction to platform-sensitive development")
  * [Mac Preferences Read and Write](<../en/Mac_Preferences_Read_and_Write.md> "Mac Preferences Read and Write")
  * [Mac Show Application Title, Version, and Company](<../en/Mac_Show_Application_Title,_Version,_and_Company.md> "Mac Show Application Title, Version, and Company")
  * [Add an Apple Help Book to your macOS app](<../en/Add_an_Apple_Help_Book_to_your_macOS_app.md> "Add an Apple Help Book to your macOS app")
  * [macOS Programming Tips](<../en/macOS_Programming_Tips.md> "macOS Programming Tips")

---

_Source: [https://wiki.freepascal.org/Mac_Preferences_and_About_Menu/ru](https://web.archive.org/web/20240101000000/https://wiki.freepascal.org/Mac_Preferences_and_About_Menu/ru)_
