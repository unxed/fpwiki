# Creating bindings for C libraries

│ **[English (en)](<../en/Creating_bindings_for_C_libraries.md>)** │  **русский (ru)** │

## Contents

  * 1 Введение
  * 2 H2PAS
    * 2.1 Обзор
    * 2.2 Процесс работы
    * 2.3 Установка инструментов
    * 2.4 Получение заголовочных файлов C
      * 2.4.1 Пример: MPICH2
    * 2.5 Создание папки для работы, выбор имени
      * 2.5.1 Пример: MPICH2
    * 2.6 Создание проекта h2pas с помощью мастера
      * 2.6.1 Пример: MPICH2
    * 2.7 Add the .h files to the h2pas-project
      * 2.7.1 Example: MPICH2
    * 2.8 Setup the h2pas options
      * 2.8.1 Example: MPICH2
    * 2.9 Run the wizard
      * 2.9.1 Example: MPICH2
    * 2.10 Writing your own converter tools
      * 2.10.1 Using the tool "Search and replace"
      * 2.10.2 Example: rename an identifier Tguint to guint
      * 2.10.3 Example: rename several identifiers
      * 2.10.4 Example: MPICH2
        * 2.10.4.1 Removing Makro MPI_DUP_FN
        * 2.10.4.2 Removing typecasts to MPI_DUP_FN
        * 2.10.4.3 Removing functions MPIO_Test, PMPIO_Test, MPIO_Wait and PMPIO_Wait
    * 2.11 Improving an existing tool
    * 2.12 Writing a custom tool
    * 2.13 Future work / Missing stuff
  * 3 Convert C to Pascal with lua script
  * 4 Manual conversion of C headers to Pascal
    * 4.1 Converting constants
    * 4.2 Converting exported routines
  * 5 Publish your bindings on Lazarus-CCR or Free Pascal
  * 6 See also



# Введение

Эта статья описывает, как подключить и использовать С-библиотеки в языке Паскаль. Для использования библиотек, написанных на языке С вы должны преобразовать все типы данных, функции и переменные в библиотеках в соответствии стандартам языка Паскаль. Выполнить данную операцию можно используя утилиту h2pas (или аналогичное ПО), либо вручную, переписав код. 

# H2PAS

## Обзор

[H2Pas](<H2Pas.md> "H2Pas/ru") это инструмент, который автоматически переводит C библиотеки. GUI для Lazarus, использующее h2pas и другие инструменты автоматизируют процесс. Он помогает создать набор правил, которые можно использовать чтобы обновить порт, отчего после обновления C библиотеки перевести ее будет намного проще. Приятной особенностью является то, что Мастер h2pas использует временные файлы, поэтому файлы заголовков остаются нетронутыми. 

## Процесс работы

  * Найдите C заголовки, которые вы хотите перевести.
  * Создайте папку для работы и придумайте имя для портированной библиотеки.
  * Создайте проект h2pas используя Мастер h2pas.
  * Добавьте заголовочные файлы в проект.
  * Настройте проект h2pas.
  * Запустите мастер.
  * Исправьте ошибки и запустите мастер еще раз.
  * Когда h2pas запустится без ошибок, протестируйте сборку и отформатируйте код.
  * Напишите пару программ для тестов.
  * Опубликуйте порт на lazarus-ccr или на сайте FreePascal.



## Установка инструментов

Инструмент h2pas поставляется с каждой обычной установкой fpc 

Установите пакет h2paswizard в IDE Lazarus. Откройте "Package->Install/Uninstall Packages", выберите в правом списке пакет H2PasWizard и щёлкните 'Install selection', потом - 'Save and rebuild IDE'. Перезапустите IDE и у вас появится новый пункт меню: Tools -> h2pas 

## Получение заголовочных файлов C

Заголовочный файл C имеет расширение .h и описывает интерфейс C библиотеки. Обычно их не предоставляют вместе с библиотекой. Для этого нужно скачать исходный код или пакет разработчика библиотеки. Например заголовочные файлы библиотеки gtk находятся в gtk+-devel. 

### Пример: MPICH2

Скачайте mpich2-1.0.3.tar.gz здесь: <http://www-unix.mcs.anl.gov/mpi/mpich2/> и распакуйте его. Файлы .h находятся в mpich2-1.0.3/src/include. 

Больше информации о MPICH для FPC вы можете найти [здесь](<../en/MPICH.md> "MPICH"). 

## Создание папки для работы, выбор имени

Создайте папку с подходящим названием. Имя не должно содержать специальных символов: пробелов, запятых и т.д. Скопируйте файлы .h в эту папку. 

### Пример: MPICH2

