# FPDoc Editor

│ **[Deutsch (de)](</FPDoc_Editor/de> "FPDoc Editor/de")** │  **[English (en)](<../en/FPDoc_Editor.md> "FPDoc Editor")** │  **[français (fr)](</FPDoc_Editor/fr> "FPDoc Editor/fr")** │  **[日本語 (ja)](</FPDoc_Editor/ja> "FPDoc Editor/ja")** │  **[polski (pl)](</FPDoc_Editor/pl> "FPDoc Editor/pl")** │  **[português (pt)](</FPDoc_Editor/pt> "FPDoc Editor/pt")** │  **русский (ru)** │    
****

## Contents

  * 1 Вступление
  * 2 Использование редактора FPDoc
  * 3 FPDoc для поставляемых исходников LCL и самого Lazarus
  * 4 Редактирование FPDoc для поставляемых исходников FPC, RTL and FCL
  * 5 Планы на будущее
    * 5.1 Уже сделано



## Вступление

FPDoc это встроенное в Lazarus средство для формирования документации к модулям Free Pascal. Подробное описание FPDoc есть на английском языке: [Free Pascal documentation tool manual](<http://www.freepascal.org/docs-html/fpdoc/fpdoc.html>). 

[![FPDocEditorDescription.png](https://wiki.freepascal.org/images/6/6c/FPDocEditorDescription.png)](</File:FPDocEditorDescription.png>)

Lazarus включает средство просмотра FPDoc-справки во всплывающих подсказках к именам в исходном коде и два редактора FPDoc, которые могут использоваться для создания и поддержки документации к исходникам. Более простой редактор интегрирован в Lazarus IDE так и называется, **FPDoc Editor** (Редактор FPDoc), и именно он описан на этой странице. Для его работы достаточнов настройках Lazarus в разделе FPDoc добавить пути к папкам с документацией. 

Существует также более мощный редактор, который называется [LazDE](<../en/Lazarus_Documentation_Editor.md> "Lazarus Documentation Editor"). **LazDE** \- старший брат **FPDoc Editor** , и полностью называется Lazarus Documentation Editor. Это самостоятельное приложение, оно не интегрировано в Lazarus IDE. Поставляется в исходниках, проект расположен в папке ($LazDir)/doceditor/lazde.lpi. Скомпилируйте проект lazde однажды (используя Lazarus), и затем запускайте LazDE независимо. Можно добавить его в меню Tools (Сервис) как "внешнее средство". 

## Использование редактора FPDoc

Чтоб воспользоваться FPDoc Editor достаточно: 

1\. Открыть пункт FPDoc Editor (Редактор FPDoc) в меню View (Вид). 

2\. В редакторе исходного кода Lazarus поставить курсор на нужный элемент кода. Вы заметите, что заголовок окна редактора FPDoc изменился, и показывает имя этого элемента вместе с именем файла документации. В редакторе FPDoc Вы можете перейти на подходящую вкладку чтобы редактировать соответствующий тег документации. И естественно, Вы можете использовать редактор FPDoc и для просмотра имеющейся документации, не начиная менять её. 

3\. Нажать кнопку Create Help (Создать элемент справки). Если Вы ещё не настроили пути для файлов FPDoc, IDE спросит, где хранить генерируемые файлы. Обычно у каждого проекта создают свою папку 'docs'. 

4\. Вписать краткое описание 

5\. Нажать кнопку Save слева или просто перейти к следующему элементу исходников (редактор автоматически сохраняет внесённые изменения по элементу, когда курсор с него уходит). 

## FPDoc для поставляемых исходников LCL и самого Lazarus

Лежат в папке ($LazDir)\docs\xml, добавьте её в пути редактора FPDoc в Tools / Options / Environment / FPDoc Editor (Сервис / Параметры / Окружение / Редактор FPDoc) 

## Редактирование FPDoc для поставляемых исходников FPC, RTL and FCL

Файлы FPDoc для исходников FPC могут быть взяты из svn: 
    
    
    cd /home/username/yourchoice/
    svn co http://svn.freepascal.org/svn/fpcdocs/trunk fpcdocs
    

Добавьте путь _/home/username/yourchoice/fpcdocs_ в Tools / Options / Environment / FPDoc Editor (Сервис / Параметры / Окружение / Редактор FPDoc) 

Для проверки результата можно посмотреть, например, на FPDoc для _TComponent.Name_. 

## Планы на будущее

Список todo сейчас содержит следующие пункты (расположены не в порядке приоритетности): 

  * Write a help editor for topics.
  * Create nicer HTML output for the hint windows.
  * Support Operators



### Уже сделано

  * Extend the link editor to show packages and identifiers
  * Add documentation tags "example" to FPDoc Editor
  * Add documentation tags "topic" to FPDoc Editor
  * Make FPDoc Editor create new elements in documentation
  * Make FPDoc Editor create new documentation files
  * Adding a small toolbar makes it possible to use the available markup tags
  * Make FPDoc Editor work on keydown in source code editor
  * Add FPDoc Editor to IDE settings (showing and position in IDE)
  * Make it work for fpc sources (rtl files already exist)
  * Add settings to environment menu
  * Make it work on project files also
  * Propose to expand documentation tags with: "todo" and "notes" (no need for that, as there are alternatives)
  * Reduce overhead even further
  * All source elements are interpreted by FPDoc Editor using codetools
  * Find inherited entries. For example TControl.Align of TButton.Align
  * Optimization: inherited Entries are parsed on idle
  * Optimization: xml files are cached, and only parsed once or if they changed on disk
  * Add a HTML viewer. This is available by installing the turbopoweriprodsgn package
  * Checks for invalid xml tags and auto repairs them

---

_Source: [https://wiki.freepascal.org/FPDoc_Editor/ru](https://web.archive.org/web/20250601000000/https://wiki.freepascal.org/FPDoc_Editor/ru)_
