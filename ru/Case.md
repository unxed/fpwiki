# Case

│ **[English (en)](<../en/Case.md>)** │  **русский (ru)** │

Зарезервированным словом **Case** начинается блок _инструкции выбора_. Инструкция **case** сравнивает значение перечислимого, порядкового или строкового типа для каждого селектора. Селектор может быть [константой](</index.php?title=Const/ru&action=edit&redlink=1> "Const/ru \(page does not exist\)"), диапазоном или списком значений, разделенных [запятыми](<Comma.md> "Comma/ru"). Поле селектора отделяется от выполняемых инструкций [двоеточием](<Colon.md> "Colon/ru"). 

Инструкция выбора также включает в себя [зарезервированные слова](<Reserved_word.md> "Reserved word/ru") [Of](</index.php?title=Of/ru&action=edit&redlink=1> "Of/ru \(page does not exist\)") и [End](<End.md> "End/ru"). [Else](<Else.md> "Else/ru") может использоваться в случае, когда не будет ни одного совпадения (в этом случае ветка **Else** будет выполняться по умолчанию). 

  


## Contents

  * 1 Примеры
  * 2 Выбор строк
  * 3 Выбор перечислимых значений
  * 4 Шестнадцатеричные числа в качестве входных значений
  * 5 Вариантная запись
  * 6 См. также



### Примеры
    
    
     case place of
       1: ShowMessage('Золотая медаль');
       2: ShowMessage('Серебряная медаль');
       3: ShowMessage('Бронзовая медаль'); 
       else ShowMessage('Повезет в следующий раз'); 
     end;
    
    
    
     function WhatIsChar( c:char ):string;
     var
       s : string;
     begin
       s := '';
       case c of
         '0' .. '9' : s := 'цифра (0-9)';
         'a' .. 'z' : s := 'строчная буква (a-z)';
         'A' .. 'Z' : s := 'прописная буква (A-Z)';
         '+' , '-'  : s := 'знак (+ или -)';
       end;
       result := s;
     end;
    

## Выбор строк

