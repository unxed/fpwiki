# FPC JVM/Language

│ **[English (en)](<../../en/FPC_JVM/Language.md> "FPC JVM/Language")** │  **русский (ru)** │    
****

## Contents

  * 1 Общая информация
  * 2 Терминология и соглашения
  * 3 Базовое поведение платформы
  * 4 Языковая поддержка
    * 4.1 Основные конструкции языка
      * 4.1.1 Неподдерживаемые конструкции
      * 4.1.2 Частично поддерживаемые конструкции
      * 4.1.3 Новые языковые конструкции
      * 4.1.4 Перегрузка
    * 4.2 Specific language features information
      * 4.2.1 Classes
      * 4.2.2 Интерфейсы (Interface)
      * 4.2.3 Implicit pointer types
      * 4.2.4 Строки
        * 4.2.4.1 String, unicodestring и java.lang.String
        * 4.2.4.2 Кодировка Ansistring и shortstring
        * 4.2.4.3 String conversions
      * 4.2.5 Type aliases and subrange types



# Общая информация

Поддержка JVM это пока лишь изменения в компиляторе. А это значит, что портирован только модуль System и модули которые импортируют лишь некоторые JDK классы. Другие RTL модули не доступны. Функционал модуля System так же очень ограничен (подробности ниже). 

Со временем, вполне возможно реализовать большинство стандартных модулей RTL таким образом, чтобы они компилировались для JVM платформы. Но это не является целью и баг-репорты о том что какого-то модуля не хватает - особой пользы не принесут. (Но конечно, патчи приветствуются!) 

Компиляция в JVM код на данный момент довольно медленная. Причина - используемый Ява ассемблер Jasmin - медленный. И причина не в том, что сам ассемблер написан на Java, а просто потому что он так написан. 

Минимальная требуемая версия JDK для программ скомпилированных FPC - 1.5. 

