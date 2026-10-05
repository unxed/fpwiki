# Templates

│ **[English (en)](<../en/Templates.md>)** │  **русский (ru)** │

## Contents

  * 1 Вступление
  * 2 Использование
    * 2.1 Наследование
    * 2.2 Ограничения
  * 3 Классы-дженерики
  * 4 Существующие библиотеки дженериков
  * 5 См. также
  * 6 Внешние ссылки



## Вступление

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Примечание:** Шаблоны, представленные на этой странице, больше не нужны, потому что поддержка [Generics](<../en/Generics.md> "Generics") в FPC полная и достаточно удобная для использования.

_Templates_ (шаблоны) - это простой механизм для решения проблемы написания дублирующего кода для реализации общего класса для конкретных типов в строго типизированном языке. Используется в основном для базовых (необъектных) типов; для объектов наследования от одного базового класса TObject обычно используется для построения общих классов. В продвинутых языках шаблоны заменяются нативно-языковой реализацией, так называемыми "дженериками". Собственная реализация дженериков в FPC 2.4.0 еще не является ни полной, ни практически используемой. Пока это не так, могут быть использованы шаблоны. 

**Плюсы:**

  * Безопасность типов - устраняет раздражение при типизации и ошибках, которые оно часто вызывает
  * Лучшее обслуживание - универсальный код класса пишется только один раз и используется для всех специализированных классов
  * Лучшее повторное использование кода - можно создать хороший набор повторно используемых классов-дженериков
  * Лучшая эффективность, масштабируемость - типы классов-дженериков могут быть лучше приспособлены к конкретным потребностям. Например, на 16-битной машине можно использовать специализированный TList с 16-битным индексным типом SmallInt.



**Минусы:**

  * Более сложны
  * Большой код - каждый специализированный класс компилируется отдельно и создает дополнительный код



## Использование

Классическим примером базового класса-дженерика является список элементов. Free Pascal в настоящее время предлагает три способа использования списков: 

  * **array of** \- классическая структура динамического массива, которая может содержать динамические элементы одного типа. Элементы доступны по прямой адресации: SomeArray[Index]. Функции SetLength и Length используются для обработки размера.
  * **TList** \- объектно-ориентированный способ, которым класс обрабатывает все операции со списками. Реализация в LCL сохраняет совместимость с VCL, где TList содержит список указателей. Если требуется список другого типа элемента, необходимо скопировать и переписать полностью новый класс или использовать приведение типов в каждом месте, где есть ссылка на элемент списка. Типизация не эффективна, например, `Pointer(Byte)`, и небезопасна для типа: `Pointer(Int64)`. Индекс TList имеет тип Integer. Если требуется индекс `Int64` или `SmallInt`, класс необходимо скопировать и переписать.
  * **TCollection** \- это более общее и безопасное, но более сложное решение для хранения списка предметов. При таком подходе программист должен создать класс элементов, который наследуется от TCollectionItem, и установить для этого вновь созданного типа класса параметр конструктора TCollection.
  * **native generics** \- пока нет полной реализации. Отсутствует поддержка ссылки на общий тип, наследования, ограничений.
  * **templates** \- использует включение кода, чтобы обойти отсутствие нативной поддержки. Можно использовать наследование и ссылки на общие типы. Ограничения задаются операторами и другими функциями в общем классе.



Шаблоны пытаются решить проблему, используя концепцию параметризованных включаемых файлов. Универсальный модуль записывается только один раз как шаблон с использованием универсальных (не указанных) типов в разделах интерфейса и реализации. Затем в процессе специализации используется шаблон класса, а общие параметры шаблона заменяются определенными типами для создания нового специализированного класса. 

  
_GenericList.inc:_
    
    
    {$IFDEF INTERFACE}     
      // TGList<T> = class
      TGList = class
        Items: array of T;
        procedure Add(Item: T);
      end;
    {$UNDEF INTERFACE}     
    {$ENDIF}     
    
    {$IFDEF IMPLEMENTATION}     
    procedure TGList.Add(Item: T);
    begin
      SetLength(Items, Length(Items) + 1);  
      Items[Length(Items) - 1] := Item;
    end;
    {$UNDEF IMPLEMENTATION}     
    {$ENDIF}
    

По мере специализации ранее созданные файлы шаблонов включаются в новый модуль. 

