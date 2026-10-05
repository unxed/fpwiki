# fcl-registry

│ **[English (en)](<../en/fcl-registry.md>)** │  **русский (ru)** │

fcl-registry это модуль [FCL](<../en/FCL.md> "FCL"), обеспечивает доступ к реестру Windows. 

## Contents

  * 1 Термины реестра
  * 2 Пример
  * 3 Доступ к 64-битному и 32-битнму реестру
  * 4 Административные права
  * 5 См. также



## Термины реестра

RootKey: куст реестра, в котором вы хотите начать доступ к реестру. Примеры: HKEY_LOCAL_MACHINE, HKEY_CURRENT_USER 

Ключ (key): куть к "каталогу" который содержит отдельные данные. Это немного трудно для понимания, но осталось для совместимости с более ранними версиями реестра. 

Имя/значение: фактическая пара имя/значение в ключе реестра "каталог". Каждый ключ может иметь значение по умолчанию, чье имя _(пустая строка)._

## Пример

Пример, который пытается получить значение по умолчанию по адресу HKLM\SOFTWARE\Classes\InnoSetupScriptFile\shell\Compile\Command: 
    
    
    uses ... registry...
    
    var
      CompileCommand: string='';
      Registry: TRegistry;
    begin
      Registry := TRegistry.Create;
      try
        // Navigate to proper "directory":
        Registry.RootKey := HKEY_LOCAL_MACHINE;
        if Registry.OpenKeyReadOnly('\SOFTWARE\Classes\InnoSetupScriptFile\shell\Compile\Command') then
          CompileCommand:=Registry.ReadString(''); //read the value of the default name
      finally
        Registry.Free;
      end;
    end;
    

## Доступ к 64-битному и 32-битнму реестру

Если вы имеете 64-разрядную Windows, реестр разделен на 64-битную и 32-битную часть (совместимость). По умолчанию, если вы запускаете 32-битный процесс, вы увидите 32-битную часть, а если 64-битное приложение — 64-битную часть. 

Вы также можете получить доступ к 32-битной части из 64-битных приложений, и наоборот. Из MSDN: <http://msdn.microsoft.com/en-us/library/windows/desktop/aa384129%28v=vs.85%29.aspx>

  * KEY_WOW64_64KEY: Доступ к 64-битному ключу из 32-битного или 64-битного приложения.
  * KEY_WOW64_32KEY: Access a 32-битному ключу из 32-битного или 64-битного приложения.



Эти ключи определены в модуле registry , так что вы можете просто использовать их: например, в свойстве Access объекта реестра, вот так: 
    
    
    Registry := TRegistry.Create;
    Try
      Registry.Access:=Registry.Access or KEY_WOW64_64KEY;
    

или в вызове registry.create, вот так: 
    
    
    TRegistry.Create(KEY_READ or KEY_WOW64_64KEY);
    

## Административные права

В зависимости от того, что вы хотите читать/писать в реестр, вам может понадобиться повышение прав администратора (Windows Vista+). Смотрите [IDE_Window:_Project_Options#Use_manifest_file_to_set_execution_level_.28Windows_only.29](<../en/IDE_Window__Project_Options.md> "IDE Window: Project Options")

## См. также

  * [Package List](<../en/Package_List.md> "Package List")

---

_Source: [https://wiki.freepascal.org/fcl-registry/ru](https://web.archive.org/web/20250218064800/https://wiki.freepascal.org/fcl-registry/ru)_