# Терминология и соглашения

  * **on the Pascal side** /**on the Java side** : refers to code written in resp. Pascal or Java. A number of features are only available when programming in Pascal, because the Java language has no similar concept.
  * **неявные указатели** \- некоторые типы, которые не являются указателями в Паскале, на уровне Ява реализованы как классы или массивы. Что делает их неявными указателями на данные, т.к. и Ява-массивы и классы - указатели. Возможно это и не очевидно, но переменные такого типа, как раз можно заставить действовать, точно так же как и на нативных платформах, чем переменные каких-либо других типов.
  * **полное имя класса** \- на данный момент компилятор не поддерживает пространство имён (такие как "org.freepascal" или "java.lang") как элемент синтаксиса. Точно так же имена модулей с точками не поддерживаются. Это означает что нельзя в качестве имён использовать строки вроде "java.lang.Object". Все Паскаль-заголовки для импортированных Ява-модулей используют аббревиатуру имени класса, используя первые буквы из имени каждого пакета, и заканчиваясь именем класса. Например "Java.lang.Object" - в паскале объявлен как "JLObject". Для типов объявленых в паскаль-исходниках синтаксис "имя_модуля.определитель" - работает как ожидается. Тем не менее, для всех определителей описанных ниже, будут указаны их полное имя Ява-класса, т.к. получить паскаль имя от ява-класса достаточно просто, обратное действие практически невозможно.



# Базовое поведение платформы

По устройству компилятора платформа Java/JVM рассматривается как 32-битная. Это лишь означает, что вся арифметика ориентируется, по умолчанию, на 32-бита, а не 64. Точно так же, как и на нативных платформах, созданный код будет без проблем запускаться и на 64-битных ява-машинах. Компилятор так же будет использовать 64-битные инструкции (опкоды) для 64-битных вычислений. 

Причина такого выбора: 

  * 32-битна арифметика выполняется эффективнее, чем 64-битная;
  * Индексы в JVM массивах всегда 32-битные;



Тип _extended_ приравнен к типу _double_ , точно так же, как и на других платформах, не поддерживающих 80-битовые числа с плавающей точкой. 

# Языковая поддержка

В связи с разницей между JVM и нативными платформами, не все особенности языка FPC могут быть поддержаны на JVM. Языковая конструкция не указанная ниже, при компиляции в JVM работает точно так же и при обычной компиляции. Однако может возникать некоторая разница в производительности и внутренней реализации. 

## Основные конструкции языка

### Неподдерживаемые конструкции

  * **Turbo Pascal объекты (objects)** \- поддержка может быть добавлена в будущем, т.к. они очень похожи на структуры (records).
  * **Bitpacking (битовая упаковка)** \- а так же любые преобразования которые влияют на размещение данных в памяти ({$packset xxx}, {$packrecords xxx}, {$packenum xxx}, ...).
  * **Class helpers** \- Возможно реализовать, но достаточно сложно. На стороне Ява они могут использоваться в лучшем случае, непосредственным вызовом методов, так как Язык Java не поддерживает автоматическое перенаправленные вызова методов от одного класса к другому.
  * **Variants** \- возможно реализовать, однако их реализация может быть не очень удобной, т.к; ява не поддерживает перегрузку операторов.
  * **Delphi-style RTTI** \- возможно может быть эмулирована.
  * **Resourcestring** \- пока неизвестно насколько трудоёмкая задача по их добавлению.
  * **Вложенные процедурные типы** \- вложенные процедурные типы. В то время как вложенные процедуры и процедурные типы поддерживаются, вложенный-процедурный тип остаётся не реализованным. Возможно будет добавлен в будущем.
  * **Не локальные goto**. - скорее всего никогда не будет реализовано. Теоретически реализовать возможно используя исключения (exceptions) и внутрепроцедурные переходы (goto).
  * **Inline assember** \- пока что нет поддержки ассемблера (Java byte code) для JVM.



### Частично поддерживаемые конструкции

  * **Функционал модуля System**. В настоящее время модуль System существенно ограничен в фукциях и не содержит процедур для поддержки некоторый стандартных функций языка (включая все виды ввода/вывода, поддержку ресурсов и вариантного типа). Больгинство этих функций могут быть реализованы в будущем. На данный момент реализовано: _randomize/random_ , _copy_ (для массивов и строк), _halt_ , _lo/hi_ , _abs_ , _sqr_ , _odd_ , endian swapping routines, _ror*/rol*/sar*/bsf*/bsr*_ , _upcase/lowercase_ , _runerror_ , _insert_ , _pos_ , _delete_ , _val_ , _str_ и большинство математических функций (_cos_ , _sin_ , и т.д.).


  * **Указатели** : существует возможность определять указатели. Однако, можно только взять адрес var/out/constref - параметра или Неявного указателя. Арифметика указателей не поддерживается. Индексирование указателей как массивов однако возможно при активированном _{$modeswitch cs_pointermath}'._ FIXME: на данный момент постоянно включено _._


  * **Variant records** : the variant parts of variant records do not overlap in memory, and hence cannot be used to map the same data in different ways. As a result, they also do not save any memory unlike on native targets.
  * **Call-by-reference parameters** (var/out/constref): the JVM does not support call-by-reference, nor taking the address of a local variable. For #Implicit_pointer_types, this is no problem since there the compiler always has a pointer to the actual data available. For other types, call-by-reference is emulated by the compiler via copy-in/copy-out, which means that changes are not immediately visible outside the called routine. The steps followed by the compiler are: 
    * construct an array of one element
    * store the parameter value in the array (in case of var/constref)
    * pass the array to the called routine
    * the routine can change the value in the array
    * on return, the potentially changed value is copied out of the array back into the original variable (in case of var/out)
  * **Untyped const/var/out parameters**. These are supported in the same way as they are in Delphi.NET, see <http://hallvards.blogspot.com/2007/10/dn4dp24-net-vs-win32-untyped-parameters.html> (with the same limitations as regular var/out parameters on the JVM target)
  * **Include files**. While include files will work fine at compile time, the Java class file format does not support referring to more than one source file. As a result, the compiler will only insert debugging line information for code in the main unit file. This limitation may be resolved in the future through the use of SMAP files as described in <http://jcp.org/aboutJava/communityprocess/final/jsr045/index.html>
  * **Resources**. Currently, files specified in **{$r xxx}** directives will be copied without any further processing into a jar file with the same name as the program, under the directory _org/freepascal/rawresources_. If you add this jar file to the Java class path when executing the program, you can load the resource files using the JDK's built-in resource file helpers (see _java.lang.Class.getResource()_ and _java.lang.Class.getResourceAsStream()_). Delphi-style resource support may be (partially) added in the future.
  * **Unit initialization code**. If a unit is used from an FPC-compiled program, then the unit initialization code will be run on startup. If the main program is a Java program, then the unit initialization code will only run when the first method is entered that accesses a global constant or variable or calls a global procedure/function from that unit. Note: using classes, types or class variables from a unit does **not** by itself trigger executing the unit's initialzation code. If you wish to manually trigger this, you can do so by adding a statement such as _Object dummy = new unitName();_ to your Java code.



