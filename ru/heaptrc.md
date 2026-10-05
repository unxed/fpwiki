# heaptrc

│ **[English (en)](<../en/heaptrc.md>)** │  **русский (ru)** │

**`heaptrc`** \- это модуль, который можно использовать для отладки выделения и освобождения блоков памяти. Он отслеживает вызовы [`getMem`](<https://www.freepascal.org/docs-html/rtl/system/getmem.html>)/[`freeMem`](<https://www.freepascal.org/docs-html/rtl/system/freemem.html>) и, неявно, операторы [`new`](<https://www.freepascal.org/docs-html/rtl/system/new.html>)/[`dispose`](<https://www.freepascal.org/docs-html/rtl/system/dispose.html>). 

[![Warning-icon.png](https://wiki.freepascal.org/images/b/b2/Warning-icon.png)](</File:Warning-icon.png>)

**Предупреждение:** Не добавляйте модуль `heaptrc` вручную. Модуль `heaptrc` необходимо загрузить до `lineinfo`, и только компилятор может это сделать. `Heaptrc` также является диспетчером памяти, поэтому не пытайтесь использовать какой-либо диспетчер памяти, включая сам `heaptrc`, в [разделе `uses`](<../en/Uses.md> "Uses"), поскольку это может привести к повреждению памяти и ложным результатам. В этом разделе содержится примечание с примером кода для универсальной обработки такого случая.

When a program using heaptrc ends, heaptrc can write out the total memory used and a list of allocated but not freed blocks to a file. When run in the console (*nix or Windows), heaptrc will print this output to screen unless otherwise instructed. In Lazarus GUI programs on Windows, the output will appear in numerous small dialog boxes, which may be very unpractical, although you can redirect output to file. 

Когда программа, использующая heaptrc, завершается, heaptrc может записать общий объем используемой памяти и список выделенных, но не освобожденных блоков в файл. При запуске в консоли (*nix или Windows) heaptrc выводит эту информацию на экран, если не указано иное. В программах Lazarus с графическим интерфейсом пользователя в Windows вывод будет отображаться во множестве небольших диалоговых окон, что может быть очень непрактичным, хотя вы можете перенаправить вывод в файл. 

[![Standard output of heaptrc on Windows \(despite of the title there is no error in the application\)](https://wiki.freepascal.org/images/7/79/Win_heaptrc_output_no_error.png)](</File:Win_heaptrc_output_no_error.png> "Standard output of heaptrc on Windows \(despite of the title there is no error in the application\)")

В * nix (включая BSD, Linux и macOS) по умолчанию для программ с графическим интерфейсом ничего не отображается ([прим.перев.](</User:Zoltanleo> "User:Zoltanleo"): для вывода в консоль под *nix необходимо подключить окно в разделе View/Debug Window/Console In/Output).  
  
[![console output win.png](https://wiki.freepascal.org/images/9/96/console_output_win.png)](</File:console_output_win.png>)  
  


См. [Leakview](<leakview.md> "leakview/ru") для получения подробной информации о том, как эффективно использовать heaptrc. 

## Contents

  * 1 Использование
    * 1.1 Использование heaptrc в FPC
    * 1.2 Использование heaptrc в Lazarus
    * 1.3 Показывать отчет только об утечке
    * 1.4 Программное отключение отчета
    * 1.5 Продолжать выполнение даже при ошибке кучи
    * 1.6 Перенаправление вывода в файл
    * 1.7 Почему heaptrc не следует добавлять в раздел uses вручную
  * 2 См.также



## Использование

### Использование heaptrc в FPC

Добавьте параметр `-gh` в командную строку компиляции или в `fpc.cfg`. 
    
    
    fpc -gh helloworld.pas
    

или обычно в сочетании с информацией о строке: 
    
    
    fpc -glh helloworld.pas
    

Это добавит heaptrc неявно, как скрытый первый модуль раздела uses программы. 

Вы можете проверить в коде, активен ли heaptrc, проверив глобальную константу [**`useHeapTrace`**](<https://www.freepascal.org/docs-html/rtl/heaptrc/useheaptrace.html>) на наличие heaptrc и ее статус следующим образом: 
    
    
    program possiblyHeaptraced(input, output, stdErr);
    begin
    	{$if declared(useHeapTrace)}
    		writeLn('Heaptrc is used.');
                    // отчеты heaptrc могут быть отключены при связывании в ... true или false
    		writeLn('Heaptrc is active: ', useHeaptrace); 
                    // впоследствии вы можете протестировать/задать любые параметры отчетов heaptrc
    	{$else}
    		writeLn('No trace of heaptrc.');
    	{$endIf}
    end.
    

Обратите внимание, что `{$if declared(useHeapTrace)}` работает только в программе, а не в модуле. 

Heaptrc не работает, если вы используете его с FPC 3.0.4 в Linux и ваша программа вызывает функцию `system.get_cmdline`, и в этом случае вы получите в конце программы много сообщений об ошибках EAccessViolation. Это похоже на ошибку, например в FPC 3.2.0-beta rev 43824 эта проблема не возникает. 

### Использование heaptrc в Lazarus

Чтобы включить это в вашем проекте Lazarus: 

  1. Идем в раздел Project Options/Compiler Options/Debugging (Параметры проекта/Параметры компилятора/Отладка) и
  2. в разделе Other debugging info (Прочая отладочная информация) включаем _Use Heaptrc unit (check for mem-leaks) (-gh)_ (Использовать модуль Heaptrc (проверка на наличие утечек памяти) (-gh))



Вы можете проверить наличие `heaptrc` таким же образом, как в приведенном выше примере, проверив наличие `useHeapTrace` в вашем файле project.lpr. 
    
    
    {$if declared(useHeapTrace)}
    	// ... что-то делаем
    	
    	// проверяем активность
    	if useHeapTrace then
    	begin
    		// ...
    	end;
    {$endIf}
    

### Показывать отчет только об утечке

Если вы хотите отображать диалоговое окно отчета `heaptrc` только в том случае, если в вашем приложении были утечки, поместите это [присвоение](<Becomes.md> "Becomes/ru") где-нибудь в исходный файл основного проекта: 
    
    
    {$if declared(useHeapTrace)}
    	globalSkipIfNoLeaks := true; // поддерживается начиная с версии отладчика 3.2.0
    {$endIf}
    

### Программное отключение отчета

Если вы хотите отключить отчет из своего кода, вы можете напрямую управлять константой `useHeapTrace` при запуске программы: 
    
    
    useHeapTrace := false;
    

Это фактически изменит поведение на нормальный менеджер памяти. 

### Продолжать выполнение даже при ошибке кучи

Если вы хотите, чтобы ваше приложение продолжало выполнение даже в случае ошибки кучи, манипулируйте константой [`haltOnError` constant](<https://www.freepascal.org/docs-html/rtl/heaptrc/haltonerror.html>) при запуске программы: 
    
    
    haltOnError := false;
    

### Перенаправление вывода в файл

Если вы хотите перенаправить отчет об утечке в файл, добавьте его в начало основного файла проекта: 
    
    
    setHeapTraceOutput('trace.log'); // поддерживается начиная с версии отладчика 3.2.0
    

### Почему heaptrc не следует добавлять в раздел uses вручную

Никогда не следует добавлять heaptrc вручную в обычную программу, потому что модуль heaptrc должен быть загружен до lineinfo для правильной работы отладки. 

Для эффективного декодирования отладочной информации dwarf требуется работающий менеджер кучи, а это означает, что heaptrc необходимо загрузить до модуля декодирования dwarf line info, который загружается с помощью -gl, поэтому heaptrc не может использоваться явно. 

Только сам компилятор может вставить модуль в нужное место в качестве скрытого первого модуля в разделе uses. Добавление модуля вручную может вызвать всевозможные нежелательные побочные эффекты. Например, информациях о сбоях, сообщаемая lineinfo, не является надежной, и не обо всех утечках сообщения будут достоверны. 

Так было всегда, но правильно задокументировано только в FPC версии 3.0.2. Так что, если вы увидите heaptrc в своем разделе uses, _удалите его!_ для правильной работы отладки и отчетов. 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Примечание:** Если ваша программа предназначена для использования альтернативного менеджера памяти, вы можете адаптировать раздел uses вашей программы следующим образом: 
    
    
    program alternativeMemoryManager(input, output, stdErr);
    {$mode objFPC}
    uses
            // Это условно переключит диспетчер памяти
            // на основе наличия heaptrc
    	{$if not declared(useHeapTrace)}
    		cMem,
    	{$endIf}
    	sysUtils, classes; // два последних предназначены для тестирования.
    
    begin
    	{$if declared(useHeapTrace)}
    		writeLn('Heaptrc is used.', ' Heaptrc is active: ', useHeaptrace);
    	{$else}
    		writeLn('No trace of heaptrc.');
    	{$endIf}
    end.
    

В приведенном выше примере условное выражение в разделе uses гарантирует, что диспетчер памяти cmem компилируется только в том случае, если параметр heaptrc не используется. 

## См.также

  * <https://bugs.freepascal.org/view.php?id=30637>
  * [leakview](<leakview.md> "leakview/ru"): Примеры включения heaptrc в Lazarus и Free Pascal.
  * [RTL Reference for unit 'heaptrc'](<http://lazarus-ccr.sourceforge.net/docs/rtl/heaptrc/> "doc:rtl/heaptrc/") (эта ссылка еще не обновлена)

---

_Source: [https://wiki.freepascal.org/heaptrc/ru](https://web.archive.org/web/20241206080040/https://wiki.freepascal.org/heaptrc/ru)_
