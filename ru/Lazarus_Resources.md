# Lazarus Resources

│ **[Deutsch (de)](</Lazarus_Resources/de> "Lazarus Resources/de")** │  **[English (en)](<../en/Lazarus_Resources.md> "Lazarus Resources")** │  **[español (es)](</Lazarus_Resources/es> "Lazarus Resources/es")** │  **[français (fr)](</Lazarus_Resources/fr> "Lazarus Resources/fr")** │  **[한국어 (ko)](</Lazarus_Resources/ko> "Lazarus Resources/ko")** │  **русский (ru)** │    
****

## Contents

  * 1 Введение
  * 2 Lazarus ресурсы
    * 2.1 Lazarus Resource Form File
    * 2.2 Получение данных из LRS ресурса
  * 3 FPC ресурсы
    * 3.1 Добавление ресурсов в программу
    * 3.2 Установлен ли windres
    * 3.3 Использование ресурсов в вашей программе



# Введение

Файл ресурсов содержит данные, встраиваемые в исполняемый файл. Это могут быть изображения, строки, информация о версии, Manifest (Windows) и формы. Сюда включены данные, к которым программист может обратиться из кода. Использование ресурсов может быть удобно, если вы хотите распространять автономные исполняемые файлы. 

До FPC 2.4 не было возможности использовать "нормальные" файлы ресурсов (*.res) в Lazarus, потому что они Win32 специфичны. Смотрите ниже #Lazarus ресурсы. 

Нормальные ресурсы доступны в последней версии FPC (включая все последние версии Lazarus). Смотрите ниже #FPC ресурсы. 

# Lazarus ресурсы

Для того чтобы использовать файлы как ресурсы Lazarus, нужно пересоздать их при помощи lazres. Lazres можно найти в папке "Tools", расположенной в установочной директории Lazarus (C:\Lazarus\Tools\\). 

Теперь можно скомпилировать файлы ресурсов Lazarus (*.lrs) при помощи командной строки. Синтаксис lazres: 

    
    
    
    lazres <имя файла ресурсов> <включаемые файлы (file1 file2 file3 ...)>

Пример: 

    
    
    
    lazres mylazarusresource.lrs image.jpg

Для использования файла ресурсов Lazarus в вашем проекте, подключите его используя дирекстиву компилятора $I в разделе **initialization** вашего модуля. 

Вы можете получить доступ к данным ресурса сразу или с использованием метода переменных LoadFromLazarusResource, которые после этого будут хранить данные. LoadFromLazarusResource принимает на вход один строковый параметр, который указывает, какой объект должен быть загружен из файла ресурсов. Пример: 
    
    
    procedure ExampleProc;
    var
      Image: TImage
    begin
      Image := TImage.Create;
      Image.Picture.LoadFromLazarusResource('image'); // Обратите внимание, что расширение файла не указывается
    end;
    
    initialization
      {$I mylazarusresource.lrs}
    

Этот код подключает файл mylazarusresource.lrs к проекту. В процедуре ExampleProc создан объект TImage и в него загружено изображение "image" из ресурса. Файл, который был встроен в ресурс, возможно был назван image.jpg. 

Все класс, наследованные от TGraphic, имеют процедуру LoadFromLazarusResource. 

## Lazarus Resource Form File

Lazarus генерирует .LRS файлы c файлами формы .LFM. 

Когда файл формы (*.lrs) отсутствует, FPC выдает следующее сообщение об ошибке: ERROR: unit1.pas(193,4) Fatal: Can't open include file "unit1.lrs" 

Чтобы решить эту проблему вы можете: 

  * использовать lazres: c:\lazarus\tools\lazres.exe unit1.lrs unit1.lfm
  * (проще): немного изменить дизайн формы и сохранить его; это пересоздаст .lrs файл без необходимости запускать lazres.



## Получение данных из LRS ресурса

Вы можете извлекать данные из ресурса так: 
    
    
    uses ...LResources...;
    
    ...
    procedure TForm1.FormCreate(Sender: TObject);
    var
      r: TLResource;
      Data: string;
    begin
      r := LazarusResources.Find('datafile1');
      if r = nil then 
        raise Exception.Create('Resource datafile1 is missing');
    
      Data := r.Value;
      //...что-то делать с данными...
    end;
    

# FPC ресурсы