Мы будем использовать папку h2p для файлов Pascal. h2p/c_sources будет использоваться для файлов .h. 
    
    
    mkdir -p h2p/c_sources
    cp mpich2-1.0.3/src/include/*.h h2p/c_sources/
    

## Создание проекта h2pas с помощью мастера

Откройте мастер h2pas: "Tools -> h2pas". Вы сможете переключаться между окном мастера и другими окнами IDE. Ваш последний проект h2pas откроется автоматически. Чтобы создать новый проект, нажмите "Settings -> New/Clear settings". Потом нажмите на кнопку "Save settings" и выберете имя файла. 

Заметка: Если вы открываете мастер в первый раз, то вам нужно будет в меню настроек задать путь к h2pas.exe. Конвертер h2pas находится там-же где и компилятор FreePascal (fpc.exe), если вы скачали FPC вместе с Lazarus, то смотрите в ...\Lazarus\fpc\_version_ \bin. Сохраните настройки чтоб не повторять те же действия снова. 

### Пример: MPICH2

Нажмите "Settings -> New/Clear settings".Потом нажмите на кнопку "Save settings" и сохраните как h2p/mpi2.h2p. 

## Add the .h files to the h2pas-project

In the "C header files" page you can add/delete the .h files. You can enable/disable .h files to convert only part of the files. 

### Example: MPICH2

Click on "C header files -> Add .h files ..." and select "mpi.h" and "mpio.h". They will be enabled automatically. Select the mpi.h file and click "Merge all but this", so that the wizard combines all header files into one unit (mpi.pas). 

## Setup the h2pas options

Under "h2pas Options" you can set the parameters for the h2pas program. 

### Example: MPICH2

  * Enable -e, -D, -p, -w and disable all others.
  * The -l library path is "mpich".
  * Output extension is ".pas"
  * Output directory is h2pas/, which is the default, so leave it empty



## Run the wizard

Click on the bottom button "Run h2pas". This will copy the <example>.h file to a temporary <example>.tmp.h file and run the tools listed under "Before h2pas". Then it runs h2pas to convert the <example>.tmp.h into <example>.inc or <example>.pas or whatever output extension you setup on the h2pas page. Then it runs the tools listed under "After h2pas" on the output files. 

If h2pas finds a syntax error, the IDE will open the example.tmp.h file and jumps to the error line. h2pas often only reports 'syntax error', which is very unspecific. See [Common problems when converting C header files](<../en/Common_problems_when_converting_C_header_files.md> "Common problems when converting C header files"). 

### Example: MPICH2

The h2pas wizard already contains all tools to convert all the specials of this header file, so h2pas runs without errors. But the created unit is not yet ready. Read further. 

Add the following to the _Undefines_ property: 
    
    
     MPI_INCLUDED
     MPIO_INCLUDE
     NEEDS_MPI_FINT
     MPI_BUILD_PROFILING
     MPICH_SUPPRESS_PROTOTYPES
     MPI_Wtime
     HAVE_MPICH2G2
     FOO
     HAVE_PRAGMA_HP_SEC_DEF
    

  
Add the following to the _Defines_ property: 
    
    
     MPICH2
     MPICH_NEW_MPIIO
    

## Writing your own converter tools

### Using the tool "Search and replace"

Many things like renaming a variable can be done by the **Search and replace** tool. Add the tool via the **Add new tool** button on either the 'Before h2pas' or the 'After h2pas' page. Then set the **SearchFor** , **ReplaceWith** , **Options** and **Caption** property. 

### Example: rename an identifier Tguint to guint

Property | Value   
---|---  
Caption | Rename Tguint to guint   
SearchFor | Tguint   
ReplaceWith | guint   
Options | [trtMatchCase,trtWholeWord]   
  
### Example: rename several identifiers

Rename Tguint to guint, Tgulong to gulong, Tgint to gint: 

Property | Value   
---|---  
Caption | Rename Tguint to guint   
SearchFor | 
    
    
    T(guint|gint|gulong)  
  
ReplaceWith | $1   
Options | [trtMatchCase,trtWholeWord,trtRegExpr]   
  
### Example: MPICH2

#### Removing Makro MPI_DUP_FN

The _MPI_DUP_FN_ is defined two times in the mpi C headers. Once as a macro to something that does not exist, and later to a real function. So, the first should be deleted. 

Add a 'Search and replace' tool to the before H2Pas tools (at the end) with the following settings: 

Property | Value   
---|---  
Caption | Remove dummy define MPI_DUP_FN   
Name | RemoveDummyDefineMPI_DUP_FN   
SearchFor | 
    
    
    ^#define\s+MPI_DUP_FN\s+MPIR_Dup_fn  
  
ReplaceWith |   
Options | [trtMatchCase,trtRegExpr]   
  
#### Removing typecasts to MPI_DUP_FN

Add a 'Search and replace' tool to the before H2Pas tools (at the end) with the following settings: 

Property | Value   
---|---  
Caption | Remove MPI_DUP_FN Makros   
Name | Remove_MPI_DUP_Func   
SearchFor | 
    
    
    ^#define .*MPI_DUP_FN\).*$  
  
ReplaceWith |   
Options | [trtMatchCase,trtRegExpr]   
  
#### Removing functions MPIO_Test, PMPIO_Test, MPIO_Wait and PMPIO_Wait

The MPIO_Test, PMPIO_Test, MPIO_Wait and PMPIO_Wait functions are defined twice and redefined via macro. So, the functions can be removed: 

Add a 'Search and replace' tool to the before H2Pas tools (at the end) with the following settings: 

Property | Value   
---|---  
Caption | 
    
    
    Remove Func P?MPIO_(Test|Wait)  
  
Name | RemoveFuncP_MPIO_Test_Wait   
SearchFor | 
    
    
    ^int P?MPIO_(Test|Wait).*$  
  
ReplaceWith |   
Options | [trtMatchCase,trtRegExpr]   
  
  


## Improving an existing tool

You found a bug and want to fix it or you want to extend one of the above tools. Great! 

Most of the above tools are defined in the h2pasconvert unit, which is part of the h2paswizard package. Basically a tool needs a classname, a description and an **Execute** method. To test/debug a tool outside the IDE and save a lot of compilation time, see the project components/simpleideintf/examples/testh2pastool.lpi. Compile it and start it on a console/terminal with the filename of a .h file as first command parameter. For example: 
    
    
    ./testh2pastool files/h2pastest.pas
    

  


## Writing a custom tool

You can write your own conversion tools and register them in the IDE easily. Start a package and a add a new unit (say unit1) for your new class. See the existing tools as example and read the prior section. When you wrote a new tool and tested it with the above simpleideintf project, then register it in the IDE: Add a **register** procedure to your unit (unit1), like the following (pseudo code): 
    
    
    uses
      Classes, ..., IDETextConverter;
    
    type
      TYourToolClass = class(TCustomTextConverterTool)
      public
        class function ClassDescription: string; override;
        function Execute(aText: TIDETextConverter): TModalResult; override;
      end;
    
    procedure Register;
    
    implementation
    
    procedure Register;
    begin
      TextConverterToolClasses.RegisterClass(TYourToolClass);
    end;
    

Do not forget to enable the **register** checkbox of the unit in the package editor, otherwise the Register procedure will not be called by the IDE. Then install your package in the IDE. 

## Future work / Missing stuff

  * A tool to convert static linked procedures to dynamic linked procedure variables
  * A tool to fix functions without parameter names by searching the .c files. This could also add a comment, in which c file it found the function.
  * Find missing identifiers and let the user check which one to comment or replace with base types.
  * Create a list of macro functions, which were half translated.



C: 
    
    
    typedef struct page
    {
       u32int present    : 1;   // Page present in memory
       u32int rw         : 1;   // Read-only if clear, readwrite if set
       u32int user       : 1;   // Supervisor level only if clear
       u32int accessed   : 1;   // Has the page been accessed since last refresh?
       u32int dirty      : 1;   // Has the page been written to since last refresh?
       u32int unused     : 7;   // Amalgamation of unused and reserved bits
       u32int frame      : 20;  // Frame address (shifted right 12 bits)
    } page_t;
    

Pascal: 
    
    
    type
        Unsigned_7  = 0 .. (1 shl 7)  - 1;
        Unsigned_20 = 0 .. (1 shl 20) - 1;
    
    type
        page_t = bitpacked record
           present  : boolean;
           rw       : boolean;
           user     : boolean;
           accessed : boolean;
           dirty    : boolean;
           unused   : Unsigned_7;
           frame    : Unsigned_20;
        end;
    

# Convert C to Pascal with lua script

I'm impressed by the power of the lua scripting language and how easy it is to do funny things with it. 
    
    
    --
    -- Please send any improvement to mingodadATgmailDOTcom.
    --
    
    inFileName = arg[1] or 'g:\\tmp\\plua\\clua\\lvm.c'
    
    fh = assert(io.open(inFileName))
    cbody = fh:read('*a')
    fh:close()
    
    pasbody = cbody
    
    function split(str, pat)
       local t = {}  -- NOTE: use {n = 0} in Lua-5.0
       local fpat = '(.-)' .. pat
       local last_end = 1
       local s, e, cap = str:find(fpat, 1)
       while s do
          if s ~= 1 or cap ~= '' then
    	table.insert(t, cap)
          end
          last_end = e+1
          s, e, cap = str:find(fpat, last_end)
       end
       if last_end <= #str then
          cap = str:sub(last_end)
          table.insert(t, cap)
       end
       return t
    end
    
    function trim(s)
      -- from PiL2 20.4
      return (s:gsub('^%s*(.-)%s*$', '%1'))
    end
    
    function trimChars(str, c1, c2)
    	return str:gsub('^%' .. c1 .. '*(.-)%' .. c2 .. '*$', '%1')
    end
    
    function getPascalType(cType)
    	if cType == 'void' then
    		return 'pointer'
    	elseif cType == 'int' then
    		return 'integer'
    	elseif cType == 'short' then
    		return 'Shortint'
    	elseif cType == 'define' then
    		return 'is_define'
    	else
    		return cType
    	end
    end
    
    
    --check var name and type
    --if var starts with a capital letter
    -- add 'T' informt of it
    --if var is preceded by * then add a 'P' for type
    
    function checkVarType(aType, aVar)
    	aType = getPascalType(aType)
    	aType = aType:gsub('^(%u)', 'T%1')
    	aVar = aVar:gsub('^(%*)', function(m)
    			aType = 'P' .. aType
    			return ''
    		end)
    	aVar = aVar:gsub('(%[[^]]+%])', function(m)
    			aType = 'array' .. m .. ' of ' .. aType
    			return ''
    		end)
    	return aType, aVar
    end
    
    function varDeclaration(aType, aVar)
    	aType, aVar = checkVarType(aType, aVar)
    	return aVar .. ' : ' .. aType
    end
    
    function checkDeclarationOrGoto(m1, m2, m3)
    	if m2 ~= 'goto' then
    		return '\n' .. m1 .. varDeclaration(m2, m3) .. ';'
    	else
    		return '\n' .. m1 .. m2 .. ' ' .. m3 .. ';'
    	end
    end
    
    function addProcFunc(mtype, mname, mparams, mbody)
    	local str, isFunc, rType, k, v
    	if mtype == 'void' then
    		str = '\nprocedure '
    		isFunc = false
    	else
    		str = '\nfunction '
    		isFunc = true
    	end
    	mtype, mname = checkVarType(mtype, mname)
    	local vparams = split(trimChars(mparams, '(',')'), ',')
    	str = str .. mname .. '('
    
    	local tparams = {}
    	for k, v in pairs(vparams) do
    		local vparam = split(trim(v), ' ')
    		if #vparam > 2 then
    			table.insert(tparams, vparam[1] .. ' ' .. varDeclaration(vparam[2], vparam[3]))
    		else
    			--print(vparam[1], vparam[2])
    			table.insert(tparams, varDeclaration(vparam[1], vparam[2] or 'is_define'))
    		end
    	end
    
    	str = str .. table.concat(tparams, '; ') .. ')'
    	if isFunc then
    		if mtype == 'int' then
    			rType = 'integer'
    		else
    			rType = mtype
    		end
    		str = str .. ':' .. rType
    	end
    
    	if mbody then
    		local tblLabels = {}
    		for v in mbody:gmatch('\n[\t ]*([%w_]+)[\t ]*:[^=]') do
    			if v ~= 'default' then
    				table.insert(tblLabels, v)
    			end
    		end
    
    		if #tblLabels > 0 then
    			str = str .. ';\nlabel\n  '.. table.concat(tblLabels, ',  ')
    		end
    
    		local declarations = {}
    
    		mbody = mbody:gsub('(\n[\t ]*)const[\t ]+([%w_]+)[\t ]+(%*?[%w_]+)[\t ]*:%=[\t ]*([^;\n]+;)','%1 %2 %3 := %4')
    
    		mbody = mbody:gsub('(\n[\t ]*)([%w_]+)[\t ]+(%*?[%w_]+)[\t ]*:%=[\t ]*([^;\n]+;)', function(m1, m2, m3, m4)
    			table.insert(declarations, varDeclaration(m2, m3) .. ';' )
    			return m1 .. m3 .. ' := ' .. m4
    		end)
    
    		mbody = mbody:gsub('\n[\t ]*const[\t ]*([%w_]+)[\t ]+(%*?[%w_]+)[\t ]*;', function(m1,m2)
    				table.insert(declarations, varDeclaration(m1, m2) .. ';')
    				return ''
    			end)
    
    		mbody = mbody:gsub('\n([\t ]*)([%w_]+)[\t ]+(%*?[%w_ ,]+);', function(m1,m2,m3)
    				if m2 ~= 'goto' then
    					table.insert(declarations, varDeclaration(m2, m3) .. ';')
    					return ''
    				else
    					return '\n' .. m1 .. m2 .. ' ' .. m3 .. ';'
    				end
    			end)
    
    
    		if #declarations > 0 then
    			local unique_decl = {}
    			table.sort(declarations)
    			local lastV = ''
    			local k2, v2
    			for k2,v2 in pairs(declarations) do
    				if v2 ~= lastV then
    					lastV = v2
    					table.insert(unique_decl, v2)
    				end
    			end
    			str = str .. ';\nvar\n  '.. table.concat(unique_decl, '\n  ')  .. '\n'
    		else
    			str = str .. ';\n'
    		end
    
    		str = str .. 'begin' .. trimChars(mbody, '{','}') .. 'end'
    	end
    	return str .. ';'
    end
    
    --*************************************
    --
    -- Order of substitutions are important
    --
    --*************************************
    
    --=assignements statements
    	pasbody = pasbody:gsub("([\t ]*)([%w_]+)[\t ]*=[\t ]*([^=;\n]+);", '%1%2 := %3;')
    
    --return
    	pasbody = pasbody:gsub('([\t ]*)return[\t ]+([^;]+);', '%1exit(%2);')
    
    --NULL
    	pasbody = pasbody:gsub('([^%w]+)NULL([^%w]+)', '%1nil%2')
    
    --void functions to procedure declaratios
    	pasbody = pasbody:gsub('([%w_*]+)[\t ]+([%w_*]+)[\t ]*(%b())[%s]*(%b{})', addProcFunc)
    	--pasbody = pasbody:gsub('\n([%w_]+)%s+([^%s]+)%s*(%b());', addProcFunc)
    
    --const/static
    	tblLeaveCommented = {
    		'const', 'static'
    	}
    	for k,v in pairs(tblLeaveCommented) do
    		pasbody = pasbody:gsub('([\t ]*)(' .. v .. ')([\t ]*)', '%1{%2}%3')
    	end
    
    --defines to const
    	isConstEmmited = false
    	function addConst(m1,m2)
    		local sm
    		sm = ''
    		if not isConstEmmited then
    			isConstEmmited = true
    			sm = '\nconst'
    		end
    		return sm .. '\n\t' .. m1 .. ' = ' .. m2 .. ';'
    	end
    
    	isConstEmmited = false
    	pasbody = pasbody:gsub('\n#define%s+([^%s]+)%s+(%b())', '\nconst %1 = %2;')
    	isConstEmmited = false
    	pasbody = pasbody:gsub('\n#define%s+([^%s]+)[ \t]+([^%s]+)', '\nconst %1 = %2;')
    
    --includes
    	tblUses = {}
    	pasbody = pasbody:gsub('\n#[ \t]*(include)%s+([^\n]+)', function(m1,m2)
    			for w in m2:gmatch('"([^.]+)%..+"') do
    				table.insert(tblUses, w)
    			end
    			return '\n//#' .. m1 .. ' ' .. m2
    		end)
    	pasbody = '//uses ' .. table.concat(tblUses, ', ') .. ';\n' .. pasbody
    
    --defines to compiler directives
    	pasbody = pasbody:gsub('\n#([^\n]+)', '\n{$%1}')
    
    --comments
    	pasbody = pasbody:gsub('\n#', '\n//#')
    	pasbody = pasbody:gsub('/%*', '(*')
    	pasbody = pasbody:gsub('%*/', '*)')
    
    --declarations
    	pasbody = pasbody:gsub('\n([\t ]*)([%w_]+)[\t ]+([%w_ ,]+);', checkDeclarationOrGoto)
    
    --structs
    	function parseStruct(m1, m2)
    		return '\n' .. m1 .. ' = record\n' .. trimChars(m2, '{', '}') .. '\nend'
    	end
    	pasbody = pasbody:gsub('\n[\t ]*struct[\t ]+([%w_]+)[\t ]*(%b{})', parseStruct)
    
    --if statements
    	parseStatementCalled = false
    
    	function parseStatement(mSpace, mStatement, mCond, mBody)
    		parseStatementCalled = true
    		local vStatement
    		if mStatement == 'if' then
    			vStatement = mStatement .. ' ' .. mCond .. ' then'
    		elseif mStatement == 'else' then
    			return mSpace .. mStatement .. ' begin' .. trimChars(mCond, '{', '}') .. ' end;'
    		elseif (mStatement == 'while') then
    			vStatement = mStatement .. ' ' .. mCond .. ' do'
    		elseif (mStatement == 'for') then
    			vStatement = mStatement .. ' ' .. mCond .. ' do'
    			--local forArgs = split(trimChars(mCond, '(', ')'), ';')
    			--return mSpace .. (forArgs[1] or '') .. ';' .. mSpace ..'while (' ..
    			--	(forArgs[2] or 'true') .. ') do' .. mSpace .. 'begin' .. mSpace .. trimChars(mBody, '{', '}') .. '\n' .. mSpace ..
    			--	(forArgs[3] or '') .. ';\n' .. mSpace .. 'end;'
    		else
    			vStatement = mStatement .. ' ' .. mCond .. '\n'
    		end
    		return mSpace .. vStatement .. ' begin' ..  trimChars(mBody, '{', '}') .. 'end;'
    	end
    
    	-- for nested blocks
    	parseStatementCalled = true
    	while(parseStatementCalled) do
    		parseStatementCalled = false
    		pasbody = pasbody:gsub('([^%w_])(if)[\t ]*(%b())[\t ]*(%b{})', parseStatement)
    	end
    
    	-- else
    	pasbody = pasbody:gsub('([\t ]+)(else)[\t ]*(%b{})', parseStatement)
    
    	pasbody = pasbody:gsub("([^%w_])if[\t ]*(%b())[\t ]*([^;\n]+);", '%1if %2 then %3;')
    
    --?: short if
    	pasbody = pasbody:gsub("([^?])?([^:;\n]):[\t ]*([^%s;]+);", ' iff(%1, %2, %3) ')
    
    --while loop
    	-- for nested blocks
    	parseStatementCalled = true
    	while(parseStatementCalled) do
    		parseStatementCalled = false
    		pasbody = pasbody:gsub('([^%w_])(while)[\t ]*(%b())[\t ]*(%b{})', parseStatement)
    	end
    
    --for loop
    	-- for nested blocks
    	parseStatementCalled = true
    	while(parseStatementCalled) do
    		parseStatementCalled = false
    		pasbody = pasbody:gsub('([^%w_])(for)[\t ]*(%b())%s*(%b{})', parseStatement)
    	end
    
    --do/while
    	-- for nested blocks
    	parseStatementCalled = true
    	while(parseStatementCalled) do
    		parseStatementCalled = false
    		pasbody = pasbody:gsub("([^%w_])do[\t ]*(%b{})[\t ]*while([^;\n]+);",
    			function(m1, m2, m3)
    				parseStatementCalled = true
    				return m1 .. 'repeat ' .. trimChars(m2, '{', '}') .. 'until not' .. m3
    			end)
    	end
    
    
    --switch/case statements
    
    	--begin/end enclose blocks around
    	-- for nested blocks
    	parseStatementCalled = true
    	while(parseStatementCalled) do
    		parseStatementCalled = false
    		pasbody = pasbody:gsub("([^%w_]case[\t ]+[^:]+)[\t ]*:[%s]*(%b{})",
    			function(m1, m2, m3)
    				parseStatementCalled = true
    				return m1 .. ': begin' .. trimChars(m2, '{', '}') .. 'end;'
    			end)
    		pasbody = pasbody:gsub("([^%w_]default[\t ]*):[%s]*(%b{})",
    			function(m1, m2, m3)
    				parseStatementCalled = true
    				return m1 .. ': begin' .. trimChars(m2, '{', '}') .. 'end;'
    			end)
    	end
    
    	--breaks remove
    	pasbody = pasbody:gsub("([^%w_]case[\t ]+[^:]+)[\t ]*:[%s]*(.-)break;", '%1: %2')
    
    	--case
    	pasbody = pasbody:gsub("([^%w_])case[\t ]+([^:]+):", '%1%2:')
    
    	-- for nested blocks
    	parseStatementCalled = true
    	while(parseStatementCalled) do
    		parseStatementCalled = false
    		pasbody = pasbody:gsub("([^%w_])switch[\t ]*(%b())[\t ]*(%b{})",
    			function(m1, m2, m3)
    				parseStatementCalled = true
    				return m1 .. 'case ' .. m2 .. ' of ' .. trimChars(m3, '{', '}') .. 'end;'
    			end)
    	end
    
    	--pasbody = pasbody:gsub("([\t ]*)case[\t ]+('.')[\t ]*:\n", '%1%2:\n')
    	--pasbody = pasbody:gsub("([\t ]*)case[\t ]+(%d+)[\t ]*:\n", '%1%2:\n')
    	--pasbody = pasbody:gsub("([\t ]*)(case)[\t ]+('.')[\t ]*:[\t ]*(%b{})", '%1%2 : %3')
    
    --pre/pos increments
    	-- (n)++ -> PosInc(n)
    	pasbody = pasbody:gsub("(%([^%s+]+%))%+%+", 'PosInc%1')
    	-- n*++ -> PosInc(n*)
    	pasbody = pasbody:gsub("([^%s*+]+)%+%+", 'PosInc(%1)')
    	-- ++(n) -> PreInc(n)
    	pasbody = pasbody:gsub("%+%+(%([^%s;+]+%))", 'PreInc%1')
    	-- ++*n -> PreInc(n)
    	pasbody = pasbody:gsub("%+%+([^%s*;+]+)", 'PreInc(%1)')
    
    	-- (n)--
    	pasbody = pasbody:gsub("(%([^%s-]+%))%-%-", 'PosDec%1')
    	-- n*--
    	pasbody = pasbody:gsub("([^%s*-]+)%-%-", 'PosDec(%1)')
    	-- --(n)
    	pasbody = pasbody:gsub("%-%-(%([^%s;-]+%))", 'PreDec%1')
    	-- --n
    	pasbody = pasbody:gsub("%-%-([^%s;-]+)", 'PreDec(%1)')
    
    --boolean operators
    	tblBooleanOperators = {
    		{'||', 'or'},
    		{'&&', 'and'},
    	}
    	for k,v in pairs(tblBooleanOperators) do
    		pasbody = pasbody:gsub('([\t ]*)' .. v[1] .. '([\t ]*)', '%1) ' .. v[2] .. ' (%2')
    	end
    
    
    --pointers
    	pasbody = pasbody:gsub("%->", '.')
    	pasbody = pasbody:gsub("%(%*([%w_]+)", '(%1^')
    	pasbody = pasbody:gsub("%*%*([%w_]+)", '%1^^')
    	pasbody = pasbody:gsub("&([%w_]+)", '@%1')
    	pasbody = pasbody:gsub("%*([%w_]+)[\t ]*:[\t ]*([%w_]+);", '%1 : ^%2;')
    	pasbody = pasbody:gsub("%^char", 'pchar')
    	--pasbody = pasbody:gsub("%*([%w_]+)[\t ]*:=[\t ]*([^;\n]+);", '%1^ := %2;')
    
    --bit operators
    	tblBitOperators = {
    		{'<<', 'shl'},
    		{'>>', 'shr'},
    		{'|', 'or'},
    		{'&', 'and'},
    		{'~', 'not'},
    	}
    
    	for k,v in pairs(tblBitOperators) do
    		pasbody = pasbody:gsub('([\t ]*)' .. v[1] .. '([\t ]*)', '%1 ' .. v[2] .. ' %2')
    	end
    
    --==eq operator
    	pasbody = pasbody:gsub("([\t ]*)%=%=([\t ]*)", '%1 = %2')
    
    --!= not eq operator
    	pasbody = pasbody:gsub("([\t ]*)!%=([\t ]*)", '%1 <> %2')
    
    --! unary
    	pasbody = pasbody:gsub('([\t ]*)!([\t ]*)', '%1 not %2')
    
    --blocks
    	--pasbody = pasbody:gsub("(%b{})", function(m1) return 'begin' ..  trimChars(m1, '{', '}') .. 'end;' end)
    	--pasbody = pasbody:gsub("(%b{})", function(m1) return 'begin' ..  trimChars(m1, '{', '}') .. 'end;' end)
    
    --strings and escaped characters
    	tblEscapedCahrs = {
    		{'\\"', '"'},
    		{'\\\\', '\\'},
    		{'\\t', "'#9"},
    		{'\\n', "'#10'"},
    		{'\\f', "'#12'"},
    	}
    	pasbody = pasbody:gsub('%b""', function(m1)
    			return "'" .. trimChars(m1:gsub("'", "''") , '"', '"')  .. "'"
    		end)
    	for k,v in pairs(tblEscapedCahrs) do
    		pasbody = pasbody:gsub(v[1], v[2])
    	end
    
    --common functions/procedure
    	tblOrigFuncNewFunc = {
    		{'printf', 'format'},
    		{'malloc', 'GetMem'},
    		{'free', 'Dispose'},
    		{'memcpy', 'Move'},
    	}
    
    	for k,v in pairs(tblOrigFuncNewFunc) do
    		pasbody = pasbody:gsub('([\t ]+)' .. v[1] .. '%(', '%1' .. v[2] .. '(')
    	end
    
    --typedefs
    	isTypeEmmited = false
    	function addType(m1,m2)
    		local sm
    		sm = ''
    		if not isTypeEmmited then
    			isTypeEmmited = true
    			sm = '\ntype'
    		end
    		return sm .. '\n\t' .. m2 .. ' = ' .. m1 .. ';'
    	end
    	--pasbody = pasbody:gsub('\ntypedef%s+struct%s+([^%s]+)%s+([^;]+)', addType)
    	--pasbody = pasbody:gsub('\ntypedef%s+([^%s]+)%s+([^;]-)', addType)
    	pasbody = pasbody:gsub('\n[\t ]*typedef[\t ]+struct[\t ]+([%w_]+)[\t ]+([^;]+)', '\n%2 = %1')
    	pasbody = pasbody:gsub('\n[\t ]*typedef[\t ]+struct%s+(%b{})%s+([%w_]+);',
    		function(m1, m2) return m2 .. ' = record' .. trimChars(m1, '{', '}') .. 'end;' end)
    
    
    -- print the results
    print(pasbody)
    

