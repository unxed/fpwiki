# FPC and Apache Modules

│ **[English (en)](<../en/FPC_and_Apache_Modules.md> "FPC and Apache Modules")** │  **русский (ru)** │    
****

## Contents

  * 1 Documentation
    * 1.1 Как работает связующее звено (How the bindings work)
    * 1.2 Как связующее звено было создано (How the bindings were created)
    * 1.3 Возможные проблемы
    * 1.4 Отладка модуля
    * 1.5 Advantages of writing a Apache module on Free Pascal
    * 1.6 Different Apache versions
  * 2 Screenshot
  * 3 Authors
  * 4 License
  * 5 Download
  * 6 Hello World Module
  * 7 Subversion
  * 8 Bug Reporting
  * 9 Change Log
  * 10 Help
  * 11 External Links



### Documentation

Содержимое книги "Writing Apache Modules with Perl and C" доступно онлайн: 

  
~~<http://162.105.203.19/apache-doc/1.htm>~~

<http://www.unix.org.ua/orelly/apache_mod/1.htm> (или в Google Books: [<http://books.google.com.br/books?id=qyzTI_eAeHUC>]) 

  
_It´s a great reference. The preface and the first chapter are very good to understand what you are doing, and the C API is the same utilized by Free Pascal Apache modules._

Это великое руководство. Предисловие и ппервая часть очень хороши для понимания того, что вы делаете, и C API также используются Free Pascal Apache модулями. 

_In this section you will find a quick start guide, examples and technical details and other things to make the life of someone writting an apache module with pascal easier, but to understand the complete theory, take a look at the book above._

В этой секции вы найдете руководство быстрого старта, примеры, технические детали и другие вещи, которые делают написание модулей Apache на паскале более легким, но чтобы понять теорию целиком, ознакомьтесь с книгой, предложенной выше. 

  
To quick understand how to setup a project to compile as a Apache module, please take a look at the [Hello World Module](<../en/FPC_and_Apache_Modules.md> "FPC and Apache Modules") section. 

Для быстрого понимания как настроить проект для компиляции в модуль Apache, пожалуйста ознакомьтесь с секцией [Hello World Module](<../en/FPC_and_Apache_Modules.md> "FPC and Apache Modules"). 

#### Как работает связующее звено (How the bindings work)

Базовый модуль Apache созданный в Free Pascal будет иметь код, похожий на этот: 
    
    
    library mod_hello;
    
    uses httpd;
    
    var
     hello_module: module; {$ifdef Unix} public name 'hello_module'; {$endif}
     default_module_ptr: Pmodule;
    
    {$ifdef Win32}
    exports
     hello_module name 'hello_module';
    {$endif}
    
    begin
      default_module_ptr := @hello_module;
      FillChar(default_module_ptr^, SizeOf(default_module_ptr^), 0);
      with default_module_ptr^ do
      begin
        version := MODULE_MAGIC_NUMBER_MAJOR;
        minor_version := MODULE_MAGIC_NUMBER_MINOR;
        module_index := -1;
        name := 'mod_hello.so';
        magic := MODULE_MAGIC_COOKIE;
      end;
    end.
    

_Apache uses a non-standard way to exchange information between the module and the server. Normally libraries export functions, but Apache instead expects a library that exports a variable. The variable is a structure with the module information. This variable needs to be filled with information as soon as the module is loaded._

Apache использует нестандартный способ обмена информацией между модулем и самим сервером. Обычно библиотеки экспортируют функции, но Apache ожидает, что библиотека экспортирует переменную. Эта переменная - это структура с информацией о модуле. Эта переменная нуждается в наполенении информацией как только модуль загрузится. 

_Now, on Windows we can easely export a variable with Free Pascal. Just put it on the exports section. On Linux, Free Pascal doesn´t yet support exporting variables, so we need a alternative, and this can be done by this pascal code:_