Начиная с версии FPC 2.4 вы можете использовать стандартные .rc (Resource Script) файлы для подключения ресурсов в ваших проектах. Для превращения .rc скрипта в бинарный ресурс (.res файл), FPC запускает внешний компилятор (windres или GoRC). Поэтому, компилятор ресурсов должен быть установлен и в переменной окружения PATH, должен быть прописан путь к нему. Для получения дополнительной информации см.: [FPC Programmer's guide, chapter 13 "Using Windows resources"](<http://www.freepascal.org/docs-html/prog/progch13.html>)

Для упрощения процесса компиляции, можно использовать уже скомпилированные файлы ресурсов (.res). Вы можете предварительно скомпилировать ресурсы любым доступным компилятором - windres (доступен на Unix и Windows), GoRC (только Windows), Microsoft Resource Compiler (rc.exe входит в Visual Studio), Borland Resource Compiler (brcc32.exe входит в Delphi, C++ Builder или продукты Rad Studio) или любой другой. 

Используйте директиву {$R filename.rc} или {$R filename.res} для включения файла ресурсов в исполняемый файл. FPC RTL предоставляет как низкоуровневые функции, так и высокоуровневые классы для работы с ресурсами. 

Низкоуровневые функции: 

  * EnumResourceTypes
  * EnumResourceNames
  * EnumResourceLanguages
  * FindResource
  * FindResourceEx
  * LoadResource
  * SizeofResource
  * LockResource
  * UnlockResource
  * FreeResource



Они совместимы с Windows API функциями [Resource Functions](<http://msdn.microsoft.com/en-us/library/ff468902\(VS.85\).aspx>). 

Базовым классом, работающим с ресурсами, является TResourceStream. LCL использует его для загрузки встроенных растровых изображений, иконок, и форм. Посмотрите на TGraphic.LoadFromResourceID или TIcon.LoadFromResourceHandle чтобы понять, как они используются в LCL. 

## Добавление ресурсов в программу

Давайте рассмотрим ситуацию, когда нужно сохранить некоторые данные в исполняемый файл и при выполнении программы необходимо извлечь их. 

Сначала мы должны сообщить компилятору, какие файлы включить в ресурс. Делаем это в .rc файле: 

_Файл mydata.rc_

    
    
    
    MYDATA         RCDATA "mydata.dat"

Здесь MYDATA это имя ресурса, RCDATA это тип ресурса (подробнее смотрите [Resource Types](<http://msdn.microsoft.com/en-us/library/ms648009\(VS.85\).aspx>)) и "mydata.dat" это файл с данными. 

Давайте скажем компилятору подключить ресурс к программе: 
    
    
    program mydata;
    
    {$R mydata.rc}
    begin
    end.
    

Незаметно, FPC указывает компилятору ресурсов, распространяемому вместе с ним, компилировать .rc скрипт в бинарный файл ресурсов .res. После, компоновщик включит его в исполняемый файл. Хотя это не заметно для программиста, но вы можете, если хотите, создать свой собственный .res файл при помощи, например, Borland Resource Compiler. Тогда, вместо использования 
    
    
    {$R mydata.rc}
    

нужно использовать 
    
    
    {$R mydata.res}
    

. 

## Установлен ли windres

На Linux/OS X, вам нужно убедиться, что компилятор ресурсов windres, который входит в состав инструментов mingw32, у вас установлен. Например на Debian вы можете выполнить: 
    
    
    aptitude install mingw32-binutils
    # this installs e.g. /usr/bin/i586-mingw32msvc-windres
    # but FPC expects just windres, so:
    ln -s /usr/bin/i586-mingw32msvc-windres /usr/bin/windres
    # alternatively (and probably more "official") use -FCi586-mingw32msvc-windres in your calls to fpc or
    # add -FCi586-mingw32msvc-windres to your fpc.cfg
    

На Windows, компиляторы ресурсов windres и gorc устанавливаются вместе с FPC и Lazarus. 

## Использование ресурсов в вашей программе

Давайте сейчас извлечем сохраненный ресурс в файл, например mydata.dat: 
    
    
    program mydata;
    uses
      SysUtils, Windows;
    {$R mydata.res}
    var
      S: TResourceStream;
      F: TFileStream;
    begin
      // create a resource stream which points to our resource
      S := TResourceStream.Create(HInstance, 'MYDATA', RT_RCDATA);
      // Please be aware of writing an apostrophes in resource type - source will not be axtracted!!!
      try
        // Создание файла mydata.dat в папке приложения
        F := TFileStream.Create(ExtractFilePath(ParamStr(0)) + 'mydata.dat', fmCreate); 
        try
          F.CopyFrom(S, S.Size); // Копирование данных из ресурса в файл
        finally
          F.Free; // destroy the file stream
        end;
      finally
        S.Free; // destroy the resource stream
      end;
    end.

---

_Source: [https://wiki.freepascal.org/Lazarus_Resources/ru](https://web.archive.org/web/20250513123741/https://wiki.freepascal.org/Lazarus_Resources/ru)_
