# leakview

│ **[English (en)](<../en/leakview.md> "leakview")** │  **русский (ru)** │    
****

**Leakview** позволяет быстро перемещаться по отчетам об утечках [HeapTrc](<../en/heaptrc.md> "heaptrc"). 

[![leakview.png](https://wiki.freepascal.org/images/9/94/leakview.png)](</File:leakview.png>)

## Использование

Утилита Leakview доступна в среде IDE в разделе _View / Leaks and Traces_ (Вид / Утечки памяти и стек вызовов). 

Leakview читает вывод [heaptrc](<../en/heaptrc.md> "heaptrc"). Чтобы это работало, вам нужно включить heaptrc в вашем коде:   
**Никогда не включайте heaptrc в свой раздел uses вручную. Это делается компилятором неявно, если указано -gh**  


### Включение heaptrc в Lazarus

Чтобы включить это в вашем проекте Lazarus: перейдите в Project Options/Compiler Options/Debugging (Параметры проекта/Параметры компилятора/Отладка) и в разделе Other debugging info (Прочая отладочная информация) включите _Use Heaptrc unit (check for mem-leaks) (-gh)_ (Использовать модуль Heaptrc (проверка на наличие утечек памяти) (-gh)) 

Чтобы получить значимые результаты heaptrc с описанием строк кода, а не только с адресами ассемблера, перейдите в Tools/Options/Debugger/general (Сервис/Параметры/Отладчик/Общие) и задайте отладчик. 

Затем вы можете позволить программе записывать вывод модуля heaptrc в файл. Добавьте следующие фрагменты кода в свой .lpr в начале кода, чтобы перенаправить вывод heaptrc в файл: 
    
    
      {$DEFINE debug}     //сделайте это здесь, или вы можете определить -dDEBUG в Project Options/Compiler Options/Custom Options (Параметры проекта/Параметры компилятора/Параметры пользователя), то есть в режиме сборки, чтобы вы могли настроить отладку с утечкой и режим сборки по умолчанию без него
    uses
      ...
      {$IFDEF debug}
      , SysUtils
      {$ENDIF}
    
    ...
    
    begin
      {$IFDEF DEBUG} 
      // Предположим, что ваш режим сборки задан как -dDEBUG в Project Options/Compiler Options/Custom Options при определении -gh
      // Это позволяет избежать помех при запуске неотладочной сборки/сборки по умолчанию без -gh
    
      // Задаем -gh вывод для пакета Leakview:
      if FileExists('heap.trc') then
        DeleteFile('heap.trc');
      SetHeapTraceOutput('heap.trc');
      {$ENDIF DEBUG}
    
      ...
    end.
    

### Включение heaptrc в FPC

В FPC вы можете указать -gh в параметрах компилятора в fpc.cfg или в командной строке.  
Вы можете проверить, скомпилирован ли ваш двоичный файл с помощью heaptrace, следующим образом: 
    
    
    {$if Declared(UseHeapTrace)}...{$ifend}
    

Затем перенаправьте вывод heaptrc в файл (вместо стандартного вывода). Вы можете использовать код, аналогичный коду Lazarus, или, в качестве альтернативы, установить переменную среды, например на *nix: 
    
    
    export HEAPTRC="log=heap.trc"
    

или Windows: 
    
    
    set HEAPTRC="log=heap.trc"

---

_Source: [https://wiki.freepascal.org/leakview/ru](https://web.archive.org/web/20230206023536/https://wiki.freepascal.org/leakview/ru)_
