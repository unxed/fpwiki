# Writing portable code regarding the processor architecture

│ **[English (en)](<../en/Writing_portable_code_regarding_the_processor_architecture.md> "Writing portable code regarding the processor architecture")** │  **[Bahasa Indonesia (id)](</Writing_portable_code_regarding_the_processor_architecture/id> "Writing portable code regarding the processor architecture/id")** │  **русский (ru)** │    
****

Существует ряд проблем, связанных с написанием кода, являющегося независимым от процессорной архитектуры. Одна из них - это порядок следования байтов, другая – разрядность процессора ([32](<../en/32_bit.md> "32 bit") или [64-битные](<../en/64_bit.md> "64 bit") ЦП). 

## Contents

  * 1 Порядок следования байтов (ПСБ)
    * 1.1 Изменение ПСБ
  * 2 Согласование
  * 3 32 Bit и 64 Bit
  * 4 Согласование вызовов
    * 4.1 Мэйнфреймы
    * 4.2 x86
    * 4.3 ARM
    * 4.4 PPC
    * 4.5 68K
      * 4.5.1 Ссылки по данной теме



## Порядок следования байтов (ПСБ)

Порядок следования байтов определяет, как ЦП будет хранить значения, имеющие размер больше одного [байта](<../en/Byte.md> "Byte"). (т.е. 16/32/64-битные [целые числа](<../en/Integer.md> "Integer")). 