_ListInteger.pas:_
    
    
    unit ListInteger;
    {$mode ObjFPC} 
    interface
    
    uses
      Classes;
    
    type
      T = Integer; // T указывается для какого-то точного типа
    {$DEFINE INTERFACE}
    {$INCLUDE 'GenericList.inc'}
    
    type
      // TListInteger<Integer> = class
      TListInteger = class(TGList)
        // Дополнительные поля и методы могут быть добавлены здесь
      end;
    
    implementation
    
    {$DEFINE IMPLEMENTATION}
    {$INCLUDE 'GenericList.inc'}
    
    end.
    

Наконец, у нас есть новый специализированный модуль, названный ListInteger, и мы можем использовать наш новый специализированный тип в некотором коде. 
    
    
    program GenericTest;
    {$mode ObjFPC}
    uses
      ListInteger;
    var
      List: TListInteger;
    begin
      try
        List := TListInteger.Create;
        List.Add(1);
        List.Add(2);
        List.Add(3);
      finally
        List.Free;
      end;
    end.
    

### Наследование

Классы-дженерики могут наследоваться друг от друга так, как это делают классы-недженерики. Реализация немного сложна, так как для каждого модуля может быть определен только один специализированный тип определенного класса. Чтобы иметь возможность указать класс-дженерик, который наследуется от другого класса-дженерика, следует сохранить некоторую схему именования. 

Предположим, что мы хотим создать расширенный список дженериков TAdvancedList, у которого есть поле Capacity. 

Определение базового класса-дженерика: 

_GenericList.inc:_
    
    
    {$IFDEF INTERFACE}     
      // TGList<TListIndex, TListItem> = class
      TGList = class
        Items: array[TListIndex] of TListItem;
        procedure Add(Item: TListItem);
      end;
    {$UNDEF INTERFACE}
    {$ENDIF}
    
    {$IFDEF IMPLEMENTATION}
    procedure TGList.Add(Item: TListItem);
    begin
      SetLength(Items, Length(Items) + 1);  
      Items[Length(Items) - 1] := Item;
    end;
    {$UNDEF IMPLEMENTATION}
    {$ENDIF}
    

Определение для расширенного класса-дженерика: 

_GenericAdvancedList.inc:_
    
    
    {$IFDEF INTERFACE}     
      TListIndex = TAdvancedListIndex;
      TListItem = TAdvancedListItem;
    {$DEFINE INTERFACE}     
    {$INCLUDE 'GenericList.inc'}
      // TGAdvancedList<TAdvancedListIndex, TAdvancedListItem> = class(TGList)
      TGAdvancedList = class
        Capacity: TAdvancedListIndex;
      end;
    {$UNDEF INTERFACE}
    {$ENDIF}
    
    {$IFDEF IMPLEMENTATION}
    {$INCLUDE 'GenericList.inc'}
    {$UNDEF IMPLEMENTATION}
    {$ENDIF}
    

Пример без некоторой специализации: 

_AdvancedListInteger.pas:_
    
    
    unit AdvancedListInteger;
    
    interface
    
    uses
      Classes;
    
    type
      TAdvancedListIndex = Integer; // указан некоторый точный тип
      TAdvancedListItem = Integer; // указан некоторый точный тип
    {$DEFINE INTERFACE}
    {$INCLUDE 'GenericAdvancedList.inc'}
    
    type
      // TAdvancedListInteger<Integer, Integer> = class
      TAdvancedListInteger = class(TGAdvancedList)
        // Дополнительные поля и методы могут быть добавлены здесь
      end;
    
    implementation
    
    {$DEFINE IMPLEMENTATION}
    {$INCLUDE 'GenericAdvancedList.inc'}
    
    end.
    

Теперь у нас есть специализированный класс из класса-дженерика, который наследуется от другого класса-дженерика. 

### Ограничения

Хорошо иметь класс, который, например, работает как контейнер для любого другого типа. Класс-контейнер не должен ничего знать о содержащемся классе. Но некоторые классы могут извлечь пользу из знания того, что можно сделать с помощью закрытого класса. В реализации нативных дженериков есть возможность ограничить группу типов, которые можно использовать в качестве параметров типа класса-дженерика. В шаблонах такой функции нет, но ограничения выполняются просто путем использования типа-дженерика в классе-дженерике. Если тип-дженерик используется для сложения и вычитания, то могут использоваться только те типы, которые поддерживают эти операции. Строка также может быть объединена с помощью оператора плюс. Но если используется умножение, то допускаются только числовые типы. Если тип-джернерик используется в качестве индекса массива, то могут использоваться только порядковые типы. Если некоторые методы, например, `Free`, используется над типом-дженериком, тогда ограничение включает все классы. Поэтому в шаблонах операции, выполняемые с дженерик-типами, ограничивают набор используемых типов для специализации. 

