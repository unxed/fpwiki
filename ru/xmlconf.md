# xmlconf

│ **[English (en)](<../en/xmlconf.md>)** │  **русский (ru)** │

Модуль XMLConf предоставляет компонент TXMLConfig. Он использует объект TXMLDocument для чтения и записи XML-файла, но происходит непосредственно от TComponent. 

## Contents

  * 1 Отказ от ответственности
  * 2 Свойства и процедуры
    * 2.1 property Filename
    * 2.2 property StartEmpty
    * 2.3 property RootName
    * 2.4 procedure Flush
    * 2.5 procedure Clear
    * 2.6 constructor Create
    * 2.7 destructor Destroy
  * 3 Работа с ключами и путями
    * 3.1 procedure OpenKey
    * 3.2 procedure CloseKey
    * 3.3 procedure ResetKey
  * 4 Получение, установка и удаление значений
    * 4.1 function GetValue: WideString / procedure SetValue(const APath: WideString; const AValue: WideString)
    * 4.2 function GetValue: Integer / procedure SetValue(const APath: WideString; AValue: Integer)
    * 4.3 function GetValue: Boolean / procedure SetValue(const APath: WideString; AValue: Boolean)
    * 4.4 procedure SetDeleteValue(const APath: WideString; const AValue, DefValue: WideString) / procedure SetDeleteValue(const APath: WideString; AValue, DefValue: Integer) / procedure SetDeleteValue(const APath: WideString; AValue, DefValue: Boolean)
    * 4.5 procedure DeletePath(const APath: WideString)
    * 4.6 procedure DeleteValue(const APath: WideString)
    * 4.7 property Modified: Boolean read FModified
  * 5 Пример
  * 6 См.также



##### Отказ от ответственности

Эта документация не является официальной, она основана на обычном процессе 1) изучения исходного кода; 2) экспериментирования; 3) размышлений: «Интересно, это ли имел в виду автор?» 

## Свойства и процедуры

##### property Filename
    
    
    property Filename: String (published)
    

Имя XML-файла. Установка другого имени файла: 

  * Очищает текущий xml-файл.
  * Освобождает объект xmldocument
  * Если _Filename_ существует и _StartEmpty_ имеет значение false: 
    * Новый xmldocument создается путем чтения файла, который должен иметь тот же root элемент, что и _RootName_ , в противном случае возникает исключение.
    * В противном случае создается новый объект xmldocument с корневым элементом _RootName_



_Я не проверял это экспериментально, но похоже, что пустой xmldocument создается (в памяти) при создании компонента, и это можно изменить. Однако его нельзя сохранить, пока у него не будет имени файла - нет имени файла по умолчанию - следовательно, все изменения неизбежно будут потеряны, когда имя файла будет установлено_. 

##### property StartEmpty
    
    
    property StartEmpty: Boolean (published)
    

Определяет, читается ли существующий xmlfile из _Filename_. 

  * По умолчанию false - читается существующий файл.



##### property RootName
    
    
    property RootName: DOMString (published)
    

Имя root элемента в файле. Должно соответствовать значению в файле при чтении существующего файла. 

  * По умолчанию - 'CONFIG'. _Проще всего оставить это в покое, если у вас нет веской причины это менять_.



##### procedure Flush
    
    
    procedure Flush (public)
    

Записывает файл xml - если задано имя файла. 

##### procedure Clear
    
    
    procedure Clear (public)
    

Воссоздает xmldocument как пустой документ. 

##### constructor Create
    
    
    constructor Create(AOwner: TComponent) (public)
    

##### destructor Destroy
    
    
    destructor Destroy (public)
    

## Работа с ключами и путями

XMLConfig поддерживает «стек» путей (как массив широких строк). Общий путь к элементу может быть `<rootName>/key1/key2/key3/key4/value="some_value".`

Это будет выглядеть в файле в виде 
    
    
    <CONFIG>
      <key1>
        <key2>
          <key3>
            <key4 value="some_value"/>
          </key3>
        </key2>
      </key1>
    </CONFIG>
    

(пренебрегая любыми другими элементами). Однако стек путей может содержать пути, а не только узлы, поэтому он может состоять как из четырех элементов глубиной `key1;key2; key3;key4`, так и точно так же только двух элементов `key1/key2;key3/key4`, или трех элементов `key1;key2/key3;key4`. 

Кроме того, при чтении или установке значения можно указать полный путь, так что можно полностью игнорировать «стек» пути и указать полный путь (после _RootName_) для каждого значения. У каждого значения есть эффективный путь, а стек путей - это просто удобный способ добраться до конкретного эффективного пути. 

##### procedure OpenKey
    
    
    procedure OpenKey(const aPath: WideString) (public)
    

"Помещает" aPath в стек. Если текущий путь был `<RootName>/key1` и `aPath = key2/key3`, после вызова OpenKey текущий путь будет `<RootName>/key1/key2/key3`. 

##### procedure CloseKey
    
    
    procedure CloseKey (public)
    

"Выталкивает" верхний путь из стека. Обратите внимание, что это последний введенный путь, а НЕ верхний(предыдущий) элемент. Таким образом, после приведенного выше примера OpenKey, за которым следует вызов CloseKey, стек возвращается к `<RootName>/key1`, а не `<RootName>/key1/key2`. 

##### procedure ResetKey
    
    
    procedure ResetKey (public)
    

Полностью очищает стек. Эффективный путь возвращается к `<RootName>`

## Получение, установка и удаление значений

Все значения фактически читаются и записываются как строки, а другие перегруженные типы предоставляют определенные переводы 'AsString'. 