Free Pascal поддерживает процессоры с двумя типами следования байтов: 

  1. Порядок следования от младшего к старшему байту: longint(4) кодируется как 04 00 00 00 (_[little endian](<http://en.wikipedia.org/wiki/Little_endian>)_)
  2. Порядок следования от старшего к младшему байту: longint(4) кодируется как 00 00 00 04 (_[big endian](<http://en.wikipedia.org/wiki/Big_endian>)_)



Примечание: 

Процессоры со средним(смешаным) порядком байтов, известным также как смешанный порядок байтов, сегодня встречаются редко. Наиболее известным представителем процессоров со смешанным порядком байтов является, устаревший сегодня, [PDP-11](<http://ru.wikipedia.org/wiki/PDP-11>) производства компании DEC. 

Каждое семейство процессоров имеет свой порядок байтов, но, как правило, они либо от младшего к старшему, либо от старшего к младшему, в зависимости от архитектуры (ARM, PPC). 

Самыми известными представителями процессоров с порядком следования байтов от младшего к старшему, являются процессоры семейства x86, используемые в ПК, и его собратья x86-64. Типичными же представителями процессоров с порядком следования байтов от старшего к младшему, являются процессоры семейства PPC (см. примечание выше) и процессоры [m68k](<http://ru.wikipedia.org/wiki/68k>), использовавшиеся в миникомпьютерах [HP3000](<http://en.wikipedia.org/wiki/HP3000>), а также в мэйнфреймах, таких, как [IBM 370](<http://ru.wikipedia.org/wiki/IBM_370>) (серия Z). 

Поскольку спецификация TCP/IP предполагает, что все заголовки в структуре протокола должны храниться с порядком байтов от старшего к младшему, то данный порядок байтов иногда упоминается как «сетевой порядок следования байтов». 

Наиболее важен порядок байтов в следующих случаях: 

  1. при обмене данными компьютерами с различными архитектурами ЦП
  2. при доступе к большому количеству данных, например, представленных в виде массива целых чисел или байтов.



  
Пример последнего: 
    
    
    type
      Q = record
        case Boolean of
          True: (i: Integer);
          False: (p: array[1..4] of Byte);
      end;
     
    var 
      x:^Q;
     
    begin
      // Указываем компилятору порядок следования байтов(ПСБ):
      {$IFDEF ENDIAN_LITTLE}
      Writeln('Программа скомпилирована для компьютеров с ПСБ от младшего к старшему (например Intel x86, ARMEL)');
      {$ENDIF}
      {$IFDEF ENDIAN_BIG}
      Writeln('Программа скомпилирована для компьютеров с ПСБ от старшего к младшему (например PowerPC, ARMEB)');
      {$ENDIF}  
      New(x);
      x^.i := 5;
      if x^.p[1] = 5 then
        WriteLn(x^.p[1],' На вашей машине ПСБ от младшего к старшему')
      else
      if x^.p[4] = 5 then
        WriteLn(x^.p[1],' На вашей машине ПСБ от старшего к младшему')
      else
        WriteLn(x^.p[1],' ',x^.p[2],' ',x^.p[3],' ',x^.p[4],' Порядок следования байтов не определён');  
      WriteLn;
     
      { Сделаем паузу для просмотра результата }
     
      Write('Нажмите Enter для выхода из программы ');
      ReadLn;
    end.
    

Данный пример на компьютерах с ПСБ от младшего к старшему выведет число 5 (т.к. число 5 хранится в памяти в виде 05 00 00 00). В то-же время, компьютеры с ПСБ от старшего к младшему (например, PowerMac), выведут 0 (поскольку число 5 хранится в памяти в виде 00 00 00 05). 

Для определения порядка следования байтов служат директивы компилятора ENDIAN_BIG и ENDIAN_LITTLE (или FPC_LITTLE_ENDIAN и FPC_BIG_ENDIAN, начиная с версии 1.9). 

### Изменение ПСБ

Модуль system имеет ряд процедур для преобразования данных между порядком следования байтов от старшего к младшему и ЦП, на котором выполняется программа ([BEtoN](<http://www.freepascal.org/docs-html/rtl/system/beton.html>), [NtoBE](<http://www.freepascal.org/docs-html/rtl/system/ntobe.html>)). Соответствующие процедуры реализованы и для машин с ПСБ от младшего к старшему: [LEtoN](<http://www.freepascal.org/docs-html/rtl/system/leton.html>), [NtoLE](<http://www.freepascal.org/docs-html/rtl/system/ntole.html>). Имеются также процедуры для смешанного порядка байтов ([SwapEndian](<http://www.freepascal.org/docs-html/rtl/system/swapendian.html>)). 

Некоторые сетевые библиотеки, например, Synapse, имеют собственные реализации конвертации ПСБ между сетью и компьютером. 

## Согласование

Некоторые процессоры могут неправильно выстроить данные в памяти. Это может привести к изменению размера типа данных запись (record), поэтому всегда используйте функцию sizeof для проверки размера записи. 

## 32 Bit и 64 Bit

Для достижения максимальной совместимости со старым кодом, FPC не изменяет размер стандартных типов данных, например, `[integer](<../en/Integer.md> "Integer")`, `[longint](<../en/longint.md> "longint")` или `[word](<../en/Word.md> "Word")` при переходе с 32-разрядного на 64-разрядный ЦП. Выражения вроде `longint([pointer](</index.php?title=pointer&action=edit&redlink=1> "pointer \(page does not exist\)")(p))` приведут к ошибке на 64-разрядных процессорах. Для написания переносимого кода в FPC введены типы `[PtrInt](</index.php?title=Ptrint&action=edit&redlink=1> "Ptrint \(page does not exist\)")` и `[PtrUInt](</index.php?title=Ptruint&action=edit&redlink=1> "Ptruint \(page does not exist\)")` в модуле system, представляющие собой знаковые и беззнаковые целочисленные типы данных такого-же размера, как и указатель (Pointer). 

Имейте в виду, что смена размера типа "pointer" также влияет на размер записей, если Вы определите запись не фиксированного размера, а через процедуры [new](</index.php?title=new&action=edit&redlink=1> "new \(page does not exist\)") или [getmem](</index.php?title=getmem&action=edit&redlink=1> "getmem \(page does not exist\)") (<x>,[sizeof](</index.php?title=sizeof&action=edit&redlink=1> "sizeof \(page does not exist\)")(<x>)). 

Это относится к открытым платформам Unix. В коммерческом мире существуют некоторые исключения, например Tru64 или ILP64. 

## Согласование вызовов

В общем случае, старайтесь не полагаться на собственные знания. Лучше воспользуйтесь следующей таблицей: 

Ключевое слово | ПСБ | Стек очищает | Выравнивание | Регистры сохранены?   
---|---|---|---|---  
нет | слева направо | функция | по умолчанию | нет   
[Register](<../en/register.md> "register") | слева направо | функция | по умолчанию | нет   
[CDecl](</index.php?title=cdecl&action=edit&redlink=1> "cdecl \(page does not exist\)") | справа налево | вызывающий | GCC выравнивание | только GCC регистры   
[Interrupt](</index.php?title=interrupt&action=edit&redlink=1> "interrupt \(page does not exist\)") | справа налево | функция | по умолчанию | да   
[Pascal](<../en/pascal.md> "pascal") | слева направо | функция | по умолчанию | нет   
[SafeCall](</index.php?title=safecall&action=edit&redlink=1> "safecall \(page does not exist\)") | справа налево | функция | по умолчанию | только GCC регистры   
[StdCall](</index.php?title=stdcall&action=edit&redlink=1> "stdcall \(page does not exist\)") | справа налево | функция | GCC выравнивание | только GCC регистры   
[OldFPCCall](<../en/oldfpccall.md> "oldfpccall") | справа налево | вызывающий | по умолчанию | нет   
  
Более подробная информация: [$CALLING](<http://www.freepascal.org/docs-html/prog/progsu87.html>). 

Ограничения по размеру параметров для различных архитектур ЦП: 

Архитектура ЦП | Размер параметров | Размер локальных переменных   
---|---|---  
i386 | 64 КБ | нет лимита   
AMD64/x86-64 | 64 КБ | нет лимита   
Motorola 68000 | 32 КБ | 32 КБ   
Motorola 68020 | 32 КБ | нет лимита   
PPC | нет лимита | нет лимита   
ARM | нет лимита | нет лимита   
SPARC | нет лимита | нет лимита   
  
### Мэйнфреймы

Для архитектур IBM 370 и Z - серии смотрите подробности [здесь](<../en/ZSeries.md> "ZSeries"). 

### x86

В процессорах x86 обычно все параметры передаются через стек. Однако, если функция вызывается в режиме совместимости с Delphi, то первые три параметра или параметры, имеющие один адрес, передаются через регистры EAX, EDX и ECX. Исключением из данного правила может являться процесс разработки ПО для ОС [Darwin](<../en/Target_Darwin.md> "Target Darwin") и [Mac OS X](<../en/Portal_Mac.md> "Portal:Mac"). 

### ARM

### PPC

Архитектура PowerPC имеет большое количество регистров, что даёт возможность передать все аргументы функции через них за один вызов. Также, есть возможность использовать стек. 

Процессоры PowerPC используют стандарты AIX или SysV при вызовах функций. Смотрите подробнее [здесь](<../en/PPC_Calling_conventions.md> "PPC Calling conventions"). 

### 68K

Процессоры 68K поддерживают согласование вызовов CDecl и Pascal. Более подробную информацию смотрите тут: [68K vs. PowerPC](<http://physinfo-mac0.ulb.ac.be/divers_html/powerpc_programming_info/intro_to_ppc/ppc4_runtime2.html>). 

После вызова процедуры или функции на этих ЦП, стек выглядит примерно так: 

| возвращаемое значение   
---|---  
| первый параметр   
| ...   
| последний параметр   
| статические ссылки (опционально)   
| адрес возврата   
A6 | предыдущие A6   
| локальные переменные   
SP | сохраненные регистры   
  
#### Ссылки по данной теме

  * [THINK Pascal](<../en/THINK_Pascal.md> "THINK Pascal") Руководство пользователя, Symantec Corporation, Cupertino, 1988
  * Physinfo, Université libre de Bruxelles: [PowerPC Calling Conventions, 68K vs. PowerPC](<http://physinfo-mac0.ulb.ac.be/divers_html/powerpc_programming_info/intro_to_ppc/ppc4_runtime2.html>)



  


navigation bar: data types  [simple data types](<../en/simple_type.md> "simple type") |  [`boolean`](<Boolean.md> "Boolean/ru") [`byte`](<Byte.md> "Byte/ru") [`cardinal`](<Cardinal.md> "Cardinal/ru") [`char`](<Char.md> "Char/ru") [`currency`](<Currency.md> "Currency/ru") [`double`](</index.php?title=Double/ru&action=edit&redlink=1> "Double/ru \(page does not exist\)") [`dword`](</index.php?title=DWord/ru&action=edit&redlink=1> "DWord/ru \(page does not exist\)") [`extended`](<Extended.md> "Extended/ru") [`int8`](</index.php?title=Int8/ru&action=edit&redlink=1> "Int8/ru \(page does not exist\)") [`int16`](</index.php?title=Int16/ru&action=edit&redlink=1> "Int16/ru \(page does not exist\)") [`int32`](</index.php?title=Int32/ru&action=edit&redlink=1> "Int32/ru \(page does not exist\)") [`int64`](<Int64.md> "Int64/ru") [`integer`](<Integer.md> "Integer/ru") [`longint`](<Longint.md> "Longint/ru") [`real`](<Real.md> "Real/ru") [`shortint`](<Shortint.md> "Shortint/ru") [`single`](</index.php?title=Single/ru&action=edit&redlink=1> "Single/ru \(page does not exist\)") [`smallint`](<Smallint.md> "Smallint/ru") [`pointer`](<Pointer.md> "Pointer/ru") [`qword`](</index.php?title=QWord/ru&action=edit&redlink=1> "QWord/ru \(page does not exist\)") [`word`](<Word.md> "Word/ru")  
---|---  
complex data types |  [`array`](<Array.md> "Array/ru") [`class`](<Class.md> "Class/ru") [`object`](</index.php?title=Object/ru&action=edit&redlink=1> "Object/ru \(page does not exist\)") [`record`](<Record.md> "Record/ru") [`set`](<Set.md> "Set/ru") [`string`](<String.md> "String/ru") [`shortstring`](</index.php?title=Shortstring/ru&action=edit&redlink=1> "Shortstring/ru \(page does not exist\)")  
  
  
  
****

---

_Source: [https://wiki.freepascal.org/Writing_portable_code_regarding_the_processor_architecture/ru](https://web.archive.org/web/20250325082633/https://wiki.freepascal.org/Writing_portable_code_regarding_the_processor_architecture/ru)_