**Теоретически ограничиваемые группы:**

  * порядковые типы
  * типы с плавающей точкой
  * записи (records)
  * массивы
  * наборы данных (sets)
  * интерфейсы
  * процедуры, функции, конструкторы
  * классы со всеми потомками



## Классы-дженерики

Есть много классов, которые могут извлечь выгоду из существования дженериков: 

  * TList - упорядоченный список элементов, методы Add, Delete, Insert, Move, Exchange, Clear, Sort
  * TDictionary - список пар ключ-значение
  * TPair - Ключ и значение для использования в TDictionary
  * TStack - Структура [LIFO](<https://ru.wikipedia.org/wiki/FIFO_%D0%B8_LIFO>), Push и Pop (реализация стека (последний пришел, первый вышел))
  * TQueue - Структура [FIFO](<https://ru.wikipedia.org/wiki/%D0%9E%D1%87%D0%B5%D1%80%D0%B5%D0%B4%D1%8C_\(%D0%BF%D1%80%D0%BE%D0%B3%D1%80%D0%B0%D0%BC%D0%BC%D0%B8%D1%80%D0%BE%D0%B2%D0%B0%D0%BD%D0%B8%D0%B5\)>), Enqueue и Dequeue (реализация очереди (первый пришел, первый вышел))
  * TRange или TInterval - структура двух значений, Расстояние
  * TSet - набор элементов, Add, Remove, Intersection, Complement, Union, Product
  * TStream - разделяется на TInputStream и TOutputStream
  * TTree - иерархическая структура с узлами
  * TMatrix - многомерный список
  * TBitmap - расширенный TMatrix с графически ориентированными методами
  * TGraph
  * TPoint - может быть 1D, 2D, 3D или многомерным
  * TVector
  * TComplexNumber



There are some interesting specialized types which can replace non-generic types: Есть несколько интересных специализированных типов, которые могут заменить типы-не-дженерики: 

Specialized generic class | Similar data type   
---|---  
TList<Char> | string   
TList<WideChar> | WideString   
TList<Byte> | array of Byte   
TList<string> | Classes.TStrings   
TList<Pointer> | Classes.TList   
TList<Boolean> | Classes.TBits   
TList<TObject> | Contnrs.TObjectList   
TList<TComponent> | Contnrs.TComponentList   
TList<TClass> | Contnrs.TClassList   
TStack<Pointer> | Contnrs.TStack   
TStack<TObject> | Contnrs.TObjectStack   
TQueue<Pointer> | Contnrs.TQueue   
TQueue<TObject> | Contnrs.TObjectQueue   
TStream<Byte> | Classes.TMemoryStream   
TDictionary<TPair<string, string>> | Classes.TStringList   
TList<TMethod> | LCLProc.TMethodList   
TTree<TTreeNode> | ComCtrls.TTreeView   
TPoint<Integer> | Classes.TPoint   
TPoint<SmallInt> | Classes.TSmallPoint   
TRectangle<Integer> | Classes.TRect   
  
## Существующие библиотеки дженериков

Каждый программист может создать собственную библиотеку универсальных классов. 

**Некоторые готовые к использованию классы:**

  * **TemplateGenerics** \- Пакет Lazarus из нескольких экспериментальных классов-дженериков. В дополнение к параметризованному типу значения списка он также предлагает параметризованный тип индекса. Последний пакет можно скачать из репозитория SVN с помощью клиента SVN.


    
    
    svn co http://svn.zdechov.net/svn/PascalClassLibrary/Generics/TemplateGenerics/ TemplateGenerics

  * **rtl-generics package** \- разработка Maciej Izak, которая входит в состав исходного кода fpc, начиная с версии 3.1.1 +, и находится в корневом каталоге компилятора в директории _\packages\rtl-generics_. Актуальную версию библиотеки можно найти на [github'е](<https://github.com/maciej-izak/generics.collections>)



## См. также

  * [Generics](<Generics.md> "Generics/ru")



## Внешние ссылки

  * [Object Pascal (Delphi) Templates](<http://www.dummzeuch.de/delphi/object_pascal_templates/english.html>)
  * [Templates in Object Pascal](<http://edn.embarcadero.com/article/27603>)
  * [Template (programming)](<http://en.wikipedia.org/wiki/Template_%28programming%29>) \- general description and priciples
  * [Generics with Delphi 2009 Win32](<http://sjrd.developpez.com/delphi/tutoriel/generics/>)

---

_Source: [https://wiki.freepascal.org/Templates/ru](https://web.archive.org/web/20250512084158/https://wiki.freepascal.org/Templates/ru)_
