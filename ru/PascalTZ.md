# PascalTZ

│ **[English (en)](<../en/PascalTZ.md>)** │  **русский (ru)** │

## Contents

  * 1 О компоненте
  * 2 Пример
  * 3 Авторы
  * 4 Лицензия
  * 5 Загрузка
  * 6 Журнал изменений
  * 7 Отчеты об ошибках
  * 8 См. также



### О компоненте

PascalTZ расшифровывается как «Pascal Time Zone»(часовые пояса Pascal). Это позволяет вам конвертировать время между местным временем в различных [часовых поясах](<https://ru.wikipedia.org/wiki/%D0%A7%D0%B0%D1%81%D0%BE%D0%B2%D0%BE%D0%B9_%D0%BF%D0%BE%D1%8F%D1%81>) и [GMT](<https://ru.wikipedia.org/wiki/%D0%A1%D1%80%D0%B5%D0%B4%D0%BD%D0%B5%D0%B5_%D0%B2%D1%80%D0%B5%D0%BC%D1%8F_%D0%BF%D0%BE_%D0%93%D1%80%D0%B8%D0%BD%D0%B2%D0%B8%D1%87%D1%83>)/[UTC](<https://ru.wikipedia.org/wiki/%D0%92%D1%81%D0%B5%D0%BC%D0%B8%D1%80%D0%BD%D0%BE%D0%B5_%D0%BA%D0%BE%D0%BE%D1%80%D0%B4%D0%B8%D0%BD%D0%B8%D1%80%D0%BE%D0%B2%D0%B0%D0%BD%D0%BD%D0%BE%D0%B5_%D0%B2%D1%80%D0%B5%D0%BC%D1%8F>), с учетом исторических изменений в правилах часовых поясов. PascalTZ использует [Time Zone Database](<https://www.iana.org/time-zones>) (часто называемую `tz` или `zoneinfo`), чтобы определить, как правильно настроивать время для различных часовых поясов. Корректность преобразований часовых поясов в будущем зависит от использования современной базы данных. Осторожно, правила часового пояса могут быть изменены правительствами по всему миру, иногда с очень коротким уведомлением. 

Компонент PascalTZ можно использовать в проектах с чистым FPC или установить как пакет разработки и среды выполнения в Lazarus IDE. Он также поставляется с тестовой средой, набором тестовых векторов преобразования часовых поясов и контрольных примеров для внутренних функций. 

Более подробную информацию можно найти на [GitHub:PascalTZ](<https://github.com/dezlov/PascalTZ>). 

### Пример
    
    
    uses
      SysUtils, DateUtils, uPascalTZ;
    
    var
      PascalTZ: TPascalTZ;
      DateTime: TDateTime;
    
    begin
      PascalTZ := TPascalTZ.Create;
    
      // Загружаем базу данных часовых поясов из каталога "tzdata" 
      // Скачиваем с: https://www.iana.org/time-zones
      PascalTZ.DatabasePath := 'tzdata';
    
      // Текущее местное и UTC-время
      DateTime := Now;
      WriteLn('Местное время: ', DateTimeToStr(DateTime));
      DateTime := LocalTimeToUniversal(DateTime);
      WriteLn('UTC-время: ', DateTimeToStr(DateTime));
    
      // Конвертируем текущее время в Парижское
      DateTime := PascalTZ.GMTToLocalTime(DateTime, 'Europe/Paris');
      WriteLn('Время в Париже: ', DateTimeToStr(DateTime));
    
      // Конвертируем Парижское время в Чикагское
      DateTime := PascalTZ.Convert(DateTime, 'Europe/Paris', 'America/Chicago');
      WriteLn('Время в Чикаго: ', DateTimeToStr(DateTime));
    
      // Проверяем, существует ли часовой пояс
      WriteLn('Africa/Lagos существует? ', PascalTZ.TimeZoneExists('Africa/Lagos'));
      WriteLn('Australia/Darwin существует? ', PascalTZ.TimeZoneExists('Australia/Darwin'));
    
      PascalTZ.Free;
    end.
    

### Авторы

Эта библиотека была первоначально опубликована [José Mejuto](</index.php?title=User:Joshy&action=edit&redlink=1> "User:Joshy \(page does not exist\)") в 2009 году и поддерживается [Денисом Козловым](</index.php?title=User:Dezlov&action=edit&redlink=1> "User:Dezlov \(page does not exist\)") с 2015 года. 

### Лицензия

[Модифицированная](<http://svn.freepascal.org/svn/lazarus/trunk/COPYING.modifiedLGPL>) [LGPL](<http://svn.freepascal.org/svn/lazarus/trunk/COPYING.LGPL>) (такая же, как FPC RTL и Lazarus LCL). 

### Загрузка

[GitHub:PascalTZ releases](<https://github.com/dezlov/PascalTZ/releases>)

### Журнал изменений

  * Version 1.0 (2009-11-10) [[1]](<https://github.com/dezlov/PascalTZ/blob/v2.0/CHANGELOG.md>)
  * Version 2.0 (2016-07-19) [[2]](<https://github.com/dezlov/PascalTZ/blob/v2.0/CHANGELOG.md>)



### Отчеты об ошибках

Отчеты об ошибках и предложения могут быть зарегистрированы в [GitHub:PascalTZ issues](<https://github.com/dezlov/PascalTZ/issues>). 

### См. также

Начиная с 2.6.2, FPC имеет функции `LocalTimeToUniversal` и `UniversalTimeToLocal` в модуле `dateutils` для преобразования между местным временем и временем UTC. Эти функции могут быть полезными, и являться альтернативой для PascalTZ, если вас не интересуют историческая/будущая дата/время (т.е. функции используют текущее летнее время и т.д. для преобразования в/из времени UTC). Эти функции позволяют задавать ручные смещения по отношению к UTC, но тогда вам будет необходимо отслеживать эти смещения вручную - а так за вас это сделает PascalTZ.

---

_Source: [https://wiki.freepascal.org/PascalTZ/ru](https://web.archive.org/web/20220101000000/https://wiki.freepascal.org/PascalTZ/ru)_
