# TString List-TString Tutorial

│ **[English (en)](<../en/TString_List-TString_Tutorial.md>)** │  **русский (ru)** │

From Free Pascal wiki


## Contents

  * 1 TStringList
    * 1.1 Простой пример
    * 1.2 Расширенный пример
  * 2 Обработка файлов
  * 3 Сравнение динамических массивов и TStringList
  * 4 Продолжайте учиться

  
---  
  
##  TStringList

TStringList (или его родитель, TStrings) очень похоже на динамический массив или набор строк (в FPC нету реализации набора строк). Он часто используется при программировании. 

###  Простой пример
    
    
    program StrList;
    {$mode objfpc}
    uses
     Classes, SysUtils;
    var
      Str: TStringList;
    begin
      Str := TStringList.Create; // Это необходимо при использовании этого 
                                 // класса (как и большинства классов)
      Str.Add('Some String!');
      Writeln('StringList содержит ' + IntToStr(Str.Count) + ' строк.');
      Readln;
      Str.Free; //Освобождение памяти, занятой классом
    end.

Это простая консольная программа, которая добавляет одну строку и выводит количество строк. Некоторые вещи, которые вы должны знать: 

**Create** \- Создаст список строк для изменения. Если вы вызвали **Create** , в конце вы должны вызвать **Free** для освобождения занимаемой памяти. Если этого не сделать, то программа не рухнет, но вся память освобождена не будет, что приведет к утечке памяти. 

**Count** \- Это свойство возвращает текущее количество строк. 

**Add** \- Этот метод добавляет одну строку в StringList. Он возвращает **Индекс** добавленной строки. 

**Delete** \- Удаляет строку из StringList. Для удаления строки, нужно ввести её индекс. Как я уже говорил: это как динамический массив. 

**IndexOf** \- Возвращает индекс строки в списке. Если строка не найдена, вернет -1. 

**Clear** \- Очищает список. 

###  Расширенный пример

Как насчет более "сочного" примера, а? 
    
    
    program StrList2;
    {$mode ObjFPC}
    uses
    Classes, SysUtils;
     
    var
      Str: TStringList;
      S: string;
      Counter: Integer;
    begin
      Str := TStringList.Create;
      Writeln('String List Test');
      repeat
        Writeln('Enter a string to add');
        Readln(S); 
        if (S = 'EXIT') then Halt; //Немедленное завершение программы
        //Если вы внимательно посмотрите, то увидите, что это приводит к утечке памяти.
        if (S <> '') then
        begin
          Counter := Str.Add(S);
          Writeln('String: ' + S + ' was Added!');
          Writeln('Index is: ' + IntToStr(Counter)); // Counter будет всегда равен 
                                                     // индексу последнего добавленного элемента.
        end 
        else 
        begin
          Writeln('No data entered...');
        end;
      until (S = 'EXIT');
      Str.Free; // Освобождение занятой памяти
    end.

##  Обработка файлов

При использовании TStringList, у вас есть две процедуры для работы с файлами: **SaveToFile** и **LoadFromFile**. SavetoFile сохранит все строки в файл. LoadFromFile откроет файл и добавить данных из файла в список строка за строкой. 
    
    
    program StrListFile;
    {$mode objfpc}
    uses
     Classes, SysUtils;
     
    var
      Str: TStringList;
    begin
      Str := TStringList.Create;
      Str.LoadFromFile('SomeFile.txt');
      Str.Add('Hello');
      Str.SaveToFile('SomeFile.txt');
      Str.Free;
    end.

Вы только что открыли файл, отредактировали его и сохранили обратно! 

##  Сравнение динамических массивов и TStringList

TStringList это просто усовершенствованная версия динамического массива. Некоторые методы имеют сходства: 

Операция  |  array of string  |  TStringList   
---|---|---  
Объявление переменной  |  StringList: array of string;  |  StringList: TStringList;   
Инициализация  |  неявный конструктор  |  StringList := TStringList.Create   
Установка размера  |  SetLength(StringList, X);  |  StringList.Size := X;   
Получение размера  |  X := Length(StringList);  |  X := StringList.Count;   
Добавление пункта  |  SetLength(StringList, Length(StringList) + 1); StringList[Length(StringList) - 1] := X;  |  StringList.Add(X);   
Удаление пункта  |  for I := Index to Length(StringList) - 2 do StringList[I] := StringList[I + 1]; SetLength(StringList, Length(StringList) - 1);  |  StringList.Delete(Index);   
Удаление всех пунктов  |  SetLength(StringList, 0);  |  StringList.Clear;   
Завершение  |  неявный деструктор  |  StringList.Free;   
  
Но TStringList предлагает гораздо больше возможностей, чем базовая структура, такая как динамический массив. 

##  Продолжайте учиться

TStringList имеет много других интересных особенностей: 

  1. Позволяет отсортировать строки. 
  2. Позволяет оставить в списке только уникальными строки. 
  3. Можно получить все строки стразу использую свойство **Text**. 
  4. Можно сохранить объект или другие данные вместе со строкой. 



Чтобы узнать все процедуры, функции и свойства, посмотрите [TStringList documentation](<http://www.freepascal.org/docs-html/rtl/classes/tstringlist.html>)... или справку в Lazarus.

---

_Source: [https://wiki.freepascal.org/TString_List-TString_Tutorial/ru](https://web.archive.org/web/20130615230538/https://wiki.freepascal.org/TString_List-TString_Tutorial/ru)_