Сейчас в Windows мы можем легко экспортировать переменные в Free Pascal. Достаточно положить их в секцию exports. В линуксе Free Pascal не поддерживает экспорт переменных, таким образом мы нуждаемся в альтернативе, и это может быть сделано этим pascal кодом: 
    
    
    var
      hello_module : module; {$ifdef unix} public name 'hello_module'; {$endif}
    

_Another way to work around this we can create a assembler file that will export the variable and then link it into our code._

Другой способ работы с этой ситуацией: мы можем создать файл assembler'а, который будет экспортировать переменную и затем связать это с нашим кодом. 

_The file will look like this:_

Файл будет выглядеть как-то так: 
    
    
    [SECTION .data]
    global hello_module
    hello_module dd 0
    

_The command line to compile this assembler code is: "nasm -f elf hello_module.asm"_

Командная строка для компиляции этого assembler кода такова: "nasm -f elf hello_module.asm" 

_And the variable declaration would be:_

А объявление переменной таково: 
    
    
    var
      hello_module : module; {$ifdef unix} cvar; external; {$endif}
    

#### Как связующее звено было создано (How the bindings were created)

_The translated headers follow some simple guidelines. First all translated declarations remain at the exact same position as they were on the .h files, unless there is a incompatibility and they have to be moved. This is to make it easier to compare the .h files with the pascal translation and find possible mistakes on translation. Second, most files become include files, and some units are created to hold them. In particular, one unit was created for each apache library (httpd, apr, aprutil and apriconv)._

Трансляционные заголовки выполняют несколько простых нормативов. Во-первых, все трансляционные объявления оставляют точно ту же позицию как они были в .h файлах, если они не несовместимы и должны быть перемещены. Это так, чтобы сделать более простым сравнение .h файлов с pascal трансляцией и нахождение возможных ошибок при трансляции. Во-вторых, множество файлов становятся include файлами и несколько unit'ов создается, чтобы владеть ими. 

_The translation was all done manually with the help of "Replace All" from the Lazarus IDE. This is because automatic translators cannot deal with the heavy use of macros by apache headers, and passing the c preprocessor produces a code that is unrecognizably different from the original headers._

Трансляция была вся полностью сделана вручную с помощью "Replace All" в Lazarus IDE. Так случилась, потому что автоматические трансляторы не могут иметь дело с огромным применением макросов заголовками Apache и прохождение C-препроцессора генерирует код, нераспозноваемо отличающийся от исходных заголовков. 

_The Apache headers heavily rely on macros to work. In fact, almost every single declaration is a macro. Below are some of the most common macros and appropriate translations:_

Заголовки Apache очень сильно зависят от работы макросов. Фактически едва ли не каждое отдельное определение - это макрос. Ниже приведены наиболее частые макросы и соответсвующие трансляции: 

  
1 - **AP_DECLARE**
    
    
    AP_DECLARE(void) ap_add_version_component(apr_pool_t *pconf, const char *component);
    
    
    procedure ap_add_version_component(pconf: Papr_pool_t; const component: PChar);
     {$IFDEF WINDOWS} stdcall; {$ELSE} cdecl; {$ENDIF}
     external LibHTTPD name LibNamePrefix + 'ap_add_version_component' + LibSuff8;
    

_The AP_DECLARE macro says that the calling convention for the function is stdcall on Windows and cdecl on other operating systems, so we need an IFDEF for this. It also says that the function name will have a prefix and a suffix on Windows. The prefix is "_" and the suffix is "@N", where N is a number multiple of 4. Typically, the number is 4 times the number of parameters on the function, but there are some exceptions. To find out possible conflicts on the function names, a software to list all exported functions of a DLL was created, and later transformed into a Lazarus example called[Libview](<../en/Libview.md> "Libview")._