### Новые языковые конструкции

  * **директива {$namespace x.y.z}**. Хотя пунктирной имена модулей "с точками" не поддерживаются, (глобальная) директива пространства имён может использоваться, чтобы указать компилятору помещать все определения в текущем модуле в указанный пакет Java.
  * **Формальные определения классов**. Они не существуют в Java, но могут быть необходимы для решения проблем с циклическими ссылками между классами Java. Они такие же, как их [Objective-C эквиваленты](<../../en/FPC_PasCocoa.md> "FPC PasCocoa"), за исключением того, что используют ключевое слово _class_ , а не _objcclass_.
  * **Внешние определения классов**. Внешние определения классов используются для импорта модулей JDK. Опять же, они очень похожи на своих [коллег из Objective-C](<../../en/FPC_PasCocoa.md> "FPC PasCocoa"), кроме того, что дополнительно может быть указан пакет (заметьте, что явно пакет может быть задан только для внешних классов, не внешние определение класса всегда использовать пакет из _{$namespace xxx}_ директивы):


    
    
    type
    JavaClassName = class [external ['package.name'] [name 'ExternalClassName']] [(JavaSuperClassName|ProtocolName [, ProtocolName, ProtocolName])] 
    [strict][private, protected, public]
     [variables declarations]
     [method declarations]
    end;
    

  * **Окончательные(final) поля**. Поля класса и экземпляра класса могут быть объявлены _окончательнми_._Окончательное_ поле класса может быть записано только в конструкторе класса, в то время _окончательное_ поле экземпляра класса может быть написано только в регулярном конструкторе. Вне этих процедур, они могут быть только прочитаны. Компилятор не в состоянии в полной мере проверить это ограничение, если вы используете указатель на поле.



### Перегрузка

Некоторые различные типы Паскаля и конвенции передачи параметров отображаются в идентичные в JVM. Причина в том, что JVM поддерживает меньше примитивных типов, чем Паскаль, и потому, что некоторые конвенции передачи параметров приходится эмулировать с использованием других типов. 

Это означает, что определенные перегрузки, которые действительны при нативной компиляции, являются недействительными при компиляции для JVM, потому что внутреннее имя метода используется для кодирования типов параметров. Компилятор обнаруживает такие ситуации и выдёт ошибку при обнаружении. 

