# Getting Lazarus

│ **[English (en)](<../en/Getting_Lazarus.md>)** │  **русский (ru)** │

## Contents

  * 1 Загрузка и установка релизной версии Lazarus
    * 1.1 Из SourceForge
    * 1.2 Конкретные для платформы
  * 2 Получение SVN клиента
    * 2.1 TortoiseSVN клиент
    * 2.2 Другие клиенты
  * 3 Получение Lazarus с сервера Subversion
    * 3.1 Версия Lazarus для разработчиков
    * 3.2 Исправленная ветка последней версии Lazarus
    * 3.3 Обновление исходников позже
  * 4 Сценарии
  * 5 Сборка и запуск Lazarus
    * 5.1 Использование командной строки
    * 5.2 Цели Make
      * 5.2.1 Помощь по make
    * 5.3 Параметры Make
  * 6 Просмотр исходников репозитория с помощью веб-браузера
  * 7 Распространение Lazarus
  * 8 См.также



## Загрузка и установка релизной версии Lazarus

### Из SourceForge

Бинарные выпуски для различных платформ доступны через площадку [Lazarus Sourceforge](<https://sourceforge.net/projects/lazarus/files/>). 

Для тех, у кого заблокирован SF, релизы Lazarus от Sourceforge отзеркалированы на: 

  * <ftp://ftp.freepascal.org/pub/lazarus/releases/>
  * и позже (через некоторое время для синхронизации) на <http://michael-ep3.physik.uni-halle.de/Lazarus/releases/>
  * и <http://mirrors.iwi.me/lazarus/>



### Конкретные для платформы

Получить Lazarus для Windows - используйте ссылку Sourceforge выше. 

[Получить Lazarus для ReactOS](<ReactOS.md> "ReactOS/ru")

[Получить Lazarus для Ubuntu](<../en/Lazarus_release_version_for_Ubuntu.md> "Lazarus release version for Ubuntu")

[Получить Lazarus для Suse](<../en/Lazarus_release_version_for_Suse.md> "Lazarus release version for Suse")

[Получить Lazarus для Mandriva](</index.php?title=Lazarus_release_version_for_Mandriva&action=edit&redlink=1> "Lazarus release version for Mandriva \(page does not exist\)")

[Получить Lazarus для Scientific Linux](<../en/Scientific_Linux.md> "Scientific Linux")

[Получить Lazarus для Raspberry Pi (Raspbian)](<../en/Lazarus_on_Raspberry_Pi.md> "Lazarus on Raspberry Pi")

[Получить Lazarus для Mac](<../en/Installing_Lazarus_on_macOS.md> "Installing Lazarus on macOS")

[Lazarus on Solaris](<../en/Lazarus_on_Solaris.md> "Lazarus on Solaris")

## Получение SVN клиента

### TortoiseSVN клиент

Клиент TortoiseSVN предназначен только для Windows. Вы можете скачать его с <http://tortoisesvn.tigris.org/>. 

  


* * *

[Прим.перев.](</User:Zoltanleo> "User:Zoltanleo"): На момент перевода статьи загрузить TortoiseSVN можно [здесь](<https://tortoisesvn.net/downloads.html>). Также можно скачать и установить руссификаторы для [x32](<https://osdn.net/projects/tortoisesvn/storage/1.11.0/Language%20Packs/LanguagePack_1.11.0.28416-win32-ru.msi>) и [x64](<https://osdn.net/projects/tortoisesvn/storage/1.11.0/Language%20Packs/LanguagePack_1.11.0.28416-x64-ru.msi>) версии программы. 

Приятным бонусом к руссификатору идет и русская справка в chm-формате   


[![TortoiseSVN chm rus.png](https://wiki.freepascal.org/images/8/8a/TortoiseSVN_chm_rus.png)](</File:TortoiseSVN_chm_rus.png>)  


* * *

[Эта страница](<../en/TortoiseSvn.md> "TortoiseSvn") демонстрирует, как загрузить и обновить исходный код Lazarus SVN, но таким же образом его можно использовать для исходного кода FPC. 

### Другие клиенты

Клиент командной строки SVN доступен на официальном SVN [веб-сайте](<http://subversion.apache.org/>) для многих платформ. 

В Linux рекомендуется установить _subversion_ , используя систему управления пакетами, предлагаемую вашим дистрибутивом. Например: 
    
    
     sudo apt-get install subversion
    

## Получение Lazarus с сервера Subversion

### Версия Lazarus для разработчиков

Чтобы получить Lazarus в первый раз, используйте командную строку: 
    
    
    svn co https://svn.freepascal.org/svn/lazarus/trunk lazarus
    

Тогда в текущем каталоге создастся новый каталог "lazarus". 

### Исправленная ветка последней версии Lazarus

Это даст вам последний релиз плюс все коммиты с исправлением ошибок с тех пор. На момент написания статьи последний [стабильный] релиз 1.8.x. 
    
    
    svn co https://svn.freepascal.org/svn/lazarus/branches/fixes_1_8/ lazarus_1_8
    

Позже просто замените последнюю числовую часть номерами последней версии. 

### Обновление исходников позже

Откройте терминал/командную строку, перейдите в каталог lazarus и введите: 
    
    
     svn update
    

Используйте вышеупомянутые URL с другими клиентами SVN и обновляйте команды соответственно. 

Чтобы скомпилировать среду, см. [Сборка и запуск Lazarus](<Getting_Lazarus.md> "Getting Lazarus/ru"). 

## Сценарии

Существуют скрипты для Windows и Linux для автоматизации загрузки и сборки Lazarus: [Scripts for Lazarus](<../en/LazarusScripts.md> "LazarusScripts")

## Сборка и запуск Lazarus

См. [Installing Lazarus](<../en/Installing_Lazarus.md> "Installing Lazarus") \- подробное руководство по установке. 

### Использование командной строки

Lazarus поставляется с автоматически сгенерированным **Makefile** (с именем "Makefile") для утилиты gnu "**make** ". 

[![Warning-icon.png](https://wiki.freepascal.org/images/b/b2/Warning-icon.png)](</File:Warning-icon.png>)

**Предупреждение:** Если у вас также установлены инструменты Embarcadero (или Borland), их "make" может конфликтовать с gnu "make". В этом случае вы можете либо удалить или переименовать конфликтующий двоичный файл Embarcadero, либо удалить его каталог из Windows PATH, либо задать путь в консоли перед вызовом _make_ : 
    
    
     set path=c:\pascal\3.0.4\bin\i386-win32;%path%
    

**make** позволяет легко компилировать Lazarus из командной строки. Сначала вам нужно установить и настроить fpc и fpc-пакеты. 

Для компиляции перейдите в каталог Lazarus в вашем терминале и наберите 
    
    
    make
    

Это должно пересобрать LCL и Lazarus IDE с установленными базовыми пакетами. 

Еще одна полезная команда 
    
    
    make bigide
    

, которая добавляет те же пакеты, что и в версии выпуска. 

Затем вы можете запустить Lazarus из той же исходной директории с помощью команды "lazarus" (в системах Unix "./lazarus"). 

Если вы хотите запустить несколько версий, вы можете настроить lazarus на использование определенных каталогов конфигурации с помощью параметра --pcp, например, : 
    
    
     lazarus --pcp=/home/YOU/myLaz19
    

### Цели Make

#### Помощь по make

Чтобы просмотреть список доступных целей для _**make**_ , наберите [make help](<../en/make_help.md> "make help"). Примечание: "**make help** " существует с [версии Лазаруса] 1.0. Вот вывод команды "**make help** ": 

  

    
    
     
    make help 
    
     Main targets
                      Without any target, target 'all' will be invoked.                          |Без какой-либо цели будет вызываться цель 'all'.
       all            build all needed, i.e. minimal IDE, lazbuild, startlazarus.                |собрать все необходимое, то есть минимальную IDE, lazbuild, startlazarus.
       clean          deletes files that 'bigide' creates. It does not clean up all possible     |удаляет файлы, которые создает 'bigide'. Он не очищает все возможные 
                      targets. Clean other target: make clean LCL_PLATFORM=qt                    |цели. Очистить другую цель: make clean LCL_PLATFORM=qt
       distclean      Clean all targets and common leftovers.                                    |Очистить все цели и общие остатки.
       lazbuild       build lazbuild and lcl with nogui widgetset                                |собрать lazbuild и lcl с набором виджетов nogui 
       bigide         as all, except that the IDE is built with a lot of extra packages          |аналогично 'all', кроме того, что IDE собрал с большим количеством дополнительных пакетов   
       useride        calls lazbuild to build an IDE with your active profile, requires lazbuild |вызывает lazbuild для сборки IDE с вашим активным профилем, требует lazbuild
       install        installs Lazarus under /usr/local/share/lazarus                            |устанавливает Lazarus в /usr/local/share/lazarus
                      You can change the directory by appending INSTALL_PREFIX=/some/path        |Вы можете изменить каталог, добавив INSTALL_PREFIX=/some/path
    
     Sub targets
       registration   build package FCL                                                          |сборка пакета FCL
       lazutils       build package LazUtils, requires registration                              |сборка пакета LazUtils, требуется регистрация
       codetools      build package CodeTools, requires lazutils                                 |сборка пакета CodeTools, требуется lazutils
       lcl            build package LCLBase and LCL, requires lazutils                           |сборка пакетов LCLBase и LCL, требует lazutils                           
       tools          build lazres, svn2revisioninc, updatepofiles, lrstolfm,                    |сборка lazres, svn2revisioninc, updatepofiles, lrstolfm,
                      requires LCL with nogui widgetset                                          |требуется LCL с набором виджетов nogui
       basecomponents build debuggerintf, lazdebuggergdbmi, lazcontrols, synedit, ideintf        |сборка debuggerintf, lazdebuggergdbmi, lazcontrols, synedit, ideintf
                      for the LCL_PLATFORM, requires lcl                                         |для LCL_PLATFORM требуется lcl                  
       bigidecomponents build many extra packages for the LCL_PLATFORM, requires basecomponents  |сборка многих дополнительных пакетов для LCL_PLATFORM, требуются basecomponents
       lhelp          build lhelp, requires bigidecomponents                                     |сборка lhelp, требуются bigidecomponents
       starter        build startlazarus, requires basecomponents                                |сборка startlazarus, требуются basecomponents
       examples       build basic examples, requires basecomponents                              |сборка базовых примеров, требуются basecomponents                                                                                                 
                      Note: There are more examples having their own directory                   |Примечание: есть еще примеры, имеющие собственный каталог                                                                          
                                                                                                                                                                           
     Flags:                                                                                                                                                                
       PP=/path/to/fpc        use another compiler                                               |использовать другой компилятор                                                                                                                  
       USESVN2REVISIONINC=0   do not update ide/revision.inc                                     |не обновлять ide/revision.inc                                                                                                             
       INSTALL_PREFIX=/usr    used by 'install' as path prefix                                   |используется 'install' в качестве префикса пути                                                                                                            
       FPC_DIR=/usr/share/fpcsrc/3.0.4   used by fpcmake when regenerating Makefiles             |используется fpcmake при регенерации Makefiles                                                                                       
       OPT='-vwnbq -gh'       append these options when calling the compiler                     |добавить эти опции при вызове компилятора                                                                          
                                                                                                                                                                           
     Usage examples:                                                                             |Примеры использования:                                                                          
                                                                                                                                                                           
     Update svn and build a minimal IDE, startlazarus and lazbuild:                              |Обновление SVN и сборка минимальной IDE, startlazarus и lazbuild:                                                                        
       make clean                                                                                                                                                          
       svn up                                                                                                                                                               
       make clean all                                                                                                                                                      
                                                                                                                                                                           
     Note: You can start lazarus with 'startlazarus'                                             |Примечание: вы можете запустить Lazarus с помощью 'startlazarus'                                                                          
     Note: Use the IDE or lazbuild to compile your projects/packages.                            |Примечание: используйте IDE или lazbuild для компиляции ваших проектов/пакетов.                                                                          
                                                                                                                                                                           
     Update svn and build an IDE with your last set of packages:                                 |Обновление SVN и сборка IDE с последним набором пакетов:                                                                        
       make clean                                                                                                                                                          
       svn up                                                                                                                                                              
       make clean lazbuild useride                                                                                                                                         
                                                                                                                                                                           
     Clean up:                                                                                   |Очистка:                                                                          
      There is no command to clean up a svn repository completely, but                           |Нет команды полностью очистить svn-репозиторий, но                                                                          
      you can use the following command under Linux/OS X:                                        |вы можете использовать следующую команду в Linux/OS X:                                                                          
      svn status | grep '\?' | sed -e 's/\? *//' | xargs rm -r                                                                                                             
                                                                                                                                                                           
      Another possibility is to create a clean copy via the svn export command.                  |Другая возможность - создать чистую копию с помощью команды svn export.                                                                          
                                                                                                                                                                           
     Install:                                                                                    |Установка:                                                                          
       Note: You can use Lazarus without installing. Just start the lazarus executable.          |Примечание: Вы можете использовать Lazarus без установки. Просто запустите исполняемый файл Lazarus.                                                                          
       1. Build Lazarus as normal user with one of the above commands.                           |1. Соберите Lazarus как обычный пользователь с помощью одной из приведенных выше команд.                                                                          
       2. Install as root into /usr                                                              |2. Установите как [пользователь] root в [каталог] /usr                                                                          
              sudo make install
          This installs startlazarus, lazarus-ide, lazbuild under /usr/bin                       |Это установит startlazarus, lazarus-ide, lazbuild в [каталог] /usr/bin
          and copies the whole lazarus source directory to /usr/share/lazarus                    |и скопирует весь исходный каталог Lazarus в /usr/share/lazarus
    

### Параметры Make

Существуют дополнительные параметры, которые могут использоваться [утилитой] **make** Lazarus'а. Смотрите полностью рабочий пример выше. 

  * **OPT** =%compiler_switches



%compiler_switches передается каждому вызову fpc. Опция полезна для уточнения установок 

Например: 
    
    
    make bigide OPT="-dNoGdkPixBufLib -gw2"
    

создает LCL и ide с предопределенным NoGdkPixBufLib и отладочной информацией dwarf2. 

  * **LCL_PLATFORM** =%platform



%platform является целью [widgetset](<../en/Widgetset.md> "Widgetset"). Это может быть win32,wince,gtk,gtk2,qt,carbon,cocoa,customdrawn. 

Например: 
    
    
    make LCL_PLATFORM=gtk2
    

Пересобирает LCL и IDE с виджетом gtk2 

  * **FPC** =%compiler_path



или 

  * **PP** =%compiler_path



% compiler_path - это путь + имя файла к пользовательскому двоичному файлу компилятора, с которым вы хотите создать цель. 

такой как:: 
    
    
    make FPC="~/Pfad/fpc"
    

## Просмотр исходников репозитория с помощью веб-браузера

Содержимое архива SVN также можно просматривать с помощью веб-браузера через интерфейс [this viewcvs](<http://www.freepascal.org/cgi-bin/viewcvs.cgi/?root=lazarus>) . 

## Распространение Lazarus

Есть несколько неофициальных сайтов, где вы также можете найти Lazarus и Free Pascal: 

  * Дистрибутив [getlazarus](<http://www.getlazarus.org>) объединяет передовые версии Lazarus с простыми в использовании установочными сценариями для нескольких платформ и дополнительными компонентами.
  * [FreeSparta](<https://freesparta.com/>) \- коммерческий дистрибутив Lazarus для Windows.
  * В Austrian University of Applied Sciences вы можете заполучить Ubuntu LiveCD, размещающемуся [по этому адресу](<http://www.sigma-server.com/liveCD/Ubuntu-7.10-NTC-Lazarus.iso>), а файл md5sum находится [здесь](<http://www.sigma-server.com/liveCD/Ubuntu-7.10-NTC-Lazarus.iso.md5>).
  * [CodeTyphon](<http://www.pilotlogic.com>) включает Lazarus с дополнительными пакетами и компонентами.
  * [LiteZarus](<https://github.com/x2nie/LiteZarus>) это дистрибутив Lazarus, предназначенный для разработки проектов, не относящихся к LCL.
  * Lazarus доступен на компакт-дисках и USB-накопителях в магазине [Blaise Pascal Magazine](<http://www.blaisepascal.eu>).



## См.также

  * [Forum: Explanation of trunk installation in Windows](<http://forum.lazarus.freepascal.org/index.php/topic,36347.msg242123.html#msg242123>)

---

_Source: [https://wiki.freepascal.org/Getting_Lazarus/ru](https://web.archive.org/web/20250115000000/https://wiki.freepascal.org/Getting_Lazarus/ru)_