##### function GetValue: WideString / procedure SetValue(const APath: WideString; const AValue: WideString)
    
    
    function  GetValue(const APath: WideString; const ADefault: WideString): WideString (public)
    
    
    
    procedure SetValue(const APath: WideString; const AValue: WideString) (public)
    

Устанавливает или получает строковое значение из _RootName_ / _Текущий_путь_из_стэка_ / _APath_. Путь создается, если он не существует в сеттере, и ADefault возвращается, если путь не существует в геттере. 

##### function GetValue: Integer / procedure SetValue(const APath: WideString; AValue: Integer)
    
    
    function GetValue(const APath: WideString; ADefault: Integer): Integer; (public)
    
    
    
    procedure SetValue(const APath: WideString; AValue: Integer); (public)
    

Целочисленные значения преобразуются в/из строк с помощью функций `IntToStr` и `strToIntDef`. 

##### function GetValue: Boolean / procedure SetValue(const APath: WideString; AValue: Boolean)
    
    
    function  GetValue(const APath: WideString; ADefault: Boolean): Boolean; (public)
    
    
    
    procedure SetValue(const APath: WideString; AValue: Boolean); (public)
    

Логические значения сохраняются как "True" или "False". 

[![Warning-icon.png](https://wiki.freepascal.org/images/b/b2/Warning-icon.png)](</File:Warning-icon.png>)

**Предупреждение:** некоторые другие компоненты FCL/LCL XML (например, производные от TCustomPropertyStorage, такие как TXMLPropStorage) хранят логические значения с использованием целочисленных значений, несмотря на использование XMLConifg в качестве компонента хранения. Это, конечно, проблема только в том случае, если вы используете одни и те же файлы из обоих компонентов или переключаетесь с одного на другой во время разработки, как это сделал я

. 

##### procedure SetDeleteValue(const APath: WideString; const AValue, DefValue: WideString) / procedure SetDeleteValue(const APath: WideString; AValue, DefValue: Integer) / procedure SetDeleteValue(const APath: WideString; AValue, DefValue: Boolean)
    
    
    procedure SetDeleteValue(const APath: WideString; const AValue, DefValue: WideString); (public)
    
    
    
    procedure SetDeleteValue(const APath: WideString; AValue, DefValue: Integer); (public)
    
    
    
    procedure SetDeleteValue(const APath: WideString; AValue, DefValue: Boolean); (public)
    

Если `AValue = DefValue`, элемент удаляется. В противном случае устанавливается значение AValue. 

_Я не использовал это, но предположительно мог бы использовать для хранения, например, значений свойств с четко определенными значениями по умолчанию, только если они отличаются от значений по умолчанию_. 

##### procedure DeletePath(const APath: WideString)
    
    
    procedure DeletePath(const APath: WideString); (public)
    

Удаляет **все** за пределами пути APath 

##### procedure DeleteValue(const APath: WideString)
    
    
    procedure DeleteValue(const APath: WideString); (public)
    

Удаляет **значение** , указанное APath. Если APath не указывает значение, он ничего не делает 

##### property Modified: Boolean read FModified
    
    
    property Modified: Boolean read FModified; (public)
    

Позволяет узнать, был ли изменен xmldocument и, следовательно, необходимо ли его очистить. 

_Однако процедура`Flush` ничего не делает, если документ не изменен, поэтому в любом случае проще просто вызвать процедуру `Flush`_. 

## Пример

Следующая последовательность инструкций: 
    
    
      XmlConfig1.Filename := FILE_NAME ;        // читаем файл, если он уже существует, но ...
      XmlConfig1.Clear ;                        // если файл уже существует, то очищаем данные, потому что хотим начать с нуля
      XmlConfig1.SetValue ('L1/L2/L3', '333') ; // добавляем немного данных
      XmlConfig1.SetValue ('L1/L2/L4', '44') ;
      XmlConfig1.SetValue ('L1/L2/L3', '33') ;  // это перезапишет предыдущее значение
      XmlConfig1.OpenKey  ('L1/L2') ;           // с этого момента все ключи будут относиться к L1/L2
      XmlConfig1.SetValue ('Lm', 'mm') ;        // они будут добавлены в L1/L2
      XmlConfig1.SetValue ('Lm/Ln', 'nn') ;
      XmlConfig1.SetValue ('/Lx/Ly', 'yy') ;    // и из-за начального символа '/' оно будет добавлено в root
      XmlConfig1.CloseKey ;
      XmlConfig1.SetValue ('L6/L7', '77') ;     // L6 будет удален
      XmlConfig1.SetValue ('L9', '99') ;
      XmlConfig1.SetValue ('L9/L99', '9999') ;  // Предыдущее L9 было атрибутом, теперь оно будет узлом
      XmlConfig1.DeletePath ('L6') ;
      XmlConfig1.Flush ;
    

даст такой XML: 
    
    
      <?xml version="1.0" encoding="utf-8"?>
      <CONFIG L9="99">
        <L1>
          <L2 L3="33" L4="44" Lm="mm">
            <Lm Ln="nn"/>
          </L2>
        </L1>
        <Lx Ly="yy"/>
        <L9 L99="9999"/>
      </CONFIG>
    

Обратите внимание, что последний элемент в параметре aPath для SetValue на самом деле является атрибутом: `XmlConfig1.SetValue ('L1/L2/L3', '333')` фактически задает атрибут `L3` для элемента `L1/L2`. 

* * *

## См.также

  * XMLConf/TXMLConf представлен в пакете [fcl-xml](<../en/fcl-xml.md> "fcl-xml").

---

_Source: [https://wiki.freepascal.org/xmlconf/ru](https://web.archive.org/web/20250212224105/https://wiki.freepascal.org/xmlconf/ru)_