AP_DECLARE макрос говорит, что соглашение вызова функции - stdcall в Windows и cdecl в других оперционных системах, для этого нам потребовалась конструкция IFDEF. Он также говориит, что имя функции будет иметь префикс и суффикс в Windows. Префикс "_" и суффикс "@N", где N - это число, кратное 4-м. Обычно, номер - это 4-хкратное число параметров функции, но есть несколько исключений. Чтобы найти возможные конфликты имен функций, было создано специальное программное обеспечение, чтобы составить список всех экспортируемых функций в создаваемой DLL, и позже оно было превращено в пример на Lazarus, названный [Libview](<../en/Libview.md> "Libview"). /*Прим. переводчика: суффикс нужен для того, чтобы дать разные имена функциям, имеющим одинаковое имя, но разные параметры ([перегруженные функции](<http://ru.wikipedia.org/wiki/Перегрузка_функций>))*/ 

2 - **AP_DECLARE_NONSTD**
    
    
    AP_DECLARE_NONSTD(const char *) ap_set_string_slot(cmd_parms *cmd, 
                                                       void *struct_ptr,
                                                       const char *arg);
    
    
    function ap_set_string_slot(cmd: Pcmd_parms; struct_ptr: Pointer; const arg: PChar): PChar;
     cdecl; external LibHTTPD name 'ap_set_string_slot';
    

_This is the same as AP_DECLARE, but the calling convention is cdecl and no suffix or prefix is present on the function name._

Это такойже пример, как и AP_DECLARE, но соглашение вызова - cdecl, без суффикса и префикса в имени функции. 

3 - **Другие комбинации**
    
    
    APR_DECLARE_NONSTD(int) apr_file_printf(apr_file_t *fptr, 
                                            const char *format, ...)
    
    
    function apr_file_printf(fptr: Papr_file_t; const format: PChar;
     others: array of const): Integer;
     cdecl; external LibAPR name 'apr_file_printf';
    

_Another possible combination are macros starting with APR. AP means libhttpd, APR is libapr, APU is libaprutil and API is for libapriconv. Also notice that this function has a variable number of parameters, that in pascal is represented by an array of const._

Другая возможная комбинация - это макрос, начинающийся с "[APR](<http://apr.apache.org/>)" (**APR** _DECLARE_NONSTD). AP означает libhttpd, APR - это libapr, APU - libaprutil, а API для libapriconv. Также замечу, что эта функция имеет переменное число параметров, что в pascal'е отражено как **array of const**. 

4 - **AP_DECLARE_HOOK**
    
    
    AP_DECLARE_HOOK(int,pre_connection,(conn_rec *c, void *csd))
    
    type
      ap_HOOK_pre_connection_t = function (c: Pconn_rec; csd: Pointer): Integer; cdecl;
    
    procedure ap_hook_pre_connection(pf: ap_HOOK_pre_connection_t; const aszPre: PPChar;
     const aszSucc: PPChar; nOrder: Integer);
     {$IFDEF WINDOWS} stdcall; {$ELSE} cdecl; {$ENDIF}
     external LibHTTPD name LibNamePrefix + 'ap_hook_pre_connection' + LibSuff16;
    

_The hooks use one of the most strange declarations possible. All hook are represent a function like ap_hook_pre_connection, with the same 4 parameters, and no return. The difference between them is the first parameter, that has a different function type in each hook function._