Here is an example of a c file converted. 
    
    
    //uses lua, llimits, lmem, lstate, lzio;
    (*
    ** $Id: lzio.c,v 1.31.1.1 2007/12/27 13:02:25 roberto Exp $
    ** a generic input stream interface
    ** See Copyright Notice in lua.h
    *)
    
    
    //#include <string.h>
    
    {$define lzio_c}
    {$define LUA_CORE}
    
    //#include 'lua.h'
    
    //#include 'llimits.h'
    //#include 'lmem.h'
    //#include 'lstate.h'
    //#include 'lzio.h'
    
    
    
    function luaZ_fill(z : PTZIO):integer;
    var
      L : Plua_State;
      buff : Pchar;
      size : size_t;
    begin
      *L := z.L;
      lua_unlock(L);
      buff := z.reader(L, z.data, @size);
      lua_lock(L);
      if (buff  =  nil ) or ( size  =  0) then exit(EOZ);
      z.n := size - 1;
      z.p := buff;
      exit(char2int(PosInc^((z.p))));
    end;
    
    
    
    function luaZ_lookahead(z : PTZIO):integer;
    begin
      if (z.n  =  0) then begin
        if (luaZ_fill(z)  =  EOZ)
          exit(EOZ);
        else begin
          PosInc(z.n);  (* luaZ_fill removed first byte; put back it *)
          z-PosDec(>p);
         end;
      end;
      exit(char2int(z^.p));
    end;
    
    
    
    procedure luaZ_init(L : Plua_State; z : PTZIO; reader : lua_Reader; data : Ppointer);
    begin
      z.L := L;
      z.reader := reader;
      z.data := data;
      z.n := 0;
      z.p := nil;
    end;
    
    
    (* --------------------------------------------------------------- read --- *)
    
    function luaZ_read(z : PTZIO; b : Ppointer; n : size_t):size_t;
    var
      m : size_t;
    begin
      while (n) do begin
        if (luaZ_lookahead(z)  =  EOZ)
          exit(n);  (* exit(number of missing bytes *)
        m = (n <= z.n) ? n : z.n);  (* min. between n and z.n *)
        Move(b, z.p, m);
        z.n -= m;
        z.p += m;
        b := (char *)b + m;
        n -= m;
      end;
      exit(0);
    end;
    
    (* ------------------------------------------------------------------------ *)
    
    function luaZ_openspace(L : Plua_State; buff : PTMbuffer; n : size_t):Pchar;
    begin
      if (n > buff.buffsize) then begin
        if (n < LUA_MINBUFFER) then n := LUA_MINBUFFER;
        luaZ_resizebuffer(L, buff, n);
      end;
      exit(buff.buffer);
    end;
    

