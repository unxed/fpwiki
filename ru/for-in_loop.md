# for-in loop

│ **[English (en)](<../en/for-in_loop.md> "for-in loop")** │  **[français (fr)](</for-in_loop/fr> "for-in loop/fr")** │  **[日本語 (ja)](</for-in_loop/ja> "for-in loop/ja")** │  **русский (ru)** │    
****

"for-in" цикл появился в delphi, начиная с версии 2005. Данная конструкция доступна сейчас с версии fpc 2.4.2. 

## Contents

  * 1 Delphi и FPC реализация
    * 1.1 Цикл применимый к строкам
    * 1.2 Цикл применимый к массиву
    * 1.3 Цикл применимый к множеству
    * 1.4 Применение к классам контейнерам
  * 2 Расширения FPC
    * 2.1 Перечисления и диапазоны
    * 2.2 Объявление перечислителей
    * 2.3 Использование имён идентификаторов отличных от MoveNext и Current
  * 3 Предлагаемые расширения
    * 3.1 Выбор перечисления для использования
    * 3.2 Получение Позиции перечисления, если возможно
  * 4 Ссылки



## Delphi и FPC реализация

Она имеет следующий синтаксис: 

### Цикл применимый к строкам
    
    
    procedure StringLoop(S: String);
    var
      C: Char;
    begin
      for C in S do
        DoSomething(C);
    end;
    

### Цикл применимый к массиву
    
    
    procedure ArrayLoop(A: Array of Byte);
    var
      B: Byte;
    begin
      for B in A do
        DoSomething(B);
    end;
    

### Цикл применимый к множеству
    
    
    type
      TColor = (cRed, cGren, cBlue);
      TColors = set of TColor;
    procedure SetLoop(Colors: TColors);
    var
      Color: TColor;
    begin
      for Color in Colors do
        DoSomething(Color);
    end;
    

### Применение к классам контейнерам

Для обхода элементов класса контейнера необходимо добавить специальный перечислитель. Перечислитель встраиваемый в класс представлен в следующем шаблоне: 
    
    
    TSomeEnumerator = class
    public
      function MoveNext: Boolean;
      property Current: TSomeType;
    end;
    

Для реализации перечислителя необходимы: метод **MoveNext** , который увеличивает позицию элемента перечисления и свойство **Current** , в котором возвращается выбранный вами тип данных. 

Следующим шагом необходимо добавить к классу контейнеру специального метода **GetEnumerator** , который будет возвращать экземпляр перечислителя. 

Пример: 
    
    
    type
      TEnumerableTree = class;
    
      TTreeEnumerator = class
      private
        FTree: TEnumerableTree;
        FCurrent: TNode;
      public
        constructor Create(ATree: TEnumerableTree); 
        function MoveNext: Boolean;
        property Current: TNode read FCurrent;
      end;
    
      TEnumerableTree = class
      public
        function GetEnumerator: TTreeEnumerator;
      end;
    
    constructor TTreeEnumerator.Create(ATree: TEnumerableTree);
    begin
      inherited Create;
      FTree := ATree;
      FCurrent := nil;
    end;
    
    function TTreeEnumerator.MoveNext: Boolean;
    begin
      // получение следующего узла дерева
      if FCurrent = nil then
        FCurrent := FTree.GetFirstNode
      else
        FCurrent := FTree.GetNextNode(FCurrent);
      Result := FCurrent <> FTree.GetLastNode;
    end;
    
    function TEnumerableTree.GetEnumerator: TTreeEnumerator;
    begin
      Result := TTreeEnumerator.Create(Self);
    end;
    

После этого вы можете использовать следующий код: 
    
    
    procedure TreeLoop(ATree: TEnumerableTree);
    var
      ANode: TNode;
    begin
      for ANode in ATree do
        DoSomething(ANode);
    end;
    

Поддержка перечислителя встроена в основные классы: TList, TStrings, TCollection, TComponent, ... 