Случаи, которые могут возникнуть включают в себя: 

  * **Указатель на класс**. Все указатели на класс отображаются на один и тот же тип Java (_java.lang.Class_). В результате перегрузки на основе различных типов классов не представляется возможным.
  * **Целочитленные типы (за исключением перечислений )**. JVM поддерживает только знаковые целочисленные типы. Это означает, что перегрузки на основе знаковых и бесзнаковых типов одинакового размера, не представляется возможным. Кроме того, _currency_ тип оборажается в _int64_ , а _AnsiChar_ в _shortint_. _WideChar (= unicodechar)_ является отдельным типом.
  * **Перечисления**. Каждый базовый тип перечисления является отдельным типом, но подтипы перечисления отображаются на тот же тип что и базовый тип.
  * **Множества**. Все множества не связанных с перечислением типов отображаются в один и тот же тип (_org.freepascal.rtl.FpcBitSet_). Все множества элементов перечислимых типов (независимо от типа элемента), также отображабются на один и тот же тип (_java.util.EnumSet_).
  * **Массивы**. Каждый массив определенного типа (целое число, класса, перечисления, ...) отображается в один и тот же тип независимо от размера или типа массива. Это включает в себя массивы фиксированного размера, открытые и динамические массивы.
  * **Ссылочные (var) параметры и массивы**. Как указывалось выше, ссылочный параметр эмулируется с использованием массивов. Это означает, что например, _var byte_ и _const array of byte_ параметр имеют один и от же тип и не могут быть перегружены. Указатели на типы, кроме неявных типов указателей также отображаюстся в массивы.



## Specific language features information

### Classes

Classes are implemented as Java classes: 

  * The system unit contains an extremely minimal _TObject_ implementation based on java.lang.Object. It basically only adds a virtual _destroy_ destructor and the _Free_ instance method, and lacks all other standard _TObject_ functionality. Notice that the destructor is only guaranteed to be called if you explicitly call _Free_ , or when the dead object is collected by the garbage collector.
  * If no super class is specified, a class will inherit from _TObject_. It is also possible to inherit from _java.lang.Object_ by specifying _JLObject_ as superclass instead.
  * There is no _TInterfacedObject_ , nor is it required. Any class type can implement any kind of interface.
  * Message handlers are not supported.
  * Java classes do not support non-virtual instance methods. The compiler emulates this Pascal language feature by translating such instance methods into _virtual; final;_ methods. This means that it is not possible to "hide" such methods in child classes by declaring other (virtual or non-virtual) methods with the same name and type signature.
  * Java classes do not support starting a new inheritance tree by reintroducing a method. This is only a problem if the reintroduced method has the same signature as a method in a parent class. If the signature is different, there is no problem and the compiler will allow it.
  * Java classes do not support virtual class methods nor virtual constructors. These are emulated by the compiler, but the resulting code is fairly slow. Do not use them in performance-sensitive code.
  * Constructors do not have a name in Java (and a fixed name in JVM bytecode). As a result, you cannot declare multiple constructors for a class with a different name but with the same parameters, as these would map to the same Java constructor signature.
  * Properties are only supported on the Pascal side. No getters or setters are automatically generated for use on the Java side.



