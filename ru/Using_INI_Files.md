# Using INI Files

│ **[العربية (ar)](</Using_INI_Files/ar> "Using INI Files/ar")** │  **[Deutsch (de)](</Using_INI_Files/de> "Using INI Files/de")** │  **[English (en)](<../en/Using_INI_Files.md> "Using INI Files")** │  **[español (es)](</Using_INI_Files/es> "Using INI Files/es")** │  **[suomi (fi)](</Using_INI_Files/fi> "Using INI Files/fi")** │  **[français (fr)](</Using_INI_Files/fr> "Using INI Files/fr")** │  **[polski (pl)](</Using_INI_Files/pl> "Using INI Files/pl")** │  **русский (ru)** │  **[中文（中国大陆） (zh_CN)](</Using_INI_Files/zh_CN> "Using INI Files/zh CN")** │    
****

## Contents

  * 1 Файлы INI
    * 1.1 Обзор
    * 1.2 Краткий состав
      * 1.2.1 Для чего это нужно
    * 1.3 Пример
    * 1.4 Свойства и методы
    * 1.5 Заключение...



## Файлы INI

### Обзор

INI-файлы могут быть использованы легко для сохранения пользовательских параметров. С модулем **INIFiles** и классом **TINIFile** вы можете легко работать с существующими INI файлами. Этот модуль находится в FCL. 

### Краткий состав

INI-файлы работают с секциями(Sections), ключами(Keys) и значениями(Values).  
Пример: 
    
    
    [Section]
    Key=Value
    

В одной секции может быть несколько ключей. В файле может быть несколько секций. 

#### Для чего это нужно

INI-файлы позволяют очень просто читать и записывать данные. 

### Пример

Создайте простое консольное приложение. 
    
    
    Progam INIex;
    
    {$mode objfpc}
    
    Uses
     SysUtils,INIFiles;{INIFiles = модуль который содержит класс для работы с INI-файлами}
    
    Var
     IniF:TINIFile;// Класс для работы с INI-файлами
    

  
При использовании TINIfile вы должны работать с существующим INI файлом. Так что откройте текстовый редактор и создайте его. Можете использовать это для примера: 
    
    
    [S1]
    Key1=Hello World
    
    [S2]
    Key2=Worldly Hello
    

Теперь вернемся к нашему коду. 
    
    
    begin
      Writeln('Creating class');
      IF(FileExists('someini.ini'))then
      begin
        Inif := TINIFile.Create('someini.ini');
        Writeln(INiF.ReadString('s1','Key1',''));
      End else Writeln('Файл не найден...');
      Readln;
    end.
    

### Свойства и методы

В классе TINIFile есть много свойств, процедур и функций которые можно использовать. 

**CaseSensitive** \- Это свойство позволяет установить используются ли названия секций и ключей с учетом регистра. По умолчанию нет. 

**ReadString** \- Имеет три постоянных параметра. Первый - секция в которой производится поиск. Второй - ключ. Третий - значение строки по умолчанию, которое будет возвращено если секция и/или ключ не найдены. 

**WriteString** Также имеет три параметра. Первый - секция. Второй - ключ. Третий - значение ключа. Если ключ и секция уже существуют, то значения ключа будет перезаписано на новое. 

**ReadSections** \- Позволит вам взять секцию из INI-файли и поместить ей в класс TStrings(или TStringList) 

**DeleteKey** \- Удаляет существующий ключ в секции. 

**EraseSection** Удаляет секцию и все её данные. 

Имеется еще много процедур и функций, но это базовые. 

### Заключение...

Тут: [[1]](<http://lazarus-ccr.sourceforge.net/docs/fcl/inifiles/index.html>) вы можете прочитать все об INI-файлах. Пожалуйста, если вы можете добавить больше информации об INI-файлах в Pascal **Дополните эту статью**

---

_Source: [https://wiki.freepascal.org/Using_INI_Files/ru](https://web.archive.org/web/20241201000000/https://wiki.freepascal.org/Using_INI_Files/ru)_
