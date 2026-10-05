# Standard Pascal

│ **[English (en)](<../en/Standard_Pascal.md>)** │  **русский (ru)** │

_Стандартный Pascal_ \- это спецификация языка Паскаль, определяющая минимальный уровень возможностей компилятора данного языка. Ниже приведены стандартные ключевые слова, которые должны поддерживаться всеми компиляторами языка: 

    [begin](<Begin.md> "Begin/ru") · [end](<End.md> "End/ru") · [for](<For.md> "For/ru") · [goto](<Goto.md> "Goto/ru") · [if](<If.md> "If/ru") · [label](<Label.md> "Label/ru") · [repeat](<Repeat.md> "Repeat/ru") · [then](<Then.md> "Then/ru") · [until](<Until.md> "Until/ru") · [while](<While.md> "While/ru") · [do](<Do.md> "Do/ru") · [type](<Type.md> "Type/ru") · [var](<Var.md> "Var/ru")

Следующие выражения являются также частью языка: 

    := ([присвоить](<Becomes.md> "Becomes/ru")) · = ([равно](<Equal.md> "Equal/ru")) · > ([больше](<Greater_than.md> "Greater than/ru")) · < ([меньше](<Less_than.md> "Less than/ru")) <> ([не равно](<Not_equal.md> "Not equal/ru"))

Существуют дополнительные ключевые слова, которые формально не являются частью стандартного языка Паскаль, но используются либо в [FPC](<FPC.md> "FPC/ru"), предоставляя дополнительные функции, либо для обеспечения совместимости с [Borland Pascal](</index.php?title=Borland_Pascal/ru&action=edit&redlink=1> "Borland Pascal/ru \(page does not exist\)") и с более ранними версиями компиляторов. Эти ключевые слова включают в себя: 

    [implementation](</index.php?title=Implementation/ru&action=edit&redlink=1> "Implementation/ru \(page does not exist\)") · [finally](</index.php?title=Finally/ru&action=edit&redlink=1> "Finally/ru \(page does not exist\)") · [try](<Try.md> "Try/ru") · [unit](<Unit.md> "Unit/ru").

## Типы

Существуют следующие стандартные типы:

[integer](<Integer.md> "Integer/ru") · [smallint](<Smallint.md> "Smallint/ru") · [longint](<Longint.md> "Longint/ru") · [real](<Real.md> "Real/ru") · [boolean](<Boolean.md> "Boolean/ru") · [string](<String.md> "String/ru") · [char](<Char.md> "Char/ru") · [byte](<Byte.md> "Byte/ru")

## Режимы, поддеживаемые Free Pascal

Free Pascal поддерживает ISO 7185 Standard Pascal с переключателем режима **-Miso** и ISO/IEC 10206 Extended Pascal с **-Mextendedpascal**. Поддержка ISO 7185 появилась с версии 3.0.0. 

## См. также

  * [Standard Pascal](<http://www.standardpascal.org>) \- Справочная информация о стандарте ANSI ISO 7185
  * [ISO 7185:1990](<http://www.iso.org/iso/home/store/catalogue_tc/catalogue_detail.htm?csnumber=13802>) \- Официальная нормативная версия стандарта Pascal
  * [ISO/IEC 10206:1991](<http://www.iso.org/iso/home/store/catalogue_tc/catalogue_detail.htm?csnumber=18237>) \- Стандарт Extended Pascal

---

_Source: [https://wiki.freepascal.org/Standard_Pascal/ru](https://web.archive.org/web/20250321095900/https://wiki.freepascal.org/Standard_Pascal/ru)_
