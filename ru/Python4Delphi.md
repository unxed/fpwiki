# Python4Delphi

│ **[English (en)](<../en/Python4Delphi.md> "Python4Delphi")** │  **русский (ru)** │ 

## Contents

  * 1 Обзор
  * 2 Порт для Лазарус
  * 3 Смотрите ещё
  * 4 FreeBSD



# Обзор

Домашняя страница: <https://code.google.com/p/python4delphi/>

С этой страницы: 

> Питон для Дельфи (Python for Delphi — P4D) это набор свободных компонентов, которые заворачивают dll Питона в Дельфи и Лазарус (FPC). Они позволяют легко выполнять скрипты Питона, создавать новые модули Питона и новые типы. Можно создавать расширения Питон в виде dll и многое другое. P4D предлагает разные уровни функциональности: 
> 
>   * Низкоуровневый доступ к API Питона
>   * Высокоуровневое двунаправленное взаимодействие с Питоном
>   * Доступ к объектам Питона, используя тип Дельфи Variant (VarPyth.pas)
>   * Заворачивание объектов Дельфи для использования в скриптах Питона с помощью RTTI (WrapDelphi.pas)
> 

> 
> P4D упрощает использование Питона в качестве скриптового языка для приложений Дельфи. 

[changelog](<https://code.google.com/p/python4delphi/source/list>) датирует последние доработки ноябрём 2012г. 

[Коммит от 21.1.2018 объявляет о совместимости как с Лазарус так и (надеюсь) с Дельфи Линукс.](<https://github.com/pyscripter/python4delphi/commit/88c06f11e70d471fc5dfd3b1e7e446c09eb5ab9a>)

# Порт для Лазарус

Порт для Лазарус: [ Использование Питона в Лазарус под Windows/Linux](<../en/Using_Python_in_Lazarus_on_Windows/Linux.md> "Using Python in Lazarus on Windows/Linux")

# Смотрите ещё

  * Старая вики (примерно 2006г.) с множеством примеров на [py4d.pbworks.com](<http://py4d.pbworks.com/w/page/9174525/FrontPage>)
  * [Yahoo group](<https://groups.yahoo.com/neo/groups/pythonfordelphi/info>)
  * Заметки с [Python for Delphi talk](<http://www.atug.com/andypatterns/pythonDelphiTalk.htm>)



# FreeBSD

Я смог заставить работать это на FreeBSD не используя компоненты времени разработки в Лазарус: 
    
    
    program simplefpcdemo;
    uses PythonEngine, dynlibs;
    
    var eng : TPythonEngine;
    begin
      eng := TPythonEngine.Create(Nil);
      eng.LoadDll;
      if eng.IsHandleValid then
        begin
          WriteLn(' evens: ', eng.EvalStringAsStr('[x*2 for x in range(10)]'));
          eng.ExecString('print "powers:", [x**2 for x in range(10)]');
        end
      else writeln('invalid library handle!', dynlibs.GetLoadErrorStr);
    end.
    

The output: 
    
    
        evens: [0, 2, 4, 6, 8, 10, 12, 14, 16, 18]
       powers: [0, 1, 4, 9, 16, 25, 36, 49, 64, 81]
    

Моя персональная рабочая копия (включая обёртку Питона для моей последней библиотеки) — <https://github.com/tangentstorm/py4d> .

---

_Source: [https://wiki.freepascal.org/Python4Delphi/ru](https://web.archive.org/web/20250124211611/https://wiki.freepascal.org/Python4Delphi/ru)_
