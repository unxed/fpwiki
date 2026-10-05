# IDE Macros in paths and filenames

│ [**Deutsch (de)**](</IDE_Macros_in_paths_and_filenames/de> "IDE Macros in paths and filenames/de") │  [**English (en)**](<../en/IDE_Macros_in_paths_and_filenames.md> "IDE Macros in paths and filenames") │  [**español (es)**](</IDE_Macros_in_paths_and_filenames/es> "IDE Macros in paths and filenames/es") │  [**français (fr)**](</IDE_Macros_in_paths_and_filenames/fr> "IDE Macros in paths and filenames/fr") │  [**日本語 (ja)**](</IDE_Macros_in_paths_and_filenames/ja> "IDE Macros in paths and filenames/ja") │  [**português (pt)**](</IDE_Macros_in_paths_and_filenames/pt> "IDE Macros in paths and filenames/pt") │  **русский (ru)** │    


## Contents

  * 1 Типы макросов
  * 2 Формат макросов IDE
  * 3 Общего назначения
  * 4 Части имени файла
  * 5 Пути и их части
  * 6 Окружение



## Типы макросов

Имеются различные типы макросов: 

  * **Макросы IDE:** они могут быть использованы почти во всех полях IDE, например, путях поиска, настраиваемых опциях, именах файлов, параметрах запуска. Они заменяются своими значениями перед вызовом внешних инструментов, таких, как компилятор или отладчик. Регистронезависимы. 
  * **Символы FPC:** они либо определены (on), либо не определены (off). Передаются через опцию командной строки **-d** , которая может быть задана в _Compiler Options / Custom Options_. Например, _-dDEBUG -dVerbose_ определит символы FPC **DEBUG** и **Verbose** , поэтому вы сможете использовать _**{$IFDEF Debug}**_. Регистронезависимы. 
  * **Макросы FPC:** они могут быть определены, например, кодом вроде такого: **{$define MYFPCMACRO:=42}**. Примером предопределённого макроса является FPC_FULLVERSION. 
  * **Макросы сборки:** это макросы IDE с ограниченной областью действия. Они определяются проектами и пакетами. Регистронезависимы. 
  * Некоторые плагины IDE имеют свои собственные макросы. 



## Формат макросов IDE

Макросы IDE используются в следующем формате (замените _macro-name_ на один из макросов, перечисленных ниже): 
    
    
    $(macro-name)
    

  
Например, такой "Каталог вывода модуля" часто используется для пакетов Lazarus: 
    
    
    lib/$(TargetCPU)-$(TargetOS)
    

  * В системе x86 Linux 32-bit это будет эквивалентно: **lib/i386-linux**
  * В системе x86 Linux 64-bit это будет эквивалентно: **lib/x86_64-linux**



  
Также есть некоторые **макрофункции** , которые используют следующий формат: 
    
    
    $macro_name(parameters)
    

  
Например, 
    
    
    $Ext(unit1.pas)
    

выдаст **.pas**. 

## Общего назначения

  * **Col** \- текущая колонка в редакторе кода 
  * **Row** \- текущая строка в редакторе кода 
  * **CurToken** \- текущая лексема около курсора в редакторе кода 
  * **EdFile** \- имя текущего файла в редакторе кода 
  * **Params** \- параметры запуска текущего проекта 
  * **Prompt** \- запрашивает значение у пользователя. Это интерактивный макрос. 
  * **RunCmdLine** \- командная строка для запуска проекта 
  * **Save** \- сохраняет текущий файл в редакторе кода 
  * **SaveAll** \- сохраняет всё 
  * **TargetCmdLine** \- путь до испольняемого модуля проекта и параметры запуска 



