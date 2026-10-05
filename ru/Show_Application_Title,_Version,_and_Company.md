# Show Application Title, Version, and Company

│ **[English (en)](<../en/Show_Application_Title,_Version,_and_Company.md>)** │  **русский (ru)** │

## Contents

  * 1 Обзор
    * 1.1 Windows
    * 1.2 Linux
    * 1.3 macOS и iOS
  * 2 Реализации
    * 2.1 FPC 3.0
    * 2.2 Реализации с использованием устаревшего FPC 2.6.x
  * 3 Советы по теме
    * 3.1 Ревизии SVN/Git/Hg/Mercurial
  * 4 Хаки только для Unix-систем
    * 4.1 Получение информации о версии Subversion в виде строки, доступной для программы
    * 4.2 Переименование окончательного исполняемого файла, чтобы включить в его название платформу и метку времени



## Обзор

### Windows

С самых ранних версий Microsoft Windows имела возможность хранить информацию о версии и т.д. в исполняемом файле. Поддержка этого была добавлена в Delphi в районе v3 и частично работала в Lazarus 0.9.24. 

### Linux

В формате файла ELF нет неявных положений о номерах версий, авторских правах и так далее, но Lazarus хранит это в разделах ресурсов. Чтение информации во время исполнения программы зависит от модулей FPC, ниже - некоторая полезная информация. 

### macOS и iOS

Для macOS информация о версии файла Lazarus может храниться в двух местах: 

  * исполняемый файл MACH-O (например, в Windows, Linux); см. ниже
  * комплект приложения (если он есть) в plist. См. [Mac Show Application Title, Version, and Company](<../en/Mac_Show_Application_Title,_Version,_and_Company.md> "Mac Show Application Title, Version, and Company") для получения дополнительной информации.



## Реализации

### FPC 3.0