# Manual conversion of C headers to Pascal

In this case one should utilize ones knowledge of C and Pascal together to make the headers. One can learn how to do the conversion by looking at examples. 

## Converting constants

C: 
    
    
    #define SOME_CONSTANT 5
    #define ANOTHER_CONSTANT 6
    

Pascal: 
    
    
    const
      SOME_CONSTANT = 5;
      ANOTHER_CONSTANT = 6;
    

## Converting exported routines

The most basic routines can be translated like this: 

C: 
    
    
    int somekind_of_method(int* param_first, int* param_second)
    void another_method(char*)
    

Pascal: 
    
    
    uses ctypes;
    
    const
      MyLib = 'mylib';
    
    function somekind_of_method(param_first, param_second: pcint): cint; external MyLib;
    procedure another_method_renamed(param1: PChar); external MyLib name 'another_method';
    

Note that one should use the ctypes unit together with the types that it declares to convert C headers instead of trying to use the basic Pascal types directly. This ensures that the bindings are correct in all platforms, as the C types might have different sizes on different architectures and operating systems. 

Also, note that pointers to basic C types can be converted to types declared in ctypes, for example, int* is a pointer to a C int, so we can use the type pcint (pointer to a c integer). The C type int can be converted to cint and in Pascal we can shorten the declaration of parameters which are declared one after as folows: "param_first, param_second: pcint". 

