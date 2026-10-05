# Conditional compilation

│ **[English (en)](<../en/Conditional_compilation.md>)** │  **русский (ru)** │

## Contents

  * 1 Что такое условная компиляция?
  * 2 Директивы в стиле Turbo Pascal
    * 2.1 $define
    * 2.2 $undef
    * 2.3 $ifdef и $endif
    * 2.4 $ifndef
    * 2.5 $else и $elseif
    * 2.6 $ifopt
  * 3 Как не нужно делать



# Что такое условная компиляция?

Условная компиляция - это компиляция или пропуск части [ исходного кода](<../en/Source_code.md> "Source code") в зависимости от того, существует условие или нет. 

Функции, которые делают это возможным в большинстве компилируемых языков, называются директивами времени компиляции. Директивы времени компиляции позволяют компилировать блок кода на основе наличия или отсутствия условия во [время компиляции](<../en/Compile_time.md> "Compile time"). Они являются частью [директив компилятора](<../en/Compiler_directive.md> "Compiler directive"). 

Они могут использоваться для различных целей, таких как: 

  * изоляция кода для конкретной платформы
  * выбор естественного языка
  * лицензирование частей с открытым и закрытым исходным кодом
  * изоляция экспериментального кода
  * версия [компилятора](<../en/Compiler.md> "Compiler")
  * версия библиотеки
  * и т.д. и т.п.



[Free Pascal](<FPC.md> "FPC/ru") поддерживает четыре различных стиля условной компиляции: 

  * [Turbo Pascal](<../en/Turbo_Pascal.md> "Turbo Pascal") и ранние директивы стиля [Delphi](<../en/Delphi.md> "Delphi")
  * директивы стиля [Mac Pascal](<../en/Mac_Pascal.md> "Mac Pascal")
  * Современные директивы в стиле Free Pascal и Delphi
  * Макросы времени компиляции



