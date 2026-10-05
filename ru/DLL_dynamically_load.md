# DLL dynamically load

[![Windows logo - 2012.svg](https://upload.wikimedia.org/wikipedia/commons/thumb/5/5f/Windows_logo_-_2012.svg/50px-Windows_logo_-_2012.svg.png)](</File:Windows_logo_-_2012.svg>)

Эта статья относится только к [Windows](</Category:Windows> "Category:Windows").

См. также: [Multiplatform Programming Guide](<../en/Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

│ **[English (en)](<../en/DLL_dynamically_load.md>)** │  **русский (ru)** │

В руководстве показано, как динамически загружается DLL (библиотека динамической компоновки). 

Библиотека DLL (DLLTest.dll), указанная в примере ниже: 
    
    
     library info;
    
     {$mode objfpc} {$H+}
    
     uses
       SysUtils;
    
     {$R *.res}
    
     // Подпрограмма DLL
     function funStringBack(strIn : string) : PChar;
       begin
         funStringBack := PChar(UpperCase(strIn));
       end ;
    
    
     // Экспортированная подпрограмма(ы)
     exports
       funStringBack;
    
     begin
     end.
    

Что я должен делать: 

  * Определить память 
    * Необходимо создать тип данных, который точно соответствует (внешней) подпрограмме, которая должна быть импортирована из DLL.
  * Зарезервировать память 
    * Память должна быть зарезервирована для переменной (поля данных), которой назначен указанный выше тип данных.
    * Память должна быть зарезервирована для дескриптора, которому дескриптор DLL будет назначен позже.
  * Назначить DLL и внешнюю подпрограмму и принимаемые данные 
    * Вызвать DLL и назначить дескриптору DLL дескриптор.
    * Указатель переменных должен быть заменен на память внешней подпрограммы.
    * Результат внешней подпрограммы должен быть принят.
  * Освободить всю память 
    * Указатель на переменные должен снова указывать на недопустимую область памяти (:= nil), чтобы освободить внешнюю подпрограмму.
    * Память DLL необходимо снова освободить.



Интегрируйте, используйте и выпустите подпрограмму DLL в вашей собственной программе: 
    
    
     uses
       Windows, ...;
    
       ...
    
     Include function funDll : string;
     type
       // Определение вызываемой подпрограммы, как задано в DLL, которая будет использоваться
       TfunStringBack = function(strIn : string) : PChar;  stdcall;
    
     var
       // Создаем подходящую переменную (поле данных) для подпрограммы DLL
       funStringBack : TfunStringBack;
       // Создаем дескриптор для DLL
       LibHandle : THandle;
    
     begin
       // Получаем дескриптор библиотеки, которая будет использоваться
       LibHandle := LoadLibrary(PChar('DLLTest.dll'));
    
       // Проверяем успешность загрузки DLL
       if LibHandle <> 0 then
         begin
           // Назначаем адрес вызова подпрограммы переменной funStringBack
           // 'funStringBack' из DLL DLLTest.dll
           Pointer(funStringBack) := GetProcAddress(LibHandle, 'funStringBack');
    
           // Проверяет, был ли возвращен действительный адрес
           if @funStringBack <> nil then
             Result := funStringBack('hello world');
         end;
    
       // освобождаем память
       funStringBack := nil;
       FreeLibrary(LibHandle);
    
     end;
    
       ...

---

_Source: [https://wiki.freepascal.org/DLL_dynamically_load/ru](https://web.archive.org/web/20250301000000/https://wiki.freepascal.org/DLL_dynamically_load/ru)_