Issues to take into account when using FPC-defined classes from Java code: 

  * Virtual constructors are exposed as class methods. These class methods have first parameter that specifies the class type of which you want to invoke the virtual constructor, and their name matches the constructor's name in the Pascal code. Example:


    
    
    type
    TMyClass = class
    constructor Create(l: longint); virtual;
    end;
    </source>
    
    becomes
    
    <syntaxhighlight lang=java>
    class TMyClass {
    TMyClass(int l) {
    ...
    }
    
    // the generic parameter declaration means "any class reference
    // type for TMyClass or one of its descendants")
    static TMyClass Create(java.lang.Class<? extends TMyClass> self, int l)
    {
    ...
    }
    

  * virtual and regular (non-static) class methods are exposed in a similar way as virtual class constructors, including the special first class parameter that indicates the class type from which the class method has been invoked (class methods in Java do not have a hidden self-parameter like in Pascal, so it has to be specified explicitly)
  * Overriding virtual class methods and constructors on the Java side can be done by overriding the class method whose name consists of the virtual constructor/class method's name followed by `__fpcvirtualclassmethod__`.
  * static class methods behave the same in Pascal as in Java



### Интерфейсы (Interface)

Интерфейсы реализованы, используя Ява интерфейсы: 

  * Нет счётчика ссылок - сборка мусора;
  * GUID не используются;
  * Не поддерживается возможность использовать разное имя метода в интерфейсы и в классе-реализации (implements);



### Implicit pointer types

As mentioned in the #Used_terminology_and_conventions, certain types that are not pointer-based in the Pascal language are implemented on top of Java classes or arrays. These data structures are pointers in Java, and as a result we can do some things with them that we cannot do with other types that map more directly onto a Java type. 

The implicit pointer types and their corresponding Java representations are listed below: 

  * **Record** : sealed subclasses of _org.freepascal.rtl.FpcBaseRecordClass_
  * **Set** : sets of enumerations are implemented as specialized _java.util.EnumSet_ instances, while other sets are implemented as _org.freepascal.rtl.FpcBitSet_
  * **Shortstring** : instances of _org.freepascal.rtl.ShortStringClass_
  * **Procedure of object** : subclasses of _org.freepascal.rtl.FpcBaseProcVarClass_
  * **non-dynamic arrays** : regular Java arrays



Specific properties of implicit pointer types are: 

  * It is always possible to take their address and store it in a pointer
  * When passing them to _var_ or _out_ parameters, changes made via the parameter are also immediately visible in the original location (and when passing them as a _constref_ parameters, changes made to the original value are also immediately visible in the parameter value)
  * Pointers to implicit pointer types can never be indexed as arrays



### Строки

#### String, unicodestring и java.lang.String

JVM поддерживает только 16-bit unicodestring тип (_java.lang.String_). По этой причине, _unicodestring_ в целом более эффективен, чем _ansistring_ или _shortstring_ на JVM. 

Тип _unicodestring_ напрямую перенесён в тип _java.lang.String_. Что так же означает, что если ваш код будет использоваться другим Ява-кодоам, все публичные строковые параметры и константы должны быть объявлены как _unicodestring_ , а не какой-либо другой строковый тип. 

Чтобы использовать тип _unicodestring_ по-умолчанию, была добавлена директива компиляции _{$modeswitch unicodestrings}_. Если директива объявлена **ВМЕСТЕ** с директивной _{$h+}_ , тип **string** преобразуется в тип _uncodestring_ по-умолчанию, а _char_ в тип _widechar_. Подобное преобразование происходит в Delphi 2009, затем исключением, что modeswitch действует на 1 модуль а не на весь код. 

Если _{$modeswitch unicodestrings}_ использован с _{$h-}_ , тогда _string_ тип преобразуется в _shortstring_ , но тип _char_ становится _widechar_. Для синтаксиса _{$mode delphi}_ режим _{$h+}_ включается по-умолчанию. 

#### Кодировка Ansistring и shortstring

"ansi" кодировка устанавливается _java_ программе при запуске. Кодировка устанавливается в зависимости от кодировки выбранной в операционной системы, в которой запущена программа Linux или Windows. Для Mac OS X, по причинем обратной совместимости кодировка всегда _MacRoman_ , и скорее всего, эту кодировку вы использовать не хотите. Для дополнительной информации о кодировке см. [Usage instructions](<../../en/FPC_JVM/Usage.md> "FPC JVM/Usage"). 

#### String conversions

The compiler will implicitly convert any string type to java.lang.String and vice versa. 

### Type aliases and subrange types

The JVM supports neither type aliases nor subrange types. As a result, none of the marked type definitions in the following example will be visible to Java code. Of course, they still work as expected in Pascal code. 
    
    
    type
      tc = class
      end;
    
      tenum = (ea,eb,ec);
    
      tc2 = tc;         // Pascal-only
      intsub = 3..41;   // Pascal-only
      enumsub = eb..ec; // Pascal-only

---

_Source: [https://wiki.freepascal.org/FPC_JVM/Language/ru](https://web.archive.org/web/20250117045856/https://wiki.freepascal.org/FPC_JVM/Language/ru)_
