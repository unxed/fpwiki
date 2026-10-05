# LAMW

│ **[English (en)](<../en/LAMW.md>)** │  **русский (ru)** │

[![Android robot.svg](https://upload.wikimedia.org/wikipedia/commons/thumb/d/d7/Android_robot.svg/50px-Android_robot.svg.png)](</File:Android_robot.svg>)

Эта статья относится только к [Android](</Category:Android> "Category:Android").

См. также: [Multiplatform Programming Guide](<../en/Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Примечание:** Эта страница взята из предыдущего урока HOW_TO_INSTALL_by_Dieselnutjob.pdf (для Windows). Теперь это вики, вы можете обновлять его, когда уверены, что оно работает и/или помогает.

**LAMW** \- это набор _мастеров Lazarus_ , который вы запускаете из меню Build и Tools. 

Он помогает устанавливать параметры, организовывать библиотеки и запускать инструменты, необходимые для создания APK-файлов, используемых для установки приложений на Android. 

Эта установка включает в себя несколько пакетов, содержащих компоненты, полезные для разработки программ Android с помощью Lazarus с использованием JNI. 

## Contents

  * 1 Как установить на Linux
  * 2 Как установить на Windows
    * 2.1 Загрузка
    * 2.2 Получение исходников
      * 2.2.1 Получение с помощью git-клиента
      * 2.2.2 Получение с помощью svn-клиента
      * 2.2.3 Распаковка из архива
    * 2.3 Установки
    * 2.4 Обновление наборов разработчиков Java
    * 2.5 Сборка и подготовка Laz4Android
    * 2.6 Настройки путей
    * 2.7 Создание своего первого проекта
  * 3 Другие демки



## Как установить на Linux

См. [LAMW на Linux](<../en/LAMW_on_Linux.md> "LAMW on Linux")

## Как установить на Windows

Это работает на Windows 8 и 10 Home Edition 64-бит. 

### Загрузка

Создайте папку C:\lamw and C:\lamw\projects 

Загрузите эти файлы в папку C:\lamw : 

  * [SDK 24.4.1](<http://dl.google.com/android/android-sdk_r24.4.1-windows.zip>)
  * [NDK r10e](<http://dl.google.com/android/repository/android-ndk-r10e-windows-x86.zip>)
  * [Ant 1.9.6](<http://archive.apache.org/dist/ant/binaries/apache-ant-1.9.6-bin.zip>)
  * [jdk-8u65-windows-i586.exe Jdk 8u65](<http://download.oracle.com/otn-pub/java/jdk/8u65-b14/jdk-8u65-windows-i586.exe>)
  * [Laz4Android 1.5-50093-FPC3.1.1](<https://sourceforge.net/projects/laz4android/files/laz4android1.5-50093-FPC3.1.1.7z/download>)



[![lamw download.png](https://wiki.freepascal.org/images/7/75/lamw_download.png)](</File:lamw_download.png>)

**Важные замечания :**

Версии, указанные здесь, в настоящее время работают, хотя некоторые из них являются старыми и даже скрыты их создателями/издателями. 

Но последние стабильные версии не всегда совместимы с инструментами (по крайней мере, NDK). 

Если более свежая стабильная версия работает хорошо, обновите Wiki. 

Возможно, у вас уже есть наборы, установленные где-то еще на вашем компьютере. Это может привести к проблемам, если вы попытаетесь использовать их заново. Безопаснее делать новые загрузки в c:\lamw. Помните об использовании соответствующих версий и не используйте ярлыки, ранее установленные с другими версиями. 

### Получение исходников

Вы должны получить исходники LAMW, используя один из 3 методов, перечисленных ниже. Они содержат мастера настройки и 3 пакета. 

#### Получение с помощью git-клиента

Здесь мы используем Git Gui, но вы можете использовать и другие более простые инструменты, такие как Tortoise GIT. 

  1. создайте папку _C:\lamw\lazandroidmodulewizard_
  2. щелкните правой кнопкой мыши на этой папке
  3. нажмите "GIT Gui here"
  4. нажмите "Clone existing repository" (Клонировать существующее хранилище) и введите <https://github.com/jmpessoa/lazandroidmodulewizard> в поле ввода "Source location".
  5. выберите папку _C:\lamw\lazandroidmodulewizard_



[![lamw git checkout.png](https://wiki.freepascal.org/images/3/34/lamw_git_checkout.png)](</File:lamw_git_checkout.png>)

#### Получение с помощью svn-клиента

Посредством Tortoise SVN 

    <https://github.com/jmpessoa/lazandroidmodulewizard> в папку _C:\lamw\lazandroidmodulewizard_

[![lamw svn checkout 1.png](https://wiki.freepascal.org/images/f/f3/lamw_svn_checkout_1.png)](</File:lamw_svn_checkout_1.png>)

[![lamw svn checkout 2.png](https://wiki.freepascal.org/images/8/81/lamw_svn_checkout_2.png)](</File:lamw_svn_checkout_2.png>)

#### Распаковка из архива

Альтернативным способом является загрузка этого [zip-файла](<https://github.com/jmpessoa/lazandroidmodulewizard/archive/master.zip>) в папку _c:\lamw_ и распаковка (по умолчанию) в папку _C:\lamw\lazandroidmodulewizard_. 

Вам придется делать это часто, потому что часто происходят обновления LAMW, и у вас в этом случае не будет под рукой какого-нибудь менеджера обновления исходного кода для быстрой проверки обновлений или участия в проекте. 

### Установки

Распакуйте android-sdk_r24.4.1-windows.zip в c:\lamw\android-sdk-windows 

Распакуйте android-ndk-r10e-windows-x86.zip в c:\lamw\android-ndk-r10e-windows-x86 

Распакуйте apache-ant-1.9.6-bin.zip в c:\lamw\apache-ant-1.9.6 

Распакуйте laz4android1.5-50093-FPC3.1.1.7z в C:\lamw\Laz4Android 

Распакуйте и запустите jdk-8u65-windows-i586.exe . Это установит JDK в C:\Program Files (x86)\Java\jdk1.8.0_65\ 

[![lamw java sdk install.png](https://wiki.freepascal.org/images/a/a4/lamw_java_sdk_install.png)](</File:lamw_java_sdk_install.png>)

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Примечание:** В зависимости от инструмента, который вы используете для распаковки zip или 7z, убедитесь, что у вас первая папка не дублируется дважды. Например, такое дублирование c:\lamw\_android-ndk-r10e-windows-x86_ \_android-ndk-r10e-windows-x86_ приведет к ошибкам на последующих этапах ...

### Обновление наборов разработчиков Java

Запустите C:\lamw\android-sdk-windows\SDK Manager.exe 

  1. cнимите выделение Android N (API 23 N Preview)
  2. cнимите выделение Android 6.0 (API 23)
  3. отметьте Android SDK Platform-tools
  4. отметьте Android SDK Build-tools
  5. отметьте Extras/Android Support Library [если найден...]
  6. отметьте Extras/Google USB Driver
  7. нажмите Install 4 packages (Установить 4 пакета) [3 пакета, если библиотека поддержки Android не найдена]
  8. примите условия лицензии и нажмите Install (Установить)
  9. подождите, пока закончится загрузка
  10. выйдите из SDK Manager



[![lamw sdk manager 1.png](https://wiki.freepascal.org/images/0/07/lamw_sdk_manager_1.png)](</File:lamw_sdk_manager_1.png>)

* * *

Запустите C:\lamw\android-sdk-windows\SDK Manager.exe снова 

  1. cнимите выделение Android N (API 23 N Preview)
  2. cнимите выделение Android 6.0 (API 23)
  3. expand Android 4.2.2 (API 17)
  4. отметьте SDK Platfrom
  5. отметьте ARM EABI v7a System Image
  6. нажмите Install 2 packages (установить 2 пакета)
  7. примите условия лицензии и нажмите Install (Установить)
  8. подождите, пока закончится загрузка
  9. выйдите из SDK Manager



[![lamw sdk manager 2.png](https://wiki.freepascal.org/images/e/ea/lamw_sdk_manager_2.png)](</File:lamw_sdk_manager_2.png>)

### Сборка и подготовка Laz4Android

Laz4Android - это просто экземпляр Lazarus. 

1\. Под Windows, запустите командное окно, затем: 
    
    
    cd \lamw\Laz4Android
    build.bat
    

[![lamw cmd laz4android 1.png](https://wiki.freepascal.org/images/0/0a/lamw_cmd_laz4android_1.png)](</File:lamw_cmd_laz4android_1.png>)

[![lamw cmd laz4android 2.png](https://wiki.freepascal.org/images/6/6f/lamw_cmd_laz4android_2.png)](</File:lamw_cmd_laz4android_2.png>)

2\. Когда процесс закончится, выйдите из командного окна 

3\. Создайте ярлык на рабочем столе для C:\lamw\laz4android\startlazarus.exe 

4\. Запустите его 

5\. Запустите IDE 

* * *

6\. Package → Open package file (.lpk) 

7\. Выберите C:\lamw\lazandroidmodulewizard\trunk\tfpandroidbridge_pack.lpk 

8\. Скомпилируйте 

9\. Use → Install 

10\. Вы хотите пересобрать Lazarus сейчас? Да 

Laz4Android перезапустится 

* * *

11\. Package → Open package file (.lpk) 

12\. Выберите C:\lamw\lazandroidmodulewizard\trunk\lazandroidwizardpack.lpk 

13\. Скопилируйте 

14\. Use → Install 

15\. Вы хотите пересобрать Lazarus сейчас? Да 

Laz4Android перезапустится 

* * *

16\. Package → Open package file (.lpk) 

17\. Выберите C:\lamw\lazandroidmodulewizard\trunk\ide_tools\amw_ide_tools.lpk 

18\. Скопилируйте 

19\. Use → Install 

20\. Вы хотите пересобрать Lazarus сейчас? Да 

Laz4Android перезапустится 

### Настройки путей

В Laz4Android: 

Tools (Сервис) -> [LAMW] Android Module Wizard -> Path settings (Настройки путей) 

    Путь к Java JDK: C:\Program Files (x86)\Java\jdk1.8.0_65
    Путь к Ant bin: C:\lamw\apache-ant-1.9.6\bin
    Путь к Android SDK: c:\lamw\android-sdk_r24.4.1-windows
    Путь к Android NDK: C:\lamw\android-ndk-r10e
    NDK version: 10e
    Путь к шаблонам Simonsay: C:\lamw\lazandroidmodulewizard\trunk\java

[![lamw path settings.png](https://wiki.freepascal.org/images/1/1a/lamw_path_settings.png)](</File:lamw_path_settings.png>)

OK. 

File -> Restart 

### Создание своего первого проекта

В Laz4Android : 

Project(Проект) → Close project(Закрыть проект) [если необходимо] 

Project Wizard (Мастер проекта) → New project (Новый проект) 

Выбираем JNI Android Module [Lamw GUI] 

Жмем _OK_

[![lamw create new project.png](https://wiki.freepascal.org/images/1/1d/lamw_create_new_project.png)](</File:lamw_create_new_project.png>)

    Path to Workspace (Путь к рабочему пространству) [Projects Folder(Папка проекта)]: C:\lamw\projects
    New Project Name (Имя нового проекта): LamwGUIProject1
    Default Package Name(Имя пакета по умолчанию): org.lamw
    Include (включать в себя) "android-support-v4.jar": no (нет)
    NDKPlatfom: 17
    MinSdkAPI: 17
    TargetSdkApi: 17
    Instruction: ARMv7a
    ARM Fpu: Soft
    Android Theme: DeviceDefault

[![lamw configure project.png](https://wiki.freepascal.org/images/b/b4/lamw_configure_project.png)](</File:lamw_configure_project.png>)

Сохраняем unit1.pas 

Жмем _Save_

Теперь, если навести указатель мыши на какой-либо модуль в разделе uses, в среде IDE появится сообщение об ошибке. 

File (Файл) → Restart (Перезапустить) 

После перезапуска, если вы наведете курсор мыши на какой-либо модуль в разделе uses, IDE больше не отображает сообщение об ошибке. 

Перетащите _jButton_ из Android Bridge на форму. 

Выберите событие OnClick, дважды кликните по нему и добавьте: 
    
    
    procedure TAndroidModule1.jButton1Click( Sender: TObject );
    begin
      ShowMessage('Hello World');
    end;
    

File (Файл) → Save All (Сохранить все) 

Run (Запуск) → Build (Собрать) 

Target(Цель): C:\lamw\projects\LamwGUIProject1\libs\armeabi\libcontrol.so: Success (Успешно) 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Примечание:** Последний шаг _Build_ (Собрать) не требуется, потому что он будет выполнен следующим шагом. Однако это может помочь диагностировать проблему.

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Примечание:** Перед следующим шагом убедитесь, что ваше устройство Android подключено к ПК с помощью USB-кабеля.

Run (Запуск) → [Lamw] Build APK and run (Собрать APK и запустить) 

Building APK...(Сборка APK...): Success (Успешно) 

Если ADB(Android Debug Bridge) работает c вашим телефоном, APK будет скопирован, установлен и запущен автоматически! 

В противном случае скопируйте C:\lamw\projects\LamwGUIProject1\bin\LamwGUIProject1-debug.apk в какую-то папку устройства Android и установите этот файл с помощью файлового менеджера устройства. 

## Другие демки

Вы найдете множество проектов, которые помогут вам с доступными функциями, в C:\lamw\trunk\demos\Eclipse. 

Вы должны: 

  1. Открыть файл проекта <project_name>/jni/controls.lpi
  2. Перейти к параметрам проекта и изменить пути NDK в соответствии с вашими установками
  3. Проект, Параметры, конфигурация проекта LAMW и проверьте Target SDK API, который должен совпадать с установленным выше.
  4. Запустите, соберите проект. Как и «Hello World» выше, он должен быть скопирован, установлен и запущен в виде приложение на устройстве Android.
  5. Пробуйте!



Если вы получаете сообщение "Application has stopped" (Приложение остановлено), может быть полезно перейти в меню Tools (Сервис), [LAMW] Android Module Wizard (Мастер модулей Android [LAMW]), Upgrade Code templates (обновление кода шаблонов). 

Выберите каталог, содержащий примеры и проект, и нажмите «ОК». 

Затем соберите проект заново и тогда должно заработать!

---

_Source: [https://wiki.freepascal.org/LAMW/ru](https://web.archive.org/web/20250420130337/https://wiki.freepascal.org/LAMW/ru)_
