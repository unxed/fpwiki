# Guide for usage of Tdbf component

**Учебник по использованию компонента Tdbf**

## Contents

  * 1 Обзор
  * 2 Документация
  * 3 Начало - создание новой таблицы
  * 4 Перекодировка
  * 5 Замеченные баги
  * 6 См. также



## Обзор

В этой статье я хочу описать некоторые особенности использования компонента Tdbf совместно со средой разработки Lazarus. При написании оиспользовались Free Pascal Compiler 2.2.1 /Lazarus 0.9.25, компонент Tdbf установлен по умолчанию. Примеры кода, приведенные ниже, проверены в Windows XP SP2 и Mandriva Linux 2007.1 Spring. 

## Документация

Я пользовался следующей документацией при изучении принципов работы с этим компонентом. Официальная документация в формате PDF доступна на [SourceForge](<https://sourceforge.net/project/showfiles.php?group_id=34085&package_id=26371>). Также мой перевод статьи по использованию этого компонента [[1]](<http://wiki.lazarus.freepascal.org/Lazarus_Tdbf_Tutorial/ru>). 

## Начало - создание новой таблицы

Подробности создания таблицы описываются в статье [[[2]](<http://wiki.lazarus.freepascal.org/Lazarus_Tdbf_Tutorial/ru#.D0.9A.D0.B0.D0.BA_.D1.81.D0.BE.D0.B7.D0.B4.D0.B0.D1.82.D1.8C_.D0.BD.D0.BE.D0.B2.D1.83.D1.8E_.D1.82.D0.B0.D0.B1.D0.BB.D0.B8.D1.86.D1.83_.D0.B1.D0.B0.D0.B7.D1.8B_.D0.B4.D0.B0.D0.BD.D0.BD.D1.8B.D1.85>)] Я хочу уточнить некоторые особенности, с которыми мне пришлось столкнуться. Например, нам необходимо создать таблицу следующей структуры, если она не существует (пример взят из реальной программы): 
    
    
    procedure CheckAndCreate;
    var path : String;
        TarifDbf : Tdbf;
    begin
      path:=ExtractFilePath(Application.EXEName);
      //Проверка существования базы данных
      if not DirectoryExists(Path+DirectorySeparator+'Base') then 
             CreateDir(Path+DirectorySeparator+'Base');
      if not FileExists(Path+'Base'+DirectorySeparator+'tarif.dbf') then begin
        TarifDbf.TableLevel := 4;
        TarifDbf.FilePathFull := Path+'Base';
        TarifDbf.TableName := 'tarif.dbf';
        TarifDbf.FieldDefs.Add('NLZ',ftInteger,2);//[0]код учреждения
        TarifDbf.FieldDefs.Add('NOTD',ftInteger,2);//[1]код отдела
        TarifDbf.FieldDefs.Add('DOLGNOST',ftString,20);//[2]название должности
        TarifDbf.FieldDefs.Add('PERSONAL',ftInteger,2);//тип персонала
        TarifDbf.FieldDefs.Add('KODKAT',ftInteger,2);//[3]квалификацоонная категория (разряд)
        TarifDbf.FieldDefs.Add('FIO1',ftString,35);//[4]Фамилия, имя, отчество сотрудника
        TarifDbf.FieldDefs.Add('TARIFRZ',ftInteger,2);//[5]Тарификационный разряд (1..25)
        TarifDbf.FieldDefs.Add('OKLAD1',ftFloat,10);//[6]Должносной оклад по тарифному разряду
        TarifDbf.FieldDefs[TarifDbf.FieldDefs.Count-1].Precision := 2;//<=
        TarifDbf.FieldDefs.Add('DOLJA1',ftFloat,5);//[16]доля ставки
        TarifDbf.FieldDefs[TarifDbf.FieldDefs.Count-1].Precision := 2;//<=
        TarifDbf.FieldDefs.Add('FOND',ftFloat,11);//[27] месячный фонд з/п
        TarifDbf.FieldDefs[TarifDbf.FieldDefs.Count-1].Precision := 2;//<=
        TarifDbf.CreateTable;
      end;
       TarifDbf.FilePathFull := Path+DirectorySeparator+'Base';
       TarifDbf.TableName := 'tarif.dbf';
       TarifDbf.Active := true;
    end;
    

Обратите внимание на фрагменты кода, отмеченные <= в коментарии - только таким образом можно создать поле типа Numeric с дробной частью. 

## Перекодировка

У компонента Tdbf есть свойство OnTranslate. Этому свойству нужно присвоить определенную процедуру: 
    
    
    uses lconvencoding;
    
    constructor TMyDbf.Create;
    begin
      FDbf.OnTranslate:= @OnTranslate;
    end;
    
    function TMyDbf.OnTranslate(Dbf: TDbf; Src, Dest: PChar; ToOem: Boolean): Integer;
    var
      S: String;
    begin
      if ToOem then
        S := ConvertEncoding(Src, 'utf8', 'cp866')
      else
        S := ConvertEncoding(Src, 'cp866', 'utf8');
      StrCopy(Dest, PChar(S));
      Result := StrLen(Dest);
    end;
    

а потом после открытия для всех полей типа TStringField поставить свойству Transliterate := True: 
    
    
    FDbf.Open;
    for i := 0 to FDbf.Fields.Count - 1 do begin
      if FDbf.Fields[i] is TStringField then
        TStringField(FDbf.Fields[i]).Transliterate := True;
    end;
    

После выполнения этих действий таблица FDbf будет автоматически перекодироваться из cp866 в utf8 при открытии и из utf8 в cp866 при записи. 

## Замеченные баги

При использовании данного компонента я выявил несколько багов. 

1\. При попытке узнать тип и размерность поля не всегда полученный результат соответствует действительности. 

2\. При упаковке таблицы с MEMO файл MEMO "раздувается" приблизительно в 10 раз вместо того, чтобы уменьшаться. (в SVN версии этот баг исправили, связан с неправильным определением размера при чтении данных из поля MEMO). 

3\. Нет нормальной поддержки перекодировки с кодовой страницы БД в текущую локаль и обратно, что создает определенные трудности при переносе БД с одной системы в другую (напр. из DOS в Windows или Linux), поэтому приходится самому реализовывать перекодировку "на лету". 

## См. также

  * [Example: TDbf (creating table and indexes, selecting of index)](<../en/Example__TDbf_\(creating_table_and_indexes,_selecting_of_index\).md> "Example: TDbf \(creating table and indexes, selecting of index\)")



  
\--[ViruZ_UA](</index.php?title=User:ViruZ&action=edit&redlink=1> "User:ViruZ \(page does not exist\)") 14:55, 20 March 2008 (CET)

---

_Source: [https://wiki.freepascal.org/Guide_for_usage_of_Tdbf_component/ru](https://web.archive.org/web/20250601000000/https://wiki.freepascal.org/Guide_for_usage_of_Tdbf_component/ru)_