Finally, note that if the return value is **void** , then we should use a procedure instead of function. 

See also the [[External Functions](<http://www.freepascal.org/docs-html/ref/refse85.html>)] section of the reference guide. 

# Publish your bindings on Lazarus-CCR or Free Pascal

You can submit your binding code to the Lazarus CCR repository (see e.g. [Using_the_Lazarus-ccr_SVN_repository](<../en/Using_the_Lazarus-ccr_SVN_repository.md> "Using the Lazarus-ccr SVN repository")) so others can use (and maintain) it, too. If the bindings are useful for general, cross-platform work, they could be included in the FreePascal distribution as well. 

  


# See also

  * [[1]](<http://gd.tuwien.ac.at/languages/pascal/fpc/docs-pdf/CinFreePascal.pdf>) Short tutorial that describes the process of using C and C++ code in FreePascal, including writing of wrapper code
  * [Common problems when converting C header files](<../en/Common_problems_when_converting_C_header_files.md> "Common problems when converting C header files")
  * [Pascal for C users](<../en/Pascal_for_C_users.md> "Pascal for C users")
  * [Lazarus/FPC Libraries](<../en/Lazarus/FPC_Libraries.md> "Lazarus/FPC Libraries")
  * [SWIG](<../en/SWIG.md> "SWIG")

---

_Source: [https://wiki.freepascal.org/Creating_bindings_for_C_libraries/ru](https://web.archive.org/web/20240101000000/https://wiki.freepascal.org/Creating_bindings_for_C_libraries/ru)_
