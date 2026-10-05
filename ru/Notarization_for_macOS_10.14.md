# Notarization for macOS 10.14.5+

│ **[English (en)](<../en/Notarization_for_macOS_10.14.md>)** │  **русский (ru)** │

[![macOSlogo.png](https://wiki.freepascal.org/images/1/15/macOSlogo.png)](</File:macOSlogo.png>)

Эта статья относится только к [macOS](</Category:macOS> "Category:macOS").

См. также: [Multiplatform Programming Guide](<../en/Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

  
****

## Contents

  * 1 Обзор
  * 2 Диалоговые окна Gatekeeper
  * 3 Требования к нотариальному заверению
    * 3.1 Заявление Apple от 23 декабря 2019 г.
  * 4 Нотариальное заверение заявки
    * 4.1 Шаг 1 - Создание образа диска
    * 4.2 Шаг 2 - Код для подписи образа диска
    * 4.3 Шаг 3. Создание пароля для конкретного приложения
    * 4.4 Шаг 4 - Загрузка образа диска в нотариальную службу
    * 4.5 Шаг 5 - Проверка процесса нотариального заверения
    * 4.6 Шаг 6 - Прикрепление удостоверения к образу диска
    * 4.7 Шаг 7 - Подтверждение нотариального заверения образа диска
    * 4.8 Шаг 8 - Подтвердить нотариальное заверение приложения
  * 5 Нотариальное заверение инструмента командной строки
    * 5.1 Вставка Info.plist в исполняемый файл командной строки
    * 5.2 Подпись исполняемого файла командной строки
    * 5.3 Отправка исполняемого файла командной строки на нотариальное заверение
    * 5.4 Проверка процесса нотариального заверения
    * 5.5 Нотариальное заверение исполняемого файла командной строки
  * 6 Устранение неполадок
    * 6.1 Процесс загрузки нотариального заверения
    * 6.2 Сбои в процессе нотариального заверения
    * 6.3 Сбои при проверке нотариального заверения
  * 7 См.также
  * 8 Внешние ссылки



## Обзор

Начиная с macOS 10.14.5 (Mojave), программное обеспечение, подписанное новым сертификатом разработчика Apple, и все новые или обновленные расширения ядра должны быть нотариально заверены для запуска. Начиная с macOS 10.15 (Catalina), все программное обеспечение, созданное после 1 июня 2019 года, и код, подписанный сертификатом разработчика Apple, необходимо нотариально заверить. _Нотариальное заверение не требуется для программного обеспечения, распространяемого через Apple App Store_ , поскольку процесс отправки в App Store уже включает эквивалентные проверки безопасности. 

Нотариальное заверение разработано Apple, чтобы дать пользователям больше уверенности в том, что программное обеспечение, подписанное разработчиками Apple, было проверено Apple на наличие вредоносных компонентов. Нотариальное заверение - это не проверка приложения. Нотариальная служба Apple - это автоматизированная система, которая сканирует программное обеспечение на наличие вредоносного содержимого, проверяет наличие проблем с подписью кода и возвращает результаты. Если проблем нет, нотариальная служба генерирует удостоверение для установки программного обеспечения; нотариальная служба также публикует это удостоверение в Интернете, где его может найти Gatekeeper ([прим.перев.](</User:Zoltanleo> "User:Zoltanleo"): букв. _привратник_). 

Когда пользователь впервые устанавливает или запускает программное обеспечение, наличие удостоверения (онлайн или прикрепленного к исполняемому файлу) сообщает Gatekeeper, что Apple нотариально заверила программное обеспечение. Затем Gatekeeper помещает описательную информацию в диалоговое окно первоначального запуска, чтобы помочь пользователю сделать осознанный выбор, запускать ли приложение. 

[![Note-icon.png](https://wiki.freepascal.org/images/b/be/Note-icon.png)](</File:Note-icon.png>)

**Примечание:** Даже если приложение не было нотариально заверено, его все равно можно запустить, щелкнув приложение, удерживая клавишу Control, или щелкнув правой кнопкой мыши для вызова контекстного меню, выбрав «Открыть» - см. диалоговое "обходное" окно Gatekeeper в галерее [ниже](<Notarization_for_macOS_10.14.md> "Notarization for macOS 10.14.5+/ru").

Подписание кода приложения без нотариального заверения - теперь совершенно бессмысленное упражнение, _если только_ вы не распространяете свое приложение только через App Store. 

## Диалоговые окна Gatekeeper

  * [![](https://wiki.freepascal.org/images/e/e1/macos-mojave-app-downloaded-from-internet-alert-dark.jpg)](</File:macos-mojave-app-downloaded-from-internet-alert-dark.jpg>)

Диалоговое окно Gatekeeper до Catalina, в котором сначала открывается нотариально не заверенное приложение (диалоговое окно Mojave macOS 10.14 ) 

  * [![](https://wiki.freepascal.org/images/c/c6/Screenshot_2020-01-02_at_22.42.01.png)](</File:Screenshot_2020-01-02_at_22.42.01.png>)

Диалоговое окно Gatekeeper, в котором сначала открывается подписанное и нотариально заверенное приложение (диалоговое окно Catalina macOS 10.15.1) 

  * [![](https://wiki.freepascal.org/images/6/6c/gatekeep_no.png)](</File:gatekeep_no.png>)

Сначала открывается диалоговое окно Gatekeeper, в котором приложение не подписано и не заверено нотариально. (диалоговое окно Catalina macOS 10.15.1) 

  * [![](https://wiki.freepascal.org/images/3/33/macos-catalina-alert-unsigned-override.jpg)](</File:macos-catalina-alert-unsigned-override.jpg>)

Диалоговое "обходное" окно Gatekeeper, доступ к которому можно получить, щелкнув приложение, удерживая клавишу Control. (диалоговое окно Catalina macOS 10.15.3) 




## Требования к нотариальному заверению

Для нотариального заверения требуются инструменты командной строки Xcode от Xcode 10 или новее. Для создания нового приложения для нотариального заверения требуется macOS 10.13.6 или новее. Для прикрепления приложения требуется macOS 10.12 или новее. 

Нотариальная служба Apple требует от вас: 

  * Включения подписывания кода для всех распространяемых вами исполняемых файлов.
  * Включения возможности заверения через [ Hardened Runtime](<../en/Hardened_runtime_for_macOS.md> "Hardened runtime for macOS") для вашего приложения и целей командной строки.
  * Использования приложения «Developer ID», расширения ядра или сертификата установщика для подписи кода.
  * Включения безопасной метки времени в свою подпись для подписи кода (включения параметра --timestamp при запуске инструмента кодовой подписи).
  * Не включать право com.apple.security.get-task-allow со значением, равным любому варианту истинного. Если в вашем программном обеспечении размещаются сторонние плагины и требуется это право для отладки плагина в контексте исполняемого файла хоста, см. [Avoid the Get-Task-Allow Entitlement](<https://developer.apple.com/documentation/xcode/notarizing_macos_software_before_distribution/resolving_common_notarization_issues#3087731>).
  * Ссылки на SDK для macOS 10.9 или новее.



3 сентября 2019 года Apple объявила, что до января 2020 года разработчики могут нотариально заверить новые версии приложений, даже если они не были заверены через [hardened](<../en/Hardened_runtime_for_macOS.md> "Hardened runtime for macOS") или полностью соответствуют обычным требованиям. **Позднее этот срок был продлен до 3 февраля 2020 года. Вышеуказанные требования к нотариальному удостоверению теперь действуют в полной мере.**

### Заявление Apple от 23 декабря 2019 г.

В июне мы объявили, что все программное обеспечение Mac, распространяемое за пределами Mac App Store, должно быть нотариально заверено Apple, чтобы оно могло работать по умолчанию на macOS Catalina. В сентябре мы временно скорректировали  предварительные условия нотариального заверения, чтобы упростить этот переход и защитить пользователей macOS Catalina, которые продолжают использовать более старые версии программного обеспечения. Начиная с 3 февраля 2020 года, все представленное программное обеспечение должно соответствовать предварительным требованиям для нотариального заверения. 

Если вы еще не сделали этого, загрузите программное обеспечение в нотариальную службу и просмотрите журнал разработчика на наличие предупреждений. Эти предупреждения станут ошибками с 3 февраля и должны быть исправлены, чтобы ваше программное обеспечение было нотариально заверенным. Программное обеспечение, нотариально заверенное до 3 февраля, по умолчанию будет продолжать работать на macOS Catalina. 

Напоминаем, что все пакеты установщика должны быть подписаны, поскольку они могут содержать исполняемый код. Образы дисков не нужно подписывать, хотя их подписание может помочь вашим пользователям проверить их содержимое. 

## Нотариальное заверение заявки

В этих инструкциях предполагается, что вы выполнили другие подготовительные шаги: [ подписание кода](<../en/Code_Signing_for_macOS.md> "Code Signing for macOS") приложения, реализованное в [ усиленной среде выполнения](<../en/Hardened_runtime_for_macOS.md> "Hardened runtime for macOS"), [ подписание кода](<../en/Code_Signing_for_macOS.md> "Code Signing for macOS") установщика pkg (если он у вас есть) и т.д. 

Вы не можете выгрузить пакет .app непосредственно в нотариальную службу, вам необходимо создать сжатый архив, содержащий приложение, или вы можете поместить приложения, расширения ядра и другое программное обеспечение в контейнер, например образ диска, и нотариально заверить контейнер. Нотариальная служба принимает образы дисков (формат UDIF), подписанный установщиком плоских пакетов, и ZIP-архивы. Он также обрабатывает вложенные контейнеры, например пакеты внутри образа диска. 

  


### Шаг 1 - Создание образа диска

Создайте образ диска, открыв Терминал и выполнив следующую команду: 
    
    
    hdiutil create -volname MyApp -srcfolder /path/to/folder -ov -format UDBZ MyApp.dmg
    

### Шаг 2 - Код для подписи образа диска

Код подписывает образ диска, открыв Терминал и выполнив следующую команду: 
    
    
    codesign -s "Developer ID Application: Your Name (Team ID)" --timestamp MyApp.dmg
    

### Шаг 3. Создание пароля для конкретного приложения

Чтобы сгенерировать пароль для конкретного приложения, обратитесь к этой [статье службы поддержки Apple](<https://support.apple.com/en-us/HT204397>). 

[![Note-icon.png](https://wiki.freepascal.org/images/b/be/Note-icon.png)](</File:Note-icon.png>)

**Примечание:** это пароль, который будет специфическим для приложения нотариального заверения (xcrun altool), а не для нотариально заверенного приложения. Поэтому вам нужно сделать это только один раз, но убедитесь, что вы скопировали сгенерированный пароль и сохранили его где-нибудь.

  


[![Note-icon.png](https://wiki.freepascal.org/images/b/be/Note-icon.png)](</File:Note-icon.png>)

**Подсказка:** Сохраните пароль в связке ключей с помощью:   
`xcrun altool -u "<your email>" -p "<app-specific pwd>" --store-password-in-keychain-item "<name for pwd>"`   
  
А затем вы можете использовать `"@keychain:<name for pwd>"` вместо `"<app-specific pwd>"` в следующих командах.

### Шаг 4 - Загрузка образа диска в нотариальную службу

[![Note-icon.png](https://wiki.freepascal.org/images/b/be/Note-icon.png)](</File:Note-icon.png>)

**Примечание:** Когда вы нотариально заверяете образ диска контейнера, altool также нотариально заверяет приложение внутри, поэтому вы можете пропустить этап нотариального заверения самого приложения.

  
Загрузите файл образа диска в нотариальную службу Apple, открыв Терминал и выполнив следующую команду: 
    
    
    xcrun altool --notarize-app --primary-bundle-id "<your identifier>" -u "<your email>" -p "<app-specific pwd>" -t osx -f /path/to/MyApp.dmg
    

_primary-bundle-id_ помогает отслеживать автоматическую корреспонденцию от службы нотариального заверения. Указанное вами значение не обязательно должно совпадать с идентификатором пакета отправленного приложения или иметь какое-либо конкретное значение. Это должно иметь смысл только для вас. Служба нотариального заверения включает значение всякий раз, когда она отправляет вам электронное письмо относительно данного `altool` представления. 

Если загрузка прошла успешно, вы должны получить следующий результат: 
    
    
    No errors uploading 'MyApp.dmg'.
    RequestUUID = 3af4e56f-162b-75bc-827f-7233f92bf20c
    

### Шаг 5 - Проверка процесса нотариального заверения

Процесс нотариального заверения обычно занимает менее часа, поэтому вы можете время от времени проверять его ход, открывая Терминал и выполняя следующую команду: 
    
    
    xcrun altool --notarization-history 0 -u "<your email>" -p "<app-specific pwd>"
    

Когда процесс нотариального заверения завершится успешно, вышеуказанная команда вернет информацию, подобную следующей: 
    
    
    Notarization History - page 0
    
    Date                      RequestUUID                          Status  Status Code Status Message   
    ------------------------- ------------------------------------ ------- ----------- ---------------- 
    2019-12-08 06:24:03 +0000 3af4e56f-162b-75bc-827f-7233f92bf20c success 0           Package Approved 
    
    Next page value: 1575786243000
    

Вы также должны получить электронное письмо от Apple, подобное приведенному ниже, в случае успешного нотариального заверения: 
    
    
    Dear <First_Name>,
    
    Your Mac software has been notarized. You can now export this software and distribute it directly to users.(rus: Программное обеспечение вашего Mac было нотариально заверено. Теперь вы можете экспортировать это программное обеспечение и распространять его непосредственно среди пользователей.)
    
    Bundle Identifier: com.example.MyApp.001
    Request Identifier: 3af4e56f-162b-75bc-827f-7233f92bf20c
    
    For details on exporting a notarized app, visit Xcode Help or the notarization guide.(rus: Для получения дополнительных сведений об экспорте нотариально заверенного приложения посетите справку Xcode или руководство по нотариальному заверению.)
    
    Best Regards,
    Apple Developer Relations
    

### Шаг 6 - Прикрепление удостоверения к образу диска

В процессе нотариального заверения создается удостоверение, которое сообщает Gatekeeper, что ваше приложение нотариально заверено. После успешного завершения нотариального заверения в следующий раз, когда любой пользователь попытается запустить ваше приложение в macOS 10.14 или более поздней версии, Gatekeeper найдет удостоверение в Интернете. Сюда входят пользователи, которые загрузили ваше приложение до нотариального заверения. 

После того, как на шаге 5 будет получено сообщение о статусе «Пакет одобрен», вы также должны прикрепить удостоверение к файлу образа диска с помощью инструмента прикрепления, чтобы в будущие дистрибутивы включили билет. Это гарантирует, что Gatekeeper сможет найти удостоверение, даже если сетевое соединение недоступно. 

Чтобы прикрепить удостоверение к файлу образа диска, откройте Терминал и выполните следующую команду: 
    
    
    xcrun stapler staple /path/to/MyApp.dmg
    

Если команда завершится успешно, вывод должен быть похож на: 
    
    
    Processing: /Path/to/MyApp.dmg
    Processing: /Path/to/MyApp.dmg
    The staple and validate action worked!
    

### Шаг 7 - Подтверждение нотариального заверения образа диска

Чтобы подтвердить нотариальное заверение образа диска, откройте Терминал и выполните следующую команду: 
    
    
    spctl -a -vv -t install MyApp.dmg
    

Успешная проверка процесса нотариального заверения должна привести к примерно следующему результату: 
    
    
    MyApp.dmg: accepted
    source=Notarized Developer ID
    origin=Developer ID Application: <Developer Name> (<TEAM_ID>)
    

### Шаг 8 - Подтвердить нотариальное заверение приложения

Чтобы подтвердить нотариальное заверение приложения, установите приложение, откройте Терминал и выполните следующую команду: 
    
    
    spctl -a -vv /Applications/MyApp.app
    

Успешное подтверждение процесса нотариального заверения должна привести к примерно следующему результату: 
    
    
    /Applications/MyApp.app: accepted
    source=Notarized Developer ID
    origin=Developer ID Application: <Developer Name> (<TEAM_ID>)
    

**Важно:** протестируйте приложение, поместив его в каталог Applications. Оно по-разному обрабатывается Gatekeeper'ом, когда находится в "установленном" месте. 

В качестве альтернативы, рекомендуемый Apple способ проверить нотариальное заверение приложения - открыть Терминал и выполнить следующую команду: 
    
    
    xcrun stapler validate MyApp.app
    

Успешная верификация процесса нотариального заверения должна привести к примерно следующему результату: 
    
    
    Processing: MyApp.app
    The validate action worked!
    

## Нотариальное заверение инструмента командной строки

Инструменты командной строки могут быть подписаны кодом, но их нелегко нотариально заверить, потому что у них нет [Application Bundle](<../en/Application_Bundle.md> "Application Bundle") и отдельного `Info.plist` [property list file](<../en/macOS_property_list_files.md> "macOS property list files"). Вы можете относительно легко вставить файл `Info.plist` в исполняемый файл инструмента командной строки. 

### Вставка Info.plist в исполняемый файл командной строки

Вот шаблон, который вы можете настроить для своей утилиты командной строки. Я извлек его из исполняемого файла утилиты командной строки, написанного на C и названного "simplereach", который я создал с помощью Xcode 11.3.1 в Mojave после включения "Create Info.plist Section in Binary"(Создать раздел Info.plist в двоичном формате) в параметрах "Packaging"(Упаковка) (я пропустил ненужные Xcode-related build info). 
    
    
    <!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
    <plist version="1.0">
    <dict>
    	<key>BuildMachineOSBuild</key>
    	<string>18G8022</string>
    	<key>CFBundleDevelopmentRegion</key>
    	<string>English</string>
    	<key>CFBundleDocumentTypes</key>
    	<array>
    		<dict>
    			<key>CFBundleTypeOSTypes</key>
    			<array>
    				<string>fold</string>
    				<string>disk</string>
    				<string>****</string>
    			</array>
    		</dict>
    	</array>
    	<key>CFBundleExecutable</key>
    	<string>simplereach</string>
    	<key>CFBundleIdentifier</key>
    	<string>com.mycompany.simplereach</string>
    	<key>CFBundleInfoDictionaryVersion</key>
    	<string>6.0</string>
    	<key>CFBundleName</key>
    	<string>simplereach</string>
    	<key>CFBundlePackageType</key>
    	<string>APPL</string>
    	<key>CFBundleShortVersionString</key>
    	<string>1.0</string>
    	<key>CFBundleSupportedPlatforms</key>
    	<array>
    		<string>MacOSX</string>
    	</array>
    	<key>CFBundleVersion</key>
    	<string>1</string>
    	<key>LSMinimumSystemVersion</key>
    	<string>10.14</string>
    </dict>
    </plist>
    

Вам нужно будет изменить несколько ключевых строк в приведенном выше файле. Замените свой: 

  * **BuildMachineOSBuild** строку можно получить из Терминала с помощью команды `sw_vers`. '18G8022' предназначена строго для Mojave 10.14.6.
  * **CFBundleDevelopmentRegion** строка, если она не "английская".
  * **CFBundleExecutable** строка с названием вашей утилиты.
  * **CFBundleIdentifier** строка (обычно ваше доменное имя в обратном порядке, за которым следует имя утилиты; не стесняйтесь использовать что-нибудь уникальное).
  * **CFBundleName** строка (используйте то же значение, что и для CFBundleExecutable).
  * **CFBundleShortVersionString** строка с номером вашей версии.
  * **LSMinimumSystemVersion** строка с минимальной версией macOS, на которой будет работать ваша утилита.



Затем вы можете вставить файл `Info.plist` в свое приложение, указав компоновщику добавить его как раздел __TEXT с именем __info_plist: 
    
    
    fpc -k"-sectcreate __TEXT __info_plist Info.plist" my_utility.pas
    

Чтобы проверить, успешно ли он вставлен, вы можете запустить следующую команду в Терминале: 
    
    
    otool -X -v -s __TEXT __info_plist my_utility
    

### Подпись исполняемого файла командной строки

Теперь, когда файл `Info.plist` был вставлен в исполняемый файл, вы готовы подписать его кодом, выполнив следующие команды в Терминале: 
    
    
    cd /path/to/my_utility
    mkdir bin
    mv my_utility bin/
    codesign -f --strict --timestamp --options=runtime -s "Developer ID Application: YOUR NAME (TEAM_ID)" bin/my_utility
    

_Критично_ , чтобы вы создали каталог `bin` и переместили в него свою утилиту перед подписанием. В противном случае создастся каталог `_CodeSignature`, который не подойдет для утилиты командной строки, и процесс нотариального заверения не состоится. 

Чтобы убедиться, что ваши усилия увенчались успехом, вы можете запустить следующую команду в Терминале: 
    
    
    codesign -vv bin/my_utility
    

Если подписание кода прошло успешно, результат должен быть примерно таким: 
    
    
    my_utility: valid on disk
    my_utility: satisfies its Designated Requirement
    

### Отправка исполняемого файла командной строки на нотариальное заверение

Поскольку вы не можете нотариально заверить пустой исполняемый файл, вам необходимо включить его в контейнер (.dmg - образ диска, .pkg - пакет установщика или файл архива ZIP). ZIP-архив является самым простым, поэтому в Терминале выполните следующую команду: 
    
    
    zip my_utility.zip bin/my_utility
    

Теперь вы готовы загрузить свою утилиту командной строки на серверы Apple для нотариального заверения. В терминале выполните следующую команду: 
    
    
    xcrun altool --notarize-app --primary-bundle-id "<your identifier>" -u "<your email>" -p "<app-specific pwd>" -t osx -f my_utility.zip
    

Если у вас еще нет пароля для приложения, см. [ выше](<Notarization_for_macOS_10.14.md> "Notarization for macOS 10.14.5+/ru"), чтобы узнать, как его сгенерировать. 

_Primary-bundle-id_ помогает отслеживать автоматическую корреспонденцию от службы нотариального заверения. Указанное вами значение не обязательно должно совпадать с идентификатором пакета отправленного приложения или иметь какое-либо конкретное значение. Это должно иметь смысл только для вас. Услуга нотариального заверения включает значение всякий раз, когда она отправляет вам электронное письмо относительно данной альтернативной заявки. 

Если загрузка прошла успешно, вы должны получить следующий результат: 
    
    
    No errors uploading 'my_utility'.
    RequestUUID = 3af4e56f-162b-75bc-827f-7233f92bf20c
    

### Проверка процесса нотариального заверения

Процесс нотариального заверения обычно занимает от 10 до 15 минут, поэтому вы можете время от времени проверять его ход, выполняя следующую команду из Терминала: 
    
    
    xcrun altool --notarization-history 0 -u "<your email>" -p "<app-specific pwd>"
    

Когда процесс нотариального заверения завершится успешно, вышеуказанная команда вернет информацию, подобную следующей: 
    
    
     Notarization History - page 0
    
     Date                      RequestUUID                          Status  Status Code Status Message   
     ------------------------- ------------------------------------ ------- ----------- ---------------- 
     2021-04-22 10:55:03 +0000 3af4e56f-162b-76bc-826f-7263f92bf20c success 0           Package Approved 
    
     Next page value: 1575786243000
    

Вы также должны получить электронное письмо от Apple, подобное приведенному ниже, в случае успешного нотариального заверения: 
    
    
     Dear <First Name>,
    
     Your Mac software has been notarized. You can now export this software and distribute it directly to users.(rus: Программное обеспечение вашего Mac было нотариально заверено. Теперь вы можете экспортировать это программное обеспечение и распространять его непосредственно среди пользователей.)
    
     Bundle Identifier: 
     Request Identifier: 70dbe301-d3a7-4931-9ec3-61c5cc80573c
    
     For details on exporting a notarized app, visit Xcode Help or the notarization guide.(rus: Для получения дополнительных сведений об экспорте нотариально заверенного приложения посетите справку Xcode или руководство по нотариальному заверению.) 
    
     Best Regards,
     Apple Developer Relations
    

В случае неудачного нотариального заверения обратитесь за помощью к разделу [Устранение неполадок](<Notarization_for_macOS_10.14.md> "Notarization for macOS 10.14.5+/ru") ниже. 

### Нотариальное заверение исполняемого файла командной строки

Чтобы проверить нотариальное заверение инструмента командной строки, откройте Терминал и выполните следующую команду: 
    
    
    spctl -a -t open --context context:primary-signature -vv my_utility 
    

Успешная проверка правильно нотариально заверенного исполняемого файла командной строки должна привести к выводу, подобному следующему: 
    
    
    my_utility: accepted
    source=Notarized Developer ID
    origin=Developer ID Application: Your Name (Z9HBGNDZZ9)
    

## Устранение неполадок

### Процесс загрузки нотариального заверения

Если процесс загрузки нотариального заверения не проходит должным образом, вы сможете найти сжатый файл журнала во временном каталоге, который может пролить свет на то, что произошло. Вы можете просмотреть его, открыв Терминал и выполнив следующие команды (замените имя вашего файла журнала, который включает идентификатор пакета вашего приложения): 
    
    
      cd ${TMPDIR}
      gzcat c51ca05f-cf49-405f-be57-26873faab5f6_3073718126753816760org.sentry.myapp-20191231004646-273.itmsp6022059143057318377.log
    

который для успешной загрузки нотариального заверения должен выглядеть примерно так: 
    
    
     [2019-12-31 19:46:46 AEDT] <main>  INFO: Starting upload for packageName: org.sentry.myapp-20191231004646-273.itmsp
     [2019-12-31 19:46:46 AEDT] <main>  INFO: Thread Pool Configuration: {processors=12, perCoreThreadMultiplier=1.65, coreThreads=6, maxThreads=14, minConnections=6, maxConnections=13, rateLimit=100000000000 KBs}
     [2019-12-31 19:46:46 AEDT] <main>  INFO: Reservation CPUSource113/v4/93/41/ce/9341ce96-e7bc-2577-eaac-4528c888a70d/metadata.xml created for file: (metadata.xml), part count:(1), file size: (538)
     [2019-12-31 19:46:46 AEDT] <main>  INFO: Reservation CPUSource113/v4/93/41/ce/9341ce96-e7bc-2577-eaac-4528c888a70d/MyApp_31122019.dmg created for file: (MyApp_31122019.dmg), part count:(1), file size: (1400933)
     [2019-12-31 19:46:46 AEDT] <RLT-002>  INFO: Starting upload for reservation: (CPUSource113/v4/93/41/ce/9341ce96-e7bc-2577-eaac-4528c888a70d/metadata.xml) file: (metadata.xml/1) size: 538
     [2019-12-31 19:46:46 AEDT] <RLT-003>  INFO: Starting upload for reservation: (CPUSource113/v4/93/41/ce/9341ce96-e7bc-2577-eaac-4528c888a70d/myapp_31122019.dmg) file: (MyApp_31122019.dmg/1) size: 1400933
     [2019-12-31 19:46:47 AEDT] <RLT-002>  INFO: Finished part upload for: (metadata.xml/1) 2.91 KB/s in 0 seconds
     [2019-12-31 19:46:47 AEDT] <RLT-002>  INFO: Finished upload for reservation: (CPUSource113/v4/93/41/ce/9341ce96-e7bc-2577-eaac-4528c888a70d/metadata.xml) file: (metadata.xml)
     [2019-12-31 19:46:47 AEDT] <pool-8-thread-1>  INFO: Reservation committed: (CPUSource113/v4/93/41/ce/9341ce96-e7bc-2577-eaac-4528c888a70d/metadata.xml) file: (metadata.xml)
     [2019-12-31 19:46:49 AEDT] <RLT-003>  INFO: Finished part upload for: (MyApp_31122019.dmg/1) 781.33 KB/s in 1 second
     [2019-12-31 19:46:49 AEDT] <RLT-003>  INFO: Finished upload for reservation: (CPUSource113/v4/93/41/ce/9341ce96-e7bc-2577-eaac-4528c888a70d/MyApp_31122019.dmg) file: (MyApp_31122019.dmg)
     [2019-12-31 19:46:49 AEDT] <main>  INFO: Waiting on reservation commit(s) to finish.
     [2019-12-31 19:46:49 AEDT] <pool-8-thread-1>  INFO: Reservation committed: (CPUSource113/v4/93/41/ce/9341ce96-e7bc-2577-eaac-4528c888a70d/MyApp_31122019.dmg) file: (MyApp_31122019.dmg)
     [2019-12-31 19:46:49 AEDT] <pool-8-thread-1>  INFO: All reservation commit(s) finished.
     [2019-12-31 19:46:49 AEDT] <main>  INFO: 456.36 KB/s in 3 seconds
    

### Сбои в процессе нотариального заверения

Если при проверке вы обнаружите, что процесс нотариального заверения не удался. Например: 
    
    
    Date                      RequestUUID                          Status  Status Code Status Message   
    ------------------------- ------------------------------------ ------- ----------- ---------------- 
    2020-02-29 13:54:08 +0000 fa2e5d79-cba0-469d-bb27-859a8f081821 invalid 2           Package Invalid  
    

Вы можете получить URL-адрес файла журнала нотариального заверения с помощью команды: 
    
    
    xcrun altool --notarization-info <RequestUUID> -u "<your email>" -p "<app-specific pwd>"
    

который должен дать информацию, подобную следующей: 
    
    
    No errors getting notarization info.
    
              Date: 2020-02-29 13:54:08 +0000
              Hash: 1b08ea5cdf97eb6fed9b959a549d4b325eb5e90f16538139a29a83f75609d87b
        LogFileURL: https://osxapps-ssl.itunes.apple.com/itunes-assets/[...]
       RequestUUID: fa2e5d79-cba0-469d-bb27-859a8f081821
            Status: invalid
       Status Code: 2
    Status Message: Package Invalid
    

Затем вы можете вырезать и вставить LogFileURL в свой веб-браузер и получить файл журнала процесса нотариального заверения, который будет выглядеть примерно так: 
    
    
    {
      "logFormatVersion": 1,
      "jobId": "fa2e5d79-cba0-469d-bb27-859a8f081821",
      "status": "Invalid",
      "statusSummary": "Archive contains critical validation errors",
      "statusCode": 4000,
      "archiveFilename": "test.dmg",
      "uploadDate": "2020-02-29T13:54:08Z",
      "sha256": "1b08ea5cdf97eb6fed9b959a549d4b325eb5e90f16538139a29a83f75609d87b",
      "ticketContents": null,
      "issues": [
        {
          "severity": "error",
          "code": null,
          "path": "test.dmg/MyApp.app/Contents/MacOS/MyApp",
          "message": "The executable does not have the hardened runtime enabled.",
          "docUrl": null,
          "architecture": "x86_64"
        }
      ]
    }
    

В приведенном выше случае сбоя это произошло из-за того, что исполняемый файл не был [ с подписанной защитой](<../en/Hardened_runtime_for_macOS.md> "Hardened runtime for macOS"). 

Также стоит упомянуть, что _всегда_ стоит извлекать файл журнала нотариального заверения для проверки предупреждений, которые еще не являются ошибками, чтобы вы могли исправить их до того, как они станут ошибками, препятствующими успешному нотариальному заверению. 

Вот пример протокола процесса нотариального заверения после успешного нотариального заверения: 
    
    
    {
      "logFormatVersion": 1,
      "jobId": "fa2e5d79-cba0-469d-bb27-859a8f081822",
      "status": "Accepted",
      "statusSummary": "Ready for distribution",
      "statusCode": 0,
      "archiveFilename": "test.dmg",
      "uploadDate": "2020-03-01T01:50:55Z",
      "sha256": "1945554c24da3bb1d587b7c9f968dcbb925b726f2f663b816301928c06593dd9",
      "ticketContents": [
        {
          "path": "test.dmg",
          "digestAlgorithm": "SHA-256",
          "cdhash": "1d4d033edbecc2baf7ff8ce0fa16691be990595f"
        },
        {
          "path": "test.dmg/MyApp.app",
          "digestAlgorithm": "SHA-256",
          "cdhash": "7f9d54e76f25807e2b425a09684aa8cb54dcd17f",
          "arch": "x86_64"
        },
        {
          "path": "test.dmg/MyApp.app/Contents/MacOS/MyApp",
          "digestAlgorithm": "SHA-256",
          "cdhash": "7f9d54e762c7907e2b425a09684aa8cb54dcd17f",
          "arch": "x86_64"
        },
        {
          "path": "test.dmg/MyApp.app/Contents/Resources/MyApp.help",
          "digestAlgorithm": "SHA-256",
          "cdhash": "b544a32a216578e7805ca93b968377cdd92f7ec6",
          "arch": "i386"
        },
        {
          "path": "test.dmg/MyApp.app/Contents/Resources/MyApp.help",
          "digestAlgorithm": "SHA-256",
          "cdhash": "b544a32a216578e7805ca93b968377cdd92f7ec6",
          "arch": "x86_64"
        },
        {
          "path": "test.dmg/MyApp.app/Contents/Resources/MyApp.help",
          "digestAlgorithm": "SHA-256",
          "cdhash": "b544a32a216578e7805ca93b968377cdd92f7ec6",
          "arch": "x86_64h"
        }
      ],
      "issues": null
    }
    

### Сбои при проверке нотариального заверения

Если процесс нотариального заверения прошел успешно, но вы получили сбой при проверке успешности нотариального заверения, вы можете использовать `Console.app` для проверки файлов журнала на предмет информации об отклонении (после попытки запустить заблокированное приложение). Имейте в виду, что вы должны открыть `Console.app` _перед_ попыткой открыть ваше `Console.app`, иначе не все сообщения могут быть зарегистрированы. Вы должны выбрать свое устройство на левой боковой панели `Console.app` и проверить в журналах процесс **XprotectService**. 

## См.также

  * [Apple Developer Certificates](<../en/Apple_Developer_Certificates.md> "Apple Developer Certificates")
  * [Code Signing for macOS](<../en/Code_Signing_for_macOS.md> "Code Signing for macOS")
  * [Hardened runtime for macOS](<../en/Hardened_runtime_for_macOS.md> "Hardened runtime for macOS")
  * [Signing and Notarization FAQ](<../en/Signing_and_Notarization_FAQ.md> "Signing and Notarization FAQ")



## Внешние ссылки

  * [Apple: Notarizing your app before distribution](<https://developer.apple.com/documentation/security/notarizing_your_app_before_distribution>)
  * [Apple: Notarizing macOS Software Before Distribution](<https://developer.apple.com/documentation/xcode/notarizing_macos_software_before_distribution>)
  * [Apple: Customizing the Notarization Workflow](<https://developer.apple.com/documentation/xcode/notarizing_macos_software_before_distribution/customizing_the_notarization_workflow>)
  * [Apple: Common notarization issues](<https://developer.apple.com/documentation/xcode/notarizing_macos_software_before_distribution/resolving_common_notarization_issues>)
  * [Apple: Using app-specific passwords](<https://support.apple.com/en-us/HT204397>)
  * [Notarize FPC](<https://github.com/neurolabusc/NotarizeFPC>) A script to notarize a command line utility and package it in an installer .pkg file.

---

_Source: [https://wiki.freepascal.org/Notarization_for_macOS_10.14.5%2B/ru](https://web.archive.org/web/20221129221416/https://wiki.freepascal.org/Notarization_for_macOS_10.14.5%2B/ru)_
