# Installing Lazarus

│ **[English (en)](<../en/Installing_Lazarus.md>)** │  **русский (ru)** │

## Contents

  * 1 Обзор
    * 1.1 Системные требования Lazarus
  * 2 Краткое руководство
    * 2.1 Linux
    * 2.2 macOS
    * 2.3 Windows
  * 3 Установка компилятора Free Pascal
    * 3.1 Установка Free Pascal в Linux
      * 3.1.1 Исполняемые файлы FPC
      * 3.1.2 Исходные файлы FPC
        * 3.1.2.1 Загрузка ежедневных исходных файлов из ветки разработки
        * 3.1.2.2 Обновление исходных файлов с помощью SVN
        * 3.1.2.3 Получение исходных файлов
        * 3.1.2.4 Получение отдельных веток
        * 3.1.2.5 Документация
    * 3.2 Установка Free Pascal в Windows
      * 3.2.1 Исполняемые файлы FPC для Windows
      * 3.2.2 Исходные файлы FPC для Windows
      * 3.2.3 Компиляция/установка FPC и Lazarus из исходных файлов с помощью SVN (Win32)
        * 3.2.3.1 Шаг #1: Создать каталоги и скачать исходные файлы
        * 3.2.3.2 Шаг #2: Создать BAT-файл для компиляции FPC
        * 3.2.3.3 Шаг #3: Собрать и установить FPC
        * 3.2.3.4 Шаг #4: Создать BAT-файл для компиляции Lazarus
        * 3.2.3.5 Шаг #5: Собрать Lazarus
    * 3.3 Установка Free Pascal в FreeBSD/auto
    * 3.4 Установка Free Pascal из исходных файлов в Linux/BSD
    * 3.5 Установка Free Pascal в macOS
  * 4 Установка Lazarus
    * 4.1 Установка Lazarus в Linux
      * 4.1.1 Ubuntu / Debian
        * 4.1.1.1 Простой способ сборки из deb-пакетов
        * 4.1.1.2 Установка с использованием rpms
        * 4.1.1.3 Установка на Raspbian Wheezy
      * 4.1.2 Установка на Mandriva
        * 4.1.2.1 Lazarus 0.9.30 на Mandriva 2010.
      * 4.1.3 Установка на Slackware
        * 4.1.3.1 Установка Lazarus на Slackware 13.0
        * 4.1.3.2 Установка Lazarus 0.9.30 для пользователей Slackware 12.0
      * 4.1.4 Установка на openSUSE
        * 4.1.4.1 Установка Lazarus 0.9.30 для пользователей openSUSE 11.1
      * 4.1.5 Установка Lazarus 1.0.8 на Fedora Core 18
      * 4.1.6 Установка Lazarus на Scientific Linux
      * 4.1.7 Установка из исходных файлов
        * 4.1.7.1 Загрузка исходных файлов Lazarus
        * 4.1.7.2 Компиляция и запуск
      * 4.1.8 Установка Lazarus в Debian GNU/Linux
    * 4.2 Установка Lazarus в Windows
      * 4.2.1 Установка портативной версии Lazarus на USB-диск
      * 4.2.2 Установка из исходных файлов
      * 4.2.3 Установка из исходных файлов с использованием стабильной версии
      * 4.2.4 Сборка Lazarus в Win98 и WinME
    * 4.3 Установка Lazarus в FreeBSD
      * 4.3.1 via Ports tree
      * 4.3.2 via pkgng
      * 4.3.3 via Lazarus repository
    * 4.4 Установка Lazarus в PC-BSD 1.0rc1+
    * 4.5 Установка Lazarus в macOS
    * 4.6 Установка Lazarus в Haiku
  * 5 Установка нескольких версий Lazarus
  * 6 Устранение проблем
    * 6.1 Isolating the component
    * 6.2 Расположение установленных компонентов
    * 6.3 Отсутствие или повреждение файла fpc.cfg
      * 6.3.1 Автоматическое создание fpc.cfg
      * 6.3.2 Ручное создание fpc.cfg
      * 6.3.3 Testing and batch file/shortcut usage
    * 6.4 Mixing FPC versions
    * 6.5 Lazarus can't find sources
    * 6.6 Пути к файлам в настройках Lazarus и проекта
  * 7 Установка старых версий



## Обзор

