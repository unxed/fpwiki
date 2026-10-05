# TStringList-TStrings Tutorial

│ **[English (en)](<../en/TStringList-TStrings_Tutorial.md>)** │  **русский (ru)** │

## Contents

  * 1 TStringList
    * 1.1 Простой пример
    * 1.2 Расширенный пример
    * 1.3 Преобразование в строки с разделителями и обратно
  * 2 Обработка файлов
  * 3 Сравнение динамических массивов и TStringList
  * 4 Продолжайте учиться



## TStringList

TStringList (или его родитель, TStrings) очень похоже на динамический массив или набор строк (в FPC нету реализации набора строк). Он часто используется при программировании. 

### Простой пример
    
    
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

### Расширенный пример

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
    

Однако, чтобы избежать возможных утечек памяти, следует всегда использовать блок [Try](<Try.md> "Try/ru") \- [Finally](</index.php?title=Finally/ru&action=edit&redlink=1> "Finally/ru \(page does not exist\)") где это возможно, чтобы получился примерно такой код: 
    
    
    var
      slist: TStringList;
    
    ...
    
    slist := TStringList.Create;
    try
      ...
      // работа с Вашим stringlist  
      ...
    finally
      if Assigned(slist) then
        FreeAndNil(slist);
    end;
    
    
    
    // Данный код прекрасно работает без двойного создания stringlist... комментарии можно отправить по адресу edgarrod71@gmail.com
    function theStringList: TStringList;
    var
     J: integer;
    begin
      result := TStringList.Create;
      for J:=0 to 10 do
        result.add(intToStr(J));
    end;
    
    procedure Caller;
    var
      SL: TStringList;
      K: integer;
    begin
      SL := theStringList;
      for K:=0 to pred(SL.Count) do
        writeln(SL[K]);
      if assigned(SL) then
        SL.Free;
    end;
    

Для перемещения по списку строк лучше использовать цикл **while-do** , чем **for-to**. Причиной является то, что список может быть пустым и в этом случае цикл **for-to** будет работать некорректно. В этом примере процедура SubscriberDel вызывается для каждого ID в списке и список идентификаторов передается в процедуру как строка. 
    
    
    procedure SubscribersDelete(const Subscriber_ID: string);
    var
      List : TStringList; 
      i : Integer;
    begin
      List := TStringList.Create;
      List.Text := Subscriber_ID;
      i := 0;
      while i < List.Count do
        begin
          SubscriberDel(List.Strings[i]);
          inc(i);
        end;
      List.Free;
    end;
    

### Преобразование в строки с разделителями и обратно

Результатом выполнения следующего кода будет stringlist, содержащий 4 элемента ('1', '2', '3' и '4'); 
    
    
    procedure Sample;
    var
        MyStringList: TStringList=nil;
    begin
         MyStringList:= TStringList.create;
         MyStringList.Delimiter := ';';
         MyStringList.DelimitedText:='1;2;3;4';
         MyStringList.free;
    end;
    

Соответственно, следующий код соберет stringlist в строку с разделителями ('1;2;3;4'): 
    
    
    function Sample2 : string;
    var
        MyStringList: TStringList=nil;
    begin
         MyStringList:= TStringList.create;
         MyStringList.Delimiter := ';';
         MyStringList.Add('1');
         MyStringList.Add('2');
         MyStringList.Add('3');
         MyStringList.Add('4');
         Result :=MyStringList.DelimitedText;
         MyStringList.free;
    end;
    

Учтите, что разделитель - это символ, а **не** строка! Если Ваш разделитель является строкой (например, „\n‟), Вы можете использовать нижеприведенный код для получения stringlist, содержащего 4 элемента ('1', '2', '3' и '4'): 
    
    
    procedure Sample;
    var
        MyStringList: TStringList=nil;
    begin
         MyStringList:= TStringList.create;
         MyStringList.text:=StringReplace('1\n2\n3\n4','\n',Lineending,[rfReplaceAll, rfIgnoreCase]);
         MyStringList.free;
    end;
    

И, наоборот, следующая [функция](<Function.md> "Function/ru") вернет „1\n2\n3‟: 
    
    
    Function Sample : string;
    var
        MyStringList: TStringList=nil;
    begin
         MyStringList:= TStringList.create;
         MyStringList.SkipLastLineBreak := True;
         MyStringList.add('1');
         MyStringList.add('2');
         MyStringList.add('3');
         result := StringReplace(MyStringList.Text,Lineending,'\n', [rfReplaceAll, rfIgnoreCase]);
         MyStringList.free;
    end;
    

## Обработка файлов

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

## Сравнение динамических массивов и TStringList

TStringList это просто усовершенствованная версия динамического массива. Некоторые методы имеют сходства: 

Операция | array of string | TStringList   
---|---|---  
Объявление переменной | StringList: array of string; | StringList: TStringList;   
Инициализация | неявный конструктор | StringList := TStringList.Create   
Установка размера | SetLength(StringList, X); | StringList.Size := X;   
Получение размера | X := Length(StringList); | X := StringList.Count;   
Добавление пункта | SetLength(StringList, Length(StringList) + 1); StringList[Length(StringList) - 1] := X; | StringList.Add(X);   
Удаление пункта | for I := Index to Length(StringList) - 2 do StringList[I] := StringList[I + 1]; SetLength(StringList, Length(StringList) - 1); | StringList.Delete(Index);   
Удаление всех пунктов | SetLength(StringList, 0); | StringList.Clear;   
Завершение | неявный деструктор | StringList.Free;   
  
Но TStringList предлагает гораздо больше возможностей, чем базовая структура, такая как динамический массив. 

## Продолжайте учиться

TStringList имеет много других интересных особенностей: 

  1. Позволяет отсортировать строки.
  2. Позволяет оставить в списке только уникальными строки.
  3. Можно получить все строки стразу использую свойство **Text**.
  4. Можно сохранить объект или другие данные вместе со строкой.



Чтобы узнать все процедуры, функции и свойства, посмотрите [TStringList documentation](<http://www.freepascal.org/docs-html/rtl/classes/tstringlist.html>)... или справку в Lazarus.

---

_Source: [https://wiki.freepascal.org/TStringList-TStrings_Tutorial/ru](https://web.archive.org/web/20250420073813/https://wiki.freepascal.org/TStringList-TStrings_Tutorial/ru)_
