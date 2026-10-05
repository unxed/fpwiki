# Custom Attributes

│ **[English (en)](<../en/Custom_Attributes.md>)** │  **русский (ru)** │

  
Пользовательские атрибуты в настоящее время позволяют украшать определения типов и published свойства классов дополнительными метаданными, которые можно запрашивать с помощью RTTI (информация о типе времени выполнения). 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Примечание:** В настоящее время (по состоянию на октябрь 2020 г.) эта функция доступна только в транке FPC (также известной как 3.3.1).

  


## Contents

  * 1 Для чего можно использовать атрибуты?
  * 2 Как объявляются атрибуты?
  * 3 Как используются атрибуты?
    * 3.1 Запрос атрибутов
  * 4 Совместимость функций атрибутов с Delphi
  * 5 Полный пример
  * 6 См.также



## Для чего можно использовать атрибуты?

Атрибут используется для связывания определенных метаданных с классом. Например, вы можете использовать атрибут, чтобы пометить класс именем соответствующей таблицы базы данных или аннотировать класс веб-службы строкой, определяющей его базовый путь. 

## Как объявляются атрибуты?

Атрибуты — это просто классы, происходящие от нового системного типа `TCustomAttribute`. Конструкторы таких потомков `TCustomAttribute` являются наиболее важной функцией для реализации, поскольку они используются для передачи дополнительных параметров атрибуту (таких, как имя связанной таблицы базы данных или базовый путь в приведенных выше примерах). 

**Важно** : если вы хотите использовать свой тип атрибута без каких-либо аргументов, вы _должны_ самостоятельно объявить конструктор без параметров, поскольку один из `TCustomAttribute` является закрытым (private). 

## Как используются атрибуты?

Атрибуты привязываются к типу или свойству путем указания хотя бы одного предложения атрибута перед типом или свойством. Для типа атрибут указывается в определении типа (например, в объявлении класса, записи или перечисления) или в повторном объявлении уникального типа (например, `TLongInt = type LongInt`). Простое переименование типов (например, `TLongInt = LongInt`) не допускается. 

Предложения атрибутов доступны только в том случае, если установлен новый переключатель режимов `PREFIXEDATTRIBUTES`, который используется по умолчанию в режимах Delphi и DelphiUnicode. 

Синтаксис предложения атрибута следующий: 
    
    
     ATTRIBUTECLAUSE::='[' ATTRIBUTELIST ']'
     ATTRIBUTELIST::=ATTRIBUTE [, ATTRIBUTELIST ]
     ATTRIBUTE::=IDENTIFIER [ ( PARAMLIST ) ]
     PARAMLIST::=CONSTEXPR [, PARAMLIST ]
    

_IDENTIFIER_ — это имя класса атрибута. Если вы называете класс атрибута заканчивающимся на "Attribute" (случай суффикса "Attribute" не имеет значения), то имя может использоваться впоследствии без суффикса "Attribute". Таким образом, `TMyAttribute` и `TMy` являются эквивалентными альтернативами в следующем примере: 
    
    
    program tcustomattr;
    
    {$mode objfpc}{$H+}
    {$modeswitch prefixedattributes}
    
    type
       TMyAttribute = class(TCustomAttribute)
         constructor Create;
         constructor Create(aArg: String);
         constructor Create(aArg: TGUID);
         constructor Create(aArg: LongInt);
       end;
    
       {$M+}
       [TMyAttribute]
       TTestClass = class
       private
         fTest: LongInt;
       published
         [TMyAttribute('Test')]
         property Test: LongInt read fTest;
       end;
       {$M-}
    
       [TMyAttribute(1234)]
       [TMy('Hello World')]
       TTestEnum = (
         teOne,
         teTwo
       );
    
       [TMyAttribute(IInterface), TMy(42)]
       TLongInt = type LongInt;
    
    constructor TMyAttribute.Create;
    begin
    end;
    
    constructor TMyAttribute.Create(aArg: String);
    begin
    end;
    
    constructor TMyAttribute.Create(aArg: LongInt);
    begin
    end;
    
    constructor TMyAttribute.Create(aArg: TGUID);
    begin
    end;
    
    begin
    
    end.
    

### Запрос атрибутов

Атрибуты могут быть доступны как модулю `TypInfo`, так и модулю `Rtti`. 

Для модуля `TypInfo` доступны следующие способы доступа к атрибутам: 

Для типов: 

  * используйте поле `AttributesTable` в `TTypeData`
  * использовать `GetAttributeTable` для `PTypeInfo`
  * используйте `GetAttribute` в таблице атрибутов вместе с индексом, чтобы получить экземпляр `TCustomAttribute`



Для свойств: 

  * используйте `AttributesTable` из `TPropInfo`
  * используйте `GetAttribute` в таблице атрибутов вместе с индексом, чтобы получить экземпляр `TCustomAttribute`
  * используйте `GetPropAttribute` в `PPropInfo` вместе с индексом, чтобы получить экземпляр `TCustomAttribute`



