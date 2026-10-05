# Command line parameters and environment variables

│ **[English (en)](<../en/Command_line_parameters_and_environment_variables.md> "Command line parameters and environment variables")** │  **[español (es)](</Command_line_parameters_and_environment_variables/es> "Command line parameters and environment variables/es")** │  **[suomi (fi)](</Command_line_parameters_and_environment_variables/fi> "Command line parameters and environment variables/fi")** │  **[français (fr)](</Command_line_parameters_and_environment_variables/fr> "Command line parameters and environment variables/fr")** │  **русский (ru)** │    
****

В большинстве (интерактивных) операционных систем [программы](<Program.md> "Program/ru") можно запускать через интерфейс командной строки [command line interface](<../en/Command-line_interface.md> "Command-line interface") (CLI), что позволяет предоставлять программе дополнительные данные. Модули [system](<../en/System_unit.md> "System unit") (и [`objPas`](<../en/Object_Pascal.md> "Object Pascal")) предоставляют базовые [функции](<Function.md> "Function/ru") для доступа к данным, предоставленным из командной строки. 

## Contents

  * 1 Обзор
    * 1.1 Терминология
    * 1.2 Основы
  * 2 Параметры командной строки
    * 2.1 Основы
    * 2.2 Простота использования
      * 2.2.1 Проверка параметра
      * 2.2.2 Чтение значения параметра
      * 2.2.3 Проверка параметров на валидность
  * 3 Переменные окружения



## Обзор

Хотя на этой странице говорится о _параметрах командной строки_ и переменных среды, фактический интерфейс командной строки (CLI) не является обязательным требованием для использования таких данных, а также для использования процедур и терминологии, описанных здесь. Однако, поскольку интерфейс командной строки более осязаем, следующие инструкции в основном относятся к пользовательскому интерфейсу оболочки. 

### Терминология

Option (переключатели)
    Опции - флаги yes/no. Обычно они имеют вид `‑‑dryrun`/`‑‑no‑dryrun` (dryrun включен или отключен соответственно). Опции также называются «переключателями».
Parameter (параметр)
    Параметры представляют собой кортежи ключ-значение. В командной строке они выглядят так: `‑‑processors 4`.
