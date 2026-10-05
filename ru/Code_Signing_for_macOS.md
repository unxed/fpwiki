# Code Signing for macOS

[![macOSlogo.png](https://wiki.freepascal.org/images/1/15/macOSlogo.png)](</File:macOSlogo.png>)

Эта статья относится только к [macOS](</Category:macOS> "Category:macOS").

См. также: [Multiplatform Programming Guide](<../en/Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

[![Apple iOS new.svg](https://upload.wikimedia.org/wikipedia/commons/thumb/4/48/Apple_iOS_new.svg/50px-Apple_iOS_new.svg.png)](</File:Apple_iOS_new.svg>)

Эта статья относится только к [iOS](</Category:iOS> "Category:iOS").

См. также: [Multiplatform Programming Guide](<../en/Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

│ **[English (en)](<../en/Code_Signing_for_macOS.md> "Code Signing for macOS")** │  **русский (ru)** │    
****

  


[![Note-icon.png](https://wiki.freepascal.org/images/b/be/Note-icon.png)](</File:Note-icon.png>)

**Примечание:** Для требований нотариального заверения Apple для расширений ядра и приложений, начиная с Mojave 10.14.5 (для расширений ядра от 7 апреля 2019 г. и для разработчиков, чье первое использование их сертификата подписи кода произошло с 7 апреля 2019 г.) и для всего программного обеспечения, начиная с Catalina 10.15 и выше, которое не распространяется через App Store, см. [Нотариальное заверение для macOS 10.14.5+](<Notarization_for_macOS_10.14.md> "Notarization for macOS 10.14.5+/ru")

## Contents

  * 1 Предисловие
  * 2 Процесс развития Gatekeeper
    * 2.1 Big Sur и более новые ОС на процессорах Apple M1 ARM64
  * 3 Диалоговые окна Gatekeeper
  * 4 Обзор
  * 5 Требования Gatekeeper'а
  * 6 Использование механизма codesign для подписи вашего приложения
  * 7 Использование механизма productsign для подписи установщика pkg
  * 8 Использование механизма codesign для подписи образа диска
  * 9 Обновление подписанного файла
  * 10 Подписание ad hoc кода (в особых случаях)
  * 11 Подписывание кода двоичного файла x86_64 на машине aarch64
  * 12 Как установить сертификаты на второй компьютер
    * 12.1 Использование приложения Keychain Access (Связка ключей)
    * 12.2 Использование Xcode
  * 13 Как проверить, какие сертификаты установлены
  * 14 См.также
  * 15 Внешние ссылки



## Предисловие

**[Code signing](<../en/Code_signing.md> "Code signing")** (подписание кода) обеспечивает как подлинность, так и целостность исполняемых файлов, загруженных из глобальных сетей, таких как Интернет. Приведенное ниже обсуждение в равной степени относится как к распространению в App Store, так и к распространению вне App Store. 

Подписание кода требуется в [iOS](<../en/Portal_iOS.md> "Portal:iOS"). В [macOS](<../en/Portal_Mac.md> "Portal:Mac") и более поздних версиях он позволяет открывать программы, загруженные из Интернета, без каких-либо предупреждений (если они подписаны сертификатом, выданным Apple), и требуется при использовании определенных функций (например, API-интерфейсы, используемые отладчиками; обратите внимание, что в этом случае достаточно самозаверяющего сертификата, помеченного как «доверенный»). Эту функцию выполняет программное обеспечение Apple Gatekeeper. 

## Процесс развития Gatekeeper

Gatekeeper (Привратник), впервые представленный в Mountain Lion (10.8, 2012 г.), представляет собой функцию безопасности Mac, предназначенную для защиты компьютеров Apple от вредоносного программного обеспечения. Привратник проверяет приложения по списку приложений, которые Apple одобрила для своего App Store или код которых был подписан разработчиками, имеющими сертификаты, выпущенные Apple, если приложение не предлагается через магазин приложений. Он не выполняет никаких проверок безопасности сам по себе, за исключением того, что приложение не было изменено с момента его подписания разработчиком, и не дает никаких гарантий в отношении разработчика, кроме того, что он платит Apple 99 долларов США в год (он же "Идентифицированный разработчик"). 

Первоначальные параметры Gatekeeper, представленные в Mountain Lion, доступ к которым можно получить из настроек "Безопасность" "Конфиденциальность" > "Общее", были следующими: 

  * App Store
  * App Store и признанные разработчики
  * Anywhere (кто угодно)



Выбрав опцию **Anywhere** , пользователь когда-то мог полностью отключить Gatekeeper. Настройка по умолчанию разрешала запуск приложений только из App Store или от разработчика, подписавшего свое приложение с помощью сертификата, выданного Apple. 

Когда в 2016 году была выпущена macOS Sierra, Apple внесла несколько важных изменений в Gatekeeper и ограничила параметры Gatekeeper следующим образом: 

  * App Store
  * App Store и признанные разработчики



Однако вы можете восстановить отсутствующую опцию Gatekeeper **Anywhere** в настройках, открыв терминал и выполнив команду: 

` $ sudo spctl --master-disable `

которая по-прежнему работает до macOS Catalina включительно. Лучшей, как и более безопасной, альтернативой было вместо этого обойти Gatekeeper, открыв приложение из контекстного меню по клику правой кнопки мыши, или по клику с зажатой `Ctrl`. Это по-прежнему вызывало бы диалоговое окно предупреждения, но теперь оно бы содержало кнопку **Открыть** для успешного запуска приложения. Этот метод обхода Gatekeeper по-прежнему работает в macOS Catalina, за исключением того, что в Catalina вам нужно запустить приложение во второй раз, удерживая `Ctrl` или щелкнув правой кнопкой мыши, чтобы успешно обойти Gatekeeper - при первом запуске приложения Gatekeeper не может быть обойден независимо от того, удерживаете ли вы `Ctrl` или щелкаете правой кнопкой мыши или нет. 

### Big Sur и более новые ОС на процессорах Apple M1 ARM64

И тогда Apple изменила правила игры. 

Когда Big Sur запущена на процессоре Apple M1 (ARM64), **весь нативный код ARM64 должен быть подписан, иначе операционная система не позволит его выполнение** ; и пользователи не могут обойти это новое требование Gatekeeper. Разработчики могут обойти его, выполнив следующую команду в окне терминала Applications > Utilities > Terminal: 

` spctl developer-mode enable-terminal `

и затем перейдя в System Preferences > Security & Privacy > Privacy, прокрутив вниз до записи "Developer Tools" (Инструменты разработчика), выполнив аутентификацию и установив флажок рядом с Terminal. 

Исполняемые файлы Intel, запущенные под Big Sur на процессоре Apple M1 через Rosetta 2, по-прежнему могут быть запущены, даже если они не подписаны, кликом ЛКМ с зажатой `Ctrl` или кликом ПКМ мыши для обхода Gatekeeper. 

Больше подробностей см.: [Big Sur изменения для разработчиков: Подписывание кода - Intel в сравнении с M1](<../en/macOS_Big_Sur_changes_for_developers.md> "macOS Big Sur changes for developers"). 

## Диалоговые окна Gatekeeper

  * [![](https://wiki.freepascal.org/images/9/9c/load_security_warning_dmg_leopard_en.png)](</File:load_security_warning_dmg_leopard_en.png>)

Простое предупреждение после открытия загруженного приложения в Leopard. 

  * [![](https://wiki.freepascal.org/images/8/89/load_security_warning_dmg_mavericks_en.png)](</File:load_security_warning_dmg_mavericks_en.png>)

Настройки по умолчанию более строгие в Mavericks, где они предотвращают запуск загруженных неподписанных приложений с помощью двойного щелчка мыши. 

  * [![](https://wiki.freepascal.org/images/7/72/load_security_warning_mavericks_context_en.png)](</File:load_security_warning_mavericks_context_en.png>)

Если приложение запускается через контекстное меню в Mavericks, отображается простое предупреждение, аналогичное тому, что было в более ранних версиях. 

  * [![](https://wiki.freepascal.org/images/7/7a/simple_load_security_warning_mavericks_en.png)](</File:simple_load_security_warning_mavericks_en.png>)

Gatekeeper показывает упрощенное предупреждение, если приложение с подписанным кодом было загружено и запущено в первый раз. При последующих запусках программа запускается сразу. 

  * [![](https://wiki.freepascal.org/images/9/91/codesigning.png)](</File:codesigning.png>)

Схема подписания кода программы, написанной с помощью Lazarus и Free Pascal 




## Обзор

Основные шаги для подписания приложения, написанного с помощью Lazarus и/или Free Pascal: 

  1. Получите Developer ID (идентификатора разработчика) разработчика от Apple и установите его в цепочку ключей вашей системы.
  2. Обратите внимание на буквенно-цифровой ключ вашего Developer ID (идентификатора разработчика) (он же TeamIdentifier).
  3. Подпишите свое приложение командой _codesign_.
  4. Подпишите свой установочный пакет с помощью команды _productsign_.



**Невозможно** использовать сертификаты от сторонних поставщиков, таких как Comodo, потому что они не пройдут Gatekeeper, для которого требуется сертификат, выданный разработчиком Apple. Также обратите внимание, что вы не можете подписывать приложения Windows с помощью сертификата разработчика Apple (на этот раз вам нужен сторонний сертификат Comodo и т.д.). 

Приложение macOS, распространяемое за пределами Mac App Store, обычно имеет 3–4 отдельных уровня подписи: 

  * Само приложение будет иметь подписанный код с использованием сертификата приложения.
  * Если у приложения есть установщик, файл пакета будет подписан сертификатом установщика.
  * Образ диска, содержащий приложение или установщик, будет подписан сертификатом приложения.
  * Образ диска будет [нотариально заверен](<Notarization_for_macOS_10.14.md> "Notarization for macOS 10.14.5+/ru"), а затем удостоверение, сгенерированное нотариальной службой, будет к нему [прикреплено](<Notarization_for_macOS_10.14.md> "Notarization for macOS 10.14.5+/ru").



## Требования Gatekeeper'а

  * Ваше приложение должно быть автономным, без неприемлемых внешних зависимостей. Единственными допустимыми внешними зависимостями являются системные библиотеки. Все остальные зависимости следует скопировать в папку пакета MyApp.app. Gatekeeper отклоняет любое приложение, имеющее несистемные внешние зависимости.


  * Все бинарные файлы внутри MyApp.app должны быть подписаны кодом.


  * Все бинарные файлы должны находиться в стандартных местах внутри папки пакета MyApp.app. См.таблицу ниже.

Стандартное местоположение для кода внутри бандла (пакета)  Местоположение | Описание   
---|---  
Contents | Верхний каталог содержимого пакета   
Contents/MacOS | Основной исполняемый файл; вспомогательные приложения и инструменты   
Contents/Frameworks | Фреймворки, библиотеки *.dylibs   
Contents/PlugIns | Плагины, как загружаемые, так и расширения   
Contents/XPCServices | Сервисы XPC   
Contents/Helpers | Вспомогательные приложения и инструменты   
Contents/Library/Automator | Действия автоматора   
Contents/Library/Spotlight | Импортеры Spotlight (средство поиска)   
Contents/Library/LoginItems | Устанавливаемые элементы входа   
Contents/Library/LaunchServices | Привилегированные вспомогательные инструменты, устанавливаемые инфраструктурой ServiceManagement.   
  
Примечание: Никакие недвоичные файлы никогда не должны находиться в папках, указанных в таблице выше. 

## Использование механизма codesign для подписи вашего приложения

**1.** Подпишите свое приложение с помощью: 
    
    
    codesign -f -o runtime --timestamp -s "Developer ID Application: YOUR NAME (TEAM_ID)" /path/to/bundle.app
    

**2.** Отображение основной информации о результате процесса подписания: 
    
    
    codesign -dv -r- /path/to/bundle.app
    

Если ваше приложение было успешно подписано, эта команда вернет информацию, подобную приведенной ниже: 
    
    
    Executable=/Users/apple/Desktop/your.app/Contents/MacOS/yourappname
    Identifier=org.yourdomain.yourappname
    Format=app bundle with Mach-O thin (x86_64)
    CodeDirectory v=20500 size=101470 flags=0x10000(runtime) hashes=3164+3 location=embedded
    Signature size=9063
    Timestamp=7 Dec 2019 at 18:36:08
    Info.plist entries=16
    TeamIdentifier=<10 alpha numeric digits>
    Runtime Version=10.14.0
    Sealed Resources version=2 rules=13 files=45
    designated => identifier "org.yourdomain.yourappname" and anchor apple generic and certificate 1[field.1.2.840.113635.100.6.2.6] /* exists */ and certificate leaf[field.1.2.840.113635.100.6.1.13] /* exists */ and certificate leaf[subject.OU] = <10 alpha numeric digits>
    

**3.** Подтвердите свою подпись: 
    
    
    codesign -vv /path/to/bundle.app
    

Если ваше приложение было успешно подписано, эта команда вернет строки "valid on disk"(действительно на диске) и "satisfies its Designated Requirement"(удовлетворяет назначенному требованию) соответственно после пути к вашему приложению. 

## Использование механизма productsign для подписи установщика pkg

**1.** Подпишите файл pkg установщика с помощью: 
    
    
    productsign --timestamp --sign "Developer ID Installer: YOUR NAME (TEAM_ID)" /path/to/unsigned.pkg /path/to/signed.pkg
    

**2.** Подтвердите свою подпись с помощью: 
    
    
    spctl -vv --assess --type install /path/to/your.pkg
    

который должен давать информацию, подобную этой, если все закончилось успешно: 
    
    
    your.pkg: accepted
    source=Developer ID
    origin=Developer ID Installer: YOUR NAME (<10 alpha numeric digits>)
    

## Использование механизма codesign для подписи образа диска

Начиная с macOS 10.11.5, вы можете применять подпись кода к сжатым образам дисков только для чтения, которые вы используете для распространения содержимого. В этом случае вам не нужно отдельно подписывать ваше приложение. 

**1.** Подпишите образ диска с помощью: 
    
    
    codesign -f -o runtime --timestamp -s "Developer ID Application: YOUR NAME (TEAM_ID)" /path/to/YourImage.dmg
    

**2.** Подтвердите свою подпись с помощью: 
    
    
    spctl -a -t open --context context:primary-signature -vv /path/to/YourImage.dmg
    

который должен выдать информацию, подобную этой, если все прошло успешно: 
    
    
    YourImage.dmg: accepted
    source=Developer ID
    origin=Developer ID Application: YOUR NAME (<10 alpha numeric digits>)
    

**После 3 февраля 2020 года Apple полностью вводит нотариальное заверение** для macOS 10.14.5+ (Mojave), в результате чего вышеуказанная проверка образа вашего диска теперь будет возвращать информацию, подобную этой: 
    
    
    YourImage.dmg: rejected
    source=Unnotarized Developer ID
    origin=Developer ID Application: YOUR NAME (<10 alpha numeric digits>)
    

Подробнее см. [Нотаризация для macOS 10.14.5+](<Notarization_for_macOS_10.14.md> "Notarization for macOS 10.14.5+/ru"). 

## Обновление подписанного файла

Всякий раз, когда вы обновляете подписанный файл, создавайте новый файл. Не копируйте новый подписанный файл поверх старого подписанного файла. 

В частности, информация о подписи кода (хэш каталога кода) висит на vnode в ядре, и изменение файла за этим кешем вызовет проблемы. Вам нужен новый vnode, а значит новый файл, то есть новый inode. Задокументировано в сессии 703 WWDC 2019 «Все о нотариальном заверении» — см. [слайд 65 (PDF)](<https://devstreaming-cdn.apple.com/videos/wwdc/2019/703yqvyjv9mi5ur6/703/703_all_about_notarization.pdf>). 

## Подписание ad hoc кода (в особых случаях)

Это особенно важно для процессоров Apple ARM64 M1, которые требуют, чтобы весь нативный код был подписан корректно (если только ad hoc), иначе операционная система не будет его выполнять, вместо этого убивая его при запуске. Чтобы подписать приложение ad hoc, выполните: 
    
    
     codesign --force --deep -s - lazarus.app
    

или, чтобы подписать исполняемый файл в особых случаях, выполните: 
    
    
     codesign --force -s - lazarus
    

Одиночный тире/дефис сам по себе для удостоверения делает его специальной подписью без сертификата. 

Вы можете проверить действительность подписанного исполняемого файла ad hoc с помощью: 
    
    
     codesign -dv -r- lazarus
    

который должен вывести текст, аналогичный приведенному ниже: 
    
    
    Executable=/Users/trev/lazarus-src/lazarus
    Identifier=lazarus
    Format=Mach-O thin (arm64)
    CodeDirectory v=20400 size=336137 flags=0x2(adhoc) hashes=10498+2 location=embedded
    Signature=adhoc
    Info.plist=not bound
    TeamIdentifier=not set
    Sealed Resources=none
    # designated => cdhash H"2e6f474452344d6fa42feee3db14a05c14d595a3"
    

В тех случаях, когда вы скомпилировали исполняемый файл самостоятельно, компоновщик macOS специально подпишет исполняемый файл для вас, и в этом случае вам не нужно ничего делать. Вы можете проверить действительность с помощью приведенной выше команды, и в этом случае в строке вывода CodeDirectory будет указано, что компоновщик подписал исполняемый файл специальной подписью следующим образом: 
    
    
     CodeDirectory v=20400 size=336032 flags=0x20002(adhoc,linker-signed) hashes=10498+0 location=embedded
    

## Подписывание кода двоичного файла x86_64 на машине aarch64

Архитектура подписи кода macOS поддерживает два разных формата хэшей: 

  * sha1, исходный формат хэша, который сейчас считается устаревшим
  * sha256, новый формат, поддержка которого была добавлена ​​в macOS 10.11



`Codesign` должен выбрать формат подписи в зависимости от цели развертывания: 

  * Если целью развертывания является ОС 10.11 или более поздняя версия, вы получаете sha256.
  * Если ваша цель развертывания ОС более ранняя, вы получите как sha1, так и sha256.



Проблема возникает из-за того, что при сборке для Intel и Apple Silicon цели развертывания разные. Вы можете установить цель развертывания на 10.9, но в Apple Silicon она повышена до минимальной системы Apple Silicon, 11.0. Итак, какую цель развертывания оно выбирает? 

Вобщем, полный ответ на этот вопрос сложен, но краткое изложение заключается в том, что оно выбирает цель развертывания текущей архитектуры, то есть Intel, если вы собираете на Intel, и Apple Silicon, если вы собираете на Apple Silicon. 

В результате у вас возникают проблемы, если ваша цель развертывания меньше 10.11 и вы подписываетесь на Apple Silicon. Когда вы запускаете, скажем, macOS 10.10, система ищет хэш sha1, не находит и жалуется. 

Обходной путь заключается в предоставлении аргумента `--digest-algorithm=sha1,sha256` для `codesign`, который переопределяет логику выбора хэша в `codesign` и заставляет его включать оба хэша. 

## Как установить сертификаты на второй компьютер

Есть два способа сделать это: 

  1. с помощью приложения Keychain Access (Связка ключей)
  2. с помощью Xcode.



### Использование приложения Keychain Access (Связка ключей)

  * На первом компьютере с сертификатами откройте приложение Keychain Access и создайте новую связку ключей. Теперь выберите существующую связку ключей для входа, а затем My Certificates (мои сертификаты). Ваши сертификаты должны отображаться справа. С левой стороны должна быть стрелка раскрытия, и ее выбор должен показать ваш закрытый ключ. Выберите каждый сертификат, скопируйте и вставьте (не перетаскивайте, иначе вы удалите его из цепочки для ключей входа) в новую цепочку для ключей.


  * Скопируйте только что созданную цепочку для ключей с первого компьютера на второй компьютер, откройте ее с помощью приложения Keychain Access и скопируйте/вставьте/перетащите сертификат в цепочку для входа на второй компьютер.



### Использование Xcode

  * На первом компьютере с сертификатами откройте Xcode и перейдите в Preferences > Accounts (Настройки > Учетные записи), выберите свою учетную запись, выберите значок шестеренки в левом нижнем углу, выберите Export Apple ID (Экспорт Apple ID) и Code Signing Assets (Активы для подписи кода), а затем сохраните файл.


  * Скопируйте файл профиля разработчика, который вы сохранили на своем первом компьютере, на второй компьютер, откройте Xcode, перейдите в Preferences > Accounts (Настройки > Учетные записи), выберите свою учетную запись, выберите символ шестеренки в левом нижнем углу, а затем выберите Import Apple ID (Импортировать Apple ID) и Code Signing Assets (Активы для подписи кода).



## Как проверить, какие сертификаты установлены

После того, как вы создали или импортировали сертификаты на свой компьютер, вы можете проверить их наличие, открыв терминал Applications > Utilities > Terminal и выполнив следующую команду: 
    
    
    $ security find-identity -p basic -v
    

Эта команда выведет список всех доступных сертификатов на этом компьютере. Убедитесь, что вы видите наличие _Developer ID Application_ и _Developer ID Installer_. Если вы являетесь участником нескольких команд, вы можете увидеть несколько сертификатов для каждой команды. 

Вы можете идентифицировать сертификаты по шестнадцатеричному номеру или описательному имени, например, "Developer ID Application: Your Name (AB123456DE)". 

Десятизначный буквенно-цифровой код в конце вашего имени — это Developer Team ID (идентификатор вашей команды разработчиков). Если вы являетесь членом нескольких команд разработчиков, у вас будет несколько сертификатов, и идентификатор команды поможет вам отличить их. 

## См.также

  * [Подписание кода: Intel в сравнеии Apple Silicon](<../en/macOS_Big_Sur_changes_for_developers.md> "macOS Big Sur changes for developers")
  * [Apple Developer Certificates](<../en/Apple_Developer_Certificates.md> "Apple Developer Certificates")
  * [Notarization for macOS 10.14.5+](<../en/Notarization_for_macOS_10.14.md> "Notarization for macOS 10.14.5+")
  * [Hardened runtime for macOS](<../en/Hardened_runtime_for_macOS.md> "Hardened runtime for macOS")
  * [Sandboxing for macOS](<../en/Sandboxing_for_macOS.md> "Sandboxing for macOS")
  * [Signing and Notarization FAQ](<../en/Signing_and_Notarization_FAQ.md> "Signing and Notarization FAQ")
  * [macOS Gatekeeper Debugging Tips](<../en/macOS_Gatekeeper_Debugging_Tips.md> "macOS Gatekeeper Debugging Tips")



## Внешние ссылки

  * [Apple Developer portal](<https://developer.apple.com/>)
  * [Apple: Code Signing Overview](<https://developer.apple.com/support/technical/code-signing/>)
  * [Apple: Code Signing Guide](<https://developer.apple.com/library/mac/documentation/Security/Conceptual/CodeSigningGuide/>)
  * [Apple: TN2206 - macOS Code Signing In Depth](<https://developer.apple.com/library/archive/technotes/tn2206/_index.html>)
  * [Apple: Code signing x86_64 binaries on aarch64](<https://developer.apple.com/forums/thread/673323>).

---

_Source: [https://wiki.freepascal.org/Code_Signing_for_macOS/ru](https://web.archive.org/web/20240101000000/https://wiki.freepascal.org/Code_Signing_for_macOS/ru)_