Для модуля `Rtti` доступны следующие способы доступа к атрибутам: 

Для типов: 

  * используйте `GetAttributes` для `TRttiType` рассматриваемого типа



Для свойств: 

  * используйте `GetAttributes` для `TRttiProperty` рассматриваемого свойства



## Совместимость функций атрибутов с Delphi

Сама функция совместима с Delphi при условии, что FPC гораздо более неумолим в отношении несвязанных свойств. Если класс атрибута неизвестен или предложения атрибута не связаны с допустимым типом или свойством, компилятор выдаст ошибку. 

RTTI от FPC не считается совместимым с Delphi. Однако он охватывает ту же функциональность. В отличие от Delphi (который использует Invoke для создания экземпляра атрибута), FPC использует функцию конструктора. Преимущество реализации FPC заключается в том, что она работает на системах, которые не имеют полной поддержки Invoke. 

Кроме того, использование переключателя режима _PREFIXEDATTRIBUTES_ отключает разделы директив для функций, методов и типов процедур/методов. 

Следующее больше не разрешено при включенном переключателе режимов _PREFIXEDATTRIBUTES_ : 
    
    
    procedure Test; [cdecl];
    begin
    end;
    

## Полный пример
    
    
    program testattributes;
    {
      Это простой пример использования настраиваемых атрибутов.
      Класс использует настраиваемый атрибут для получения статической даты.
      при запуске программы. Это только для демонстрации.
      Это демо:
      - создает
      - украшает
      - возвращает
    }
       
    {$mode delphi}{$H+}{$M+}
    {$warn 5079 off} { отключаем экспериментальное предупреждение }
    uses
      sysutils, typinfo, rtti, classes;
     
    type
       {Пользовательский атрибут для украшения класса с определенной датой }
       ADateTimeAttribute = class(TCustomAttribute)
       private
         FArg:TDateTime;
       public
       { Просто чтобы показать, что пользовательский атрибут может иметь несколько конструкторов }
         constructor Create(const aArg: TDateTime);overload;
         { Мы можем использовать предопределенное включение компилятора %DATE%
           В контексте пользовательского атрибута нам нужно
           постоянное выражение для параметра по умолчанию}
         constructor Create(const aArg: String = {$I %DATE%});overload;
        published
         property Date:TDateTime read Farg;
       end;
     
       { Класс datetime, украшенный нашим пользовательским атрибутом                          }
       { Обратите внимание, что вы можете не указывать 'Attribute', компилятор разрешает это. }
       { [ADateTime(21237.0)] использует первый конструктор, отображая дату в 1958 году       }
     
       {Это вызывает второй конструктор }
       [ADateTime]
       TMyDateTimeClass = class
       private
         FDateTime:TDateTime;
       public
         constructor create;
       published
         [ADateTime]
         property Day:TDateTime read FDatetime write FdateTime;
       end;
     
       constructor ADateTimeAttribute.Create(const aArg: TDateTime);
       begin
         FArg := aArg;
       end;
     
       constructor ADateTimeAttribute.Create(const aArg: string );
       var
         MySettings:Tformatsettings;
       begin
         { настраиваем формат даты в соответствии с тем, как 
           задан включенный формат компилятора %DATE% }
         MySettings :=DefaultFormatSettings;
         MySettings.ShortDateFormat:='yyyymmdd';
         MySettings.DateSeparator :='/';
         { Теперь конвертируем }
         FArg := StrToDateTime(aArg, MySettings);
       end;
       
       { Мы запрашиваем rtti, чтобы задать значение }
       constructor TMyDateTimeClass.Create;
       var
        Context : TRttiContext;
        AType : TRttiType;
        Attribute : TCustomAttribute;
       begin
         inherited;  
         Context := TRttiContext.Create;
         try
           AType := Context.GetType(typeinfo(TMyDateTimeClass));
           for Attribute in  AType.GetAttributes do begin
             if Attribute is ADateTimeAttribute then
               FDateTime := ADateTimeAttribute(Attribute).Date;
           end;
         finally
           Context.Free
         end;    
       end;
     
     
    var
     Test:TMyDateTimeClass;      
    begin
       Test := TMyDateTimeClass.Create;
       try
         writeln('Дата компиляции :',DateTimeToStr(Test.Day));
      finally
        test.free;
      end;
    end.
    

## См.также

  * [FPC New Features Trunk](<../en/FPC_New_Features_Trunk.md> "FPC New Features Trunk")
  * <http://docwiki.embarcadero.com/RADStudio/XE6/en/Overview_of_Attributes> \- Справочник по атрибутам Delphi
  * <http://delphi.about.com/od/oopindelphi/a/delphi-attributes-understanding-using-attributes-in-delphi.htm> Учебник Delphi по атрибутам свойств

---

_Source: [https://wiki.freepascal.org/Custom_Attributes/ru](https://web.archive.org/web/20250427005530/https://wiki.freepascal.org/Custom_Attributes/ru)_
