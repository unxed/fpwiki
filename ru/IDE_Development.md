# IDE Development

│ **[English (en)](<../en/IDE_Development.md> "IDE Development")** │  **[한국어 (ko)](</IDE_Development/ko> "IDE Development/ko")** │  **русский (ru)** │    
****

Эта страница содержит заметки для разработчиков ядра lazarus для продолжения разработки. 

Примечание: Этот пункт ссылается на 

  * [Lazarus 0.9.26 release notes](<../en/Lazarus_0.9.md> "Lazarus 0.9.26 release notes")
  * [Lazarus 0.9.28 release notes](<../en/Lazarus_0.9.md> "Lazarus 0.9.28 release notes")
  * [Lazarus 0.9.30 release notes](<../en/Lazarus_0.9.md> "Lazarus 0.9.30 release notes")
  * [Lazarus 1.0 release notes](<../en/Lazarus_1.md> "Lazarus 1.0 release notes")
  * [Lazarus 1.2.0 release_notes](<../en/Lazarus_1.2.md> "Lazarus 1.2.0 release notes")
  * [Lazarus 1.4.0 release_notes](<../en/Lazarus_1.4.md> "Lazarus 1.4.0 release notes")
  * [Lazarus 1.6.0 release_notes](<../en/Lazarus_1.6.md> "Lazarus 1.6.0 release notes")
  * [Lazarus 1.8.0 release_notes](<../en/Lazarus_1.8.md> "Lazarus 1.8.0 release notes")
  * [Lazarus 1.10.0 release_notes](<../en/Lazarus_1.10.md> "Lazarus 1.10.0 release notes")



  


## Contents

  * 1 Multi form properties / using DataModules from other forms in the designer
    * 1.1 Краткое описание
    * 1.2 Работает
    * 1.3 ToDo
  * 2 Переводы, i18n, lrt файлы, po файлы
    * 2.1 Краткое описание
    * 2.2 Работает
    * 2.3 ToDo
  * 3 Фреймы
    * 3.1 Краткое описание
    * 3.2 Работает
    * 3.3 ToDo
  * 4 VFI - Visual Form Inheritance
    * 4.1 Краткое описание
    * 4.2 Работает
    * 4.3 ToDo
  * 5 Форматирование кода jedi
    * 5.1 Краткое описание
    * 5.2 Работает
    * 5.3 ToDo
  * 6 Отладка
    * 6.1 Краткое описание
    * 6.2 Работает
    * 6.3 ToDo
  * 7 Release Notes



# Multi form properties / using DataModules from other forms in the designer

## Краткое описание

Эта функция позволяет использовать компоненты других дизайнеров форм. Типичным примером является использование TDataSource в DataModule для DataSource свойства управления БД. Дизайнерские формы могут только ссылаться друг на друга, если они имеют CreateForm заявление в файле LPR (Параметры проекта / Формы). 

## Работает

  * Manual referencing via source code works.
  * TPersistentPropertyEditor.GetValues - List all possible values. Check for class compatibility and if the target form is listed in the CreateForm statements of the lpr file and if the target unit does not belong to a package that will conflict if used.
  * TPersistentPropertyEditor.GetValue - Show component path
  * TPersistentPropertyEditor.SetValue - search the component via the given path
  * When component is deleted, the property must be set to nil - This is not the job of the IDE, but should be achieved by the normal TComponent FreeNotification feature. Maybe eventually a check could be added, if this is implemented properly and force a nil on error.
  * When a component is renamed the property does not need to be updated, because the form is open and use the pointer not the name. But the designer must be flagged 'modified', because the lfm has changed. See below.
  * When reference form is opened, then target forms are opened too.
  * When target form is closed, then only the designer is closed. The component is kept hidden.
  * When all referring forms are closed/hidden, the hidden component will be automatically freed.
  * Circle dependencies are allowed.
  * When referenced component is renamed, the using units must be set modified.
  * When referenced component is deleted, the using units must be set modified.
  * Opening a unit now checks if designer is already created
  * Reopen/Revert a form - all connected forms are now closed



## ToDo

  * Allow to connect to forms, that are used in the uses section. Technically there is _no_ connection between the uses section and the form streaming, because form streaming uses global variables. But the IDE must somehow find the referenced form. Without the uses section the IDE must in worst case search and read every reachable lfm file on disk. So the uses section is a good compromise between speed and flexibility.



# Переводы, i18n, lrt файлы, po файлы

## Краткое описание

Это функция автоматического создания и обновления .po файлов для пакетов и проектов. 

## Работает

  * Диалог настроек для проекта/пакета для включения i18n
  * Сбор TTranslateStrings из RTTI пока designer form writing и записывание их в lrt файл.
  * Копирование новых строк из rst и lrt файлов в один .po файл для каждого проекта
  * Загрузка .po файлов
  * Удаление неиспользуемых строк из .po файла для пакетов/проектов
  * Обновление переведенных .po файлов, похоже на updatepofiles tool.



## ToDo

  * Копирование новых строк из rst и lrt файлов в один .po файл для каждого пакета (реализован, необходим диалог настройки для изменения имени файла po или необходимо соглашение для использования того же имени для пакетов)
  * Сбор всех .po файлов проекта и всех испольуемых пакетов в директорию. Эта директория может быть использована для проекта при загрузки строк runtime. И установщики могут копировать содержимое.
  * стандартизация имен файлов и путей, для того чтобы загружать переводы runtime, возможно без написания кода.



# Фреймы

