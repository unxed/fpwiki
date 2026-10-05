# DesignGuidelines

│ **[Deutsch (de)](</DesignGuidelines/de> "DesignGuidelines/de")** │  **[English (en)](<../en/DesignGuidelines.md> "DesignGuidelines")** │  **[español (es)](</DesignGuidelines/es> "DesignGuidelines/es")** │  **[français (fr)](</DesignGuidelines/fr> "DesignGuidelines/fr")** │  **[日本語 (ja)](</DesignGuidelines/ja> "DesignGuidelines/ja")** │  **[한국어 (ko)](</DesignGuidelines/ko> "DesignGuidelines/ko")** │  **[português (pt)](</DesignGuidelines/pt> "DesignGuidelines/pt")** │  **русский (ru)** │    
****

## Указания по кодированию для Lazarus

Стиль кодирования
    

  * Поскольку один стиль легче для чтения, Lazarus следует стилю, принятому для Delphi: [Object Pascal Style Guide](<http://edn.embarcadero.com/article/10280>). Конечно, почти любой найдёт там моменты, которые посчитает менее удобными для чтения, чем в других стилях. Это нормально, просто попробуйте следовать этому стилю хотя бы на 90%. В качестве примера смотрите LCL.
  * Старайтесь избегать взаимных (циклических) ссылок модулей (**uses** в секциях **implementation**). Причины:


  1. FPC имеет проблемы с ними.
  2. Это позволяет разделить модули и пакеты, когда они становятся слишком большими.


  * Минимизируйте количество вызовов LCL интефейсами при выполнении действия, запрошенного LCL. Интерфейсы только уведомляют LCL, не заставляя что-либо делать. Решение принимает LCL.
  * Соглашения об именовании: см. [Nomenclature](<../en/Nomenclature.md> "Nomenclature")
  * Весь код должен работать при всех включенных проверках (range, I/O, overflow, stack). Помимо того, что это помогает отладке, некоторые пользователи устанавливают эти проверки в fpc.cfg, поэтому они применяются ко всему Lazarus, включая пакеты и примеры.
  * Имена в комментариях: Комментарии должны помогать читателю. Они - не для прославления писателя. Вы можете добавить имя, если:


  1. Вы занимаетесь поддержкой, т.е. являетесь тем, кто принимает отчёты об ошибках.
  2. Имя удобно для поисковых запросов, чтобы найти помощь по этой теме.



Новые файлы
    

  * Каждый файл должен начинаться с заголовка, содержащего лицензию и несколько строк, описывающих содержание.
  * Исходные файлы на языке Pascal (.pas, .pp, .inc, .lfm, .lrs) должны иметь имена в нижнем регистре. Можно использовать CamelCase в именах модулей.



Включаемые файлы
    

  * Должны начинаться директивой {%MainUnit }



Пакеты
    

  * Должны иметь запись .lpl в packager/globallinks/
  * Должны иметь автора, описание и лицензию



Пункты главного меню
    

  * Должны иметь клавишу в keymapping.pp



* * *

_Надёжная версия может быть найдена в[svn](<http://svn.freepascal.org/svn/lazarus/trunk/docs/DesignGuidelines.txt>). Предложения по улучшению могут быть добавлены на странице обсуждения_. 

### GUI

См. [GUI design guidelines](<../en/GUI_design_guidelines.md> "GUI design guidelines")

## См. также

  * [Описание стиля кодирования Delphi: Object Pascal Style Guide (Charles Calvert)](<http://edn.embarcadero.com/article/10280>)
  * Стандарт кодирования для Free Pascal (FPC) (используется для компилятора и другого кода FPC): [Coding style/ru](<Coding_style.md> "Coding style/ru")

---

_Source: [https://wiki.freepascal.org/DesignGuidelines/ru](https://web.archive.org/web/20250323144739/https://wiki.freepascal.org/DesignGuidelines/ru)_
