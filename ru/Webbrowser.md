# Webbrowser

│ **[Deutsch (de)](</Webbrowser/de> "Webbrowser/de")** │  **[English (en)](<../en/Webbrowser.md> "Webbrowser")** │  **[español (es)](</Webbrowser/es> "Webbrowser/es")** │  **русский (ru)** │    
****

## Contents

  * 1 Запуск веб-браузера для просмотра страницы/адреса
    * 1.1 OpenURL
    * 1.2 Определение браузера, используемого по умолчанию
    * 1.3 Запуск браузера
  * 2 Использование компонента turbopower internet pro
  * 3 QT Webkit
  * 4 THtmlPort
  * 5 GeckoPort
    * 5.1 LazActiveX
    * 5.2 LazWebKit
  * 6 See also



# Запуск веб-браузера для просмотра страницы/адреса

## OpenURL

OpenURL — это простейший способ открыть URL. Эта процедура определит, какой программой адрес должен быть открыт по умолчанию, и откроет в ней указанный URL. Она может использоваться для адресов вида 'mailto:aname@AnAddress?subject=:::A subject line', веб-адресов вида 'http://' и адресов файлов в виде 'file:///'. 
    
    
    uses LCLIntf;
    ...
    OpenURL('http://www.lazarus.freepascal.org');
    

## Определение браузера, используемого по умолчанию

Для каждой платформы существует свой механизм, позволяющий определять программы, используемые по умолчанию. Модуль LCL _lazhelphtml_ содержит класс THTMLBrowserHelpViewer, позволяющий запускать веб-браузер для просмотра справки LCL. Можно использовать его (класса THTMLBrowserHelpViewer) метод FindDefaultBrowser, чтобы определить браузер, используемый по умолчанию, и запустить его. Далее пример: 
    
    
    uses 
      Classes, ..., LCLProc, LazHelpHTML;
    
    ...
    
    implementation
    
    procedure TMainForm.Button1Click(Sender: TObject);
    var
      v: THTMLBrowserHelpViewer;
      BrowserPath, BrowserParams: string;
    begin
      v:=THTMLBrowserHelpViewer.Create(nil);
      v.FindDefaultBrowser(BrowserPath,BrowserParams);
      debugln(['Path=',BrowserPath,' Params=',BrowserParams]);
      v.Free;
    end;
    

В Убунту (Linux) вы можете получить, как пример: 
    
    
    Browser=/usr/bin/xdg-open Params=%s
    

В Windows: 
    
    
    Browser=C:\windows\system32\rundll32.exe Params=url.dll,FileProtocolHandler %s
    

## Запуск браузера

Если вы знаете, что такое командная строка и параметры, вы можете использовать класс TProcessUTF8, чтобы запустить браузер: 
    
    
    uses 
      Classes, ..., LCLProc, LazHelpHTML, UTF8Process;
    
    ...
    
    implementation
    
    procedure TMainForm.Button1Click(Sender: TObject);
    var
      v: THTMLBrowserHelpViewer;
      BrowserPath, BrowserParams: string;
      p: LongInt;
      URL: String;
      BrowserProcess: TProcessUTF8;
    begin
      v:=THTMLBrowserHelpViewer.Create(nil);
      try
        v.FindDefaultBrowser(BrowserPath,BrowserParams);
        debugln(['Path=',BrowserPath,' Params=',BrowserParams]);
    
        URL:='http://www.lazarus.freepascal.org';
        p:=System.Pos('%s', BrowserParams);
        System.Delete(BrowserParams,p,2);
        System.Insert(URL,BrowserParams,p);
    
        // запуск браузера
        BrowserProcess:=TProcessUTF8.Create(nil);
        try
          BrowserProcess.CommandLine:=BrowserPath+' '+BrowserParams;
          BrowserProcess.Execute;
        finally
          BrowserProcess.Free;
        end;
      finally
        v.Free;
      end;
    end;
    

# Использование компонента turbopower internet pro

Lazarus предоставляет пакет TurboPowerIPro (lazarus/components/turbopower_ipro/turbopoweripro.lpk), который имеет следующие возможности: 

  * Содержит компоненты, которые можно поместить на форму. Когда вы установите пакет в IDE, появятся несколько новых компонентов на палитре (панели компонентов), которые можно бросить на форму, также, как и другие компоненты LCL.
  * Он полностью написан на паскале, и будет работать на всех платформах "из коробки" без каких-либо дополнительных манипуляций.
  * Вы получите полный контроль над тем, какие файлы/адреса будут открываться.
  * Все-таки у него меньше возможностей, чем у полноценного веб-браузера. Не умеет проигрывать мультимедийные данные, использовать ява-скрипт или flash. Эти функции должны быть добавлены вами.



Можно найти несколько примеров в lazarus/components/turbopower_ipro/examples, демонстрирующих как открывать локальный html-файл с изображениями, гиперссылками и кнопками истории посещений. 

# QT Webkit

Если вы используете набор визуальных компонентов Qt, то вы сможете использовать Qt Webkit для того, чтобы вставить браузер в LCL-форму. Lazarus LCL/Qt WebKit доступен по адресу [[1]](<http://users.telenet.be/Jan.Van.hijfte/qtforfpc/fpcqt4.html>). 

# THtmlPort

THtmlPort - это версия HTML-компонентов, разработанных Дэйвом Болдуином, для Lazarus/Free Pascal, включает THtmlViewer, TFrameViewer и TFrameBrowser. [THtmlPort вики](<http://wiki.lazarus.freepascal.org/THtmlPort>)

# GeckoPort

GeckoPort - это адаптированная для Lazarus/Free Pascal версия Gecko SDK, разработанного Таканори Ито для Delphi, включая компонент TGeckoBrowser. 

[GeckoPort вики](<http://wiki.lazarus.freepascal.org/GeckoPort>)

## [LazActiveX](<../en/LazActiveX.md> "LazActiveX")

Импортированная библиотека объектов Internet Explorer. 

## LazWebKit

Webkit для GTK2 [lazwebkit](<http://sourceforge.net/p/lazwebkit/wiki/Home/>)

# See also

  * [Executing External Programs](<../en/Executing_External_Programs.md> "Executing External Programs")

---

_Source: [https://wiki.freepascal.org/Webbrowser/ru](https://web.archive.org/web/20250122190546/https://wiki.freepascal.org/Webbrowser/ru)_
