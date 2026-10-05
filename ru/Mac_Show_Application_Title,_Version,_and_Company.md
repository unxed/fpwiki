# Mac Show Application Title, Version, and Company

[![macOSlogo.png](https://wiki.freepascal.org/images/1/15/macOSlogo.png)](</File:macOSlogo.png>)

Эта статья относится только к [macOS](</Category:macOS> "Category:macOS").

См. также: [Multiplatform Programming Guide](<../en/Multiplatform_Programming_Guide.md> "Multiplatform Programming Guide")

│ **[English (en)](<../en/Mac_Show_Application_Title,_Version,_and_Company.md> "Mac Show Application Title, Version, and Company")** │  **русский (ru)** │ 

[![Warning-icon.png](https://wiki.freepascal.org/images/b/b2/Warning-icon.png)](</File:Warning-icon.png>)

**Предупреждение:** Это работает, только **если** у программного обеспечения есть пакет приложений; в противном случае см. пожалуйста [Show Application Title, Version, and Company](<Show_Application_Title,_Version,_and_Company.md> "Show Application Title, Version, and Company/ru")

Для тех, кто хочет показать название приложения, версию и компанию для приложения в Mac OSX, это можно сделать с помощью следующего метода. 

**Осторожно** _CFBundleGetMainBundle_ на самом деле не возвращает **nil** , если у приложения нет пакета. Вместо этого он пытается создать этот дескриптор. См. [документацию Apple](<https://developer.apple.com/documentation/corefoundation/1537085-cfbundlegetmainbundle?language=objc>). Таким образом, мы должны также проверить наличие _ValueRef_. 
    
    
    // КОД ДЛЯ ПОКАЗА НАЗВАНИЯ, ВЕРСИИ И КОМПАНИИ ПРИЛОЖЕНИЯ
    uses MacOSAll, CarbonProc, StrUtils;
    
    var
      BundleID: String;
      BundleName: String;
      BundleRef: CFBundleRef;
      BundleVer: String;
      CompanyName: String;
      KeyRef: CFStringRef;
      ValueRef: CFTypeRef;
    
    function GetInfoPlistString(const KeyName : string) : string;
    begin
      try
        Result := '';
        BundleRef := CFBundleGetMainBundle;
        if BundleRef = nil then Exit;  {Executable not in an app bundle?}
        KeyRef := CFStringCreateWithPascalString(nil,KeyName,kCFStringEncodingUTF8);
        ValueRef := CFBundleGetValueForInfoDictionaryKey(BundleRef, KeyRef);
        if ValueRef = nil then Exit;  {Executable not in an app bundle!}
        if CFGetTypeID(ValueRef) <> CFStringGetTypeID then Exit;  {Value not a string?}
        Result := CFStringToStr(ValueRef);
      except
      on E : Exception do
        ShowMessage(E.Message);
      end;
      FreeCFString(KeyRef);
    end;
    
    procedure TForm1.FormCreate(Sender: TObject);
    begin
       try
         Form1.Caption := 'About '+Application.Title;
         StaticTextAppTitle.Caption := Application.Title;
         BundleID := GetInfoPlistString('CFBundleIdentifier');
         '''// Предполагается, что имя компании имеет формат: com.Company.AppName'''
         CompanyName := AnsiMidStr(BundleID,AnsiPos('.',BundleID)+1,Length(BundleID));
         CompanyName := AnsiMidStr(CompanyName,0,AnsiPos('.',CompanyName)-1);
         BundleVer := GetInfoPlistString('CFBundleVersion');
         StaticTextAppVer.Caption := Application.Title+' version '+BundleVer;
         StaticTextCompany.Caption := CompanyName;
       except
       on E : Exception do
              ShowMessage(E.Message);
       end;
    end;
    

Пример вывода: 

[![About1.png](https://wiki.freepascal.org/images/a/a7/About1.png)](</File:About1.png>)

## См.также

  * [Mac Preferences and About Menu](<../en/Mac_Preferences_and_About_Menu.md> "Mac Preferences and About Menu")
  * [Show Application Title, Version, and Company](<../en/Show_Application_Title,_Version,_and_Company.md> "Show Application Title, Version, and Company").

---

_Source: [https://wiki.freepascal.org/Mac_Show_Application_Title%2C_Version%2C_and_Company/ru](https://web.archive.org/web/20230202235500/https://wiki.freepascal.org/Mac_Show_Application_Title%2C_Version%2C_and_Company/ru)_
