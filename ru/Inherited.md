# Inherited

│ **[Deutsch (de)](</Inherited/de> "Inherited/de")** │  **[English (en)](<../en/Inherited.md> "Inherited")** │  **[suomi (fi)](</Inherited/fi> "Inherited/fi")** │  **[français (fr)](</Inherited/fr> "Inherited/fr")** │  **русский (ru)** │    
****

В переопределяемом виртуальном [методе](<../en/Method.md> "Method") часто необходимо вызывать реализацию виртуального метода родительского [`class`](<Class.md> "Class/ru"). Это можно сделать с помощью [зарезервированного слова](<Reserved_word.md> "Reserved word/ru") `inherited`. Аналогично, [ ключевое слово](<Keyword.md> "Keyword/ru") `inherited` может использоваться для вызова любого метода родительского `class`. 

Вот простейший пример: 
    
    
    Type  
      TMyClass = Class(TComponent)  
        Constructor Create(AOwner : TComponent); override;  
      end; 
    
    Constructor TMyClass.Create(AOwner : TComponent);  
    begin  
      Inherited;  
      // Что-то делаем еще  
    end;
    

## Случаи конструкторов и деструкторов

[`Constructor`](<Constructor.md> "Constructor/ru"), Пример 1 : 
    
    
      ...
      TTest.Create;
      begin
        Inherited; // Ставится всегда в начале конструктора и запускает конструктор (только код) родительского класса
        ...
      end;
    

`Constructor`, Пример 2 : 
    
    
      ...
      TTest.Create(...);
      begin
        Inherited Create(...); // Ставится всегда в начале конструктора и запускает конструктор (только код) родительского класса
        ...
      end;
      ...
    

[`Destructor`](<../en/Destructor.md> "Destructor"), Пример 3 : 
    
    
      TTest.Destroy;
      begin
        ...
        Inherited;  // Ставится всегда в конце деструктора и запускает деструктор (только код) родительского класса
      end;
      ...
    

## Переопределение виртуальных методов
    
    
    type  
      TMyClass = class(TStrings)  
        function GetObject(Index: Integer): TObject; override;  
      end; 
    
    function TMyClass.GetObject(Index: Integer): TObject;
    begin
      // Получаем результат из метода родительского класса 
      Result := inherited GetObject(Index);  
      // Делаем что-нибудь дальше
    end;

---

_Source: [https://wiki.freepascal.org/Inherited/ru](https://web.archive.org/web/20250517164531/https://wiki.freepascal.org/Inherited/ru)_