Язык Free Pascal позволяет использовать оператор "Case of" для строковых переменных. 
    
    
    const
      ISO_3166_1_ALPHA_3_ANDORRA  = 'AND';
      ISO_3166_1_ALPHA_3_AUSTRALIA = 'AUS';
      ISO_3166_1_ALPHA_3_AUSTRIA = 'AUT';
      ISO_3166_1_ALPHA_3_BELGIUM = 'BEL';
      ISO_3166_1_ALPHA_3_BRAZIL = 'BRA';
      ISO_3166_1_ALPHA_3_CANADA = 'CAN';
      ISO_3166_1_ALPHA_3_CHINA = 'CHN';
      ISO_3166_1_ALPHA_3_CZECH_REPUBLIC = 'CZE';
      ISO_3166_1_ALPHA_3_CYPRUS = 'CYP';
      ISO_3166_1_ALPHA_3_GERMANY = 'DEU';
      ISO_3166_1_ALPHA_3_DENMARK = 'DNK';
      ISO_3166_1_ALPHA_3_SPAIN = 'ESP';
      ISO_3166_1_ALPHA_3_ESTONIA = 'EST';
      ISO_3166_1_ALPHA_3_FINLAND = 'FIN';
      ISO_3166_1_ALPHA_3_FRANCE = 'FRA';
      ISO_3166_1_ALPHA_3_GREECE = 'GRC';
      ISO_3166_1_ALPHA_3_INDIA = 'IND';
      ISO_3166_1_ALPHA_3_IRELAND = 'IRL';
      ISO_3166_1_ALPHA_3_ITALY = 'ITA';
      ISO_3166_1_ALPHA_3_JAPAN = 'JPN';
      ISO_3166_1_ALPHA_3_LITHUANIA = 'LTU';
      ISO_3166_1_ALPHA_3_LATVIA = 'LVA';
      ISO_3166_1_ALPHA_3_LUXEMBOURG = 'LUX';
      ISO_3166_1_ALPHA_3_MALTA = 'MLT';
      ISO_3166_1_ALPHA_3_MEXICO = 'MEX';
      ISO_3166_1_ALPHA_3_MONACO = 'MCO';
      ISO_3166_1_ALPHA_3_MONTENEGRO = 'MNE';
      ISO_3166_1_ALPHA_3_NAURU = 'NRU';
      ISO_3166_1_ALPHA_3_NETHERLANDS = 'NLD';
      ISO_3166_1_ALPHA_3_NORWAY = 'NOR';
      ISO_3166_1_ALPHA_3_POLAND = 'POL';
      ISO_3166_1_ALPHA_3_PORTUGAL = 'PRT';
      ISO_3166_1_ALPHA_3_REPUBLIC_OF_KOREA = 'KOR';
      ISO_3166_1_ALPHA_3_RUSSIAN_FEDERATION = 'RUS';
      ISO_3166_1_ALPHA_3_SAN_MARINO = 'SMR';
      ISO_3166_1_ALPHA_3_SLOVAKIA = 'SVK';
      ISO_3166_1_ALPHA_3_SLOVENIA = 'SVN';
      ISO_3166_1_ALPHA_3_SWEDEN = 'SWE' ;
      ISO_3166_1_ALPHA_3_SWITZERLAND = 'CHE';
      ISO_3166_1_ALPHA_3_UNITED_KINGDOM_OF_GREAT_BRITAIN_AND_NORTHERN_IRELAND = 'GBR';
      ISO_3166_1_ALPHA_3_UNITED_STATES_OF_AMERICA = 'USA';
    
    
      ISO_4217_EURO = 'EUR';
      ISO_4217_AUSTRALIAN_DOLLAR = 'AUD';
      ISO_4217_BRAZILIAN_REAL = 'BRL';
      ISO_4217_CANADIAN_DOLLAR = 'CAD';
      ISO_4217_CHINESE_YUAN = 'CNY';
      ISO_4217_CZECH_KORUNA = 'CZK' ;
      ISO_4217_DANISH_KRONE = 'DKK' ;
      ISO_4217_INDIAN_RUPEE = 'INR';
      ISO_4217_POUND_STERLING = 'GBP' ;
      ISO_4217_JAPANESE_YEN  = 'JPY';
      ISO_4217_MEXICAN_PESO = 'MXN';
      ISO_4217_NORWEGIAN_KRONE = 'NOK' ;
      ISO_4217_POLISH_ZLOTY = 'PLN' ;
      ISO_4217_RUSSIAN_RUBLE = 'RUB' ;
      ISO_4217_SOUTH_KOREAN_WON = 'KRW';
      ISO_4217_SWEDISH_KRONA = 'SEK' ;
      ISO_4217_SWISS_FRANC = 'CHF' ;
      ISO_4217_UNITED_STATES_DOLLAR = 'USD' ;
    
    function ISO_4217_currency_name ( ISO_3166_1_alpha_3_code: string): string;
    begin
      Case ISO_3166_1_alpha_3_code  of
        // Еврозона
        ISO_3166_1_ALPHA_3_ANDORRA, ISO_3166_1_ALPHA_3_AUSTRIA,
        ISO_3166_1_ALPHA_3_BELGIUM,ISO_3166_1_ALPHA_3_CYPRUS,
        ISO_3166_1_ALPHA_3_GERMANY, ISO_3166_1_ALPHA_3_SPAIN,
        ISO_3166_1_ALPHA_3_ESTONIA, ISO_3166_1_ALPHA_3_FINLAND,
        ISO_3166_1_ALPHA_3_FRANCE, ISO_3166_1_ALPHA_3_GREECE,
        ISO_3166_1_ALPHA_3_IRELAND, ISO_3166_1_ALPHA_3_ITALY,
        ISO_3166_1_ALPHA_3_LITHUANIA, ISO_3166_1_ALPHA_3_LATVIA,
        ISO_3166_1_ALPHA_3_LUXEMBOURG, ISO_3166_1_ALPHA_3_MONACO,
        ISO_3166_1_ALPHA_3_MALTA, ISO_3166_1_ALPHA_3_MONTENEGRO,
        ISO_3166_1_ALPHA_3_NETHERLANDS, ISO_3166_1_ALPHA_3_PORTUGAL,
        ISO_3166_1_ALPHA_3_SAN_MARINO, ISO_3166_1_ALPHA_3_SLOVAKIA,
        ISO_3166_1_ALPHA_3_SLOVENIA : result := ISO_4217_EURO ;
    
        ISO_3166_1_ALPHA_3_AUSTRALIA, ISO_3166_1_ALPHA_3_NAURU :
              result := ISO_4217_AUSTRALIAN_DOLLAR;
        ISO_3166_1_ALPHA_3_BRAZIL : result := ISO_4217_BRAZILIAN_REAL;
        ISO_3166_1_ALPHA_3_CANADA : result := ISO_4217_CANADIAN_DOLLAR;
        ISO_3166_1_ALPHA_3_CHINA : result := ISO_4217_CHINESE_YUAN;
        ISO_3166_1_ALPHA_3_CZECH_REPUBLIC : result := ISO_4217_CZECH_KORUNA ;
        ISO_3166_1_ALPHA_3_DENMARK : result := ISO_4217_DANISH_KRONE ;
        ISO_3166_1_ALPHA_3_INDIA : result := ISO_4217_INDIAN_RUPEE;
        ISO_3166_1_ALPHA_3_JAPAN : result := ISO_4217_JAPANESE_YEN;
        ISO_3166_1_ALPHA_3_MEXICO : result := ISO_4217_MEXICAN_PESO;
        ISO_3166_1_ALPHA_3_NORWAY : result := ISO_4217_NORWEGIAN_KRONE ;
        ISO_3166_1_ALPHA_3_POLAND : result := ISO_4217_POLISH_ZLOTY ;
        ISO_3166_1_ALPHA_3_REPUBLIC_OF_KOREA : result := ISO_4217_SOUTH_KOREAN_WON ;
        ISO_3166_1_ALPHA_3_RUSSIAN_FEDERATION : result := ISO_4217_RUSSIAN_RUBLE ;
        ISO_3166_1_ALPHA_3_SWEDEN : result := ISO_4217_SWEDISH_KRONA ;
        ISO_3166_1_ALPHA_3_SWITZERLAND : result := ISO_4217_SWISS_FRANC ;
        ISO_3166_1_ALPHA_3_UNITED_KINGDOM_OF_GREAT_BRITAIN_AND_NORTHERN_IRELAND :
              result := ISO_4217_POUND_STERLING ;
        ISO_3166_1_ALPHA_3_UNITED_STATES_OF_AMERICA :
              result := ISO_4217_UNITED_STATES_DOLLAR ;
      end;
    end;
    