Ознакомится с более подробной инструкцией по сборке можно [здесь](<http://www.stack.nl/~marcov/buildfaq.pdf>). 

Для загрузки исполняемых файлов Lazarus смотрите [Загрузка и установка релизной версии Lazarus](<http://wiki.lazarus.freepascal.org/Getting_Lazarus/ru#Загрузка_и_установка_релизной_версии_Lazarus>)

Для тех, кто просто хочет установить Lazarus и начать использовать его для написания программ, наиболее простым способом является загрузка и установка последней стабильной версии исполняемых файлов (таких, как rpm-пакеты для Linux, установочные exe-файлы для Windows или dmg-пакеты для Mac OS X). Вы можете прочитать об этом в разделах "Исполняемые файлы FPC" для Linux и Windows или в первых параграфах в разделах установки Lazarus в средах Linux или Windows; оставшуюся часть информации можно спокойно пропустить. 

  


Для тех, кто хочет принимать участие в разработке компилятора, IDE или использовать актуальные инструменты, необходима установка из исходных файлов. 

Lazarus состоит из двух основных частей: 

  * LCL - Lazarus Component Library, библиотека компонентов Lazarus
  * IDE - RAD, среда быстрой разработки



В свою очередь они зависят от: 

  * FPC - компилятор Free Pascal
  * FCL - Free Pascal Component library, библиотека компонентов компилятора FPC, содержащая большое количество невизуальных компонентов, используемых в Lazarus



### Системные требования Lazarus

  1. компилятор Free Pascal, пакеты и исходные файлы. (*важно*: той же версии/даты сборки)
  2. поддерживаемый набор виджетов 

    

Win32
    может быть использован внутренний набор Win32 API или набор виджетов Qt.
Linux/xxxBSD
    GTK+ 2.x or Qt : Most Linux distributions and *BSDs already install the GTK+ 2.x libraries. You can also find them at <http://www.gtk.org>.   
Qt is also supported with all distributions (auto installed if you prefer KDE).   

Mac OS X
    You need the Apple developer tools. See Installing under Mac OS X below. Qt can be used too.



FAQ - Frequently Asked Questions (часто задаваемые вопросы) доступны по ссылке <http://www.lazarus.freepascal.org> . Некоторые вопросы можно найти в файле 'FAQ'. 

Qt widget set is supported on Linux 32/64,Win32/64,Mac OS X, Haiku and embedded linux (qtopia) platforms.  
Более подробно узнать про установку можно по ссылке <http://wiki.lazarus.freepascal.org/index.php/Qt_Interface>   


В следующих разделах описывается как можно получить компилятор Free Pascal и как правильно установить Lazarus. 

## Краткое руководство

Хотя остальная часть этой страницы содержит много ценной информации, многим пользователям может понадобиться не более, чем следующие «краткие руководства» - возможно? 

#### Linux

  * См. [Installing Lazarus on Linux](<../en/Installing_Lazarus_on_Linux.md> "Installing Lazarus on Linux")



#### macOS

  * См. [Установка Lazarus на MacOS X](</index.php?title=Installing_Lazarus_on_MacOS_X/ru&action=edit&redlink=1> "Installing Lazarus on MacOS X/ru \(page does not exist\)")



#### Windows

Безусловно, самый простой и распространенный способ установки Lazarus на Windows - это перейти на [сайт загрузки Lazarus](<https://sourceforge.net/projects/lazarus/files/>) SourceForge, выбрать подходящий комбинированный пакет FPC/Lazarus, скачать и установить. 

## Установка компилятора Free Pascal

Существует подробная инструкция, как устанавливать и собирать компилятор Free Pascal, доступная по ссылке <http://www.stack.nl/~marcov/buildfaq.pdf> \- для некоторых пользователей она может быть слишком подробной, но при этом в ней дана исчерпывающая информация. 

Для Lazarus 1.8 требуется fpc (Free Pascal) версии 3.0.4 или выше, а также необходимо, чтобы библиотеки fpc (исполняемые файлы) и исходные файлы fpc были одинаковых версий. 

### Установка Free Pascal в Linux

#### Исполняемые файлы FPC

Последнюю стабильную версию Free Pascal 3.0.4 можно получить на сайте Free Pascal (перейдите по ссылке <http://www.freepascal.org/download.var> и далее выберите зеркало) или на сайте SourceForge (<http://sourceforge.net/projects/lazarus>). 

В разделе загрузок Lazarus (<http://www.lazarus.freepascal.org> ) можно получить RPM- или DEB-пакеты компилятора Free Pascal (компилятор для Linux). Если вам не нужны RPM или debian дистрибутивы, вы можете загрузить и установить tar-архивы с <http://www.freepascal.org>. Если вы хотите самостоятельно скомпилировать исполняемые файлы, смотрите в разделе BSD. 

Инструкции: 

[![Warning-icon.png](https://wiki.freepascal.org/images/b/b2/Warning-icon.png)](</File:Warning-icon.png>)

**Предупреждение:** если вы **не используете RPM- или debian-пакеты** (даже если вы планируете использовать другие) лучшим вариантом будет получение актуальной стабильной версии fpc (на данный момент 3.0.4) и установка Lazarus из исходных файлов.

  


Запустите Linux и войдите в систему под **root'** ом. 

Загрузите актуальные версии файлов с <https://sourceforge.net/projects/lazarus/files/>. Например: 

    

  * fpc-3.0.4-1.i686.rpm
  * fpc-src-3.0.4-1.i686.rpm
  * lazarus-2.0.6-1.i686.rpm



и установите их с помощью команды: 

    

  * rpm -Uvh *.rpm



Пользователям системы Debian рекомендуется использовать deb-пакеты, но можно использовать и другие (внимание: они не создают fpc.cfg), например установить tar-архивы. 

#### Исходные файлы FPC

Исходные файлы FPC хранятся в репозитории Subversion (SVN), в котором отслеживаются все изменения. Если у вас уже есть исходные файлы, пожалуйста, посмотрите инструкции по их установке в разделе #Installing Free Pascal from source under Linux/BSD. 

##### Загрузка ежедневных исходных файлов из ветки разработки

Вы можете загрузить ежедневно обновляемые исходные файлы в форме упакованных ночных сборок из репозитория SVN: они обновляются ежедневно и отображают состояние исходных файлов. Они не протестированы, могут не работать или даже привести к нарушению работы вашей системы. Файлы сохранены на сайте, который содержит архив SVN. Версия FPC может варьироваться. Расположены по адресу: [[1]](<http://www.hu.freepascal.org/lazarus/>)

##### Обновление исходных файлов с помощью SVN

В качестве альтернативы ежедневным zip-файлам исходников SVN, репозиторий SVN сделан доступным для всех с доступом только для чтения. Это означает, что вы можете получить прямой доступ к коду, и у вас будут действительно доступные исходники на последний момент. Это также метод, который требует меньшей полосы пропускания после первой загрузки (извлечение в SVN lingo). 

##### Получение исходных файлов

Как это можно сделать? Для начала вам необходим установленный клиент для SVN. Используйте менеджер установки и установите утилиту, например, TortoiseSVN для Windows или смотрите подробней на сайте [[2]](<http://subversion.tigris.org/>). 

Использование командной строки SVN: измените текущую директорию (cd) на родительскую директорию, которая используется для работы, например, чтобы получить полные исходные файлы из репозитория в поддиректорию fpc домашней директории и напишите 
    
    
    cd ~
    svn checkout http://svn.freepascal.org/svn/fpc/trunk fpc
    

Для обновления исходных файлов, загруженных с помощью команды, указанной выше 
    
    
    cd ~
    svn update fpc
    

##### Получение отдельных веток

Если текущая версия транка находится в состоянии быстрого изменения и не подходит для большого использования, если вы не хотите работать с самим компилятором, вы можете остаться на версии, которая обновлена с исправлениями. Чтобы сделать это, вы должны найти стабильную ветку, которую вы хотите отслеживать, вместо девелоперской **trunk** версии по умолчанию. Пример ниже показывает, как вы можете отслеживать версию fixes_2_6; конечно замените [номер версии] по мере необходимости в зависимости от того, какие ветки вы хотите отслеживать. 

Этот пример хранит исправления в другой папке в вашем домашнем каталоге - не имеет смысла помещать две версии исходного кода в один каталог ... 
    
    
    cd ~
    svn checkout http://svn.freepascal.org/svn/fpc/branches/fixes_2_6 fpc_fixes_2_6
    

Обычное обновление: 
    
    
    cd ~
    svn update fpc_fixes_2_6
    

##### Документация

Исходные файлы документации расположены в отдельном репозитории с именем fpcdocs, поэтому для их получения необходима следующая команда 
    
    
    cd ~
    svn checkout http://svn.freepascal.org/svn/fpcdocs/trunk fpcdocs
    

Если вы хотите больше узнать о subversion, прочитайте эту отличную [книгу по Subversion](<http://svnbook.red-bean.com/>), которая также бесплатно доступна в разных форматах. 

Более подробную информацию смотрите на [сайте Free Pascal](<http://www.freepascal.org/develop.var>). 

### Установка Free Pascal в Windows

#### Исполняемые файлы FPC для Windows

Самым простым способом получения установочных файлов Free Pascal является загрузка текущей стабильной версии исполняемых файлов Lazarus для Windows из репозитория SourceForge [[3]](<http://sourceforge.net/projects/lazarus/files/>) \- эти файлы содержат текущие стабильные версии компилятора и необходимых библиотек Free Pascal, а также среду разработки Lazarus. 

Если вы хотите установить из исходных файлов читайте дальше! 

Вы можете загрузить zip-архив с fpc в разделе загрузок Free Pascal (перейдите по ссылке <http://www.freepascal.org/download.var> и далее выберите зеркало). Установка из исходных файлов -- смотрите следующий раздел, чтобы узнать как это сделать -- не для новичков, так как для этого вам необходим изначально установленный компилятор. 

#### Исходные файлы FPC для Windows

<<<< Смотрите выше в разделе Исходные файлы FPC для Linux, где описано использование SVN >>>>

Наиболее простым способом получить исходные файлы Free Pascal является использование SVN; смотрите следующий раздел для более подробной информации. Также вы можете загрузить установочный пакет целиком -- смотрите на сайте <http://www.freepascal.org/develop.var> ежедневные сборки ветки 3.0.x. 

Исходные файлы FPC для Windows с помощью SVN 

Вам необходим установленный SVN-клиент, например [TortoiseSVN](<http://tortoisesvn.tigris.org/>), для выполнения дальнейших шагов. Точные команды отличаются для разных SVN-клиентов; приведенные ниже команды должны использоваться для SVN-клиента, который можно скачать здесь. 

Для начала создайте директорию, в которую вы хотите поместить исходные файлы. С этим может справиться любой пользователь. Создайте директорию для fpc (например C:\Source), после чего чего выполните следующую команду в командной строке: 
    
    
    C:\Source> svn co http://svn.freepascal.org/svn/fpc/trunk fpc
    

Совет: для загрузки/обновления файлов с последними изменениями вы просто можете выполнить 
    
    
    C:\> cd Source\FPC
    C:\Source\FPC> svn up
    

Смотрите: <http://www.freepascal.org/down/i386/win32.var> . Загрузите FPC одним большим файлом, распакуйте его и запустите install.exe. 

Добавьте путь к директории fpc в переменную среды: 

  * Win98: отредактируйте autoexec.bat и добавьте строку: PATH=%PATH%;C:\pp\bin\bin\win32
  * WinXP/2k: Мой компьютер (правая кнопка мыши по ярлыку) -> Свойства -> Дополнительно (вкладка) -> Переменные среды -> Системные переменные -> Изменить "PATH", добавить здесь "C:\pp\bin\bin\win32".



После этого перезагрузите windows. 

После установки исполняемых файлов FPC вы можете собирать исходные файлы FPC из subversion. 

Советы: 

  * Для Windows 7 (и выше) требуются повышенные права пользователя для работы в командной строке. Щелкните правой кнопкой мыши по ярлыку "Командная строка" в меню Пуск и выберите "Запустить от имени Администратора".
  * YOUR-PREFIX полностью зависит от места установки FPC. При составлении данной статьи исполняемые файлы по умолчанию располагаются в директории "C:\FPC" и помещаются в "C:\FPC\3.0.4". В системе Linux сценарии "make install" были скорректированы для создания новой поддиректории ЕСЛИ версия FPC изменилась с момента последней сборки. В сценариях для Windows это не сделано. Поэтому, если вы знаете, что имя поддиректории, например 3.1.1, то вы можете указать это. Однако, по причине частой смены версий, рекомендуется просто выбрать и сохранить один PREFIX вне зависимости от версий FPC. Хорошим значением для prefix является C:\FPC, но вы также должны убедиться, что директория C:\FPC\bin\i386-win32\ добавлена в переменную среды PATH (смотрите выше, как прописать путь и изменить версию исполняемых файлов на только что скомпилированные).



Инструкции: 

  * В командной строке перейдите к расположенным исходным файлам FPC, например напишите "cd c:\Developer\FPC"
  * Для сборки FPC напишите "make all"
  * Для перезаписи существующих файлов FPC напишите "make install PREFIX=YOUR-PREFIX"
  * Для установки исходных файлов напишите "make install sourceinstall PREFIX=YOUR-PREFIX"



#### Компиляция/установка FPC и Lazarus из исходных файлов с помощью SVN (Win32)

14 Dec 2017 версия FPC **3.0.4** или **trunk** \- версия Lazarus **1.9.x**

##### Шаг #1: Создать каталоги и скачать исходные файлы

Создайте следующие каталоги: 
    
    
     c:\freepascal\
     c:\freepascal\binutils\
     c:\freepascal\binutils\i386-win32\
     c:\freepascal\fpc\
     c:\freepascal\fpc\3.0.4\
     c:\freepascal\laz\
    

или для fpc из trunk: 
    
    
     c:\freepascal\
     c:\freepascal\binutils\
     c:\freepascal\binutils\i386-win32\
     c:\freepascal\fpc\
     c:\freepascal\fpc\trunk\
     c:\freepascal\laz\
    

Вам понадобится последняя _стабильная_ версия компилятора для сборки новой версии компилятора. Скачайте файл ppc386 (сам компилятор) с FTP (по ссылке ниже) и распакуйте его в c:\freepascal\binutils\ 
    
    
     <ftp://ftp.freepascal.org/pub/fpc/dist/3.0.4/bootstrap/i386-win32-ppc386.zip>
    

После установки [TortoiseSVN](<http://tortoisesvn.tigris.org/>) скачайте исходные файлы из SVN используя URL-адрес для каждого каталога: 
    
    
     Dir: c:\freepascal\binutils\i386-win32\
     URL: <http://svn.freepascal.org/svn/fpcbuild/branches/fixes_3_0/install/binw32>
    

или для fpc из trunk: 
    
    
     Dir: c:\freepascal\binutils\i386-win32\
     URL: <http://svn.freepascal.org/svn/fpcbuild/trunk/install/binw32>
    

  

    
    
     Dir: c:\freepascal\fpc\3.0.4
     URL: <http://svn.freepascal.org/svn/fpc/branches/fixes_3_0/>
    

или для fpc из trunk: 
    
    
     Dir: c:\freepascal\fpc\trunk
     URL: <http://svn.freepascal.org/svn/fpc/trunk/>
    

  

    
    
     Dir: c:\freepascal\laz
     URL: <http://svn.freepascal.org/svn/lazarus/trunk>
    

##### Шаг #2: Создать BAT-файл для компиляции FPC

После того, как всё скачалось, нам необходимо создать BAT-файл для компиляции исходных файлов FPC. Создайте новый файл c:\freepascal\makefpc.bat и скопируйте в него следующий сценарий: 
    
    
    @echo on
    set myroot=c:\freepascal
    set myFPC=%myroot%\fpc\3.0.4
    set mybinutils=%myroot%\binutils
    set PATH=%mybinutils%\i386-win32;%myFPC%\bin\i386-win32;%PATH%
    cd %myFPC%
    rd /s /q  %myfpc%\examples
    svn cleanup . --remove-unversioned  --remove-ignored
    make distclean all install INSTALL_PREFIX=%myFPC% PP=%mybinutils%\ppc386.exe DATA2INC=%myFPC%\utils\data2inc.exe
    cd /d %myFPC%\bin\i386-win32
    fpcmkcfg -d basepath=%myFPC% -o .\fpc.cfg 
    copy fpc.exe %mybinutils%\i386-win32
    

или для fpc из trunk: 
    
    
    @echo on
    set myroot=c:\freepascal
    set myFPC=%myroot%\fpc\trunk
    set mybinutils=%myroot%\binutils
    set PATH=%mybinutils%\i386-win32;%myFPC%\bin\i386-win32;%PATH%
    cd %myFPC%
    rd /s /q  %myfpc%\examples
    svn cleanup . --remove-unversioned  --remove-ignored
    make distclean all install INSTALL_PREFIX=%myFPC% PP=%mybinutils%\ppc386.exe DATA2INC=%myFPC%\utils\data2inc.exe
    cd /d %myFPC%\bin\i386-win32
    del fpc.cfg
    fpcmkcfg -d basepath=%myFPC% -o .\fpc.cfg 
    copy fpc.exe %mybinutils%\i386-win32
    

Для компиляции кросс-платформенных программ в режиме x86_64 добавьте следующие строки после первой команды make: 
    
    
    make all OS_TARGET=win64 CPU_TARGET=x86_64 INSTALL_PREFIX=%myFPC% PP=%mybinutils%\ppc386.exe DATA2INC=%myFPC%\utils\data2inc.exe
    make crossinstall OS_TARGET=win64 CPU_TARGET=x86_64 INSTALL_PREFIX=%myFPC% PP=%mybinutils%\ppc386.exe DATA2INC=%myFPC%\utils\data2inc.exe
    

При использовании данных сценариев скомпилированные файлы компилятора и необходимые директории для корректной его работы будут созданы непосредственно в корневой директории, куда были загружены исходные файлы. В некоторых случаях это не удобно, например, если необходимо _перенести_ всю директорию с компилятором на другой компьютер или просто сделать несколько директорий разных версий (или ревизий) компилятора и при необходимости пользоваться каждой из них путем выбора нужного пути в свойствах проекта. Для этого необходимо модифицировать сценарий. Ниже представлен пример сценария для создания полностью автономной директории с компилятором и необходимыми файлами (для универсальности добавлены инструкции, позволяющие собрать компилятор версий **x86** и **x64**): 
    
    
    @echo on
    set myroot=c:\freepascal
    
    set instdir=%myroot%\fpc\3.0.5
    
    set myFPC=%myroot%\fpc\3.0.4
    set mybinutils=%myroot%\binutils
    set PATH=%mybinutils%\i386-win32;%myFPC%\bin\i386-win32;%PATH%
    cd %myFPC%
    rd /s /q  %myfpc%\examples
    svn cleanup . --remove-unversioned  --remove-ignored
    make distclean all install INSTALL_PREFIX=%instdir% PP=%mybinutils%\ppc386.exe DATA2INC=%myFPC%\utils\data2inc.exe
    make all OS_TARGET=win64 CPU_TARGET=x86_64 INSTALL_PREFIX=%instdir% PP=%mybinutils%\ppc386.exe DATA2INC=%myFPC%\utils\data2inc.exe
    make crossinstall OS_TARGET=win64 CPU_TARGET=x86_64 INSTALL_PREFIX=%instdir% PP=%mybinutils%\ppc386.exe DATA2INC=%myFPC%\utils\data2inc.exe
    cd /d %instdir%\bin\i386-win32
    fpcmkcfg -d basepath=%instdir% -o .\fpc.cfg
    copy fpc.exe %mybinutils%\i386-win32
    
    xcopy %myFPC%\rtl %instdir%\source\rtl /H /Y /C /R /S /I
    xcopy %myFPC%\packages %instdir%\source\packages /H /Y /C /R /S /I
    

В данном случае дополнительная переменная **instdir** задает новую директорию для компилятора. Последние 2 команды предназначены для копирования содержимого директорий **rtl** и **packages** в новую директорию компилятора. 

Совет: при переносе в другое место полученной директории для корректной работы компилятора необходимо отредактировать файл **fpc.cfg**. 

##### Шаг #3: Собрать и установить FPC

С помощью командной строки (cmd.exe) перейдите в директорию c:\freepascal и выполните сценарий, который мы только что написали: 
    
    
    cd /d c:\freepascal
    makefpc.bat
    

##### Шаг #4: Создать BAT-файл для компиляции Lazarus

Если Lazarus компилируется впервые, создайте новый файл c:\freepascal\makelaz.bat и скопируйте в него следующий сценарий: 
    
    
    set myroot=c:\freepascal
    set myFPC=%myroot%\fpc\3.0.4
    set mybinutils=%myroot%\binutils
    set PATH=%mybinutils%\i386-win32;%myFPC%\bin\i386-win32;%PATH%
    cd %myroot%\laz
    make clean all OPT="-glw2 -Xg"
    

или для fpc из trunk: 
    
    
    set myroot=c:\freepascal
    set myFPC=%myroot%\fpc\trunk
    set mybinutils=%myroot%\binutils
    set PATH=%mybinutils%\i386-win32;%myFPC%\bin\i386-win32;%PATH%
    cd %myroot%\laz
    make clean all OPT="-glw2 -Xg"
    

  
Подсказка: данный BAT-файл необходимо использовать только для первой компиляции. В дальнейшем вы можете собирать Lazarus используя команду меню Tools > Build Lazarus. 

##### Шаг #5: Собрать Lazarus

С помощью командной строки перейдите в директорию c:\freepascal и выполните сценарий: makelaz.bat 

  
Готово! 

### Установка Free Pascal в FreeBSD/auto

Последняя коллекция портов для FreeBSD имеет версию 3.0.4 и находится в ports/lang/fpc. Но они разбросаны по более чем 96 пакетам (!) и исходный код может быть получен из /usr/ports/distfiles/freepascal. 

Данная команда может быть выполнена с правами администратора (из-под root'а). 
    
    
    []# cd /usr/ports/lang/fpc && make install && make clean
    

Другая возможность заключается в использовании portinstall. 
    
    
    []# portinstall fpc
    

Как только fpc установлен, вы можете проверить его работоспособность просто выполнив команду 
    
    
    []$ fpc
    

### Установка Free Pascal из исходных файлов в Linux/BSD

Для эффективности вам нужно 

Если у вас есть файл со всеми исходниками FPC или обоих (FPC и Lazarus): 

  1. напр., для fpc (последний снапшот на момент правки статьи) предпочтителен экспорт с [ftp](<ftp://ftp.freepascal.org/fpc/snapshot/v33/>) (без SVN/dirs).
  2. снапшот исходника Lazarus.


  * начальный компилятор, последняя версия релиза (fpc 3.0.4 на момент правки статьи) является единственной гарантированной версией. Вы можете скачать загрузочный компилятор или использовать систему управления пакетами/программного обеспечения вашего дистрибутива, чтобы установить его



Процесс сборки FPC: 

  * Получить необходимые файлы (начиная с компилятора), исходный файл FPC или исходный svn-каталог
  * При использовании исходных файлов FPC: extract/de-tgz в рабочий каталог
  * Сборка: войдите в work/fpc/ и запустите


    
    
    # можно использовать:   
    export MAKE=`which make` ; echo $MAKE 
    $MAKE all OPT='-gl' PP=/path/to/startingcompiler-name-ppc386
    # $MAKE применяется в linux и gmake в BSD 
    # /path/to/ может быть пропущен, когда ppc386 прописан в путях поиска
    

  * Установка fpc. Снова [заходим] в work/fpc, запускаем


    
    
    $MAKE install PP=compiler/ppc386 PREFIX=$THEPREFIX
    #заменяем PP=compiler/ppc386 на соответствующий компилятор, если это не Intel x86
    #THEPREFIX= обычно это /usr/local или просто /usr (но, например, на NetBSD для портов это /usr/pkg)
    

  * Создаем симлинк:


    
    
    ln -s $THEPREFIX/lib/fpc/3.0.0/ppc386 $THEPREFIX/bin/ppc386
    

  * Устанавливаем исходники:


    
    
    $MAKE install sourceinstall PREFIX=$THEPREFIX
    

  * Создаем симлинк к исходникам по умолчанию для Lazarus:


    
    
    ln -sf $THEPREFIX/share/src/3.0.0/fpc /usr/share/fpcsrc
    

  * Настраиваем файл конфигурации fpc.cfg:


    
    
    $THEPREFIX/lib/fpc/3.0.0/samplecfg $THEPREFIX/lib/fpc/3.0.0 $ETCDIR
    

  * При желании можете проверить, выдает ли ppc386 -i (или какой-либо другой компилятор, используемый вашей архитектурой) [какую-либо] информацию, иначе будет предупреждение о том, что пользователю необходимо добавить $PREFIX/bin в его текущий путь. Попробуйте скомпилировать программу с -viwn и посмотрите, не выдаст ли это ошибку.



Примечание: 

  * Если вам нужны списки пакетов fpcmake, вам нужно сгенерировать или предоставить их самостоятельно ([с помощью] порта или в дополнительном архиве) в любом случае, выполнить фиктивную установку в /tmp/pack и определить файлы, установленные с 
        
        find . >ll
        



  * $THEPREFIX и $ETCDIR должен [иметь возможность] быть настраиваемым пользователем. В противном случае локальная установка невозможна.


  * BSDHIER=1 для всех команд make действуют соглашения об иерархии BSD.



### Установка Free Pascal в macOS

См [Установка Lazarus на macOS](<../en/Installing_Lazarus_on_macOS.md> "Installing Lazarus on macOS")

## Установка Lazarus

### Установка Lazarus в Linux

#### Ubuntu / Debian

Рекомендуется использовать установщик обновлений [fpcUP](<../en/fpcup.md> "fpcup") для начинающих пользователей Lazarus, который устанавливает FPC и Lazarus за один раз в единую структуру подкаталогов (~/development). 

* * *

([Прим.перев.](</User:Zoltanleo> "User:Zoltanleo"): а еще лучше использовать форк этой утилиты [fpcupdeluxe](<../en/fpcupdeluxe.md> "fpcupdeluxe") от [LongDirtyAnimAlf](<https://github.com/LongDirtyAnimAlf>), последний релиз которой всегда можно скачать [здесь](<https://github.com/LongDirtyAnimAlf/fpcupdeluxe/releases>)) 

* * *

Чтобы получить текущую рабочую установку Lazarus, загрузите файлы .deb для Free Pascal и Lazarus из репозитория SourceForge. Вот как: [Получение Lazarus из нашего репозитория Ubuntu](<../en/Lazarus_release_version_for_Ubuntu.md> "Lazarus release version for Ubuntu"). 

Обратите внимание, что установка из исходных кодов Ubuntu по умолчанию не приведет к установке Free Pascal Source Libraries - используйте метод выше. 

[![Note-icon.png](https://wiki.freepascal.org/images/b/be/Note-icon.png)](</File:Note-icon.png>)

**Примечание:** по крайней мере в Linux Ubuntu команда для запуска Lazarus из консоли - [startlazarus](<../en/startlazarus.md> "startlazarus"). Иначе, если вы установили его из пакета Debian, у вас должен быть пункт меню Lazarus в разделе Application/Programming(«Приложение/Программирование»). (Проблема: существует неоднозначность с программой, также называемой "lazarus" из пакета "tct", доступного для Ubuntu).

  


##### Простой способ сборки из deb-пакетов

Самый простой способ получить текущую рабочую установку Lazarus - это загрузить свои собственные пакеты .deb, следуя инструкциям отсюда: [Как настроить хранилище FPC и Lazarus Ubuntu](<../en/How_to_setup_a_FPC_and_Lazarus_Ubuntu_repository.md> "How to setup a FPC and Lazarus Ubuntu repository")

##### Установка с использованием rpms

Следующим самым простым способом установки является установка RPM-пакетов для Free Pascal и Lazarus из репозитория SourceForge. 

Вам необходимо загрузить выбранные версии следующих компонентов: 

  * компилятор (например, fpc-3.0.4-1.i686.rpm)
  * исходный код библиотеки pascal (например, fpc-src-3.0.4-1.i686.rpm)
  * Lazarus (например, lazarus-1.8.4-0.i686.rpm).



Удалите старые пакеты: 
    
    
    rpm -ev lazarus
    rpm -ev fpc
    rpm -ev fpc-src
    

Установите новые пакеты: 
    
    
    rpm -ivh fpc-*
    rpm -ivh lazarus-*
    

##### Установка на Raspbian Wheezy

Raspbian - это специальная версия Debian для компьютера Raspberry Pi размером с кредитную карту. См. [Lazarus on Raspberry Pi](<../en/Lazarus_on_Raspberry_Pi.md> "Lazarus on Raspberry Pi") для получения детальной информации. 

#### Установка на Mandriva

##### Lazarus 0.9.30 на Mandriva 2010.

Установите как указано ниже, однако при компиляции программы вы можете получить два сообщения об ошибках, сообщающих, что вам не хватает pixbuf 2.0 и lgtk-x11-2.0. Чтобы исправить это, установите libdgk_pixbuf2.0_0-devel и libgtk + 2.0_0-devel из установщика программного обеспечения. 

#### Установка на Slackware

##### Установка Lazarus на Slackware 13.0

Нет реальной разницы от версий slackware 12.2 или 12.0, поэтому описанная ниже процедура должна работать точно так же. 

##### Установка Lazarus 0.9.30 для пользователей Slackware 12.0

Это работало в Slackware-12.0 на компьютере Pentium-3: 

  * Free Pascal Compiler (FPC) устанавливается в режиме SUPERUSER
  * Lazarus устанавливается в режиме USER
  * Компилятор FPC будет перекомпилирован
  * Исходный код IDE Lazarus загружается с сервера SVN.



\-- Загружаем "fpc-2.4.2.i386-linux.tar" в папку пользователя и устанавливаем его. Переходим в эту папку и вводим: 
    
    
    bash-3.1$ tar -xvf fpc-2.4.2.i386-linux.tar
    bash-3.1$ su 
    bash-3.1$ password:xxxxxx
    bash-3.1# sh install.sh
    ...
    (используем ответы по умолчанию для следующих 6 вопросов: нажимаем "Enter" 6 раз)
    ...
    bash-3.1# exit
    bash-3.2$ fpc
    free pascal compiler version 2.4.2 [] for i386 
    Copyright (c) 2010 ...
    ...
    ...
    

Вы установили компилятор. Возможно, вы захотите перекомпилировать его или нет, но в любом случае вам нужно будет загрузить исходный код в соответствии с требованиями IDE lazarus. 

\-- загружаем "fpc-2.4.2.source.tar.gz" во временную папку. Если вы не хотите перекомпилировать исходный код, просто пропустите этот раздел. Если вы хотите запустить свою собственную скомпилированную версию FPC, просто введите: 
    
    
    bash-3.1$ tar -xvzf fpc-2.4.2.source.tar.gz
    bash-3.1$ cd fpc-2.4.2
    bash-3.1$ make clean all
    bash-3.1$ su
    bash-3.1$ password: xxxxxxxx
    bash-3.1# make install
    bash-3.1# exit
    bash-3.1$ fpc
    free pascal compiler version 2.4.2 [...] for i386 
    Copyright (c) 2010 ...
    ...
    ...
    

Теперь у вас есть собственная скомпилированная версия. 

\-- Редактор Lazarus IDE должен видеть исходный код FPC. Даже если вы не хотите перекомпилировать FPC, вам нужен его исходный код. Однако для экономии места я сохраняю только чистую (не скомпилированную) копию исходного кода. Я снова начинаю с файла "fpc-2.4.2.source.tar.gz" и копирую его в папку /usr/local/src/FPC: 
    
    
    bash-3.1$ su
    bash-3.1$ password: xxxxxxxx
    bash-3.1# cp fpc-2.4.2.source.tar.gz /usr/local/src/
    bash-3.1# cd /usr/local/src/
    bash-3.1# tar -xvzf fpc-2.4.2.source.tar.gz
    bash-3.1# rm fpc-2.4.2.source.tar.gz
    bash-3.1# exit
    

Номер версии включен в название папки "fpc-2.4.2". Таким образом, я могу сохранить многие версии исходного кода компилятора и в конечном итоге переключаться между ними. 

\-- The lazarus IDE is kept in my USER "~/lazarus" folder and always compiled in USER mode. I usually download the "lazarus-0.9.30-0.tar.gz" file, but here we can also get the latest development version from the SVN server: 
    
    
    bash-3.1$ svn co http://svn.freepascal.org/svn/lazarus/trunk lazarus
    

This is for the first time you load it. Next time you will only need to type: 
    
    
    bash-3.1$ svn update
    

If you do not have SVN installed on your computer, here is how to get it quickly: 

\--Download the two files: "subversion-1.4.6.tar.gz" and "subversion-deps-1.4.5.tar.gz" (or later versions). Type: 
    
    
    bash-3.1$ tar -xvzf subversion-1.4.6.tar.gz
    bash-3.1$ tar -xvzf subversion-deps-1.4.6.tar.gz
    bash-3.1$ cd subversion-1.4.6
    bash-3.1$ ./configure
    bash-3.1$ make
    bash-3.1$ su
    bash-3.1$ password:xxxxxx
    bash-3.1# make install
    bash-3.1$ exit
    

\-- At this point you have the folder "~/lazarus" containing the source code. You should compile it very simply: 
    
    
      bash-3.1$ make clean all
    

After a few minutes, the compiler stops: 
    
    
    ...
    ...
    Linking ../Lazbuild
    987 linescompiled ...
    make [2] leaving ...
    make [1] leaving ...
    bash-3.1$
    

\-- Just type: 
    
    
    bash-3.1$ ./lazarus
    

WOW! You get a message: "Free Pascal sources not found". Just follow the instructions and indicate your Free Pascal Compiler source directory in the panel: "Environment->Options->Files". As explained earlier, on my computer this should point to "/usr/local/src/fpc-2.4.2". Note that when you change this folder, you should click on "Environment / Rescan_FPC_source_directory". 

Voila! 

#### Установка на openSUSE

##### Установка Lazarus 0.9.30 для пользователей openSUSE 11.1

Free Pascal Compiler requires:  
\- Gnu binutils (gnu as, gnu ld, gnu make) These utils can be installed by: 
    
    
    zypper in -t pattern devel_basis
    

Lazarus also requires these components: 1) glib2 devel 2) gtk2 devel 

**Important** : The lazarus rpm requires the gtk 2 version, not the version 1.2. 

These libraries can be installed by: 
    
    
    zypper -n install gtk2 glib2
    

Download these binary files (RPM) 

    

  * fpc-2.4.2-0.i686.rpm (yes i686 and not i386)
  * fpc-src-2.4.2-0.i686.rpm
  * lazarus-0.9.30-0.i686.rpm



install them opening a terminal session (mouse's right button -> Menu: Open in terminal) 
    
    
    rpm -Uvh fpc-2.4.2-0.i686.rpm
    rpm -Uvh fpc-src-2.4.2-0.i686.rpm
    rpm -Uvh lazarus-0.9.30-0.i686.rpm
    

#### Установка Lazarus 1.0.8 на Fedora Core 18

After a fresh install of Fedora Core 18 **yum** is installed, but fails with an error messages. 

  * Open **Software** , check for new software and update all packages.
  * Reboot.
  * Install packages **gtk2-devel** , **fpc** and **subversion**. Fedora Core 18 already has fpc 2.6.2.
  * Open a Terminal and do


    
    
    svn co svn+<ssh://svn.freepascal.org/FPC/svn/lazarus/tags/lazarus_1_0_8> lazarus
    cd lazarus
    make bigide
    ./lazarus
    

#### Установка Lazarus на Scientific Linux

Scientific Linux is an RPM-based distribution focussing on science and research. See [Scientific Linux](<../en/Scientific_Linux.md> "Scientific Linux") for details. 

#### Установка из исходных файлов

If you prefer to install from source and compile the files yourself, follow these instructions. Because the whole lazarus stuff is installed into one directory, uninstall is very easy and you don't need to be root to install lazarus. You can get tgz files for fpc, fpcsrc and lazarus from the downloads section or you can download it directly via svn. 

Here is an example of installing 0.9.28 to Ubuntu 6.06. If you understand Linux commands and bash script, you can get what steps are needed. Just copy the script (change the version number when new version has been released), paste it into a text editor, and save it as something like "install_lazarus.sh". Give it execute permission, and run it in a console. 

[![Note-icon.png](https://wiki.freepascal.org/images/b/be/Note-icon.png)](</File:Note-icon.png>)

**Примечание:** In this example, fpc is installed in /opt. So when prompted '**Install prefix'** , enter '/opt/fpc'. 
    
    
    #!/bin/sh
      
    #installing required packages
    sudo apt-get install build-essential
    sudo apt-get install libgtk2.0-dev
    sudo apt-get install libgdk-pixbuf-dev
      
    #installing Free Pascal source
    cd /opt
    sudo wget http://nchc.dl.sourceforge.net/lazarus/fpc-src-2.4.2.source.tgz
    sudo tar -xvf fpc-src-2.4.2.source.tgz
    sudo mv fpc fpcsrc
      
    #installing Free Pascal
    sudo mkdir fpc
    cd fpc
    sudo wget http://nchc.dl.sourceforge.net/lazarus/fpc-2.4.2.i686-linux.tar
    sudo tar -xvf fpc-2.4.2.i686-linux.tar
    echo "Enter '/opt/fpc' when prompted 'Install prefix'"
    sudo sh install.sh
      
    #adding fpc path to the PATH
    echo "#FPC PATH" >> ~/.bash_profile
    echo "if [ -d /opt/fpc/bin ] ; then" >> ~/.bash_profile 
    echo PATH=/opt/fpc/bin:"${PATH}" >> ~/.bash_profile
    echo "fi" >> ~/.bash_profile
      
    #installing Lazarus
    cd ../
    sudo wget http://nchc.dl.sourceforge.net/lazarus/lazarus-0.9.30-0.tar.gz
    # sudo wget https://downloads.sourceforge.net/project/lazarus/Lazarus%20Zip%20_%20GZip/Lazarus%201.8.0RC5/lazarus-1.8.0-RC5.tar.gz
    sudo tar -zxvf lazarus-0.9.30-0.tar.gz
    PATH=/opt/fpc/bin:"${PATH}"
    sudo chmod -R 777 lazarus
    cd lazarus
    make clean all
    ./lazarus
    

[![Note-icon.png](https://wiki.freepascal.org/images/b/be/Note-icon.png)](</File:Note-icon.png>)

**Примечание:** You have to manually set fpc-src path in the Environmental Options.

##### Загрузка исходных файлов Lazarus

Both the Lazarus and FPC source code reside in SVN/subversion repositories. SVN provides an easy way to update your sources by only downloading the changes. This is the recommended way and saves you a lot of time. A connection to the internet is needed for this, but you don't need to be root. 

Please note these instructions are for subversion. 

Lazarus does not need any special permissions, neither during installation nor at runtime. 

If you decide to use TortoiseSVN, remember to check "command line client tools" during its installation if you want Lazarus to show the SVN revision number in the About dialog.

Now getting the sources
    
    
    
     svn checkout http://svn.freepascal.org/svn/lazarus/trunk/ lazarus
    

(replace the last lazarus with any other dir where you want to place your sources) 

On subsequent occasions, to update simply type 
    
    
     svn update lazarus
    

For more information on Subversion, see: <http://subversion.tigris.org/>

##### Компиляция и запуск

Whether you checkout from cvs or svn, the next step is: 

compile lazarus
    
    
    
     cd lazarus
     make  (gmake on BSD)
    

If fpc is installed correctly, the compilation should work without problems. If not, see FAQ. 

Start lazarus
    
    
     ./lazarus
    

The IDE should start. If you started lazarus in a terminal, you can see some notes about missing settings. This is normal at first start. The IDE automatically tries to find out where the Free Pascal compiler and its sources are installed by searching in the most common directories. 

Check the paths
    
    Use the IDE menu to go to 

    Environment -> Environment Options -> Files

The 'FPC Source directory' should point to your fpc source directory. This directory normally ends with /fpc/ or /fpcsrc/ (e.g. /usr/src/fpcsrc or /home/username/freepascal/fpc) and contains directories like 'compiler', 'docs', 'fcl', 'rtl' and 'packages'. 

See here for the documentation about this dialog: [IDE Options](<../en/IDE_Window__Environment_Options.md> "IDE Window: Environment Options"). 

Hint
    

To update lazarus you can use 
    
    
     svn update lazarus
    

then for either update pathway: 
    
    
     make clean all   (gmake on BSD)
    

This will rebuild lazarus and create an IDE without lazarus packages. To link your installed packages do **after** the above: 
    
    
     ./lazbuild --build-ide=
    

You may have to append other options if for example you use a custom config directory (ie. add --pcp="C:\Documents and Settings\<USER>\Local Settings\Application Data\lazarus-tests"). See [lazbuild](<../en/lazbuild.md> "lazbuild"). 

#### Установка Lazarus в Debian GNU/Linux

There are preliminary Debian packages for lazarus available for download. They are not the latest versions, however. Make sure you read /usr/share/doc/lazarus/README.Debian carefully before you start using it. Feedback is needed and appreciated; please send your comments to Carlos Laviola <claviola@debian.org>. 

Note that for a fully working Lazarus install, no older or incompatible versions of, for example, the fpc source or fpc compiler must be installed. Remove them by typing 
    
    
    dpkg -r <package name>
    

without .deb extension. And then install the newest versions as described. 

### Установка Lazarus в Windows

Текущие стабильные версии исполняемых файлов Lazarus для Windows устанавливаются очень легко и могут работать 'из коробки'. 

Исключение составляют Win98 и ME, для которых необходимо указывать специальный флаг при компиляции. Смотрите Установка из исходных файлов. 

#### Установка портативной версии Lazarus на USB-диск

It is even possible to install the whole Lazarus/FPC package on a portable USB drive (capacity at least 256 MB), for use in environments where you are not allowed to install software on your Windows workstation or where you haven't got administrator privileges. You do have to be a little careful about adjusting the paths in the compiler and environment options and the fpc.cfg file. It may also be necessary to keep the directory for test compilation on your portable drive. 

<< Q:DOES ANYONE KNOW HOW TO SET UP RELATIVE PATHS IN THESE TAGS AND FILES, SO THAT THE ADDRESSING WORKS WHEN YOU MOVE THE USB DEVICE TO ANOTHER MACHINE WHERE IT HAS A DIFFERENT DRIVE LETTER? 

A: This is what I do. It's relatively convoluted, but it's the best solution I've found. I have a "bin" directory on my USB drive, where I have several scripts and utilities installed. Inside that directory is a batch file called "setenv.bat" which sets an environment variable called THUMBDRIVE. It is set by using this command in the batch file: 
    
    
    set THUMBDRIVE=%CD:~0,2%
    

This is used in setenv.bat to set some paths to other things I have installed on the USB drive. I also have a link in the root directory of the thumb drive with this property: 
    
    
    %SystemRoot%\system32\cmd.exe /k bin\setenv
    

so that when I click on that link when the thumb drive folder is diplayed after inserting it, it will open a command prompt at the thumb drive with the environment variables set from setenv.bat. 

Also inside the bin directory is [sed](<http://sed.sf.net>) (the actual binary is one I obtained from the mingw distribution). So I created another batch file called fixlaz.bat which takes one argument, the drive letter which is currently in the Lazarus/fpc settings files that you want to change (note that this is the previous drive letter the last time you ran fixlaz.bat, not the current one of your USB drive which fixlaz.bat already knows). You will need to create this batch file to fit where you installed Lazarus in the root directory structure of the drive if you didn't install it directly in the root folder, and then repeat these lines also for the editoroptions.xml and fpc.cfg files (fpc.cfg is the the fpc bin directory, which might be buried deep in the lazarus folder): 
    
    
    copy %THUMBDRIVE%\lazarus\environmentoptions.xml %THUMBDRIVE%\lazarus\environmentoptions.bak
    sed -e 's/%1/%THUMBDRIVE%/g' %THUMBDRIVE%\lazarus\environmentoptions.bak > %THUMBDRIVE%\lazarus\environmentoptions.xml
    

So to use it, I would type at the command prompt of the USB drive: 
    
    
    fixlaz G:
    

if "G:" was the previous drive letter used the last time I ran it. This will then scan the file(s) and replace "G:" with the current drive letter of the USB drive, which is in the %THUMBDRIVE% environment variable (after running setenv.bat). Note that you could write it to save the current drive letter in a separate file, so that you wouldn't have to remember it yourself the next time. But this works well enough for me right now. >>

The binary package is available for Linux and Windows from 

<http://sourceforge.net/project/showfiles.php?group_id=89339>

Download the latest release (currently Lazarus-0.9.30-0-win32.exe) and launch the application. You will be taken through a typical Windows installation, in which the FPC compiler and source libraries are installed within the same directory structure as Lazarus, and the IDE should launch and operate without significant problems, provided you have uninstalled(!!!) any previous version of Lazarus and/or FPC (often found in the C:\pp directory). 

You can also use a Lazarus Snapshot. For download locations see [Lazarus Snapshots Downloads](<../en/Lazarus_Snapshots_Downloads.md> "Lazarus Snapshots Downloads"). 

Tip: It's perhaps a good idea to reboot your Windows after you installed Lazarus and before you try to install additional lazarus components as zeoslib fore example. 

#### Установка из исходных файлов

If you prefer to install from sources, then follow these instructions. 

Please note these instructions are for SubVersion. 

Open a command prompt window. Start->Run...>CMD or choose MS-DOS icon. You will use this window to enter the commands below 

You have to download the lazarus source from one of the [snapshots servers](<../en/Lazarus_Snapshots_Downloads.md> "Lazarus Snapshots Downloads"). Then unzip it to c:\lazarus for example [below called $(LazarusDir)]. 

Or you use SVN (example for text mode SVN; adapt to GUI tools such as TortoiseSVN if you want to): 
    
    
    mkdir c:\lazarus
    cd /d c:\lazarus
    svn checkout http://svn.freepascal.org/svn/lazarus/trunk/ c:\lazarus
    

You have to install at least the latests stable FPC version (e.g. FPC 2.6.4, but an FPC 3.1.1 snapshot is also possible). 

  
Type (replace $(LazarusDir) with the path you have unzipped/checked out Lazarus; replace 
    
    
    cd /d $(LazarusDir)
    rem Of course change the first path variable to
    rem the path of your FPC compiler
    set path=c:\freepascal\bin\i386-win32;$(LazarusDir)
    make
    

  * Win9x: use make OPT="-dWIN9XPLATFORM" (Lazarus trunk (and upcoming 1.4)) otherwise the lazarus.exe will not be able to run on this platform.



If this works, you can type: lazarus.exe. 

You can compile examples also: 
    
    
    cd /d $(LazarusDir)\examples
    make
    

~~If you extracted lazarus to another drive, eg.: d:\lazarus, tt can happen that you need the gnu make utility to make it. If you have it, you can take its path to set path=...; <gmakepath> but it is simpler not to choose drive d:\~~

#### Установка из исходных файлов с использованием стабильной версии

An alternative version of the instructions above. 

1> First of all install the latest stable Lazarus to obtain a good starting FPC, for example in C:\lazarus_1_4 

2> Now use TortoiseSVN to checkout <http://svn.freepascal.org/svn/lazarus/trunk/> into c:\lazarus 

3> Make the following C:\lazarus\build.bat file: 

Replace $(LazarusDir) with your Lazarus did and make sure the FPC version number matches 
    
    
    SET PATH=$(LazarusDir)\fpc\2.6.4\bin\i386-win32\
    make bigide
    

Now create a shortcut in your desktop to start Lazarus and put the following command to start Lazarus which will make sure that it separates the config files from the stable and the SVN versions: 
    
    
    $(LazarusDir)\startlazarus.exe --pcp=$(LazarusDir)\configdir
    

Always start Lazarus from this shortcut, never directly from the executable. At the first time you start Lazarus configure you FPC dir, FPC sources dir and Lazarus dir. 

#### Сборка Lazarus в Win98 и WinME

Так как среда разработки Lazarus по умолчанию вызывает функцию "CreateToolhelp32Snapshot", которая отсутствует на платформе Win9x, IDE не запустится в Win9x "из коробки". 

Для того, чтобы сделать это запустите пересборку IDE Lazarus с ключом "-dWIN9XPLATFORM". 

### Установка Lazarus в FreeBSD

Следующие инструкции относятся только для версии FreeBSD 9 и выше. 

#### via Ports tree

The latest version of Lazarus available in the FreeBSD port tree, is v 1.6.4 We can use that to install Lazarus. 
    
    
    []# cd /usr/ports/editors/lazarus && make install clean clean-depends
    

If you start Lazarus IDE and you get a messages about missing source files, then go to **Environment - Environment Options - Files Tab: FPC source library** and enter the directory of the FPC source files. It can be obtained from /usr/ports/distfiles/freepascal 

#### via pkgng
    
    
    []# pkg install editors/lazarus
    

At this point Lazarus might complain about missing source files. If you don't have them:- 
    
    
     mkdir /usr/ports/distfiles
     mkdir /usr/ports/distfiles/freepscal
     cd /usr/ports/distfiles/freepascal
     wget <http://distcache.freebsd.org/ports-distfiles/freepascal/fpc-3.0.2.source.tar.gz>
     tar xvfz fpc-3.0.2.source.tar.gz
    

Change the 3.0.2 to whatever version of fpc you have installed. Then go to **Environment - Environment Options - Files Tab: FPC source library** and enter 
    
    
     /usr/ports/distfiles/freepascal/fpc-3.0.2
    

If when you try to compile something you get this error 
    
    
     Error: resource compiler "fpcres" not found, switching to external mode
    

Then you can install fpcres like this 
    
    
    []# pkg install lang/fpc-utils
    

#### via Lazarus repository

This option will often be used if you want to follow Lazarus Trunk, a Fixes branch, or some other release (eg: compiling from a source tarball). 

  * Use the SubVersion or Git repositories to checkout a copy of the source code you want, or unpack a downloaded source archive into a suitable location
  * The readme.txt file in Lazarus directory mentions 'make clean all'. This works if you are using Linux. Under FreeBSD 9.1 I had to replace 'make' with 'gmake'.


    
    
     cd /patch/to/lazarus_source
     gmake clean all
    

### Установка Lazarus в PC-BSD 1.0rc1+

You can install Lazarus on PB-BSD by simply downloading the Lazarus PBI from [PBI Dir](<http://www.pbidir.com/>)

Note that you must install glib* port from /usr/port/devel/glib* or glib packages by pkg_add -r glib12 glib20. I will fix this in new PBI releases. 

**[other OpenBSD/NetBSD/DragonFlyBSD goes here]**

### Установка Lazarus в macOS

See [Installing Lazarus on macOS](<../en/Installing_Lazarus_on_macOS.md> "Installing Lazarus on macOS"). 

### Установка Lazarus в Haiku

Lazarus requires Qt under Haiku. Qt is not installed by default under Haiku. You need to install package available from this site : <http://qt-haiku.ru/>

Currently, there is no binary package to install Lazarus. 

You will have to compile Lazarus from sources. 

Detailed instructions to build Lazarus under Haiku are available here : [Installing Lazarus on Haiku](<../en/Installing_Lazarus_on_Haiku.md> "Installing Lazarus on Haiku")

# Установка нескольких версий Lazarus

Please see [Multiple Lazarus](<../en/Multiple_Lazarus.md> "Multiple Lazarus") for details on having more than one Lazarus version installed on one system. We cover issues that can arise due to multiple Lazarus installs here, because they can also happen when installing over a previous version. 

# Устранение проблем

These hints should (hopefully) be applicable across platforms; please adjust paths etc. according to your platform. Also, problems and solutions might be mentioned in the [Lazarus_Faq](<../en/Lazarus_Faq.md> "Lazarus Faq"). 

## Isolating the component

There may be multiple problems when installing Free Pascal and Lazarus. If you have problems with starting Lazarus or compiling with it, first make sure your underlying FPC installation works, e.g. by trying to compile a simple example program. 

## Расположение установленных компонентов

On Linux/Unix/macOS, package/distribution provided installations will often install FPC in /bin or /usr/bin, with the libraries in /lib or /usr/lib, and probably the configuration in /etc/fpc.cfg Source (Subversion) downloads often install in /usr/local/bin (libraries in /usr/local/lib). 

## Отсутствие или повреждение файла fpc.cfg

Файл fpc.cfg является файлом настроек FPC. В системе Windows его можно найти в директории с исполняемыми файлами компилятора fpc. On Unix/Linux/macOS, it is first searched in /etc/fpc.cfg, then Only some problems that often occur are covered. Please see the Free Pascal User's guide, e.g. chapter Compiler usage/configuration file for more details, including the search order for fpc.cfg 

Если его нет, FPC не скомпилирует вашу программу должным образом. 

### Автоматическое создание fpc.cfg

Вы можете попытаться создать fpc.cfg с помощью специальной утилиты: 
    
    
    rem Измените путь к директории с исполняемыми файлами FPC
    cd /d C:\freepascal\fpc\2.4.5\bin\i386-win32
    rem Создание fpc.cfg; basepath - это "корневая" директория fpc
    fpcmkcfg -d basepath=C:\freepascal\fpc\2.4.5 -o .\fpc.cfg
    

[![Note-icon.png](https://wiki.freepascal.org/images/b/be/Note-icon.png)](</File:Note-icon.png>)

**Примечание:** earlier instructions had \bin\i386-win32 appended to the basepath. This is not needed since at least FPC 2.4.4.

### Ручное создание fpc.cfg

If the above doesn't work for you, you can copy/paste the code below to a new file named fpc.cfg to your fpc executable directory (or ~/.fpc.cfg or other location). Modify the paths so they match your situation. 

$FPCVERSION is used to automatically determine the version of FPC being used for the compile. Using this variable allows this single fpc.cfg to be used with multiple fpc versions that have the same path structure making it easier to maintain multiple FPC installs. 
    
    
    #
    # Config file generated by fpcmkcfg on 24/12/2010 - 18:17:42
    # Example fpc.cfg for Free Pascal Compiler
    #
    
    # ----------------------
    # Defines (preprocessor)
    # ----------------------
    
    #
    # nested #IFNDEF, #IFDEF, #ENDIF, #ELSE, #DEFINE, #UNDEF are allowed
    #
    # -d is the same as #DEFINE
    # -u is the same as #UNDEF
    #
    
    #
    # Some examples (for switches see below, and the -? helppages)
    #
    # Try compiling with the -dRELEASE or -dDEBUG on the commandline
    #
    
    # For a release compile with optimizes and strip debuginfo
    #IFDEF RELEASE
    -O2
    -Xs
    #WRITE Compiling Release Version
    #ENDIF
    
    # For a debug version compile with debuginfo and all codegeneration checks on
    #IFDEF DEBUG
    -glh
    -Crtoi
    #WRITE Compiling Debug Version
    #ENDIF
    
    # ----------------
    # Parsing switches
    # ----------------
    
    # Pascal language mode
    #      -Mfpc      free pascal dialect (default)
    #      -Mobjfpc   switch some Delphi 2 extensions on
    #      -Mdelphi   tries to be Delphi compatible
    #      -Mtp       tries to be TP/BP 7.0 compatible
    #      -Mgpc      tries to be gpc compatible
    #      -Mmacpas   tries to be compatible to the macintosh pascal dialects
    #
    # Turn on Object Pascal extensions by default
    #-Mobjfpc
    
    # Assembler reader mode
    #      -Rdefault  use default assembler
    #      -Ratt      read AT&T style assembler
    #      -Rintel    read Intel style assembler
    #
    # All assembler blocks are AT&T styled by default
    #-Ratt
    
    # Semantic checking
    #      -S2        same as -Mobjfpc
    #      -Sc        supports operators like C (*=,+=,/= and -=)
    #      -Sa        include assertion code.
    #      -Sd        same as -Mdelphi
    #      -Se<x>     compiler stops after the <x> errors (default is 1)
    #      -Sg        allow LABEL and GOTO
    #      -Sh        Use ansistrings
    #      -Si        support C++ styled INLINE
    #      -SI<x>     set interface style to <x>
    #         -SIcomCOM compatible interface (default)
    #         -SIcorbaCORBA compatible interface
    #      -Sm        support macros like C (global)
    #      -So        same as -Mtp
    #      -Sp        same as -Mgpc
    #      -Ss        constructor name must be init (destructor must be done)
    #
    # Allow goto, inline, C-operators, C-vars
    -Sgic
    
    # ---------------
    # Code generation
    # ---------------
    
    # Uncomment the next line if you always want static/dynamic units by default
    # (can be overruled with -CD, -CS at the commandline)
    #-CS
    #-CD
    
    # Set the default heapsize to 8Mb
    #-Ch8000000
    
    # Set default codegeneration checks (iocheck, overflow, range, stack)
    #-Ci
    #-Co
    #-Cr
    #-Ct
    
    # Optimizer switches for i386 compiler
    # -Os        generate smaller code
    # -O1        level 1 optimizations (quick optimizations)
    # -O2        level 2 optimizations (-O1 + slower optimizations)
    # -O3        level 3 optimizations (same as -O2u)
    # -Oa=N      set alignment to N
    # -OoX       switch on optimalization X. 
    # -OoNOX     switch off optimalization X.
    #               X is one of REGVAR UNCERTAIN STACKFRAME PEEPHOLE ASMCSE LOOPUNROLL
    # -OpCPU     set target processor.
    #             CPU is one of 386, PENTIUM, PENTIUM2, PENTIUM3, PENTIUM4, PENTIUMM 
    
    
    # -----------------------
    # Set Filenames and Paths
    # -----------------------
    
    # Both slashes and backslashes are allowed in paths
    
    # path to the messagefile, not necessary anymore but can be used to override
    # the default language
    #-FrC:\freepascal\fpc\$FPCVERSION/msg/errore.msg
    #-FrC:\freepascal\fpc\$FPCVERSION/msg/errorn.msg
    #-FrC:\freepascal\fpc\$FPCVERSION/msg/errores.msg
    #-FrC:\freepascal\fpc\$FPCVERSION/msg/errord.msg
    #-FrC:\freepascal\fpc\$FPCVERSION/msg/errorr.msg
    
    #IFDEF FPCAPACHE_1_13
    -FuC:\freepascal\fpc\$FPCVERSION/units/$FPCTARGET/httpd13/
    #ELSE
    #IFDEF FPCAPACHE_2_0
    -FuC:\freepascal\fpc\$FPCVERSION/units/$FPCTARGET/httpd20
    #ELSE
    -FuC:\freepascal\fpc\$FPCVERSION/units/$FPCTARGET/httpd22
    #ENDIF
    #ENDIF
    
    # searchpath for units and other system dependent things
    -FuC:\freepascal\fpc\$FPCVERSION/units/$FPCTARGET/
    -FuC:\freepascal\fpc\$FPCVERSION/units/$FPCTARGET/*
    -FuC:\freepascal\fpc\$FPCVERSION/units/$FPCTARGET/rtl
    
    # path to the gcclib
    
    
    # searchpath for libraries
    #-FlC:\freepascal\fpc\$FPCVERSION/lib
    #-Fl/lib;/usr/lib
    
    # searchpath for tools
    -FDC:\freepascal\fpc\$FPCVERSION/bin/$FPCTARGET
    
    #IFNDEF CPUI386
    #IFNDEF CPUAMD64
    #DEFINE NEEDCROSSBINUTILS
    #ENDIF
    #ENDIF
    
    #IFNDEF Win32
    #DEFINE NEEDCROSSBINUTILS
    #ENDIF
    
    # binutils prefix for cross compiling
    #IFDEF FPC_CROSSCOMPILING
    #IFDEF NEEDCROSSBINUTILS
    -XP$FPCTARGET-
    #ENDIF
    #ENDIF
    
    
    # -------------
    # Linking
    # -------------
    
    # generate always debugging information for GDB (slows down the compiling
    # process)
    #      -gc        generate checks for pointers
    #      -gd        use dbx
    #      -gg        use gsym
    #      -gh        use heap trace unit (for memory leak debugging)
    #      -gl        use line info unit to show more info for backtraces
    #      -gv        generates programs tracable with valgrind
    #      -gw        generate dwarf debugging info
    #
    # Enable debuginfo and use the line info unit by default
    #-gl
    
    # always pass an option to the linker
    #-k-s
    
    # Always strip debuginfo from the executable
    -Xs
    
    
    # -------------
    # Miscellaneous
    # -------------
    
    # Write always a nice FPC logo ;)
    -l
    
    # Verbosity
    #      e : Show errors (default)       d : Show debug info
    #      w : Show warnings               u : Show unit info
    #      n : Show notes                  t : Show tried/used files
    #      h : Show hints                  m : Show defined macros
    #      i : Show general info           p : Show compiled procedures
    #      l : Show linenumbers            c : Show conditionals
    #      a : Show everything             0 : Show nothing (except errors)
    #      b : Show all procedure          r : Rhide/GCC compatibility mode
    #          declarations if an error    x : Executable info (Win32 only)
    #          occurs
    #
    # Display Info, Warnings, Notes and Hints
    -viwn
    # If you don't want so much verbosity use
    #-vw
    

### Testing and batch file/shortcut usage

You can also explicitly specify a fpc.cfg to use:<bash>fpc -n -@/path/to/fpc.cfg</bash>. Handy for testing, creating shortcuts/shell scripts, having multiple FPC installations etc. 

If that does not work, you can try to create fpc.cfg manually: copy over an existing fpc.cfg from a working installation and change the paths. 

## Mixing FPC versions

If your FPC cannot find its sources/ppu files while fpc.cfg has the right paths, another cause could be that the fpc executable is a different version than the (link to) ppc<architecture> compiler. 

When compiling with fpc, fpc starts e.g ppc386 to compile for x86 targets. Please make sure fpc and ppc<architecture> are compiled by the same version/sourced from the same package. 

## Lazarus can't find sources

There may be a message saying Lazarus can't find the sources; if so, go to the menu at the top and select Environment -> Environment options and insert the name of the directory where you expect to find your lazarus files (eg ~/FreePascal/lazarus), your compiler (e.g. /usr/local/bin/ppc386), the Free Pascal source directory (e.g. /usr/local/lib/fpc/$version/fpc or a directory in your own space e.g. ~/FreePascal/fpc). Then close the dialog and start programming. 

## Пути к файлам в настройках Lazarus и проекта

Учтите, что вы можете установить пути (для компилятора, модулей FPC и т.д.) в общих настройках параметров. Параметры, которые вы укажете в _Параметрах проекта_ , могут переопределять параметры, используемые для каждого проекта. Если вы испытываете проблемы, связанные с путями в данном проекте, но не в других проектах, вам необходимо проверить, не ошиблись ли вы в переопределении параметров в общих настройках. 

# Установка старых версий

Смотрите [Installation hints for old versions](<../en/Installation_hints_for_old_versions.md> "Installation hints for old versions")

---

_Source: [https://wiki.freepascal.org/Installing_Lazarus/ru](https://web.archive.org/web/20230128012726/https://wiki.freepascal.org/Installing_Lazarus/ru)_