Реализация в FPC 3.0.x с использованием [fcl-res](<../en/fcl-res.md> "fcl-res"): см. [[announcement in User Changes 3.0](<http://wiki.lazarus.freepascal.org/User_Changes_3.0#TVersionInfo_.28fileinfo_unit.29_re-implemented_in_a_platform_independent_way%7C>)] 

В приведенном ниже коде показано, как получить информацию об исполняемых файлах из 

  * .exe/.dll/.ocx (формат Windows) файлов
  * исполняемых файлов ELF, скомпилированных в Lazarus (Linux),
  * исполняемых файлов MACH-O, скомпилированных в Lazarus (OSX)


    
    
    program printfileinfo;
    {Отображает информацию о версии файла для 
    - исполняемых файлов Windows PE 
    - исполняемых Linux ELF (скомпилированных в Lazarus)
    - исполняемых файлов MACH-O OSX (скомпилированных в Lazarus)
    Запускается на Windows, Linux, OSX...
    }
    
    {$mode objfpc}{$H+}
    {$ifdef mswindows}{$apptype console}{$endif}
    uses
      {$IFDEF UNIX}{$IFDEF UseCThreads}
      cthreads,
      {$ENDIF}{$ENDIF}
      Classes,sysutils
      // FPC 3.0 fileinfo читает ресурсы exe, пока вы регистрируете соответствующие модули
      , fileinfo
      , winpeimagereader {это нужно для чтения информации exe}
      , elfreader {необходимо для чтения исполняемых файлов ELF}
      , machoreader {необходимо для чтения исполняемых файлов MACH-O}
      ;
    
    var
      FileVerInfo: TFileVersionInfo;
    
    {$R *.res}
    
    begin
      FileVerInfo:=TFileVersionInfo.Create(nil);
      try
        FileVerInfo.ReadFileInfo;
        writeln('Company: ',FileVerInfo.VersionStrings.Values['CompanyName']);
        writeln('File description: ',FileVerInfo.VersionStrings.Values['FileDescription']);
        writeln('File version: ',FileVerInfo.VersionStrings.Values['FileVersion']);
        writeln('Internal name: ',FileVerInfo.VersionStrings.Values['InternalName']);
        writeln('Legal copyright: ',FileVerInfo.VersionStrings.Values['LegalCopyright']);
        writeln('Original filename: ',FileVerInfo.VersionStrings.Values['OriginalFilename']);
        writeln('Product name: ',FileVerInfo.VersionStrings.Values['ProductName']);
        writeln('Product version: ',FileVerInfo.VersionStrings.Values['ProductVersion']);
      finally
        FileVerInfo.Free;
      end;
    end.
    

### Реализации с использованием устаревшего FPC 2.6.x

Использует [fcl-res](<../en/fcl-res.md> "fcl-res"); вклад через список рассылки: [[1]](<http://lists.lazarus.freepascal.org/pipermail/lazarus/attachments/20100723/8db6b97e/attachment.ksh>)

Используйте это как 
    
    
    uses
      resource, versiontypes, versionresource;
    
     FUNCTION resourceVersionInfo: STRING;
     
     (* В отличие от большинства AboutText (см.ниже), это требует значительных усилий  *)
     (* во время выполнения программы для извлечения номеров версий/выпусков/сборок из *)
     (* информации о ресурсах, добавленной в двоичный файл.                            *)
     
     VAR     Stream: TResourceStream;
             vr: TVersionResource;
             fi: TVersionFixedInfo;
     
     BEGIN
       RESULT:= '';
       TRY
     
     (* Это вызывет исключение, если информация о версии не была включена в двоичный *)
     (* файл (Lazarus Project -> Project Options -> Version Info -> Version           *)
     (* numbering).                                                                   *)
    
         Stream:= TResourceStream.CreateFromID(HINSTANCE, 1, PChar(RT_VERSION));
         TRY
           vr:= TVersionResource.Create;
           TRY
             vr.SetCustomRawDataStream(Stream);
             fi:= vr.FixedInfo;
             RESULT := 'Version ' + IntToStr(fi.FileVersion[0]) + '.' + IntToStr(fi.FileVersion[1]) +
                    ' release ' + IntToStr(fi.FileVersion[2]) + ' build ' + IntToStr(fi.FileVersion[3]) + eol;
             vr.SetCustomRawDataStream(nil)
           FINALLY
             vr.Free
           END
         FINALLY
           Stream.Free
         END
       EXCEPT
       END
     END { resourceVersionInfo } ;
    

  
Использоаавние vinfo: [здесь](<http://forum.lazarus.freepascal.org/index.php?topic=12435.0>) и [здесь](<http://forum.lazarus.freepascal.org/index.php?topic=13957.0>)

## Советы по теме

### Ревизии SVN/Git/Hg/Mercurial

Используйте `$(lazarusdir)/tools/svn2revisioninc` для получения номера ревизии (из репозиториев subversion, git или mercurial) в файле revision.inc, например что-то вроде: 
    
    
    // Создан при помощи Svn2RevisionInc
    const RevisionStr = '43594';
    

## Хаки только для Unix-систем

Они работают с Linux на различных платформах и, вероятно, с Solaris при условии, что установлены утилиты, производные от GNU. 

### Получение информации о версии Subversion в виде строки, доступной для программы

Вставьте это в Project options -> Compilation -> Execute before -> Command: 
    
    
     /bin/sh -c "echo -n C`svnversion -n`C |tr A-G %-+ >project_svnrevision.inc"
    

Note quote and backtick positions. The tr is converting C into another layer of quotes which is necessary for things to work as required. Обратите внимание на кавычки и обратные кавычки. `Tr` преобразует `C` в другой уровень цитат, который необходим для того, чтобы все работало должным образом. 

Поместите это в программу: 
    
    
       (*$IFDEF UNIX   *)
               rev= (*$I project_svnrevision.inc *) ;
       (*$ELSE         *)
               rev= 'unimplemented';
       (*$ENDIF        *)
    

Обратите внимание, что это должна быть строка, так как номер версии будет иметь нечисловой суффикс, если проект был обновлен с момента его последней фиксации. 

### Переименование окончательного исполняемого файла, чтобы включить в его название платформу и метку времени

Вставьте это в Project options -> Paths -> Unit output directory: 
    
    
     lib/$(TargetCPU)-$(TargetOS)
    

Вставьте это в Project options -> Paths -> Target file name: 
    
    
     UnyokedBackend-$(TargetCPU)-$(TargetOS)-$(LCLWidgetType)
    

Убедитесь, что установлена галочка "Apply conventions"(Применить соглашения). Это может немного отличаться в зависимости от версии IDE и компилятора/компоновщика. 

Вставьте это в Project options -> Compilation -> Execute after -> Command: 
    
    
     /bin/sh -c "mv libunyokedbackend-$(TargetCPU)-$(TargetOS)-$(LCLWidgetType).so UnyokedBackend-$(TargetCPU)-$(TargetOS)-$(LCLWidgetType).`date +%F.%R`.so"
    

Это должна быть одна строка. Обратите внимание на кавычки и обратные кавычки.

---

_Source: [https://wiki.freepascal.org/Show_Application_Title%2C_Version%2C_and_Company/ru](https://web.archive.org/web/20241212120158/https://wiki.freepascal.org/Show_Application_Title%2C_Version%2C_and_Company/ru)_
