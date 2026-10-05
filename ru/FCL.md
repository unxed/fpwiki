# FCL

│ **[English (en)](<../en/FCL.md>)** │  **русский (ru)** │

_Free Component Library_ (**FCL**) - бесплатная и свободная библиотека компонентов Free Pascal. Она состоит из набора модулей, предоставляющих классы и компоненты для общих задач. FCL стремиться быть совместимой с библиотекой визуальных компонентов Delphi - VCL. Однако, FCL ограничивается только не визуальными компонентами. Lazarus так же имеет собственную библиотеку компонентов - LCL (Lazarus component library), с которой вы можете ознакомиться здесь: [LCL Components](<../en/LCL_Components.md> "LCL Components"). 

  


## Contents

  * 1 Использование
  * 2 Подпакеты
  * 3 Документация
  * 4 Пример
  * 5 FCL Компоненты



### Использование

Чтобы использовать FCL компонент необходимо включить имя [модуля](<../en/Unit.md> "Unit"), в котором он реализован, в список после ключевого слова **uses** вашей программы или модуля(см. пример ниже). По умолчанию компилятор будет искать указанный модуль в папках FCL. Вы также можете указать компилятору явный путь поиска, используя параметр командной строки вида: -Fu<папка-к-fcl-модулям>. 

  


### Подпакеты

Полный список подпакетов FCL, можно найти здесь: [Package List](<../en/Package_List.md> "Package List")

Среди них можно выделить: 

  * [fcl-base](<../en/fcl-base.md> "fcl-base") Основные модули (включает, например [обработчик выражений](<../en/How_To_Use_TFPExpressionParser.md> "How To Use TFPExpressionParser"))
  * [fcl-async](<../en/fcl-async.md> "fcl-async") Асинхронный ввод/вывод (последовательный?)
  * [fcl-db](<../en/fcl-db.md> "fcl-db") Общая поддержка баз данных + драйвера к ним
  * [fcl-fpcunit](<../en/fcl-fpcunit.md> "fcl-fpcunit") Модуль тестирования
  * [fcl-image](<../en/fcl-image.md> "fcl-image") Считывание и запись растровых изображений (этакий fpimage)
  * [fcl-json](<../en/fcl-json.md> "fcl-json") Позволяет работать с потоковыми объектами javascript
  * [fcl-net](<../en/fcl-net.md> "fcl-net") Модули для работы с сетью
  * [fcl-passrc](<../en/fcl-passrc.md> "fcl-passrc") Обработка и преобразование языка Pascal
  * [fcl-process](<../en/fcl-process.md> "fcl-process") Управление процессами
  * [fcl-registry](<../en/fcl-registry.md> "fcl-registry") Реестр
  * [fcl-res](<../en/fcl-res.md> "fcl-res") Обработка ресурсов
  * [fcl-stl](</index.php?title=fcl-stl&action=edit&redlink=1> "fcl-stl \(page does not exist\)") Универсальная библиотека (стандартная библиотека шаблонов)
  * [fcl-web](<../en/fcl-web.md> "fcl-web") Помощник для веб-разработки
  * [fcl-xml](<../en/fcl-xml.md> "fcl-xml") XML (DOM) модуль и связанные с ним модули.



### Документация

В настоящее время, FCL не полностью документирован (не стесняйтесь вносить свой вклад; также посмотрите: [ссылка на 'fcl'](<http://lazarus-ccr.sourceforge.net/docs/fcl> "doc:fcl")). Для совместимых с Delphi модулей, вы можете обратиться к документации по Delphi. Вы всегда можете посмотреть исходные файлы в [хранилище исходного кода](<http://www.freepascal.org/cgi-bin/viewcvs.cgi/trunk/packages/>). 

### Пример

Следующая программа демонстрирует использование класса TObjectList в FCL модуле Contnrs: 

  

    
    
     program TObjectListExample;
     {$mode ObjFPC} 
     uses
       Classes, { из RTL для TObject }
       Contnrs; { из FCL для TObjectList }
     
     type
        TMyObject = class(TObject)  { просто некий класс приложения }
        private
          FName: String; { с строковым полем }
        public
          constructor Create(AName: String); { и конструктором }
          property Name: String read FName; { а так же свойством для чтения }
       end;
     
     constructor TMyObject.Create(AName: String);
     begin
       inherited Create;
       FName := AName;
     end;
     
     var
       VObjectList: TObjectList; { для списка объектов; это ссылка на такой список! }
     
     begin
       VObjectList := TObjectList.Create;  { создать пустой список }
       with VObjectList do
       begin
         Add(TMyObject.Create('Это первый'));
         Writeln((Last as TMyObject).Name);
         Add(TMyObject.Create('Это второй'));
         Writeln((Last as TMyObject).Name);
       end;
     end.
    

Эта программа должна быть скомпилирована в объектно-ориентированном режиме, например: -Mobjfpc или -Mdelphi. 

### FCL Компоненты

Это не полный список (чтобы избежать дублирования). Он содержит только некоторые важные компоненты, или компоненты, которые в противном случае не легко найти. 

Classes
    Основные классы для Object Pascal в Delphi режиме
Contnrs
    Некоторые общие классы-контейнеры
[FPCUnit](<../en/fpcunit.md> "fpcunit")
    Модуль тестирования (основан на модуле Kent Beck's. См. [JUnit](<http://www.junit.org/>)),[FPCUnit tutorial (pdf)](<http://www.freepascal.org/~michael/articles/fpcunit/fpcunit.pdf>)
XMLRead, XMLWrite и DOM
    Подробно в [XML Учебнике](<XML_Tutorial.md> "XML Tutorial/ru")

---

_Source: [https://wiki.freepascal.org/FCL/ru](https://web.archive.org/web/20250213003142/https://wiki.freepascal.org/FCL/ru)_
