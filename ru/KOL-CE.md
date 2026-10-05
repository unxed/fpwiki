# KOL-CE

[![Windows logo - 2012.svg](https://upload.wikimedia.org/wikipedia/commons/thumb/5/5f/Windows_logo_-_2012.svg/50px-Windows_logo_-_2012.svg.png)](</File:Windows_logo_-_2012.svg>)

Эта статья относится только к [Windows](</Category:Windows> "Category:Windows").

См. также: [Multiplatform Programming Guide](<../en/Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

[![WinCE Logo.png](https://wiki.freepascal.org/images/8/86/WinCE_Logo.png)](</File:WinCE_Logo.png>)

Эта статья относится только к [Windows CE](</Category:WinCE> "Category:WinCE").

См. также: [Multiplatform Programming Guide](<../en/Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

│ **[English (en)](<../en/KOL-CE.md>)** │  **русский (ru)** │

## Contents

  * 1 Введение
    * 1.1 Требования
    * 1.2 Поддерживаемые платформы:
  * 2 Загрузка
  * 3 Установка
  * 4 Особенности использования MCK
    * 4.1 Создание нового проекта
    * 4.2 Добавление формы
    * 4.3 Особенности написания кода
    * 4.4 Важно
    * 4.5 Run-time создание форм
  * 5 WinCE
    * 5.1 Установка
    * 5.2 Редактирование переменной окружения **PATH**
      * 5.2.1 в Windows XP
      * 5.2.2 в Windows 7
    * 5.3 Hints and notes
    * 5.4 Известные проблемы
  * 6 Документация
  * 7 Приложения, использующие KOL-CE
  * 8 См. также
  * 9 Contacts



## Введение

KOL-CE - это [KOL&MCK](<http://f0460945.xsph.ru/rindex.htm>) от Владимира Кладова, портированная под Free Pascal/Lazarus. KOL-CE создана Юрием Сидоровым и распространяется под [wxWindows Library Licence](<http://www.opensource.org/licenses/wxwindows.php>). 

KOL-CE позволяет создавать очень компактные Win32/WinCE GUI приложения (начиная от ~40KB приложения с пустой формой). 

MCK это пакет, позволяющий проектировать приложения KOL-CE визуально, используя Lazarus IDE. 

Изначально KOL-CE планировался как KOL, портированный только для [WinCE](<../en/WinCE_port.md> "WinCE port"). Но потом было решено оставить функции Win32 и доделать его работу с FPC. Оригинальный KOL предназначалась для Delphi и работает во Free Pascal с ошибками. А оригинал MCK вообще нельзя было использовать для Lazarus. 

### Требования

  * Free Pascal compiler 2.2.0 или позже для Win32.
  * arm-wince cross compiler 2.2.0 или позже для Win32 (for [WinCE](<../en/WinCE_port.md> "WinCE port") development).
  * Lazarus 0.9.26 или позже для Win32.



### Поддерживаемые платформы:

  * Все 32-битные Windows: от Windows 95 до Windows 7.
  * [Windows CE](<../en/WinCE_port.md> "WinCE port") для PocketPC и смартфонов.



## Загрузка

Последний релиз KOL-CE можно скачать [здесь](<http://sourceforge.net/project/showfiles.php?group_id=188451>). 

Также вы можете получить свежие исходники KOL-CE из [SVN](<https://kol-ce.svn.sourceforge.net/svnroot/kol-ce/trunk>). 

## Установка

* * *

**Важно:** После KOL-CE 2.80.2 определение `DisableFakeMethods` больше не требуется. 

Если раньше вы использовали KOL-CE 2.80.1 или ранее, вам надо пересобрать Lazarus **без** определения `DisableFakeMethods` перед установкой пакета MCK. Иначе **обработчики событий не будут работать**! 

Сделаем это: 

  1. Запустить Lazarus.
  2. Выбрать пункт **Tools > Configure "Build Lazarus"...** (**Сервис > Параметры сборки Lazarus...**).
  3. Выбрать **Clean Up + Build all**(Очистить всё + собрать всё) на странице **Quick Build Options** (простые параметры сборки).
  4. Открыть страницу **Advanced Build Options** (Расширенные параметры сборки) и **remove** `-dDisableFakeMethods` из поля ввода **Options** .
  5. Кликнуть по кнопке **Build** для пересборки Lazarus.



* * *

[![MCK package](https://wiki.freepascal.org/images/2/2e/MirrorKOLPackage.png)](</File:MirrorKOLPackage.png> "MCK package")

  1. Загрузите KOL-CE и поместите куда-нибудь рядом с Лазарусом.
  2. Запустите Lazarus и выберите пункты меню **Components > Load package file**. Укажите папку **MCK** и выберите файл `**MirrorKOLPackage.lpk**`.
  3. Появится окно содержимого пакета. Нажмите кнопку **Install**.
  4. Лазарус скомпилирует пакет MCK перезапустится.
  5. После этого на палитре компонентов появится вкладка KOL.



[![KOL components palette](https://wiki.freepascal.org/images/d/d1/KOLComponents.png)](</File:KOLComponents.png> "KOL components palette")

СОВЕТ: Если вы видите не все компоненты KOL на палитре компонентов, увеличьте размер окна по вертикали. Появится вторая строка с недостающими копонентами (см. скриншот). 

Обновить пакет MCK очень легко. Просто перезапишите обновленные KOL-CE исходники поверх (вместо) старых, откройте пакет MCK через Lazarus и нажмите **Install** , пакет перекомпилится, и будет вам счастье! 

## Особенности использования MCK

### Создание нового проекта

[![MCK form](https://wiki.freepascal.org/images/0/04/MCKForm.png)](</File:MCKForm.png> "MCK form")

  1. Запустите Lazarus, выберите **File > New...** (**Файл > Создать ...**).
  2. Выберите **KOL Application** в секции **Project** и нежмите **OK**.
  3. Будет создан новый KOL проект.
  4. Сохраните проект под новым именем.
  5. Продукт готов к использованию, срок годности ограничен вашей фантазией =). Можете развлекаться!



### Добавление формы

  1. Выберите **File > New...** (**Файл > Создать ...**).
  2. Выберите **KOL Form**(иногда просто **Form**) в секции **File** (**Модуль**) и нажмите **OK**.
  3. Сохраните модуль с формой под новым именем.



### Особенности написания кода

Не используйте элементы из RTL/FCL/LCL, особенно из SysUtils, Classes, Forms и подобных им. Всё необходимое придётся искать в модулях KOL, Windows, Messages. А может быть, вы напишете что-то сами (или скопируете из других исходников). Если вы пишете в зеркальном проекте (mirror project) - Обычно поместите его в обработчики событий. Вы также можете добавлять любой код где пожелаете, но избегайте изменения первой секции объявления класса вашей зеркальной LCL формы. И не изменяйте автоматически созданные inc-файлы. Всегда помните, что код, который вы пишете в зеркальном проекте, должен быть принят и LCL, и KOL. LCL - на этапе компиляции зеркального проекта (и это необходимо, потому что иначе конвертация зеркального проекта в отражённый KOL проект будет невозможна). А KOL - на этапе компиляции написанного кода в KOL. 

### Важно

Для решения конфликта между словами `LCL.Self` и `KOL.@Self`, которые интерпретируются по-разному в KOL и LCL, добавлен специальный компонент - `Form`. В LCL, свойство `Form` типа компонент `TKOLForm` "возвращает" `Self`, То есть форму. А в KOL, `Form: PControl` \- это поле-объект, содержащий сам объект-форму. Поэтому, вместо 
    
    
    MainForm.caption := 'Hello!', где MainForm - ваша форма, пишите
    Form.Caption := 'Hello!'; 
    

(Старое обращение Caption := 'Hello!'; компилируется нормально при преобразовании зеркального проекта в KOL, однако в самом(без MCK) KOL это будет неправильно). Но через `Form` можно обращаться только к свойствам формы - не к дочерним компонентам. Вам доступны дочерние компоненты и обработчики событий черес саму форму. например: 
    
    
    Button1.Caption := 'OK';
    Button1Click(Form);
    

### Run-time создание форм

Можно создать несколько экземпляров одной и той же формы во время выполнения программы. Притом можно сделать не АвтоСоздаваемую форму, а создавать её программно, когда это необходимо. Используйте глобальную функцию `NewForm1` (замените Form1 на имя вашей зеркальной формы) например: 
    
    
    NewForm1( TempForm1, Applet );
    

Для таких созданных во время выполнения форм справедливо то же правило, что и дляавтосозданных - обращайтесь к сваойствам формы через свойство-объект `Form`

## WinCE

#### Установка

[![arm-wince target](https://wiki.freepascal.org/images/7/7a/arm-wince-target.png)](</File:arm-wince-target.png> "arm-wince target")

Для начала надо установить кросскомпилятор arm-wince for Win32 для компиляции исполняемых файлов WinCE. Можно скачать [здесь](<http://www.freepascal.org/download.var>). Нужно скачать lazarus с кросскомпилятором в комплекте и затем установить поверх уже установленного Lazarus'a, то есть в ту же папку. 

Чтобы компилировать под arm-wince откройте параметры компилятора для вашего проекта, выбрав пункт меню **Project > Compiler options...**. Откройте вкладку **Code** (**Код**) и поменяйте операционную систему на WinCE, а целевое семейство процессоров на arm. 

На вкладке **Пути** поменяйте библиотеку виджетов LCL на WinCe. 

**ПРИМЕЧАНИЕ:** Вы можете получить следующие сообщения об ошибке во время компиляции вашего KOL-CE проекта для WinCE: 
    
    
    Compiling resource KOL-CE.rc
    arm-wince-windres.exe: no resources
    KOL.PAS(57901) Error: Error while linking
    KOL.PAS(57901) Fatal: There were 1 errors compiling module, stopping
    

В таком случае вам нужно изменить переменную окружения **PATH** **Windows** и добавить путь к папке, где расположены файлы win32 FPC.  
Чтобы узнать, какой путь добавить, перейдите на**Окружение** >**Параметры** в Lazarus и посмотреть путь к компилятору.  
**Перед изменением PATH закройте Lazarus**.  


#### Редактирование переменной окружения **PATH**

##### в Windows XP

1.Правой кнопкой мыши щелкните по значку "Мой компьютер" и выберите пункт меню "Свойства". 2.Перейдите на вкладку "Дополнительно". 3.Нажмите кнопку "Переменные среды". 4.Выберите в списке переменную path, нажмите **изменить** и через точку с запятой добавьте путь к папке. 

##### в Windows 7

1.Нажмите пуск. 2.Нажмите правой кнопкой мыши по пункту **Компьютер**.Выберите пункт меню свойства. 3.Нажмите на **Дополнительные параметры системы** (список слева). 4.На вкладке **Дополнительно** нажмите кнопку **Переменные среды**. 5.Выберите в списке переменную path, нажмите **изменить** и через точку с запятой добавьте путь к папке. 

#### Hints and notes

  * Чтобы сделать полноэкранный вид, не пытайтесь изменить позицию окна и его размер. Если попытаться это сделать, форма будет выглядеть как диалог с заголовком и кнопкой закрыть. В MCK установите **defaultSize** и **defaultPosition** of TKOLForm в True для достижения цели.



#### Известные проблемы

  * Следующие компоненты не поддерживаются: RichEdit.
  * Прозрачность и двойная буферизация также не поддерживаются.
  * Работают значения свойства gradient panel styles только gsVertical и gsHorizontal.
  * Горизонтальное выравнивание текста не работет в однострочном компоненте edit. Используйте memo если вам оно нужно.
  * Vertical text alignment does not work for panel and label.



## Документация

  * Посетите официальный сайт KOL&MCK для получения документации и информации: <http://kolmck.ru>
  * Прочитайте документацию MCK в файле **KOLmirrorReadme.txt** , лежащем в папке MCK.



## Приложения, использующие KOL-CE

  * [Password Manager XP Mobile](<http://www.cp-lab.com/windows-mobile.html>)
  * [ChARMeD disassembler](<http://blog.carolos.za.net/2007/05/charmed-for-pocket-pc-beta-030.html>)
  * [WMWifiRouter](<http://www.jongma.org/WMWifiRouter/>)



## См. также

  * [Официальный вебсайт KOL&MCK](<http://f0460945.xsph.ru/rindex.htm>)
  * Страничку KOL-CE на SourceForge <http://sourceforge.net/projects/kol-ce/>
  * [Портирование на WinCE c помощью Free Pascal](<http://wiki.lazarus.freepascal.org/WinCE_port/ru>)
  * [Windows CE Interface](<../en/Windows_CE_Interface.md> "Windows CE Interface")
  * [Windows CE Development Notes](<../en/Windows_CE_Development_Notes.md> "Windows CE Development Notes")



## Contacts

Report bugs, submit patches and ask questions at project's page at SourceForge: <http://sourceforge.net/projects/kol-ce/>

---

_Source: [https://wiki.freepascal.org/KOL-CE/ru](https://web.archive.org/web/20250301000000/https://wiki.freepascal.org/KOL-CE/ru)_