Также поддержку перечислителя можно добавить в класс контейнер, если вы реализует в нём следующий интерфейс: 
    
    
      IEnumerable = interface(IInterface)
        function GetEnumerator: IEnumerator;
      end;
    

Интерфейс **IEnumerator** определён следующим образом: 
    
    
      IEnumerator = interface(IInterface)
        function GetCurrent: TObject;
        function MoveNext: Boolean;
        procedure Reset;
        property Current: TObject read GetCurrent;
      end;
    

## Расширения FPC

Приведенные ниже примеры не поддерживаются Delphi, и предназначены только для FPC. 

### Перечисления и диапазоны

В Delphi невозможно использовать в цикле типы перечислений и диапазонов: 
    
    
    type
      TColor = (clRed, clBlue, clBlack);
      TRange = 'a'..'z';
    var
      Color: TColor;
      ch: Char;
    begin
      for Color in TColor do
        DoSomething(Color);
      for ch in TRange do
        DoSomethingOther(ch);
    end.
    

### Объявление перечислителей

В Delphi также невозможно добавить перечислитель не изменяя класс и добавить перечислитель к следующим типам не-классы/объекты/записи/интерфейсы. В FPC добавление перечислитиля для любого типа производится новым оператором **operator Enumerator**. Смотрите следующий пример: 
    
    
    type
      TMyRecord = record F1: Integer; F2: array of TMyType; end;
      TMyArrayEnumerator = class
        constructor Create(const A: TMyRecord);
        function Current: TMyType;
        function MoveNext: Boolean;
      end;
    
      // Это новый встроенный оператор
      operator Enumerator(const A: TMyRecord): TMyArrayEnumerator;
      begin
        Result := TMyArrayEnumerator.Create(A);
      end;
    
    var
      A: MyRecord;
      V: TMyType
    begin
      for V in A do
        DoSomething(V);
    end.
    

Примером использования перечислений может быть следующая реализация эффективного перебора строк в кодировке UTF-8: 
    
    
    type
      TUTF8StringEnumerator = class
      private
        FByteIndex: Integer;
        FCharIndex: Integer;
      public
        constructor Create(const A: UTF8String);
        function Current: UTF8Char;
        function MoveNext: Boolean;
      end;
    
      operator Enumerator(A: UTF8String): TUTF8StringEnumerator;
      begin
        Result := TUTF8String.Create(A);
      end;
    
    var
      s: UTF8String;
      ch: UTF8Char;
      i: Integer;
    begin
      // Здесь требуется выполнить O(N^2) операций
      for i := 1 to Length(s) do
        DoSomething(ch[i]);
      // А здесь, только O(N) операций
      for ch in s do
        DoSomething(ch);
    end.
    

### Использование имён идентификаторов отличных от MoveNext и Current

В Delphi вы должны использовать только функцию с именем 'MoveNext' и свойство с именем 'Current' для перечислений. В FPC можно использовать любые разрешенные имена. Для того, чтобы указать компилятору какая функция перечисляет элементы необходимо указать следующий модификатор 'enumerator MoveNext', а для свойства текущего элемента 'enumerator Current'. Смотрите следующий пример: 
    
    
    type
      { TMyListEnumerator }
    
      TMyListEnumerator = object
      private
        FCurrent: Integer;
      public
        constructor Create;
        destructor Destroy;
        function StepNext: Boolean; enumerator MoveNext;
        property Value: Integer read FCurrent; enumerator Current;
      end;
    
      TMyList = class
      end;
    
    { TMyListEnumerator }
    
    constructor TMyListEnumerator.Create;
    begin
      FCurrent := 0;
    end;
    
    destructor TMyListEnumerator.Destroy;
    begin
      inherited;
    end;
    
    function TMyListEnumerator.StepNext: Boolean;
    begin
      inc(FCurrent);
      Result := FCurrent <= 3;
    end;
    
    operator enumerator (AList: TMyList): TMyListEnumerator;
    begin
      Result.Create;
    end;
    
    var
      List: TMyList;
      i: integer;
    begin
      List := TMyList.Create;
      for i in List do
        WriteLn(i);
      List.Free;
    end.
    