Хуки ([hook](<http://en.wikipedia.org/wiki/Hook_\(programming\)>)) используют одно из наиболее странных запутанных возможных объявлений. Любой хук - это представление функции, подобной ap_hook_pre_connection с 4-мя оданаковыми параметрами и ничего не возвращающей, а разница между хуками - это первый параметр, который имеет разные типы - функции в каждой хук-функции. 

_AP_DECLARE_HOOK macro receives 3 parameters. The first is the return type of the hook function type. The second plus the prefix ap_hook_ form the name of the function, and the third are the parameters of the hook function._

Макрос AP_DECLARE_HOOK получает три параметра. Первый - это тип возвращаемого значения типом хук-функции. Второй параметр, приложенный к ap_hook_, формирует имя функции. Третий - это пареметры хук-функции. /*Прим. переводчика: к сожалению автору перевода пока что не удалось понять что является хук-фукнцией, типом хук-функции и хуком в данном случае, что отразилось на способности понятно передать смысл текста на русском языке*/ 

#### Возможные проблемы

1 - **Apache version number**

_Apache expects that the module is compiled specifically for the exact same version as the server. Because we have a single translation for all 2.0.x versions, you can just change the MODULE_MAGIC_NUMBER_MAJOR to what Apache expects and it should work. On the file ap_mmn.inc you can find a list of apache magic numbers for almost all 2.0.x Apache versions._

Apache ожидает, что модуль скопилирован именно под туже саую версию, что и сервер. Т.к. мы имеем единую трансляцию для всех 2.0.x версий, вы можете всего лишь изменить MODULE_MAGIC_NUMBER_MAJOR на тот, что ожидает Apache и это должно сработать. В файле ap_mmn.inc вы можете найти список apache magic номеров для всех 2.0.x Apache версий. 

2 - **Using string functions**
    
    
    note := apr_pstrcat(p, [PChar('x_child_init('), sname, PChar(')'), nil]);
    

Be careful when using apr_pstrcat and other string function. Note that the unlike c, you have to enclose the extra parameters with a [ ]. Also note that the extra parameters are untyped, so your code may compile, but crash the server if you use pascal strings. To avoid this, make sure to cast all strings to PChar when using c string functions. 

Будте осторожны при использовании apr_pstrcat и других функций по работе со строками. Заметьте, что в отличии от C, вы должны вложить дополнительные параметры в скобки [ ]. Также заметьте, что дополнительные параметры не типизированы, так ваш код может быть скомпилирован, но убить сервер, если вы используете pascal строки. Чтобы избежать этой ситуации, обеспечьте преобразование всех строк к PChar, когда используете функции по работе со строками языка C. 

3 - **Загрузка двух или более pascal модулей Apache**

By default, Free Pascal (actually, the linker used by Free Pascal) creates libraries that are not relocatable. This means that if two Free Pascal generated libraries are loaded by a program, there will be a conflict, and the second library will fail to be loaded. 

По умолчанию Free Pascal (именно линкер, используемый Free Pascal'ем) создает непермещаемые (not relocatable) библиотеки. Это означает, что, если две библиотеки, сгенерированные в Free Pascal будут загружены программой, произойдет конфликт, и вторая библиотека не сможет быть загруженной. 

To fix this you must pass the -WR option to the compiler. On Lazarus you can add this option on "Compiler Options" -> "Other" -> "Custom Options". Using Lazarus 0.9.16 and FPC 2.0.2 that comes with it on Windows XP, even passing -WR option the libraries will fail to load at the same time. Using Free Pascal 2.0.2 installed separately from Lazarus, and compiling from the command line, the libraries will work correctly. This problem may be solved with the release of 2.0.4, or not. 

Чтобы это исправить вы должны послать -WR опцию компилятору. В Lazarus вы можете добавить эту опцию в "Compiler Options" -> "Other" -> "Custom Options". Использование Lazarus 0.9.16 и FPC 2.0.2 (которая идет с ним) в Windows XP даже с -WR опцией не даст загрузить библиотеки одновременно. Используя Free Pascal 2.0.2, установленного отдельно от Lazarus и компилируя из командной строки, библиотеки будут работать корректно. Эта проблема возможно будет решена в релизе 2.0.4, а может и нет. 

There is more detailed information [here](<http://www.freepascal.org/docs-html/prog/progse53.html#x226-22800012.5>). 

[Здесь](<http://www.freepascal.org/docs-html/prog/progse53.html#x226-22800012.5>) есть более детальная информация. 

#### Отладка модуля

_If there are problems on your module that make Apache crash, or fail to start, it won´t produce any error.log. To debug this problems, run apache from the command line using the Gnu Debugger (gdb)._

Если в вашем модуле есть проблема, приводящая к падению сервера Apahce или сбою при старте, то никакой error.log не будет сгенерирован. Для отладки этой проблемы запустите Apache из командной строки используя [Gnu Debugger](<http://www.gnu.org/software/gdb/>) (gdb) 

_First, start gdb pointing to the executable. The command is "gdb Apache.exe" on Windows. And then type the command "run" on gdb console. Now Apache will show error messages on the terminal._

Во-первых запустите gdb указывая на исполняемый файл. В Windows это команда "gdb Apache.exe". И потом введите команду "run" в gdb консоль. Теперь сервер Apache будет показывать сообщения об ошибках в терминал. 

#### Advantages of writing a Apache module on Free Pascal

This section presents reasons to write modules on pascal instead of c, for example. On the famous "Writing Apache modules with perl and c" book there is a entire chapter dedicated only to the build process of a c module with more then one source file. In fact, the compile process is so complex, that a helper program called apxs is utilized. 

  
If your module is written on Pascal the process is much easier. Want to compile the module? Just hit CTRL+F9 on Lazarus ide. Multiple source files? Fine, just put it on the uses clause. This allows the programmer to focus on module logic instead of understanding the details of a makefile and apxs software, and thus gain productivity. 

#### Different Apache versions

Using this headers, it is possible to have a single source code recompiled for Apache 1.3, 2.0 or 2.2. A module compiled for 2.0.x will work on all versions that start with 2.0, but won´t work on 2.2 or 1.3 apaches. 

  
Apache 1.3 is much older then 2.0, and has many incompatibilities. It is still possible to have the same source, but many IFDEFs will be necessary to work around the differences of these versions. It is much easier to write a module that works for apache 2.0 and 2.2, as their APIs have only minor differences. 

  
There is a tutorial [here](<http://www.ddj.com/184405150>) about the differences between Apache 1.3 and 2.0. 

### Screenshot

### Authors

[Felipe Monteiro de Carvalho](</User:Sekelsenmat> "User:Sekelsenmat")

  
**Special thanks to:**

  
Giovanni Premuda for making this possible through the [Lazarus Bounties](<../en/Bounties.md> "Bounties"). 

  
Moacir Schmidt for helping me understand how a apache module works. 

### License

The same license as the apache c headers: 

Licensed under the Apache License, Version 2.0 (the "License"); you may not use this file except in compliance with the License. You may obtain a copy of the License at 

<http://www.apache.org/licenses/LICENSE-2.0>

Unless required by applicable law or agreed to in writing, software distributed under the License is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied. See the License for the specific language governing permissions and limitations under the License. 

### Download

The source is included with FPC >= 2.2.0 at fpcsrc/packages/base/httpd 

An old snapshot is also available here: [http://sourceforge.net/project/showfiles.php?group_id=92177&package_id=197409](<http://sourceforge.net/project/showfiles.php?group_id=92177&package_id=197409>)

Status: Stable 

### Hello World Module

This is an example apache module that will write an internet page when the user enters in a specific directory on the server. Follow the instructions bellow to compile and setup the module. 

  
1 - To set the build environment for this module you can either create a new library or open the example project that comes with the bindings. If it´s a new project, don´t forget to to add all apache folders to your "Other Unit Files" and "Other sources" on the "Compiler Options" dialog on the Lazarus IDE, or add them on the command line. 

For Apache 2.0, the folders are: 
    
    
    httpd_2_0/
    httpd_2_0/apr/
    httpd_2_0/apriconv/
    httpd_2_0/aprutil/
    

For Apache 2.2, they are: 
    
    
    httpd_2_2/
    httpd_2_2/apr/
    httpd_2_2/apriconv/
    httpd_2_2/aprutil/
    

And for Apache 1.3, they are: 
    
    
    httpd_1_3/
    httpd_1_3/apr/
    

Of course you will only add 1 set of directories, for the target version. 

Also, on the "Project Options" dialog, set the target file name to mod_hello.so 

  
2 - Next compile the module and copy mod_hello.so file to the modules folder of your Apache installation. 

  
3 - Open httpd.conf file of your server and add the following lines on a suitable place: 

  

    
    
    LoadModule test_module modules/mod_hello.so
    
    <Location /pmod>
        SetHandler testapache-handler
    </Location>
    

  
4 - Restart your Apache server, and use a web browser to access the location: myserver/pmod 

  
In the case of a local server this will be: localhost/pmod 

If you see a webpage on this location, then the module is working perfectly. 
    
    
    {*******************************************************************
    *  Test library of the Apache Pascal Headers
    *******************************************************************}
    library mod_hello;
    
    {*******************************************************************
    *  The mode must be objfpc on this unit because the unix code uses
    * some extensions introduced on Free Pascal
    *******************************************************************}
    {$ifdef fpc}
      {$mode objfpc}{$H+}
    {$endif}
    
    {$IFDEF WIN32}
      {$DEFINE WINDOWS}
    {$ENDIF}
    
    {$define Apache2_0}
    
    uses SysUtils, httpd {$ifndef Apache1_3}, apr{$endif};
    
    var
     test_module: module; {$ifdef Unix} public name 'test_module'; {$endif}
     default_module_ptr: Pmodule;
    
    const
      MODULE_NAME = 'mod_hello.so';
      
    {*******************************************************************
    *  Free Pascal only supports exporting variables on Windows
    *******************************************************************}
    {$ifdef WINDOWS}
    exports
     test_module name 'test_module';
    {$endif}
    
    {*******************************************************************
    *  Handles apache requests
    *******************************************************************}
    function DefaultHandler(r: Prequest_rec): Integer; cdecl;
    var
      RequestedHandler: string;
    begin
      RequestedHandler := r^.handler;
    
      { We decline to handle a request if hello-handler is not the value of r->handler }
      if not SameText(RequestedHandler, 'testapache-handler') then
      begin
        Result := DECLINED;
        Exit;
      end;
    
      { The following line just prints a message to the errorlog }
      ap_log_error(MODULE_NAME, 54, APLOG_NOERRNO or APLOG_NOTICE,
       {$ifndef Apache1_3}0,{$endif} r^.server,
       'mod_hello: %s', [PChar('Before content is output')]);
    
      { We set the content type before doing anything else }
      {$ifdef Apache1_3}
        r^.content_type := 'text/html';
    //    ap_send_http_header(r);
      {$else}
        ap_set_content_type(r, 'text/html');
      {$endif}
      
      { If the request is for a header only, and not a request for
       the whole content, then return OK now. We don't have to do
       anything else. }
      if (r^.header_only <> 0) then
      begin
        Result := OK;
        Exit;
      end;
    
      { Now we just print the contents of the document using the
       ap_rputs and ap_rprintf functions. More information about
       the use of these can be found in http_protocol.inc }
      ap_rputs('<HTML>' + LineEnding, r);
      ap_rputs('<HEAD>' + LineEnding, r);
      ap_rputs('<TITLE>Hello There</TITLE>' + LineEnding, r);
      ap_rputs('</HEAD>' + LineEnding, r);
      ap_rputs('<BODY BGCOLOR="#FFFFFF">' + LineEnding ,r);
      ap_rputs('<H1>Hello world</H1>' + LineEnding, r);
      ap_rputs('This is the first Apache Module working with the new binding from Free Pascal' + LineEnding, r);
    //  ap_rprintf(r, '<br>A sample line generated by ap_rprintf<br>' + LineEnding, []);
      ap_rputs('</BODY></HTML>' + LineEnding, r);
    
      { We can either return OK or DECLINED at this point. If we return
             * OK, then no other modules will attempt to process this request }
      Result := OK;
    end;
    
    {*******************************************************************
    *  Registers the hooks
    *******************************************************************}
    {$ifdef apache1_3}
    
    procedure hw_init(s: PServer_rec; p: PPool); cdecl;
    begin
    end;
    
    var
      hw_handlers: array[0..0] of handler_rec =
      (
        (content_type: 'hw_app'; handler: @DefaultHandler)
      );
    
    {$else}
    
    procedure RegisterHooks(p: Papr_pool_t); cdecl;
    begin
      ap_hook_handler(@DefaultHandler, nil, nil, APR_HOOK_MIDDLE);
    end;
    
    {$endif}
    
    {*******************************************************************
    *  Library initialization code
    *******************************************************************}
    begin
      default_module_ptr := @test_module;
      FillChar(default_module_ptr^, SizeOf(default_module_ptr^), 0);
    
      {$ifdef apache1_3}
        STANDARD_MODULE_STUFF(test_module);
    
        with test_module do
        begin
          name := MODULE_NAME;
          init := @hw_init;
          handlers := hw_handlers;
        end;
      {$else}
        STANDARD20_MODULE_STUFF(test_module);
    
        with test_module do
        begin
          name := MODULE_NAME;
          register_hooks := @RegisterHooks;
        end;
      {$endif}
    end.
    

### Subversion

You can download the subversion version of this project using this command: 

svn checkout <http://svn.freepascal.org/svn/fpc/trunk/packages/base/httpd> httpd 

You can also download the full fpc 2.1.1 repository and it will be included. 

### Bug Reporting

Tests are necessary to verify if all functions and structures are declared properly. Tests were done on Windows and Linux operating systems. It is necessary to test if the modules work correctly in other operating system. 

You can post Bug Reports here: 

**Bug 1**

  * in version 0.2 in declaration of procedure ap_log_error external function name is ap_log_perror, for procedure ap_log_perror external function name ap_log_error, aren't this two swaped?
  * after swaping, using apache 2.0.59, windows xp sp2, mod_hello.lpr didn't work, error.log showed: "mod_hello: \x06" and then "Parent: child process exited with status 2147483649 -- Restarting.", if I commented out the line that calls ap_log_error() it worked. Oddly I could make it work by doing ap_log_error() cdecl instead of stdcall.



    I can reproduce the behavior here. Strangely on Apache source code that function do is declared as stdcall on Windows. Also strange that ap_log_perror worked as stdcall and with a wrong parameter. Well, maybe the headers are wrong somehow. A fix for this will be on the next version scheduled for next week. --[Sekelsenmat](</User:Sekelsenmat> "User:Sekelsenmat") 16:07, 4 September 2006 (CEST)

    Apache module have invalid pointer to handle in structure request_req (request_rec.handler) under Ubuntu Linux. Problem solved than apr_off_t was changed type from Integer to Int64. (version 0.3 Apache 2.2.3)

### Change Log

  * 16.07.06 Apache headers version 0.1 released


  1. httpd 2.0.58 almost fully translated.
  2. mod_example module partially translated to pascal
  3. Tested only on Windows


  * 24.07.06 version 0.2 released


  1. mod_example fully translated to pascal
  2. mod_speling translated to pascal
  3. Many small fixes, and more headers translated
  4. Now tested and working on both Windows and Linux


  * 11.10.06 version 0.3 released


  1. httpd 2.2.3 headers fully translated and working
  2. minor fixes on the 2.0.58 headers
  3. Partially translated apache 1.3.37 headers



### Help

Please send help requests to the Free Pascal mailling list or on apache-modules mailling list. 

### External Links

  * Apache mailling lists: <http://httpd.apache.org/lists.html>


  * The Apache API documentation: <http://httpd.apache.org/docs/>


  * Apache page about modules: <http://modules.apache.org/>


  * Tutorials about writting input and output filters with Apache modules: <http://www.onlamp.com/pub/ct/38>


  * The 3rd Toolbox maganize from 2007 contains an article in german about FPC and Apache Modules: <http://www.toolbox-mag.de/archiv2007.html>

---

_Source: [https://wiki.freepascal.org/FPC_and_Apache_Modules/ru](https://web.archive.org/web/20250301000000/https://wiki.freepascal.org/FPC_and_Apache_Modules/ru)_