[![Note-icon.png](https://wiki.freepascal.org/images/b/be/Note-icon.png)](</File:Note-icon.png>)

**Примечание:** здесь синтаксис не [чувствителен к регистру](<../en/case-sensitive.md> "case-sensitive"), поскольку соответствует всему синтаксису [Pascal](<../en/Pascal.md> "Pascal"). Мы будем использовать как строчные, так и прописные примеры. Мы покажем вам разницу между режимами и как их эффективно использовать.

# Директивы в стиле Turbo Pascal

Директивами в стиле Turbo Pascal являются `{$DEFINE}`, `{$IFDEF}`, `{$ENDIF}`, `{$IFNDEF}`, `{$IFOPT}`, `{$ELSE}`, `{$ELSEIF}` и `{$UNDEF}`. 

Мы опишем директивы в контексте стиля. Некоторые определения имеют расширенное значение в другом стиле. 

Это означает, что в дальнейшем мы можем расширить значение некоторых директив, таких как, например, `{$DEFINE}`, в контексте макросов. 

## $define

Директива `{$DEFINE}` просто объявляет символ, который мы позже можем использовать для условной компиляции: 
    
    
    {$DEFINE name} //Это объявляет символ под названием "name"
    

Обратите внимание, что вы также можете объявить символ из [командной строки](<../en/Command-line_interface.md> "Command-line interface") или , например, **-dDEBUG** [IDE](<../en/IDE.md> "IDE"), который будет являться эквивалентом командной строки 
    
    
    {$DEFINE DEBUG}
    

в исходном коде. 

## $undef

Директива `{$UNDEF}` отменяет объявление ранее определенного символа. Вот пример, который автор использует на практике: 
    
    
    // Некоторый старый исходный код, "загрязненый" обилием {$IFDEF FPC}, который больше 
    // не нуждается в зависимости от версии Delphi, с которой он должен быть совместим. 
    // Я всегда проверяю это, пробуя это поверх программы или модуля:
    {$IFDEF FPC}
      {$MODE DELPHI}
      {$UNDEF FPC}
      {$DEFINE VER150} 
     // код теперь будет компилироваться так, как если бы это был Delphi 7, при условии, что исходный код Delphi действительно был написан для Delphi 7 и выше.
    {$ENDIF}
    

## $ifdef и $endif

Самый простой способ объявить блок условного кода так: 
    
    
    unit cross;
    {$IFDEF FPC}{$MODE DELPHI}{$ENDIF}
    

Приведенный выше пример довольно распространен для исходного кода, который должен компилироваться как в Delphi, так и в Free Pascal. 

Если компилятором является Delphi, то ничего не делается, но если компилятором является Free Pascal, он переключит Free Pascal для компиляции и использования режима синтаксиса Delphi. 

Этот условный символ "FPC" объявлен в системе - там их длинный список. Синтаксис блоков `{$IFDEF}` и `{$ENDIF}` является симметричным: каждый `{$IFDEF}` имеет свой собственный `{$ENDIF}`. 

Чтобы помочь вам распознать соответствующие блоки, вы можете использовать, например, отступ, но вы также можете использовать функцию [комментариев](<../en/Comments.md> "Comments"): 
    
    
    {$IFDEF FPC эта часть специфична для Free Pascal}
    // некий Free Pascal специфичный код
    {$ENDIF Free Pascal специфичный код}
    

[![Warning-icon.png](https://wiki.freepascal.org/images/b/b2/Warning-icon.png)](</File:Warning-icon.png>)

**Предупреждение:** Эта особенность комментариев часто не совсем понятна. Некоторые люди - как и в более старой версии этой записи вики - предположили, что вы можете вкладывать `{$IFDEF}`, потому что компилятор, кажется, принимает синтаксис. Но первое неверно, а второе верно: да, компилятор принимает приведенный ниже синтаксис, но это не вложенное `{$IFDEF}`, а одиночное условие `{$IFDEF}`, а остальное - комментарий! Приведенный ниже код выполнит _writeln_ **тогда и только тогда, когда будет объявлен** `{$define red}`. В этом примере `{$ifdef blue}` является комментарием! Даже если `{$define blue}` допустим.
    
    
    // программа формально будет скомпилирована, но полностью неработоспособна
    {$define blue}  
    begin
    {$ifdef red or $ifdef blue}// все после red - это комментарий 
      writeln ('red or blue'); // этот код никогда не выполнится
    {$endif red or blue}       // все, что объявлено после $endif, является комментарием.
    end.
    

## $ifndef

Это противоположно `{$IFDEF}`, и ниже лежащий код выполнится, если объявленное условие **не** выполняется. Простой пример: 
    
    
    {$IFNDEF FPC эта часть не для Free Pascal}
    // некий конкретный код, который Free Pascal не должен компилировать
    {$ENDIF код для других компиляторов, кроме Free Pascal}
    

## $else и $elseif

`{$ELSE}` используется для компиляции кода, который не принадлежит блоку кода, который объявлен соответствующим `{$IFDEF}`. Он также действителен в контексте `{$IFOPT}`, `{$IF}` или `{$IFC}`, которые мы обсудим позже. 
    
    
    {$IFDEF red}  
       writeln('Red объявлен');  
    {$ELSE  no red}  
      {$IFDEF blue}  
       writeln('Blue объявлен, а red  - не объявлен');  
      {$ELSE no blue}  
      writeln('Ни red, ни blue не объявлены'); 
      {$ENDIF blue}  
    {$ENDIF red}
    

Такие вложенные условные выражения, написанные в приведенном выше синтаксисе, могут стать очень путанными и нечитаемыми. К счастью, мы можем упростить это, используя `{$ELSEIF}`. Код ниже является расширенным эквивалентом первого примера: 
    
    
    {$IF Defined(red)}  
      writeln('Red объявлен');  
    {$ELSEIF Defined(blue)}  
      writeln('Blue объявлен');  
    {$ELSEIF Defined(green)}  
      writeln('Green объявлен');   
    {$ELSE}
      writeln('Ни red, ни blue, ни green не объявлены. Должен быть black...или что-нибудь еще...');
    {$ENDIF}
    

Как вы можете видеть, это гораздо более читабельно. 

## $ifopt

С помощью `{$IFOPT}` мы можем проверить, задана ли определенная опция компиляции. 

Из руководства по программированию: 
    
    
     {$IFOPT switch} скомпилирует текст, который следует за ним, если переключатель switch 
     в данный момент будет находиться в указанном состоянии. Если он не находится в указанном состоянии, 
     то компиляция продолжится после соответствующей директивы {$ELSE} или {$ENDIF}.
    

Например: 
    
    
     {$IFOPT M+}  
       Writeln('Compiled with type information');  
     {$ENDIF}
    

Скомпилирует оператор Writeln, только если включена генерация информации о типе. 

[![Note-icon.png](https://wiki.freepascal.org/images/b/be/Note-icon.png)](</File:Note-icon.png>)

**Примечание:** директива `{$IFOPT}` принимает только короткие опции, т.е. `{$IFOPT TYPEINFO}` не будет принята.

Обычно этот пример используется для проверки, задан ли режим DEBUG: 
    
    
    {$IFOPT D+}{$NOTE debug mode is active}{$ENDIF}
    

Такие определения также могут находиться в конфигурационных файлах, таких как fpc.cfg, которые также содержат полное объяснение того, как использовать: 
    
    
    # ----------------------
    # Defines (preprocessor)
    # ----------------------
    #
    # nested #IFNDEF, #IFDEF, #ENDIF, #ELSE, #DEFINE, #UNDEF are allowed
    #
    # -d is the same as #DEFINE
    # -u is the same as #UNDEF
    #
    #
    # Some examples (for switches see below, and the -? help pages)
    #
    # Try compiling with the -dRELEASE or -dDEBUG on the command line
    #
    # For a release compile with optimizes and strip debug info
    #IFDEF RELEASE
      -O2
      -Xs
      #WRITE Compiling Release Version
    #ENDIF
    

# Как не нужно делать

Что не так с этим кодом? Вы можете это заметить? 
    
    
    var
      MyFilesize:
      {$ifdef Win32} 
        Cardinal 
      {$else}
        int64
      {$endif}
    

Вот ответ: 

  * Free Pascal компилируется для большего количества типов процессоров, чем [32](<../en/32_bit.md> "32 bit") и [64 bit](<../en/64_bit.md> "64 bit"), например, для 8 и 16 бит.
  * на большинстве 64-битных платформ максимальный размер файла - это [QWord](<../en/QWord.md> "QWord"), а не [Int64](<../en/Int64.md> "Int64").



Этот программист попал в ловушку, которая распространена: если вы используете объявление, убедитесь, что ваша логика надежна. В противном случае такой код может легко вызвать несчастные случаи. Компилятор не будет ловить ваши логические ошибки! 

Всегда хорошо осознавать такие вещи, тем более что такие вещи легко исправить. 
    
    
    var
      MyFilesize:
      {$if defined(Win32)} 
        Cardinal 
      {$elseif defined(Win64)}
        Qword;
      {$else}
         {$error этот код написан для win32 или win64}
      {$endif}
    

Помимо этого, есть решение для этого конкретного примера, которое вообще не использует условные выражения: 
    
    
    var
      MyFilesize:NativeUint;
    

Что не так с этим кодом? Вы можете это заметить? 
    
    
      
    {$IFDEF BLUE AND $IFDEF RED} Form1.Color := clYellow; {$ENDIF}
    {$IFNDEF RED AND $IFNDEF BLUE} Form1.Color := clAqua; {$ENDIF}
    

Вот ответ: 

  * Ну, я уже написал **комментарий** , который предупредил вас .. так что посмотрите на предупреждение .... Вы должны быть в состоянии определить это ...
  * Директивы компилятора переопределяют компилятор ... будьте осторожны с этим [топором Юджина](<https://ru.wikipedia.org/wiki/Careful_with_That_Axe,_Eugene>).

Directives, definitions and conditionals definitions   
---  
[global compiler directives](<../en/global_compiler_directives.md> "global compiler directives") • [local compiler directives](<../en/local_compiler_directives.md> "local compiler directives")  
[Conditional Compiler Options](<../en/Conditional_Compiler_Options.md> "Conditional Compiler Options") • [Conditional compilation](<../en/Conditional_compilation.md> "Conditional compilation") • [Macros and Conditionals](<../en/Macros_and_Conditionals.md> "Macros and Conditionals") • [Platform defines](<../en/Platform_defines.md> "Platform defines")  
[$IF](<../en/$IF.md> "$IF")  
  
  
****

---

_Source: [https://wiki.freepascal.org/Conditional_compilation/ru](https://web.archive.org/web/20250601000000/https://wiki.freepascal.org/Conditional_compilation/ru)_
