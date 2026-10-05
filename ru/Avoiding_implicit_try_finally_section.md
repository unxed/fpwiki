# Avoiding implicit try finally section

│ **[English (en)](<../en/Avoiding_implicit_try_finally_section.md> "Avoiding implicit try finally section")** │  **[suomi (fi)](</Avoiding_implicit_try_finally_section/fi> "Avoiding implicit try finally section/fi")** │  **[Bahasa Indonesia (id)](</Avoiding_implicit_try_finally_section/id> "Avoiding implicit try finally section/id")** │  **русский (ru)** │    
****

## Contents

  * 1 Обзор
  * 2 Возможные решения
  * 3 Возможные риски
  * 4 Пример программы



## Обзор

Иногда полезно знать, что компилятор может обернуть код в неявный try ... finally блок. По сути дела это необходимо, когда вы используете переменную любого типа, которая должна быть инициализирована / деинициализирована (освобождена). Иными словами, стандартные процедуры Initialize() и Finalize() должны что-то сделать с ней. Это может быть актуально, например, при использовании переменных стандартных типов подобных AnsiStrings, Variants или динамических массивов. 

Для примера, рассмотрим следующею процедуру: 
    
    
    procedure P;
    var 
      S: AnsiString;
    begin
      ... делаем что-то с S ...
    end;
    

Данная процедура, фактически компилируется как: 
    
    
    procedure P;
    var 
      S: AnsiString;
    begin
     Initialize(S);
     try
      ... делаем что-то с S ...
     finally Finalize(S) end;
    end;
    

Это необходимо, чтобы быть уверенным, что счетчик ссылок на переменную S, будет правильным, если процедура P завершится с ошибками. Однако, в некоторых случаях это может существенно повлиять на скорость данного кода. 

Единственный правильный способ узнать, что происходит – просмотр вывода кода ассемблер. 

Более подробную информацию, можете найти здесь: <http://www.mail-archive.com/fpc-devel@lists.freepascal.org/msg01367.html>

## Возможные решения

  * используйте директиву компилятора {$implicitexceptions off}, но только в окончательных версиях программы, представляющих собой готовый программный продукт. Отладка программы с этой директивой, может стать проблемной, из-за особенностей обнаружения утечек памяти и её повреждения.


  * разбить редко использующийся код в катером находится не явный блок try…finally на отдельные процедуры. (Вы можете использовать процедуры внутри других процедур)


  * использовать параметры-константы, а не параметры-значение (ключевое слово const при объявлении параметров). Это исключает необходимость менять счетчик ссылок, но временные переменные внутри процедур все еще могут быть проблемой.


  * использовать глобальные переменные. Однако, в данном вопросе, следует проявить осторожность.


  * использовать типы переменных, не зависящих от счётчика ссылок, например shortstring.



## Возможные риски

[![Warning-icon.png](https://wiki.freepascal.org/images/b/b2/Warning-icon.png)](</File:Warning-icon.png>)

**Предупреждение:** Используйте осторожно вызов данных исключений. Если вы оставите их без обработки, то возможны утечки памяти.

В 2007 году, директива $implicitexceptions была добавлена к модулю strutils. Если вы используете данный модуль, то обратите внимание на следующие нюансы: 

  * Процедура, использующая другую процедуру, в которой вызывается исключение не безопасна. Например – strtoint, но не strtointdef.
  * Процедура, которая вызывает исключения, сама по себе небезопасна.
  * Не используйте очень большие процедуры из-за риска ошибок и низкой производительности их кода. Например, форматирование даты и времени.
  * Использование чисел с плавающей запятой может вызвать исключения, которые будут перехвачены исключениями модуля sysutils.



Если вы обнаружили проблемы с этими изменениями, пожалуйста, свяжитесь с Marco. 

## Пример программы

Небольшая демонстрация программа, которая: 

  * При запуске показывает, что отсутствие неявного try ... finally блока, может сделать код намного быстрее. Результаты её тестирования:


    
    
    Время Foo_Normal: 141
    Время Foo_Faster: 17
    

  * Показывает использование неявного блока try ... finally (без изменения сути или безопасности кода) в некоторых случаях (когда вам не нужно использовать AnsiString AnsiString/Variant/и т.п. каждый раз, когда процедура вызывается, а, например, только если параметр не имеет определенного значения).



  

    
    
    {$mode objfpc}{$H+}
    
    uses
      {BaseUnix, Unix нужны только для реализации функции Clock} BaseUnix, Unix,
      SysUtils;
    
    function Clock: Int64;
    var Dummy: tms;
    begin
     Clock := FpTimes(Dummy);
    end;
    
    procedure Foo_Normal(i: Integer);
    var S: string;
    begin
     if i = -1 then
     begin
      S := 'Некоторые операции с AnsiString';
      raise Exception.Create(S);
     end;
    end;
    
    procedure Foo_Faster(i: Integer);
    
      procedure RaiseError;
      var S: string;
      begin
       S := 'Некоторые операции с AnsiString';
       raise Exception.Create(S);
      end;
    
    begin
     if i = -1 then RaiseError;
    end;
    
    { Обратите внимание, что, когда я называю Foo_Normal и Foo_ResourceString
      i всегда >= 0 так что исключения никогда не происходит.
      Поэтому строковые константы SNormal и SResString не используется. }
    
    const
      TestCount = 10000000;
    var
      i: Integer;
      Start: Int64;
    begin
     Start := Clock;
     for i := 0 to TestCount do Foo_Normal(i);
     Writeln('Время Foo_Normal: ', Clock - Start);
    
     Start := Clock;
     for i := 0 to TestCount do Foo_Faster(i);
     Writeln('Время Foo_Faster: ', Clock - Start);
    end.

---

_Source: [https://wiki.freepascal.org/Avoiding_implicit_try_finally_section/ru](https://web.archive.org/web/20250113130051/https://wiki.freepascal.org/Avoiding_implicit_try_finally_section/ru)_
