# How to setup a FPC and Lazarus Ubuntu repository

[![Logo-ubuntu cof-orange-hex.svg](https://upload.wikimedia.org/wikipedia/commons/thumb/a/ab/Logo-ubuntu_cof-orange-hex.svg/50px-Logo-ubuntu_cof-orange-hex.svg.png)](</File:Logo-ubuntu_cof-orange-hex.svg>)

Эта статья относится только к [Ubuntu](</Category:Ubuntu> "Category:Ubuntu").

См. также: [Multiplatform Programming Guide](<../en/Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

│ **[English (en)](<../en/How_to_setup_a_FPC_and_Lazarus_Ubuntu_repository.md> "How to setup a FPC and Lazarus Ubuntu repository")** │  **[français (fr)](</How_to_setup_a_FPC_and_Lazarus_Ubuntu_repository/fr> "How to setup a FPC and Lazarus Ubuntu repository/fr")** │  **[日本語 (ja)](</How_to_setup_a_FPC_and_Lazarus_Ubuntu_repository/ja> "How to setup a FPC and Lazarus Ubuntu repository/ja")** │  **[português (pt)](</How_to_setup_a_FPC_and_Lazarus_Ubuntu_repository/pt> "How to setup a FPC and Lazarus Ubuntu repository/pt")** │  **русский (ru)** │  **[slovenčina (sk)](</How_to_setup_a_FPC_and_Lazarus_Ubuntu_repository/sk> "How to setup a FPC and Lazarus Ubuntu repository/sk")** │    
****

## Contents

  * 1 Остерегайтесь загрузчиков!
  * 2 Что такое хранилище?
  * 3 Кому это нужно?
  * 4 Структура каталогов
  * 5 Deb-файлы
    * 5.1 Создание файлов deb самостоятельно
      * 5.1.1 Установка пакетов разработки
      * 5.1.2 Сборка новых deb-файлов
      * 5.1.3 Замена deb-файлов в хранилище
  * 6 PGP-ключи
    * 6.1 Создание PGP-ключа
    * 6.2 Загрузить ключ на сервер открытых ключей
    * 6.3 Запомнить ID-ключа
  * 7 Обновление файлов хранилища
  * 8 Добавление хранилища на клиента
    * 8.1 Добавление ключа
    * 8.2 Добавление хранилища
  * 9 Установка Lazarus



## Остерегайтесь загрузчиков!

Если вы хотите скачать Lazarus, перейдите по ссылке [Ubuntu repository](<../en/Lazarus_release_version_for_Ubuntu.md> "Lazarus release version for Ubuntu"). 

Эта страница описывает, как настроить хранилище самостоятельно, оно не предназначено для обычных пользователей. 

## Что такое хранилище?

Репозиторий(хранилище) Ubuntu - это каталог. Он может храниться на локальном диске, на веб-сервере или на FTP-сервере. Чтобы использовать его, вы добавляете его путь в ваш /etc/apt/sources.list и устанавливаете ключ pgp. Затем вы можете просто установить lazarus с помощью вашего любимого графического интерфейса (например, synaptic), и fpc, fpc-src и lazarus будут загружены, установлены и обновлены автоматически. 

## Кому это нужно?

Администраторам, которые хотят установить FPC + Lazarus на пул компьютеров. Как в школе. Или новички, которые просто хотят быстро это проверить. 

## Структура каталогов

Предположим, вы хотите создать репозиторий, доступный через веб-сервер apache. Тогда вам нужно настроить каталог, такой как /var/www/lazarus, который будет доступен для публичного чтения и доступен для записи только пользователю root. 

Создайте подкаталог для каждой цели, которую вы хотите поддерживать: 
    
    
    mkdir -p /var/www/lazarus/dists/lazarus-testing/universe/binary-i386
    mkdir -p /var/www/lazarus/dists/lazarus-testing/universe/binary-amd64
    

## Deb-файлы

Поместите fpc, fpc-src и lazarus внутрь deb-файлов. 

### Создание файлов deb самостоятельно

Вы можете создать deb-файлы с помощью скриптов в tools/install/ из исходников lazarus. 

#### Установка пакетов разработки

  * установите пакеты разработки:


    
    
    sudo apt-get install libgtk2.0-dev libgtk1.2-dev libgdk-pixbuf-dev libgpmg1-dev fakeroot libncurses5-dev
    

  * Установите последнюю стабильную FPC. Это необходимо для сборки нового FPC и Lazarus:



либо deb-файлы с официального сайта, либо tar.gz. например: 
    
    
    sudo apt-get install fp-compiler
    

  * Загрузите исходники FPC. Чтобы получить текущую версию разработки, вы можете использовать команду ниже. Чтобы получить более стабильную версию, см. [Installing_Lazarus#FPC Sources](<../en/Installing_Lazarus.md> "Installing Lazarus"):


    
    
    svn co http://svn.freepascal.org/svn/fpc/trunk fpc
    

  * Загрузите исходники Lazarus:


    
    
    svn co http://svn.freepascal.org/svn/lazarus/trunk lazarus
    

#### Сборка новых deb-файлов

  * перейдите в каталог установки сценария lazarus:


    
    
    cd lazarus/tools/install
    

  * соберите deb-файлы FPC. Следующий скрипт создаст один deb-файл fpc, используя дату в качестве версии. В качестве параметра вы должны указать путь к исходникам FPC, которые вы скачали выше:


    
    
    sudo ./create_fpc_deb.sh fpc /path/to/the/sources/of/fpc/
    

  * установите новый deb-файл fpc. Это необходимо для сборки deb-файла для lazarus, который зависит от нового deb-файла fpc. Не забудьте сначала удалить старую FPC.


    
    
    sudo dpkg -i fpc_2.2.5-090517_i386.deb
    

  * соберите deb-файл fpc-src. Это работает почти так же, как и [описано] выше (параметр fpc-src вместо fpc):


    
    
    ./create_fpc_deb.sh fpc-src /path/to/the/sources/of/fpc/
    

  * соберите deb-файл lazarus. Вы можете собрать нормальный Lazarus, используя gtk2:


    
    
    ./create_lazarus_deb.sh append-revision
    

или lazarus, используя gtk1: 
    
    
    ./create_lazarus_deb.sh gtk1 append-revision
    

  * Для проверки пакета lazarus вы можете установить новый deb-файл fpc-src и новый deb-файл lazarus с помощью:


    
    
    sudo dpkg -i fpc-src_2.2.5-090517_i386.deb lazarus_0.9.27.20004-0_i386.deb
    

#### Замена deb-файлов в хранилище

Теперь у вас есть 3 deb-файла. 

Вы можете установить их напрямую, но сначала удалите собранную версию 

  * Если вы установили fp-компилятор, вам необходимо:


    
    
    dpkg -r fp-compiler fp-units-rtl fp-compiler
    

  * Скопируйте их в свое хранилище:


    
    
    cp fpc_2.3.1-070726_i386.deb fpc-src_2.3.1-070726_i386.deb lazarus_0.9.23.11636-0_i386.deb \
    /var/www/lazarus/dists/lazarus-testing/universe/binary-i386/
    

  * Не забудьте удалить старые версии.



## PGP-ключи

Вам нужно подписать deb-файлы ключом PGP, чтобы целевые системы могли быть уверены, что файлы не заменил злоумышленник. 

### Создание PGP-ключа

Вы можете использовать такие инструменты, как [seahorse](<https://ru.wikipedia.org/wiki/Seahorse>) или [thunderbird](<https://addons.thunderbird.net/ru/thunderbird/addon/enigmail/>), чтобы создать ключ PGP. 

  * Установите seahorse
  * Запустите seahorse
  * Key(ключ) > Create new key(Создать новый ключ)
  * Всплывающее окно запросит тип ключа. Выберите _PGP Key_.
  * Дайте полное имя и адрес электронной почты и нажмите _Create_(Создать).
  * Фраза-пароль необходима для шифрования созданных файлов. Таким образом, никто не сможет использовать ключи, кроме вас, даже если им удастся украсть ваши файлы. Если вы думаете, что ваши файлы никогда не будут украдены или прочитаны другими, вы можете оставить их пустыми.
  * Создание ключей займет несколько минут



### Загрузить ключ на сервер открытых ключей

Чтобы поделиться ключом, вы можете загрузить ключ на сервер открытых ключей. 

  * запустите seahorse
  * Edit(Редактировать) > Preferences(Настройки) > Key servers(Серверы ключей) > Publish key to(Опубликовать ключ на): выберите key server. Например: hkp://pgp.mit.edu:11371. Закройте диалоговое окно.
  * Remote(Удаленно) > Sync(Синхронизировать) и publish key(опубликовать ключ) > Sync(Синхронизировать).



### Запомнить ID-ключа

Вам понадобится идентификатор ключа позже. Идентификатор ключа показывается seahorse. Но вы можете увидеть это посредством: 
    
    
    gpg --list-keys
    

## Обновление файлов хранилища

Поместите следующий скрипт в /var/www/lazarus, отредактируйте его под свои нужды и запустите: 
    
    
    #!/usr/bin/env bash
    
    set -x
    
    GPGHome=/home/gaertner/.gnupg/
    MainDir=dists/lazarus-testing
    
    for Arch in i386 amd64; do
      Dir=$MainDir/universe/binary-$Arch
    
      # create index
      apt-ftparchive packages $Dir > $Dir/Packages
      cat $Dir/Packages | gzip -9c > $Dir/Packages.gz
      cat $Dir/Packages | bzip2 > $Dir/Packages.bz2
    done
    
    # create Release file
    rm -f $MainDir/Release*
    Date=`date`
    echo "Origin: Lazarus" >> $MainDir/Release
    echo "Label: Lazarus" >> $MainDir/Release
    echo "Suite: unstable" >> $MainDir/Release
    echo "Codename: lazarus-testing" >> $MainDir/Release
    echo "Version: 1.0" >> $MainDir/Release
    echo "Date: $Date" >> $MainDir/Release
    echo "Architectures: amd64 i386" >> $MainDir/Release
    echo "Components: universe" >> $MainDir/Release
    echo "Description: Lazarus testing 1.0" >> $MainDir/Release
    
    apt-ftparchive release $MainDir >> $MainDir/Release
    
    # sign Release file
    gpg --sign --homedir=$GPGHome -ba -o $MainDir/Release.gpg $MainDir/Release
    
    # end.
    

Это создаст индексные файлы _Packages_ , _Packages.bz2_ и _Packages.gz_. И это создаст файл _Release_ , содержащий контрольные суммы deb-пакетов, и подпишет их (_Release.gpg_). 

## Добавление хранилища на клиента

_ВАЖНО_ : Если вы пришли сюда только для загрузки Lazarus для Ubuntu/Debian, используйте стабильный репозиторий [Ubuntu repository](<../en/Getting_Lazarus.md> "Getting Lazarus"). Хранилище ниже является нестабильным, тестируемым хранилищем. 

Следующие шаги должны быть выполнены на каждом компьютере, где вы хотите использовать свой репозиторий. 

### Добавление ключа

Загрузите ключ с сервера открытого ключа: 
    
    
    gpg --keyserver hkp://pgp.mit.edu:11371 --recv-keys 3A5B1204
    

[Приведенный для примера ID] 3A5B1204 следует заменить ID вашего ключа. Проверьте на выходе, что вы получили правильный ключ. 

Добавьте его в систему apt: 
    
    
    gpg --export 3A5B1204 | sudo apt-key add -
    

Вы можете увидеть список ключей apt посредством: 
    
    
    sudo apt-key list
    

### Добавление хранилища

Вы можете использовать для этого synaptic или отредактировать /etc/apt/sources.list напрямую. Добавьте строку: 
    
    
    deb http://progprak.scale.uni-koeln.de/lazarus/ lazarus-testing universe
    

Замените http-путь своим собственным. 

## Установка Lazarus

Например: 
    
    
    sudo apt-get update
    sudo apt-get install lazarus

---

_Source: [https://wiki.freepascal.org/How_to_setup_a_FPC_and_Lazarus_Ubuntu_repository/ru](https://web.archive.org/web/20250601000000/https://wiki.freepascal.org/How_to_setup_a_FPC_and_Lazarus_Ubuntu_repository/ru)_
