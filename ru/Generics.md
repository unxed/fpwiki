# Generics

│ **[English (en)](<../en/Generics.md>)** │  **русский (ru)** │

## Contents

  * 1 Вступление
  * 2 Модуль fgl
    * 2.1 Как начать
  * 3 Пользовательские классы дженериков
  * 4 Другие пункты
  * 5 Примеры
  * 6 См. также
  * 7 Внешние ссылки



## Вступление

[FPC](<../en/FPC.md> "FPC") имеет официальную поддержку дженериков(обобщений) в синтаксисе [`{$mode ObjFPC}`](<../en/Mode_ObjFPC.md> "Mode ObjFPC"), начиная с версии 2.2, и [`{$mode Delphi}`](<../en/Mode_Delphi.md> "Mode Delphi") синтаксис начиная с версии 2.6.0.   
Дженерики иногда называют параметризованными типами.   
Причина, по которой FPC поддерживает два разных диалекта, Objfpc и Delphi, заключается в том, что у FPC были дженерики до Delphi. Можно использовать модули, написанные в синтаксисе Objfpc, в других модулях, использующих синтаксис Delphi, и наоборот. 

**Free Generics Library** или **FGL** является нативной реализацией [class templates](<../en/Templates.md> "Templates"), написанной в обобщенном синтаксисе Objfpc.   
**Rtl-generics package** является реализацией [class templates](<../en/Templates.md> "Templates"), написанной в синтаксисе дженериков Delphi, и пытается быть совместимым с библиотекой дженериков Delphi. Этот пакет является стандартным в FPC 3.1.1.+, но есть версия, доступная для FPC 3.0.4.  
Обе [библиотеки], **FGL** и rtl-generics могут использоваться в обоих режимах. 

## Модуль fgl

Самый простой способ начать работу с дженериками - это использовать [fgl unit](<http://freepascal.org/docs-html/current/rtl/fgl/index.html>), который является модулем-прототипом для классов дженериков базовой системы. Пока он содержит несколько основных классов: 

  * TFPGList
  * TFPGObjectList
  * TFPGInterfacedObjectList
  * TFPGMap



### Как начать

В следующем простом примере показано, как хранить несколько экземпляров определенного пользователем класса в списке: 
    
    
    {$mode objfpc}
    uses fgl;
    
    type
       TMyClass = class(TObject)
          fld1 : string;
       end;
    
       TMyList = specialize TFPGObjectList<TMyClass>;
    
    var
       list : TMyList;
       c : TMyClass;
    
    begin
       // создаем список и добавляем элемент
       list := TMyList.Create;
       c := TMyClass.Create;
       c.fld1 := 'c1';
       list.Add(c);
       // получаем элемент из списка
       c := list[0];
    

## Пользовательские классы дженериков

Если дженерики, определенные в модуле fgl, не соответствуют вашим потребностям, вам может потребоваться определить свои собственные классы дженериков с нуля, используя базовые языковые примитивы. 

Класс-дженерик определяется с помощью [keyword](</index.php?title=keyword&action=edit&redlink=1> "keyword \(page does not exist\)") (ключевого слова) **generic** перед именем класса и используется в объявлении класса: 
    
    
    type
      generic TList<T> = class
        Items: array of T;
        procedure Add(Value: T);
      end;
    

Пример реализации класса-дженерика: 
    
    
    implementation
    
    procedure TList.Add(Value: T);
    begin
      SetLength(Items, Length(Items) + 1);
      Items[Length(Items) - 1] := Value;
    end;
    

Класс-дженерик может быть просто специализирован для определенного типа с помощью ключевого слова **specialize**. 
    
    
    Type  
      TIntegerList = specialize TList<Integer>;
      TPointerList = specialize TList<Pointer>;
      TStringList = specialize TList<string>;
    

## Другие пункты

  1. Компилятор анализирует общий тип, но вместо генерации кода он сохраняет все токены в буфере токенов внутри файла PPU.
  2. Компилятор разбирает специализацию; для этого он загружает буфер токенов из файла PPU и анализирует его снова. Он заменяет общие параметры (в большинстве примеров "T") конкретным заданным типом (например, **LongInt** , **TObject**).



Код в основном выглядит так, как будто тот же класс был написан как общий, но с заменой T на данный тип. 

Поэтому в теории не должно быть различий в скорости между «нормальным» классом и классом-дженериком. 

## Примеры

Пример использования дженериков для написания функции gmax(), которая принимает максимум две еще-не-типизированные переменные. Обратите внимание, что функции имеют пространство имен по имени класса. Недостатком может быть то, что дженерики не могут быть перегружены. 
    
    
    program UseGenerics;
    
    {$mode objfpc}{$H+}
    
    type
      generic TFakeClass<_GT> = class
        class function gmax(a,b: _GT):_GT;
      end;
    
      TFakeClassInt = specialize TFakeClass<integer>;
      TFakeClassDouble = specialize TFakeClass<double>;
    
      class function TFakeClass.gmax(a,b: _GT):_GT;
      begin
        if a > b then 
          result := a
        else 
          result := b;
      end;
    
    begin
        // показать большее из двух целых [чисел]
      writeln( 'Integer GMax:', TFakeClassInt.gmax( 23, 56 ) );
        // показать большее из двух [чисел] с плавающей точкой
      writeln( 'Double GMax:', TFakeClassDouble.gmax( 23.89, 56.5) );
      readln();
    end.
    

## См. также

  * [Templates](<Templates.md> "Templates/ru")
  * [Generics proposals](<../en/Generics_proposals.md> "Generics proposals")
  * [Data Structures, Containers, Collections](<../en/Data_Structures,_Containers,_Collections.md> "Data Structures, Containers, Collections")



## Внешние ссылки

  * [Free Pascal: Reference guide - Generics](<http://www.freepascal.org/docs-html/ref/refch8.html>)
  * [Используем дженерики в Delphi!](<https://habr.com/ru/articles/314780/>)
  * [Использование стандартных дженериков Delphi для работы с наборами данных](<http://proghouse.ru/programming/49-delphi-generics>)

---

_Source: [https://wiki.freepascal.org/Generics/ru](https://web.archive.org/web/20250601000000/https://wiki.freepascal.org/Generics/ru)_