## Части имени файла

  * **Ext(filename)** \- макрофункция для ExtractFileExt. Пример: вернёт **.exe** под Windows. 
  * **MakeDir(filename)** \- макрофункция для AppendPathDelim 
  * **MakeFile(filename)** \- макрофункция для ChompPathDelim 
  * **MakeExe(filename)** \- меняет расширение файла на **.exe** под Windows, ничего не делает под Linux, BSD, OS X. При кросс-компиляции он создаст имя файла для целевой ОС. Для получения имени файла хост-ОС используйте $MakeExe(ide,filename). Для получения имени файла указанной ОС используйте $MakeExe(os,filename). Вариант с двумя параметрами существует с версии 1.1. 
  * **MakeLib(filename)** \- меняет расширение файла на .dll под Windows, под Linux/BSD меняет на libname.so в нижнем регистре, под OS X - на libname.so (с версии 0.9.29) 
  * **Name(filename)** \- макрофункция для ExtractFileName 
  * **NameOnly(filename)** \- макрофункция для ExtractFileNameOnly 
  * **Path(filename)** \- макрофункция для ExtractFilePath 



## Пути и их части

  * **CompPath** \- путь к компилятору. До версии 1.3 он возвращал значение опции IDE (_Tools / Options ..._). Он не может использовать макросы FPCVer, FPC_FULLVERSION, поскольку эти макросы берутся из компилятора. Вы можете использовать макросы TargetOS, TargetCPU, SrcOS **только** если вы установите их. Вы можете использовать макросы LazarusDir и Env. С версии 1.3 макрос возвращает компилятор проекта. Он возвращает значение по умолчанию опции IDE: 
    * когда используется параметр _IDE_ : $CompPath(IDE) 
    * в самой опции 'компилятор проекта' 
    * когда компилятор проекта недоступен (_Build_ , _Compile_) 
    * когда компилятор проекта - не fpc или ppcsomething 
    * когда IDE собрана 
  * **ConfDir** \- каталог, где IDE хранит свои файлы конфигурации (такие, как PrimaryConfigPath). 
  * **ExeExt** \- расширение исполняемого файла для операционной системы независимо от целевой ОС проекта. Для получения расширения для целевой ОС текущего проекта используйте $MakeExe(). 
  * **FallbackOutputRoot** \- каталог, куда IDE помещает файлы ppu, если выходной каталог пакета недоступен для записи. По умолчанию: $(PrimaryConfigPath)/lib. С версии 0.9.31. 
  * **FPCSrcDir** \- Каталог с исходниками FPC, указанный в настройках окружения 
  * **FPCMsgFile** \- файл сообщений компилятора, указанный в настройках окружения Lazarus (с версии 0.9.31). 
  * **InstantFPCCache** \- путь к instantfpccache. Вывод запуска _instantfpc --get-cache_ (с версии 0.9.31). 
  * **LazarusDir** \- каталог с исходным кодом Lazarus, указанный в настройках окружения Lazarus. Макросы запрещены. 
  * **Make** \- путь к утилите make (gmake под BSD) (с версии 0.9.29) 
  * **PrimaryConfigPath** \- каталог конфигурационных файлов IDE (с версии 0.9.31). См. **SecondaryConfigPath**. 
  * **ProjFile** \- полное имя файла главного исходника текущего проекта (.lpr) 
  * **ProjIncPath** \- include path of project directory 
  * **ProjOutDir** \- путь к каталогу вывода проекта (например, где создаются файлы .ppu) (с версии 0.9.27) 
  * **ProjPath** \- каталог проекта (где находится файл .lpi) 
  * **ProjPublishDir** \- каталог публикации текущего проекта 
  * **ProjSrcPath** \- source path of project directory 
  * **ProjUnitPath** \- unit path of project directory 
  * **Project(param)** \- макрофункция для различных значений: 
    * **Project(UnitPath)** \- unit path of project directory 
    * **Project(SrcPath)** \- source path of project directory 
    * **Project(IncPath)** \- include path of project directory 
    * **Project(InfoFile)** \- имя файла информации о проекте (.lpi) (с r15287, 0.9.25) 
    * **Project(OutputDir)** \- каталог, где создаются файлы ppu проекта (с версии 0.9.27) 
  * **Макросы пакетов** \- могут быть использованы в полях пакета. Например, в путях поиска пакета. Для использования их в другом месте, передавайте имя пакета как параметр. 
    * **$(PkgName)** \- в пакете: даёт имя пакета (с версии 0.9.31) 
    * **$PkgName(id)** \- макрофункция для имени пакета, ID которого передан как параметр (с версии 0.9.31) 
    * **$(PkgDir)** \- макрос для каталога пакета (где находится .lpk) 
    * **$PkgDir(id)** \- макрофункция для каталога пакета (где находится .lpk), ID которого передан как параметр. 
    * **$PkgIncPath(id)** \- macro function for the include path of a package ID given as parameter 
    * **$PkgOutDir(id)** \- макрофункция для каталога вывода пакета (например, где создаются файлы ppu) 
    * **$PkgSrcPath(id)** \- macro function for the source path (unit path + src path) of a package ID given as parameter 
    * **$PkgUnitPath(id)** \- macro function for the unit path of a package ID given as parameter 
  * **SecondaryConfigPath** \- каталог шаблонов конфигурации IDE (с версии 0.9.31). См. **PrimaryConfigPath**
  * **TargetFile** \- выходной файл текущего проекта (например, исполняемый или библиотека) 
  * **TestDir** \- Тестовый каталог, указанный в настройках окружения Lazarus 



