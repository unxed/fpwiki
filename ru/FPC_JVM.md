# FPC JVM

│ **[English (en)](<../en/FPC_JVM.md> "FPC JVM")** │  **русский (ru)** │    
****

## Contents

  * 1 Обзор
  * 2 Сборки
  * 3 Пример
    * 3.1 Компиляция
    * 3.2 Запуск в Windows
    * 3.3 Запуск в Unix системах
    * 3.4 Подробности
  * 4 Дополнительная информация



# Обзор

FPC-backend для [виртуальной машины Java (JVM)](<http://ru.wikipedia.org/wiki/Java_Virtual_Machine>) генерирует байт-код ява, соответствующий JDK спецификации версии 1.5 (и более поздних версий). На настоящий момент поддерживаются не все возможности языка FPC, но наибольшая часть поддерживается (или будет добавлена в будущем). Команда разработчиков сделала всё возможное, чтобы не вносить каких-либо дополнительных изменений в язык с поддержкой Ява платформы. 

Эта реализация FPC JVM backend никак не связана с проэктом [Project Cooper](<http://www.remobjects.com/cooper.aspx>) от RemObjects, и никак не поддерживает язык Oxygene. 

# Сборки

Сборки компилятора (svn r19598, 2011/11/07) указаны ниже. Это кросс-компиляторы, которые работают под указанные системы и компилируют в ява-код. Создаваемый ява-код никаким образом к системам не привязан. 

Инструкция по установке: 

  * распакуйте архив
  * измените fpc.cfg (_bin\fpc.cfg_ (Windows), _etc/fpc.cfg_ (other platforms)) находящийся в архиве так, чтобы он указывал на директорию, в которой находятся распакованные файлы;
  * для компиляции используйте ppcjvm



Тестовые проекты, которые использовались во время разработки, можно найти здесь: <http://svn.freepascal.org/svn/fpc/branches/jvmbackend/tests/test/jvm>

  * Ссылки на готовые сборки: 
    * [Windows](<http://sourceforge.net/projects/freepascal/files/JVM/2.7.1-r19830-snapshot3/fpcjvmwin32-snapshot3.zip/download>) (i386) ([зеркало](<ftp://ftp.freepascal.org/pub/fpc/contrib/jvm/fpcjvmwin32-snapshot3.zip>))
    * [Mac OS X](<http://sourceforge.net/projects/freepascal/files/JVM/2.7.1-r19830-snapshot3/fpcjvmmacosx-snapshot3.tbz/download>) (universal binary, Mac OS X 10.5 or later) ([зеркало](<ftp://ftp.freepascal.org/pub/fpc/contrib/jvm/fpcjvmmacosx-snapshot3.tbz>))
    * [Linux](<http://sourceforge.net/projects/freepascal/files/JVM/2.7.1-r19830-snapshot3/fpcjvmlinux-snapshot3.tbz/download>) (i386) ([зеркало](<ftp://ftp.freepascal.org/pub/fpc/contrib/jvm/fpcjvmlinux-snapshot3.tbz>))



Если интересующая вас система не представлена или вас интересует непосредственно сборка компилятора/rtl, то в отдельном архиве представлены только используемые Ява-компоненты (Jasmin, javapp, BCEL) . Инструкции по сборке приведены ниже. 

  * [FPC JVM utilities](<ftp://ftp.freepascal.org/pub/fpc/contrib/jvm/fpcjvmutilities.zip>) (этот файл вам **НЕ** нужен, если вы уже скачали один из файлов, указанный выше)



# Пример

## Компиляция

Пример можно скачать <http://svn.freepascal.org/svn/fpc/branches/jvmbackend/tests/test/jvm/trange1.pp>
    
    
     ppcjvm -O2 -g trange1
    

## Запуск в Windows

_Note: the path to the units has changed since the previous snapshots!_
    
    
     java -cp C:\full\path\to\fpcjvm\units\jvm-java\rtl;. trange1
    

Замените '_C:\full\path\to\fpcjvm\units\jvm-java\rtl_ на полный путь к директории _units\jvm-java\rtl_ распакованную из архива сборки. 

## Запуск в Unix системах

_Note: the path to the units has changed since the previous snapshots!_
    
    
     java -cp /full/path/to/fpcjvm/units/jvm-java/rtl:. trange1
    

Замените _/full/path/to/fpcjvm/units/jvm-java/rtl_ на полный путь к директории _units/jvm-java/rtl_ распакованную из архива сборки. 

## Подробности

Смотри [usage information](<../en/FPC_JVM/Usage.md> "FPC JVM/Usage") для дополнительной информации. 

# Дополнительная информация

  * [Особенности использования](<../en/FPC_JVM/Usage.md> "FPC JVM/Usage")
  * [Поддерживаемые конструкции языка](<FPC_JVM/Language.md> "FPC JVM/Language/ru")
  * [Помощь по отладке Ява-паскаля](<../en/FPC_JVM/Debugging.md> "FPC JVM/Debugging")
  * [Сборка компилятора и Ява утилит](<../en/FPC_JVM/Building.md> "FPC JVM/Building")
  * [Информация о внутренних изменениях в компиляторе и RTL](<FPC_JVM/Internals.md> "FPC JVM/Internals/ru") (представляет интерес для разработчиков компилятора и/или RTL)

---

_Source: [https://wiki.freepascal.org/FPC_JVM/ru](https://web.archive.org/web/20240701000000/https://wiki.freepascal.org/FPC_JVM/ru)_