Argument (аргумент)
    Аргументы — это простые слова. В командной строке они обычно разделяются пробелами (назначают переменную [IFS](<https://en.wikipedia.org/wiki/Input_Field_Separators>) в некоторых оболочках). В узком смысле, аргументы — это все предоставленные слова, которые не являются ни опциями, ни (частью) параметров. В общем, _аргументы_ — это все слова в командной строке, которые никак не интерпретируются.
Environment variable (переменные окружения)
    Переменные окружения относятся к помеченной части хранилища в среде. Это пары имя-значение.

Обратите внимание, CLI/OS предоставляет программе только те аргументы, которые она должна получить: например, экранированные символы новой строки, перенаправление файла (каналы или в файл) и присвоение переменных среды не будут перенаправлены в программу. 

### Основы

Поскольку понятия «опция» и «параметры» уже являются своего рода высокоуровневыми, существуют разные стили (короткие опции против длинных опций), и модуль system стремится предоставить _базовые_ средства для работы, все аргументы командной строки просто перечисляютсяить, начиная с нуля. Никакой интерпретации не делается. 

Функция [`paramStr`](<https://www.freepascal.org/docs-html/rtl/system/paramstr.html>) возвращает n-й аргумент в командной строке. `ParamStr(0)` пытается вернуть имя и, возможно, полный путь к исполняемому программному файлу. 

## Параметры командной строки

### Основы

Программа на Паскале может получить доступ к аргументам командной строки, используя [функцию](<Function.md> "Function/ru")`paramStr` в сочетании с [`paramCount`](<https://www.freepascal.org/docs-html/rtl/system/paramcount.html>). `ParamCount` возвращает количество предоставленных аргументов. 
    
    
    program listArguments(input, output, stdErr);
    {$mode objFPC}
    var
    	i: integer;
    begin
    	writeLn({$ifDef Darwin}
    			// в Mac OS X возвращаемое значение зависит от метода вызова
    			'This program was invoked via: ',
    		{$else}
    			// Turbo Pascal-совместимый paramStr(0) возвращает местоположение
    			'This program is/was stored at: ',
    		{$endIf}
    		paramStr(0));
    	
    	for i := 1 to paramCount() do
    	begin
    		writeLn(i:2, '. argument: ', paramStr(i));
    	end;
    end.
    

Запуск этой программы (на не-Mac OS Ⅹ платформе) может выглядеть следующим образом: 
    
    
    $ ./listArguments foo bar able
    This program is/was stored at: /tmp/listArguments
     1. argument: foo
     2. argument: bar
     3. argument: able
    

Модуль [RTL](<../en/RTL.md> "RTL") `system` предоставляет версию `paramStr`, которая возвращает короткие строки [`shortString`](<../en/Shortstring.md> "Shortstring"). Из-за особенностей реализации `shortString` ограничены 255 символами. Однако пользователь может указать более длинные аргументы командной строки. 

Чтобы получить к ним доступ, модуль [`objPas`](<https://www.freepascal.org/docs-html/rtl/objpas/index.html>) переопределяет `paramStr`, который вместо этого возвращает строки [`ansiString`](<../en/Ansistring.md> "Ansistring"), которые не имеют этого ограничения длины. Модуль `objPas` автоматически включается в режимы компилятора [`{$mode objFPC}`](<../en/Mode_ObjFPC.md> "Mode ObjFPC") (строка2) и [`{$mode Delphi}`](<../en/Mode_Delphi.md> "Mode Delphi"). 

Распределенная функция `paramStr` пытается быть совместимой с [Turbo Pascal](<../en/Turbo_Pascal.md> "Turbo Pascal"). В TP `paramStr(0)` возвращает местоположение программы. Однако операционная система должна поддерживать это. В частности, в Mac OS Ⅹ значение `paramStr(0)` зависит от метода вызова, от того, как запускается приложение. 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Примечание:** Поскольку `paramStr(0)` зависит от функциональности ОС, он не подходит для [кроссплатформенных программ](<Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide/ru").

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Примечание:** На юниксоидных платформах невозможно точно определить местоположение файла только по строке. Может быть несколько жестких ссылок, указывающих на один и тот же индексный дескриптор, файл мог быть уже удален, части пути к файлу могли измениться, помимо других деталей. По этим причинам `paramStr(0)` на юниксоидных платформах бесполезен.

Модуль [`getOpts`](<https://www.freepascal.org/docs-html/rtl/getopts/index.html>) предоставляет несколько других подпрограмм для вызова программы в соответствии с определенным стилем. 

### Простота использования

Хорошая программа должна выдавать справочное сообщение при вызове с неправильными параметрами и должна следовать общепринятому способу предоставления параметров. Модуль [`custApp`](<https://www.freepascal.org/docs-html/fcl/custapp/index.html>), поставляемый с FPC, предоставляет класс [`TCustomApplication`](<https://www.freepascal.org/docs-html/fcl/custapp/tcustomapplication.html>), предоставляющий функции для простой проверки и чтения параметров. Конечно, вы по-прежнему можете получить доступ к параметрам напрямую через `paramStr` и `paramCount`. 

Каждое приложение [LCL](<../en/LCL.md> "LCL") использует это автоматически. Объект Application — это TCustomApplication. 

Если вы хотите написать не LCL-программу, то создайте в [Lazarus](<../en/Lazarus.md> "Lazarus") новый проект типа «Консольное приложение». Это создаст файл project1.lpr с некоторыми приятными дополнениями, которые нужны почти всем программам. Перейдите к методу `doRun`. 

#### Проверка параметра

С TCustomApplication вы можете получить доступ к параметрам по имени. Например, ваша программа должна выводить текст справки, когда пользователь задает общий параметр справки _-h_. _-h_ является коротким вариантом параметра. Длинная форма _\--help_. Чтобы проверить, вызвал ли пользователь программу с помощью _-h_ или _\--help_ , вы можете использовать следующий код: 
    
    
    if hasOption('h', 'help') then
    begin
    	writeHelp;
    	halt;
    end;
    

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Примечание:** В LCL-форме вы должны добавить `application.` перед `hasOption`.

Например: 
    
    
    if application.hasOption('h', 'help') then
    begin
    	writeHelp;
    	halt;
    end;
    

Если вы хотите поддерживать только короткий вариант, используйте: 
    
    
    if hasOption('h', '') then
    

Если вы хотите поддерживать только длинный вариант, используйте: 
    
    
    if hasOption('help') then
    

#### Чтение значения параметра

Каждому параметру может быть присвоено значение. Например: 
    
    
    $ project1 -f filename
    

или в длинной форме: 
    
    
    $ project1 --file=filename
    

Чтобы получить `filename`, используйте: 
    
    
    writeLn('f=', getOptionValue('f', 'file'));
    

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Примечание:** если вы получаете сообщение об ошибке "`Option at position 1 needs an argument : f.`" (Переключатель в позиции 1 нуждается в аргументе: f), то вы забыли добавить опцию при вызове [`checkOptions`](<https://www.freepascal.org/docs-html/fcl/custapp/tcustomapplication.checkoptions.html>).

#### Проверка параметров на валидность

Параметры командной строки представляют собой произвольный текст, поэтому пользователь при вводе может легко допускать ошибки. Отсюда проверка синтаксиса параметров обязательна. Для этого вы можете использовать метод [CheckOptions](<http://lazarus-ccr.sourceforge.net/docs/fcl/custapp/tcustomapplication.checkoptions.html> "doc:fcl/custapp/tcustomapplication.checkoptions.html"). 

Вы можете определить, какие параметры разрешены, какие из них нуждаются в параметре, а в случае синтаксической ошибки вы можете получить сообщение об ошибке, а также неправильные параметры для печати полезных и подробных ошибок. 

Пример: 
    
    
    errorMsg := checkOptions('hf:', 'help file:');
    

Это позволяет передавать короткие переключатели: `‑f value` и `‑h`.  
Это - передавать длинные переключатели: `‑‑help` или `‑‑file=filename`.  
А это не позволяет передачу параметра `‑‑help` _со значением_ или `‑‑file` _без значения_.  


Пример передачи параметра: 
    
    
    procedure TMainForm.FormShow(Sender: TObject);
    var
        I: Integer;
        Params : TStringList
        LongOpts : array [1..2] of string = ('debug-sync', 'config-dir:');
    begin
        Params := TStringList.Create;
        try
            Application.GetNonOptions('hgo:', LongOpts, Params);
            for I := 0 to Params.Count -1 do
                debugln('Extra Param ' + inttostr(I) + ' is ' + Params[I]);  }
        finally
            FreeAndNil(Params);
        end;
    end;
    

Этот пример находит параметры и не смешивает их с переключателями или параметрами командной строки. В этом примере приложение также принимает параметры --debug-sync и --config-dir=somedir, но здесь они не указаны. 

Обратите внимание, что Application.GetNonOptions() принимает длинные параметры в виде массива, а Application.CheckOptions принимает те же данные, но в виде строки. Немного грустно! 

## Переменные окружения

Модуль [`sysUtils`](<../en/sysutils.md> "sysutils") определяет три основные функции для получения переменных среды. 

  * [`getEnvironmentVariableCount`](<https://www.freepascal.org/docs-html/rtl/sysutils/getenvironmentvariablecount.html>) возвращает общее количество переменных среды.
  * [`getEnvironmentString`](<https://www.freepascal.org/docs-html/rtl/sysutils/getenvironmentstring.html>) возвращает переменную среды по ее индексу
  * [`getEnvironmentVariable`](<https://www.freepascal.org/docs-html/rtl/sysutils/getenvironmentvariable.html>) возвращает переменную среды по ее ключу



Пример: 
    
    
    program environmentVariablesList(input, output, stdErr);
    uses
    	sysUtils;
    var
    	i: integer;
    begin
    	for i := 0 to getEnvironmentVariableCount() - 1 do
    	begin
    		writeLn(getEnvironmentString(i));
    	end;
    end.
    

Также можно загрузить все переменные среды в объект [`TStringList`](<https://www.freepascal.org/docs-html/rtl/classes/tstringlist.html>) и легко получить доступ к паре имя-значение. Списки строк автоматически учитывают символ-разделитель, который по умолчанию является знаком равенства [(`'='`)](<../en/=.md> "="). 
    
    
    program environmentVariablesSorted(input, output, stdErr);
    {$mode objFPC}
    uses
    	classes, sysUtils;
    var
    	i: integer;
    	environmentVariables: tStringList;
    begin
    	environmentVariables := tStringList.create();
    	
    	try
    		// загружаем все переменные в список строк
    		for i := 0 to getEnvironmentVariableCount() - 1 do
    		begin
    			environmentVariables.append(getEnvironmentString(i));
    		end;
    		
    		environmentVariables.sort;
    		for i := 0 to environmentVariables.count - 1 do
    		begin
    			writeLn(environmentVariables.names[i]:20,
    				' = ',
    				environmentVariables.valueFromIndex[i]);
    		end;
    	finally
    		environmentVariables.free;
    	end;
    end.
    

Существует также [метод](<../en/Method.md> "Method") `TCustomApplication` для одновременного получения всех переменных среды в виде [`TStringList`](<../en/TStringList.md> "TStringList"). Тогда схема может выглядеть так: 
    
    
    var
    	environmentVariables: tStringList;
    begin
    	environmentVariables := tStringList.create();
    	try
    		application.getEnvironmentList(environmentVariables);
    		…
    	finally 
    		environmentVariables.free;
    	end;
    end;

---

_Source: [https://wiki.freepascal.org/Command_line_parameters_and_environment_variables/ru](https://web.archive.org/web/20240711001428/https://wiki.freepascal.org/Command_line_parameters_and_environment_variables/ru)_