## Краткое описание

Фреймы - это специальные дочерние формы. Цель состоит в том, чтобы изменять их, как формы в дизайнере IDE, и использовать их в качестве компонентов в дизайнере форм. 

## Работает

compile lazarus clean with passing **-dEnableTFrame** (not necessary starting from svn release 16909) 

  * Manually creating via source
  * gtk1, gtk2, qt, win32/win64
  * Create a new frame
  * Open a frame in the designer
  * Save a frame
  * Close a frame
  * Revert a frame
  * Design a frame (adding, selecting, moving controls, ...)
  * Opening a nested frame
  * Opening a nested frame with ancestors
  * Support for the inline keyword for lfm and lrs streams.
  * Add a frame to a form, add unit to uses section, package to project dependencies 
    * Auto opening frame if not yet loaded
  * Close a frame that is currently used by a nested frame
  * Close a form with an embedded frame
  * Save a form with an embedded frame
  * Delete a frame embedded in a form
  * Inherited nested Components (Components with the frame as Owner and not the Form) 
    * Show inherited nested components properties in OI
    * Forbid deleting nested inherited components
    * Forbid renaming nested inherited components
    * Show inherited nested components events in OI as ClassName.MethodName
    * When dblclick on inherited nested components events in OI create an event with the following code: Frame1.FrameResize(Sender);
    * When Ctrl+Click on inherited nested components events in OI jump to inherited code.
    * show nested components in OI component tree
    * Write nested component to stream
  * Forbid putting a component onto a nested frame, because TWriter does not support that.
  * Revert a form with an embedded frame



## ToDo

  * Propagate changes of ancestor to nested controls
  * Учебник
  * Документация
  * Обновление



# VFI - Visual Form Inheritance

## Краткое описание

The .lfm streams of ancestors are read/applied before the current lfm. The .lfm of the descendant is the difference between ancestor and current state. The IDE allows to open and edit inherited forms. VFI does not only apply to TForm, but TDataModule, TFrame and any registered base designer class. For the full features compile with -dEnableTFrame. 

Формы могут наследоваться от других форм. lfm потоки предков читаются/применяется для нынешнего lfm. lfm потомка содержит разницу между предком и текущего состояния. IDE позволяет открывать и редактировать унаследованные формы. VFI не только для TForm, но TDataModule, TFrame и любой зарегистрированный базовый класс дизайнера. Для полных возможностей компилировать с -dEnableTFrame. 

## Работает

  * find ancestor unit, lfm and class
  * automatically open ancestor form hidden in background
  * open descendant and read ancestor lfm first
  * show hidden ancestor form
  * hide ancestor form
  * automatically close hidden ancestor form if not needed anymore
  * create new ancestor form via 'File / New ... / Inherited Items'
  * forbid deleting inherited components
  * forbid renaming inherited components



## ToDo

  * Унаследованные события (частично работает)
  * Применять изменения предка к потомкам, когда оба открыты
  * Учебник
  * Документация



# Форматирование кода jedi

## Краткое описание

## Работает

  * Pressing ctrl+D will format your source file
  * Settings are stored and read by IDE from JCFSettings.cfg file
  * port the jcf option dialogs



## ToDo

  * использовать jcf для отступа



# Отладка

## Краткое описание

## Работает

  * show breakpoints etc in asm window
  * implement/design interface on debugger class to show/handle threads
  * attach to process



## ToDo

  * implement/design interface on debuggerclass to handle os exceptions
  * implement/design interface on debugger class to show messages
  * implement/design interface on debugger class to show loaded modules
  * implement debugging of forked processes
  * implement launching process on xterm (mainly for console apps)
  * find include files in fpc sources



# Release Notes

  * [Lazarus 1.10.0 release_notes](<../en/Lazarus_1.10.md> "Lazarus 1.10.0 release notes")
  * [Lazarus 1.8.0 release_notes](<../en/Lazarus_1.8.md> "Lazarus 1.8.0 release notes")
  * [Lazarus 1.6.0 release_notes](<../en/Lazarus_1.6.md> "Lazarus 1.6.0 release notes")
  * [Lazarus 1.4.0 release notes](<../en/Lazarus_1.4.md> "Lazarus 1.4.0 release notes")
  * [Lazarus 1.2.0 release notes](<../en/Lazarus_1.2.md> "Lazarus 1.2.0 release notes")
  * [Lazarus 1.0 release notes](<../en/Lazarus_1.md> "Lazarus 1.0 release notes")
  * [Lazarus 0.9.30 release notes](<../en/Lazarus_0.9.md> "Lazarus 0.9.30 release notes")
  * [Lazarus 0.9.28.2 release notes](<../en/Lazarus_0.9.28.md> "Lazarus 0.9.28.2 release notes")
  * [Lazarus 0.9.28 release notes](<../en/Lazarus_0.9.md> "Lazarus 0.9.28 release notes")
  * [Lazarus 0.9.26.2 release notes](<../en/Lazarus_0.9.26.md> "Lazarus 0.9.26.2 release notes")
  * [Lazarus 0.9.26 release notes](<../en/Lazarus_0.9.md> "Lazarus 0.9.26 release notes")
  * [Lazarus 0.9.24 release notes](<../en/Lazarus_0.9.md> "Lazarus 0.9.24 release notes")

---

_Source: [https://wiki.freepascal.org/IDE_Development/ru](https://web.archive.org/web/20240716031523/https://wiki.freepascal.org/IDE_Development/ru)_
