# Pascal Script

│ **[English (en)](<../en/Pascal_Script.md>)** │  **русский (ru)** │

  
**Pascal Script** \- это [Object Pascal](<../en/Object_Pascal.md> "Object Pascal")/[Delphi](<../en/Delphi.md> "Delphi")/[Lazarus](<../en/Lazarus.md> "Lazarus")-совместимый интерпретатор с компилятором байт-кода, который предоставляет среду [scripting](<../en/PascalScript.md> "PascalScript") для прикладных программ. В настоящее время он работает в Windows и Linux на 32-битном и 64-битном процессорах Intel. Он был создан и поддерживается Carlo Kok, защищен авторским правом [RemObjects software](<http://www.remobjects.com>) как бесплатное ПО с полным исходным кодом. Исправление нескольких несовместимостей между ROPS (RemObjects Pascal Script) и FreePascal 2.0.1 было сделано Bogusław Brandys с большой помощью многих разработчиков из IRC-каналов #fpc и # lazarus-ide. Благодарю вас. 

Его основными характеристиками являются: 

  * поддерживается почти весь синтаксис Object Pascal
  * Поддерживаются классы Delphi/Lazarus (однако они не могут быть объявлены внутри скрипта)
  * может создавать полностью работоспособные GUI-формы с компонентами
  * легко импортировать новые классы в скриптовый движок



Загрузка содержит пакет компонентов для Delphi (различные версии) и Lazarus + несколько примеров для Delphi (которые могут работать или не работать под FreePascal+Lazarus) Это незавершенная работа ... 

Этот компонент теперь разработан для кросс-платформенных приложений, однако он ограничен только 32-разрядной платформой Intel. Я хотел бы, чтобы он когда-нибудь работал под PowerPC и 64-разрядными архитектурами. (Примечание: Текущая версия, похоже, поддерживает 64-битные машины, согласно RemObjects.) 

## Contents

  * 1 Скриншоты
  * 2 Лицензия
  * 3 Загрузка
  * 4 Журнал изменений
    * 4.1 Зависимости / Системные требования
    * 4.2 Установка
    * 4.3 Ошибки компиляции
  * 5 Использование
  * 6 Пример приложения
  * 7 См. также



## Скриншоты

Вот несколько скриншотов, как это выглядит под Lazarus: 

  * [![под Linux](https://wiki.freepascal.org/images/0/0f/Rops_linux.png)](</File:Rops_linux.png> "под Linux")

под Linux 

  * [![под Windows](https://wiki.freepascal.org/images/6/65/Rops_windows.png)](</File:Rops_windows.png> "под Windows")

под Windows 

  * [![под Windows](https://wiki.freepascal.org/images/e/e1/maXbox_mini_LAZARUS.png)](</File:maXbox_mini_LAZARUS.png> "под Windows")

под Windows 




## Лицензия

BSD подобная, см. [ полный текст](<../en/Pascal_Script/License.md> "Pascal Script/License"). 

## Загрузка

  * От RemObjects (FPC + Lazarus is supported)



    Это главная страница RemObjects [Pascal Script distribution](<http://www.remobjects.com/ps.aspx>). Имеются ссылки для загрузки бинарных пакетов.
    Вы можете получить исходный код из своего репозитория SubVersion по команде
    
    
    svn co -r HEAD http://code.remobjects.com/svn/pascalscript pascalscript
    

  * Новый репозиторий: <https://github.com/remobjects/pascalscript> <git://github.com/remobjects/pascalscript.git>



## Журнал изменений

  * Версия 1.0 от 21.10.2005
  * ("Официальная" поддержка FPC, как видно c 21.07.2006)



### Зависимости / Системные требования

  * None
  * Status: Beta (ToDo: update info)
  * Issues: (ToDo: update info)
  * Needs testing on Windows.
  * Needs testing on Linux.
  * Almost working ;-)



### Установка

  * Создайте папку lazarus\components\pascalscript
  * Распакуйте файлы в папку
  * Откройте Лазарус
  * Откройте пакет pascalscript.lpk из меню Component/Open package file (.lpk)
  * Нажмите Compile
  * Нажмите Install



### Ошибки компиляции

При компиляции для установки пакета компилятор спотыкается на двух строках в файле uPSR_forms.pas: 
    
    
    RegisterMethod(@TAPPLICATION.HELPCOMMAND, 'HELPCOMMAND'); // <-- вот эта
    RegisterMethod(@TAPPLICATION.HELPCONTEXT, 'HELPCONTEXT');
    RegisterMethod(@TAPPLICATION.HELPJUMP, 'HELPJUMP');       // <-- и еще одна
    

Просто закомментируйте строки. Эти методы еще не реализованы в LCL. 

## Использование

Бросьте компонент PascalScript на форму и несколько плагинов. (TODO:finish) 

Если вы получите сообщение об ошибке "Fatal: Can't find unit uPSCompiler used by ...", откройте пакет pascalscript, а в разделе "Дополнительно"» выберите "добавить в проект". 

См. проект с примером. 

Также см. это [articles](<https://github.com/remobjects/pascalscript/wiki>) от RemObjects. 

## Пример приложения

Пример приложения для интерпретатора небольших консольных приложений: [Pascal Script Examples (psce)](<../en/Pascal_Script_Examples.md> "Pascal Script Examples")

Примеры демок компонентов с графическим интерфейсом Lazarus: [[[1]](<http://sourceforge.net/projects/maxbox/files/Lazarus/PASCALSCRIPT_LAZARUS.zip/download>)] 

## См. также

  * [Pascal Script tab](<../en/Pascal_Script_tab.md> "Pascal Script tab")

---

_Source: [https://wiki.freepascal.org/Pascal_Script/ru](https://web.archive.org/web/20250321105346/https://wiki.freepascal.org/Pascal_Script/ru)_