## Выбор перечислимых значений

Если вы хотите использовать перечислимые типы, вот очень понятный пример: 
    
    
     type
        TMedal = (mGold, mSilver, mBronze);  // в случае если вы хотите, чтобы Золотая начиналась с 1, поместите это значение (Gold=1, ...но
     const
        MedalNames: array [TMedal] of string[6] = ('Золотая', 'Серебряная', 'Бронзовая'); // в [TMedal] вы должны изменить его на [Gold..Bronze] ;)  предложения по адресу edgarrod71@gmail.com
     {[ ... ]}
    var
      place: integer;  /// place начинается с 0;
      M: TMedal;
     {[ ... ]}
      M := TMedal(place);
      case M of
          mGold:   ShowMessage(Medalnames[M] + ' медаль');
          mSilver: ShowMessage(Medalnames[M] + ' медаль');
          mBronze: ShowMessage(Medalnames[M] + ' медаль');
      else
          ShowMessage('Повезет в следующий раз! :)'); 
      end;
    

Другой пример без использования массива MedalNames... 
    
    
     type
        TMedal = (Gold, Silver, Bronze);  // без "m"
     {[ ... ]}
    var
      place: integer;  /// place начинается с 0;
      M: TMedal;
      S: string;
     {[ ... ]}
      M := TMedal(place);
      case M of
          Gold:   writeStr(S, M);
          Silver: writeStr(S, M);
          Bronze: writeStr(S, M);
      else
          S := 'Повезет в следующий раз! ;)'; 
      end;
      ShowMessage(S);
    

  


## Шестнадцатеричные числа в качестве входных значений

Если используются [шестнадцатеричные](<Hexadecimal.md> "Hexadecimal/ru") числа, то они рассматриваются как беззнаковые: 
    
    
     case place of
       $1: ShowMessage('Золотая медаль');
       $2: ShowMessage('Серебряная медаль');
       $3: ShowMessage('Бронзовая медаль'); 
       else ShowMessage('Повезет в следующий раз'); 
     end;
    

  
Если шестнадцатеричная переменная представлена в другом виде, например как [Shortint](<Shortint.md> "Shortint/ru"), то необходимо преобразовать значение: 
    
    
     case place of
       Shortint($C3): ShowMessage('Золотая медаль');  //C3= -61;
       Shortint($34): ShowMessage('Серебряная медаль');
       Shortint($58): ShowMessage('Бронзовая медаль'); 
       else ShowMessage('Повезет в следующий раз'); 
     end;
    

или можно воспользоваться другим решением: 
    
    
    var
      myplace : ShortInt;
      place   : Byte absolute myplace;
    begin
       myplace:=-61;
       case place of
         $C3: ShowMessage('Золотая медаль');
         $34: ShowMessage('Серебряная медаль');
         $58: ShowMessage('Бронзовая медаль');
       else ShowMessage('Повезет в следующий раз');
       end;
    

## Вариантная запись

**Case** также может использоваться в вариантной [записи](<Record.md> "Record/ru"). Вариантная запись по-другому называется _объединение_ (например, тип данных union в языке C). 
    
    
      type
          
       ScaleKelvin = 223 .. 323;
       ScaleCelsius = -50 .. 50;
        
       TemperatureScale = ( celsius, kelvin ) ;
       Temperature = record
        case scale :   TemperatureScale of
         celsius : (celsius_value : ScaleCelsius);
         kelvin : (kelvin_value : ScaleKelvin);
       end;
    

## См. также

  * [The Case statement](<http://www.freepascal.org/docs-html/ref/refsu56.html#x162-18400013.2.2>)
  * [Record types](<http://www.freepascal.org/docs-html/ref/refsu19.html#x43-500003.3.2>)
  * [Tao Yue: Learn Pascal!](<http://lazarus-ccr.sourceforge.net/pascal/pas3cb.html>) Case
  * [Video: Free Pascal Tutorial 8 - Case Statements](<http://www.youtube.com/watch?v=pMr2xtUu3x0>)
  * [ShowMessage](<Dialog_Examples.md> "Dialog Examples/ru")
  * [Type](<Type.md> "Type/ru")

---

_Source: [https://wiki.freepascal.org/Case/ru](https://web.archive.org/web/20250316170652/https://wiki.freepascal.org/Case/ru)_