## Предлагаемые расширения

### Выбор перечисления для использования

Возможно использовать различные перечисления. Например, вы можете обойти дерево, используя различный порядок. Хорошо известны алгоритмы: предварительный порядок, обратный порядок, симметричный порядок и обход в ширину. Поэтому, полезно иметь возможность выбора перечисления. Например, используя следующий синтаксис: 
    
    
    type
      TTreeEnumeratorType = (tePreOrder, tePostOrder, teInOrder, teBreadthFirst)
    procedure TraverseTree(Tree: TTree);
    var
      Node: TNode;
    begin
      // Вариант 1. Для экземляров класса мы можем вызвать метод Tree.GetEnumerator(teInOrder). 
      // Для классов мы можем вызвать метод класса
      for Node in Tree using GetEnumerator(teInOrder) do
        Dosomething(Node);
    
      // Вариант 2. Или мы можем вызвать глобальную функцию
      for Node in Tree using GetEnumerator(Tree, teInOrder) do
        Dosomething(Node);
    
      // Вариант 3. В предыдущем варианте 'in Tree' бесполезно, поэтому следующий код - это упрощенная форма:
      for Node using GetEnumerator(Tree, teInOrder) do
        Dosomething(Node);
    
      // Вариант 4. Мы можем попытаться избежать нового контекстного ключевого слова 'using', вызывая метод:
      for Node in Tree.GetSomeEnumerator(teInOrder) do
        Dosomething(Node);
      // но это принесет двусмысленность для компилятора, т.к. Tree.GetSomeEnumerator(teInOrder) может быть транслирован в
      // Tree.GetSomeEnumerator(teInOrder).GetEnumerator
      // Эта двусмысленность может быть разрешена проверкой, где класс применяет интерфейс IEnumerator
    end;
    
    // для базового типа мы вызовем только подходящую функцию
    procedure TraverseString(S: String);
    var
      C: Char;
    begin
      for C in S using GetReverseStringEnumerator(S) do
        DoSomething(C);
    end;
    

### Получение Позиции перечисления, если возможно

Наконец, возможно извлечь любую информацию из интератора, кроме текущего элемента. Иногда любые данные, такие как текущий индекс, могут быть полезны: 
    
    
    type
      TUTF8StringEnumerator = class
      private
        FByteIndex: Integer;
        FCharIndex: Integer;
      public
        constructor Create(const A: UTF8String);
        function Current: UTF8Char;
        function CurrentIndex: Integer;
        function MoveNext: Boolean;
      end;
    
      operator GetEnumerator(A: UF8String): TUF8StringEnumerator;
      begin
        Result := TUF8String.Create(A);
      end;
    
    var
      s: UF8String;
      ch: UF8Char;
      i: Integer;
    begin
    
      // Неэффективный метод, как написано выше
      for i := 1 to Length(s) do
        Writeln(i, ': ', ch[i]);
    
      // Нормальный, но уродливый
      i := 1;
      for ch in s do begin
        Writeln(i, ': ', ch);
        Inc(i);
      end;
    
      // Предлагаемое расширение
      for ch in s index i do
        Writeln(i, ': ', ch);
    end.
    

Запомните, что индекс может вернуть произвольный тип, не обязательно целый тип. Например,в случае обхода дерева индекс может вернуть массив узлов на пути от корня к текущему узлу. 

## Ссылки

  * <http://17slon.com/blogs/gabr/2007/03/fun-with-enumerators.html>

---

_Source: [https://wiki.freepascal.org/for-in_loop/ru](https://web.archive.org/web/20250418095616/https://wiki.freepascal.org/for-in_loop/ru)_
