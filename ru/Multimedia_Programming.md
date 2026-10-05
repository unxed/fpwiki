# Multimedia Programming

│ [**Deutsch (de)**](</Multimedia_Programming/de> "Multimedia Programming/de") │  [**English (en)**](<../en/Multimedia_Programming.md> "Multimedia Programming") │  [**日本語 (ja)**](</Multimedia_Programming/ja> "Multimedia Programming/ja") │  [**한국어 (ko)**](</Multimedia_Programming/ko> "Multimedia Programming/ko") │  **русский (ru)** │  [**中文（中国大陆）‎ (zh_CN)**](</Multimedia_Programming/zh_CN> "Multimedia Programming/zh CN") │    
****

## Contents

  * 1 Воспроизведение видео
    * 1.1 Запуск видеопроигрывателя
    * 1.2 VFW - Video для Windows
    * 1.3 MPlayer для Linux gtk2/X
  * 2 Воспроизведение звуков
    * 2.1 Только для Windows: использование Windows API
    * 2.2 Сторонние библиотеки для работы со звуком
  * 3 Смотрите так же



# Воспроизведение видео

## Запуск видеопроигрывателя

Для запуска внешнего проигрывателя удобно воспользоваться классом TProcessUTF8. Нижеприведенный код запускает проигрыватель из папки PathToPlayer и передаёт ему в качестве параметра командной строки расположение проигрываемого видео файла: 
    
    
    uses 
      Classes, ..., LCLProc, UTF8Process;
    
    ...
    
    implementation
    
    procedure TMainForm.Button1Click(Sender: TObject);
    var
      Player: TProcessUTF8;
    begin
      Player:=TProcessUTF8.Create(nil);
      try
        Player.CommandLine:=PathToPlayer+' '+ParametersAndMediaFile;
        Player.Execute;
      finally
        Player.Free;
      end;
    end;

Следующий пример демонстрирует, как запустить и воспроизвести видео в проигрывателе mplayer из под Linux: 
    
    
    uses 
      Classes, ..., FileUtil, LCLProc, UTF8Process;
    
    ...
    
    implementation
    
    procedure TMainForm.Button1Click(Sender: TObject);
    var
      Player: TProcessUTF8;
      Filename: String;
      PlayerPath: String;
      PlayerParams: String;
    begin
      Filename:='/home/username/video.mpg';//путь к нужному видео файлу
      PlayerPath:=FindDefaultExecutablePath('mplayer');//ищем расположение в системе программы mplayer
      PlayerParams:='"'+Filename+'"';//задаём в качестве параметра командной строки путь к файлу
      Player:=TProcessUTF8.Create(nil);
      try
        Player.CommandLine:=PlayerPath+' '+PlayerParams;//формируем командную строку
        Player.Execute;//запускаем программу
      finally
        Player.Free;
      end;
    end;

## VFW - Video для Windows

Захват и воспроизведение видео потоков от TV карты или веб-камеры при работе в ОС Windows ([VFW](<../en/Glossary.md> "Glossary") [API](<../en/Glossary.md> "Glossary")), можно задействовать, используя [SysRec](<../en/SysRec.md> "SysRec"). 

## MPlayer для Linux gtk2/X

**mplayer** \- это проигрыватель с открытым исходным кодом. С помощью компонента mplayer для Lazarus, вы сможете написать собственный видеопроигрыватель на его основе. Скачать его можно здесь: 
    
    
    svn co https://lazarus-ccr.svn.sourceforge.net/svnroot/lazarus-ccr/components/mplayer mplayer
    

# Воспроизведение звуков

## Только для Windows: использование Windows API

Вы можете использовать Windows API для воспроизведения wav файлов: 
    
    
    ...
    uses MMSystem;
    ...
    sndPlaySound('C:\sounds\test.wav', snd_Async or snd_NoDefault);

Данный код вызовет ошибку, если в пути поиска к файлу будут содержаться не латинские буквы. Поэтому более правильно производить конвертацию строки с путём поиска в системную кодировку, как в данном примере: 
    
    
     sndPlaySound(pchar(UTF8ToSys('C:\sounds\test.wav')), snd_Async or snd_NoDefault);

Помните, это не кросс-платформенный код! 

## Сторонние библиотеки для работы со звуком

Ниже приведён небольшой список библиотек, каторые могут быть использованы в Lazarus: 

  * [Audio Component Suite](<../en/ACS.md> "ACS")
  * <http://www.noeska.com/doal/tutorials.aspx> \- Open AL
  * <http://sourceforge.net/projects/audorra/> \- Audorra Library
  * <http://sourceforge.net/projects/omega-engine/files/> \- OMEGA Engine. Проект мертв, но очень прост в использовании:


    
    
    Media_Play('Music.mp3', TRUE);

  * <http://www.afterwarp.net/resources/soundlib> \- Squall sound



# Смотрите так же

  * [Аудио библиотеки](<../en/Audio_libraries.md> "Audio libraries")

---

_Source: [https://wiki.freepascal.org/Multimedia_Programming/ru](https://web.archive.org/web/20250601000000/https://wiki.freepascal.org/Multimedia_Programming/ru)_
