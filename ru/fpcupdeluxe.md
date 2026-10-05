# fpcupdeluxe

│ **[English (en)](<../en/fpcupdeluxe.md>)** │  **русский (ru)** │

## Contents

  * 1 Обзор
  * 2 Процесс установки FPC и Lazarus
  * 3 Продвинутые настройки
  * 4 Кросскомпиляция
    * 4.1 Кросскомпиляция из Windows и Linux под Darwin: сложный вариант
    * 4.2 Предостережения, наблюдения, Устранение неполадок
  * 5 Запуск установленного экземпляра Lazarus и fpc
  * 6 Внешние ссылки



## Обзор

[![](https://wiki.freepascal.org/images/2/28/fpcupdeluxe.JPG)](</File:fpcupdeluxe.JPG>)

[](</File:fpcupdeluxe.JPG> "Enlarge")

fpupdeluxe on Windows

  * fpc**up** _deluxe_ \- графический установщик для FPC и Lazarus. Основан на [fpcup](<../en/fpcup.md> "fpcup")
  * [исходники fpcupdeluxe](<https://github.com/newpascal/fpcupdeluxe>)
  * [последний релиз fpcupdeluxe](<https://github.com/newpascal/fpcupdeluxe/releases/latest>)



## Процесс установки FPC и Lazarus

Основная цель fpcdeluxe - предоставить средства для установки и обновления нескольких версий FPC и Lazarus автономным способом. Это означает, что установка с помощью fpcupdeluxe не окажет влияния на вашу систему (не помешает ей): следовательно, вы можете установить несколько версий FPC и Lazarus. 

Чтобы процесс установки заработал, **вы ДОЛЖНЫ использовать сгенерированную ссылку, чтобы начать установку**! 

Эта ссылка для запуска содержит строку типа: 
    
    
    Exec=/P/s/lazarus/lazarus --pcp="/P/s/config_lazarus"
    

, указывающая Lazarus на использование [pcp](<../en/pcp.md> "pcp"), то есть **primary configuration**(первичной конфигурации), задающей **путь** "/P/s/config_lazarus" или аналогичного подкаталога в /home/ в зависимости от вашего выбора, сделанного из графического интерфейса. В пути поиска не требуется указывать путь к fpc или еще к чему-либо. 

## Продвинутые настройки

[![](https://wiki.freepascal.org/images/2/26/fpcupdeluxe_advanced.JPG)](</File:fpcupdeluxe_advanced.JPG>)

[](</File:fpcupdeluxe_advanced.JPG> "Enlarge")

Fpcupdeluxe advanced settings screen.

На приведенном выше снимке экрана (нажмите Setup+ на главном экране) показаны расширенные настройки fpcupdeluxe. 

Некоторые примеры продвинутых настроек: \--> для отладки самой FPC добавьте "-g -gl -O-" в поле "FPC options". 

## Кросскомпиляция

Fpcupdeluxe упрощает процесс кросскомпиляции: просто выберите CPU и ОС и нажмите «Установить кросскомпилятор». Если нужны библиотеки и/или бинарные инструменты, fpcupdeluxe попытается получить их онлайн. Воспользуйтесь ссылкой, чтобы увидеть, что доступно на данный момент! 

  * [Linux 64bit binary toolchains](<https://github.com/newpascal/fpcupdeluxe/releases/tag/linuxx64crossbins_v1.0>)
  * [Linux 32bit binary toolchains](<https://github.com/newpascal/fpcupdeluxe/releases/tag/linuxi386crossbins_v1.0>)
  * [Windows binary toolchains](<https://github.com/newpascal/fpcupdeluxe/releases/tag/wincrossbins_v1.0>)
  * [System libraries](<https://github.com/newpascal/fpcupdeluxe/releases/tag/crosslibs_v1.0>)



### Кросскомпиляция из Windows и Linux под Darwin: сложный вариант

С помощью fpcupdeluxe, транков NewPascal или FPC и набора инструментов под названием osxcross/cctools-port вы можете выполнить кросскомпиляцию под Darwin. Эта справка будет сконцентрирована на ручной настройке перехода из Windows/Linux в Дарвин (fpcupdeluxe также может выполнить всю автоматическую магию для перехода в Дарвин; см. выше). 

Во-первых, вам нужно получить osxcross/cctools-port и скомпилировать его самостоятельно. 

  * [osxcross original](<https://github.com/tpoechtrager/osxcross>)  
[osxcross для FPC](<https://github.com/LongDirtyAnimAlf/osxcross>)
  * [cctools-port original](<https://github.com/tpoechtrager/cctools-port>)  
[cctools-port для FPC](<https://github.com/LongDirtyAnimAlf/cctools-port>)



Для macOS вам потребуется [osxcross для FPC](<https://github.com/LongDirtyAnimAlf/osxcross>), который был адаптирован для использования FPC. 

Для iOS/iPhone вам понадобится [cctools-port для FPC](<https://github.com/LongDirtyAnimAlf/cctools-port>), который был адаптирован для использования FPC. 

Получите себе SDK, либо с вашего собственного Mac, либо из онлайн-источников: [Различные Mac-SDK](<https://github.com/phracker/MacOSX-SDKs/releases>)

SDK для iPhone также можно получить из онлайн-источников: [Различные SDK для iPhone](<https://github.com/theos/sdks>)

Соберите osxcross/cctools-port в соответствии с инструкциями. Например, на Windows при помощи Cygwin. 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Примечание:** Обратите внимание: для кросскомпиляции из Windows под Darwin необходимо установить транк NewPascal или FPC!

Используйте fpcupdeluxe для установки транка NewPascal или FPC. 

Перед сборкой кросскомпилятора вы должны сообщить fpcupdeluxe, где найти библиотеки (SDK) и бинарные инструменты (собранные с помощью osxcross/cctools-port) Darwin. Два скриншота ниже показывают, как это сделать: 

  * Выберите тип CPU и OS (Darwin). Выберите пункт "custom". Теперь вы сможете использовать кнопки и указывать fpcupdeluxe в нужных местах.




[![](https://wiki.freepascal.org/images/b/bc/fpcupdeluxe_darwinlibs.JPG)](</File:fpcupdeluxe_darwinlibs.JPG>)

[](</File:fpcupdeluxe_darwinlibs.JPG> "Enlarge")

Point fpcupdeluxe towards the library location

[![](https://wiki.freepascal.org/images/3/3f/fpcupdeluxe_darwinbins.JPG)](</File:fpcupdeluxe_darwinbins.JPG>)

[](</File:fpcupdeluxe_darwinbins.JPG> "Enlarge")

Point fpcupdeluxe towards the binary tools location

  * Теперь на главном экране вы можете выбрать свой CPU и OS (Darwin) и собрать кросскомпилятор! 

  * **Опять же, fpcupdeluxe также имеет встроенные инструменты для кросскомпиляции под Darwin: используя автомагию fpcupdeluxe, это будет очень легко !!!**



### Предостережения, наблюдения, [Устранение неполадок](</index.php?title=Troubleshooting/ru&action=edit&redlink=1> "Troubleshooting/ru \(page does not exist\)")

  * Было замечено, что **f.** часто работает при первом запуске на пустой установочной директории, но запускается последующая, то есть, в случае добавления элементов управления в палитру, добавление системного asf. .chm-[справки](</help> "help") очень часто терпит неудачу. В случае сбоя перекомпиляции / [make](<../en/make.md> "make") среды IDE двоичный файл Lazarus может отсутствовать, поэтому, если вы ранее не сделали резервную копию файла, вы не сможете больше запускать Lazarus.
  * скопируйте и сохраните файл журнала для справки / устранения неполадок.
  * добавить системную [справку](</help> "help") очень просто: просто пометьте ее в разделе "advanced settings"(продвинутые настройки), сэкономив немного ручного труда.
  * полная установка fpc + Laz занимает около 10 минут.
  * попробуйте новые настройки --pcp, чтобы почувствовать это, вы всегда можете удалить конфигурацию и начать заново (сохраните чистое свежее содержимое каталога --pcp сразу после установки).



## Запуск установленного экземпляра Lazarus и fpc

Для запуска графического интерфейса Lazarus создается сценарий (в домашнем каталоге пользователя) и значок запуска (на рабочем столе), которое открывает Lazarus с правильным путем и средой конфигурации. Например, в Linux при установке в папку с именем "/home/user/trunk" fpcupdeluxe создает: 
    
    
     /home/user/Desktop/Lazarus_trunk.desktop (launcher)
     /home/user/Lazarus_trunk  (shell script)
    

Для работы с командной строкой Linux FPC/lazbuild можно использовать следующий сценарий оболочки (предполагается, что оболочка bash и используемый в примере каталог установки "/home/user/trunk"): 
    
    
      #!/bin/sh
      # fpcupdeluxe: FPC home startlink script
      export PATH="/home/user/trunk/fpc/bin/x86_64-linux:/home/user/trunk/lazarus:${PATH}"
      export PPC_CONFIG_PATH="/home/user/trunk/fpc/bin/x86_64-linux"
      exec /bin/bash
    

Эквивалентный пакетный файл Windows для настройки среды для работы из командной строки (используемый в примере каталог установки "f:\trunk"): 
    
    
      REM fpcupdeluxe: FPC home startlink script
      SET PATH=F:\trunk\fpc\bin\x86_64-win64;F:\trunk\lazarus;%PATH%
      SET PPC_CONFIG_PATH=F:\trunk\fpc\bin\x86_64-win64
      cmd.exe
    

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Примечание:** Установка путей таким образом не является глобальной и влияет только на текущую оболочку терминала и дочерние оболочки.

## Внешние ссылки

  * См. подробное руководство для _BUILD / MAKE_ : [FAQ по сборке](<http://www.stack.nl/~marcov/buildfaq.pdf>)
  * [Fpcup by Reinier](<http://wiki.lazarus.freepascal.org/fpcup>)
  * [Fpcupdeluxe source](<https://github.com/newpascal/fpcupdeluxe>)
  * [Latest release](<https://github.com/newpascal/fpcupdeluxe/releases/latest>)
  * [NewPascal](<http://newpascal.org>)

---

_Source: [https://wiki.freepascal.org/fpcupdeluxe/ru](https://web.archive.org/web/20241201000000/https://wiki.freepascal.org/fpcupdeluxe/ru)_