## Окружение

  * **BuildMode** \- название активного режима сборки (с версии 1.1) 
  * **Env(name)** \- макрофункция для переменных окружения, выданных для IDE (не для проекта и не для отладчика) (см. [GetEnvironmentVariableUTF8](<http://lazarus-ccr.sourceforge.net/docs/lcl/fileutil/getenvironmentvariableutf8.html>)) (с версии 0.9.27) 
  * **IDEBuildOptions** \- дополнительные параметры в диалоге 'Параметры сборки Lazarus' (с версии 0.9.29). В Makefile: пусто. 
  * **LanguageID** \- Язык IDE, например, **en** \- для английского, **de** \- для немецкого 
  * **LanguageName** \- Название текущего языка IDE, переведённое на этот язык. Например, **Deutsch** \- для немецкого, **Русский** \- для русского. 
  * **LazVer** \- Строка версии IDE (с версии 1.3). 
  * **FPCVer** \- Версия FPC (с версии 0.9.25). Например, '2.4.2'. Эта версия извлекается из $(CompPath) компилятора, путь указывается в настройках окружения Lazarus. **FPCVer** зависит от пути компилятора проекта. 
  * **FPC_FULLVERSION** \- Версия FPC в виде одного числа (с версии 1.3). Например, _20701_. Эта версия извлекается из $(CompPath) компилятора, путь указывается в настройках окружения Lazarus. **FPC_FullVersion** зависит от пути компилятора проекта. 
  * **SrcOS** \- 'unix' для linux, darwin, bsd. 'win' для win32, win64, wince. С версии 0.9.31: используйте **$SrcOS(SomeOS)** для получения SrcOS конкретной ОС, например, **$SrcOS($TargetOS(IDE))** \- для получения SrcOS исполняемого файла IDE. **SrcOS** зависит от _TargetOS_. 
  * **TargetOS** \- Целевая ОС текущего проекта. С версии 0.9.31: используйте **$TargetOS(IDE)** для получения ОС исполняемого файла IDE. Если проект не установил **TargetOS** , IDE использует платформу по умолчанию компилятора проекта (1.3 и ниже использует как платформу по умолчанию платформу IDE). В Makefile это конвертируется в _%(OS_TARGET)_. 
  * **TargetCPU** \- аналогично с _TargetOS_. В Makefile это конвертируется в _%(CPU_TARGET)_. 
  * **LCLWidgetType** \- набор виджетов LCL текущего проекта. В Makefile это конвертируеется в _%(LCL_PLATFORM)_.

---

_Source: [https://wiki.freepascal.org/IDE_Macros_in_paths_and_filenames/ru](https://web.archive.org/web/20180519212036/https://wiki.freepascal.org/IDE_Macros_in_paths_and_filenames/ru)_
