# WindowsVersion

[![Windows logo - 2012.svg](https://upload.wikimedia.org/wikipedia/commons/thumb/5/5f/Windows_logo_-_2012.svg/50px-Windows_logo_-_2012.svg.png)](</File:Windows_logo_-_2012.svg>)

Эта статья относится только к [Windows](</Category:Windows> "Category:Windows").

См. также: [Multiplatform Programming Guide](<../en/Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

│ **[English (en)](<../en/WindowsVersion.md>)** │  **русский (ru)** │

Эта статья посвящена программированию для Windows. 

Функция ниже определяет номер текущей версии Windows. 
    
    
    uses
      Windows, SysUtils, ...;
      ...
    {
    Значение номеров версий Windows:
    5.0 => Windows 2000
    5.1 => Windows XP
    5.2 => Windows XP64 или Windows 2003 Server
    6.0 => Windows Vista или Windows 2008 Server
    6.1 => Windows 7 или Windows 2008 Server R2
    6.2 => Windows 8 или Windows Server 2012
    6.3 => Windows 8.1 или Windows Server 2012 RS
    }
    
    function funGetWinVer: string;
    // Возвращает номер версии Windows в виде строки
    begin
      Result := IntToStr(LOBYTE(LOWORD(GetVersion)));
      Result := Result + '.';
      Result := Result + IntToStr(HIBYTE(LOWORD(GetVersion)));
    end;
    

Кроме того, модуль [SysUtils](</index.php?title=SysUtils&action=edit&redlink=1> "SysUtils \(page does not exist\)") предоставляет информацию о версии Windows в переменных: 
    
    
    program EchoWinVersion;
    uses sysutils;
    ...
    begin
      Writeln('Win32 Platform    : ', Win32Platform    );
      Writeln('Win32 Major Version: ', Win32MajorVersion);
      Writeln('Win32 Minor Version: ', Win32MinorVersion);
      Writeln('Win32 Build Number : ', Win32BuildNumber );
      Writeln('Win32 CSD Version  : ', Win32CSDVersion  );
      readln;
    end.
    

Выходные данные для Windows 7 с пакетом обновления 1 (SP1) выглядят следующим образом: 
    
    
    Win32 Platform    : 2
    Win32 Major Version: 6
    Win32 Minor Version: 1
    Win32 Build Number : 7601
    Win32 CSD Version  : Service Pack 1
    

Для Windows 10 Pro Build 18362 (64-разрядная версия) результат выглядит следующим образом: 
    
    
    Win32 Platform    : 2
    Win32 Major Version: 6
    Win32 Minor Version: 2
    Win32 Build Number : 9200
    Win32 CSD Version  : 
    

что явно неверно :( 

Однако для идентификации Windows 10 (и более ранних версий) работает следующее: 
    
    
    ...
    Uses ..., Win32Proc;
    ...
    
    Procedure GetWinVer;
    Begin
    if WindowsVersion = wv95 then Memo.Lines.Add('Windows 95')
      else if WindowsVersion = wvNT4 then Memo.Lines.Add('Windows NT v.4')
      else if WindowsVersion = wv98 then Memo.Lines.Add('Windows 98')
      else if WindowsVersion = wvMe then Memo.Lines.Add('Windows ME')
      else if WindowsVersion = wv2000 then Memo.Lines.Add('Windows 2000')
      else if WindowsVersion = wvXP then Memo.Lines.Add('Windows XP')
      else if WindowsVersion = wvServer2003 then Memo.Lines.Add('Windows Server 2003/Windows XP64')
      else if WindowsVersion = wvVista then Memo.Lines.Add('Windows Vista')
      else if WindowsVersion = wv7 then Memo.Lines.Add('Windows 7')
      else if WindowsVersion = wv10 then Memo.Lines.Add('Windows 10')
      else Memo.Lines.Add('Windows Unknown Version!');         
    End.
    

Для 64-разрядной версии Windows 10 оно выводит «Windows 10».

---

_Source: [https://wiki.freepascal.org/WindowsVersion/ru](https://web.archive.org/web/20250122190805/https://wiki.freepascal.org/WindowsVersion/ru)_
