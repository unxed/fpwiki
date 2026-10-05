# Traducción Automática de Formularios

## **Como utilizar la traducción automática ¡sin tanto dolor!**

Sigan estos sencillos pasos: 

1) Deben agregar a su proyecto este archivo llamado Traductor.pas (o como quieran llamarlo) 
    
    
    unit Traductor;
     
    { 
      Original of V.I.Volchenko and Lazarus Developers Team
      Modified by Oscar Flor (Delphino) from DefaultTranstator.pas to use po files.
      Enhancements will be very appreciated
    }
     
    {$mode objfpc}{$H+}
     
    interface
     
    uses
      Classes, SysUtils, LResources, Translations, Controls, typinfo, FileUtil,
      LCLProc;
     
    type
     
      { TDefaultTranslator }
     
      TDefaultTranslator = class(TAbstractTranslator)
      private
        FPOFile: TPOFile;
      public
        constructor Create(POFileName: string);
        destructor Destroy; override;
     
        procedure TranslateStringProperty(Sender: TObject; const Instance: TPersistent;
          PropInfo: PPropInfo; var Content: string); override;
        procedure TranslateUnitResourceStrings(UnitName : string);
     
        property POFile : TPOFile read FPOFile;
      end;
     
    implementation
     
    function FindLocaleFileName: string;
    var
      Lang, T: string;
      i: integer;
     
      function GetLocaleFileName(const LangID: string): string;
      var
        LangShortID: string;
      begin
        if LangID <> EmptyStr then
        begin
     
          //ParamStrUTF8(0) is said not to work properly in linux, but I've tested it
          Result := ExtractFilePath(ParamStrUTF8(0)) + LangID +
            DirectorySeparator + ChangeFileExt(ExtractFileName(ParamStrUTF8(0)), '.po');
     
          if FileExistsUTF8(Result) then
            Exit;
     
          Result := ExtractFilePath(ParamStrUTF8(0)) + 'languages' + DirectorySeparator + LangID +
            DirectorySeparator + ChangeFileExt(ExtractFileName(ParamStrUTF8(0)), '.po');
     
          if FileExistsUTF8(Result) then
            Exit;
     
          Result := ExtractFilePath(ParamStrUTF8(0)) + 'locale' + DirectorySeparator
            + LangID + DirectorySeparator + ChangeFileExt(ExtractFileName(ParamStrUTF8(0)), '.po');
     
          if FileExistsUTF8(Result) then
            Exit;
     
          Result := ExtractFilePath(ParamStrUTF8(0)) + 'locale' + DirectorySeparator
            + LangID + DirectorySeparator + 'LC_MESSAGES' + DirectorySeparator +
            ChangeFileExt(ExtractFileName(ParamStrUTF8(0)), '.po');
     
          if FileExistsUTF8(Result) then
            Exit;
     
          {$IFDEF UNIX}
          //In unix-like systems we can try to search for global locale
          Result := '/usr/share/locale/' + LangID + '/LC_MESSAGES/' +
            ChangeFileExt(ExtractFileName(ParamStrUTF8(0)), '.po');
     
          if FileExistsUTF8(Result) then
            Exit;
          {$ENDIF}
     
          //Let us search for reducted files
          LangShortID := copy(LangID, 1, 2);
     
          //At first, check all was checked
          Result      := ExtractFilePath(ParamStrUTF8(0)) + LangShortID +
            DirectorySeparator + ChangeFileExt(ExtractFileName(ParamStrUTF8(0)), '.po');
     
          if FileExistsUTF8(Result) then
            Exit;
     
          Result := ExtractFilePath(ParamStrUTF8(0)) + 'languages' + DirectorySeparator +
            LangShortID + DirectorySeparator + ChangeFileExt(
            ExtractFileName(ParamStrUTF8(0)), '.po');
     
          if FileExistsUTF8(Result) then
            Exit;
     
          Result := ExtractFilePath(ParamStrUTF8(0)) + 'locale' + DirectorySeparator
            + LangShortID + DirectorySeparator + ChangeFileExt(
            ExtractFileName(ParamStrUTF8(0)), '.po');
     
          if FileExistsUTF8(Result) then
            Exit;
     
          Result := ExtractFilePath(ParamStrUTF8(0)) + 'locale' + DirectorySeparator
            + LangID + DirectorySeparator + 'LC_MESSAGES' + DirectorySeparator +
            ChangeFileExt(ExtractFileName(ParamStrUTF8(0)), '.po');
     
          if FileExistsUTF8(Result) then
            Exit;
     
          //Full language in file name - this will be default for the project
          //We need more carefull handling, as it MAY result in incorrect filename
          try
            Result := ExtractFilePath(ParamStrUTF8(0)) + ChangeFileExt(
              ExtractFileName(ParamStrUTF8(0)), '.' + LangID) + '.po';
     
            if FileExistsUTF8(Result) then
              Exit;
     
            //Common location (like in Lazarus)
            Result := ExtractFilePath(ParamStrUTF8(0)) + 'locale' + DirectorySeparator +
              ChangeFileExt(ExtractFileName(ParamStrUTF8(0)), '.' + LangID) + '.po';
     
            if FileExistsUTF8(Result) then
              Exit;
     
            Result := ExtractFilePath(ParamStrUTF8(0)) + 'languages' +
              DirectorySeparator + ChangeFileExt(ExtractFileName(ParamStrUTF8(0)), '.' + LangID) + '.po';
     
            if FileExistsUTF8(Result) then
              Exit;
          except
            Result := EmptyStr;//Or do something else (useless)
          end; // try
     
          {$IFDEF UNIX}
          Result := '/usr/share/locale/' + LangShortID + '/LC_MESSAGES/' +
            ChangeFileExt(ExtractFileName(ParamStrUTF8(0)), '.po');
     
          if FileExistsUTF8(Result) then
            Exit;
          {$ENDIF}
     
          Result := ExtractFilePath(ParamStrUTF8(0)) + ChangeFileExt(
            ExtractFileName(ParamStrUTF8(0)), '.' + LangShortID) + '.po';
     
          if FileExistsUTF8(Result) then
            Exit;
     
          Result := ExtractFilePath(ParamStrUTF8(0)) + 'locale' + DirectorySeparator +
            ChangeFileExt(ExtractFileName(ParamStrUTF8(0)), '.' + LangShortID) + '.po';
     
          if FileExistsUTF8(Result) then
            Exit;
     
          Result := ExtractFilePath(ParamStrUTF8(0)) + 'languages' + DirectorySeparator +
            ChangeFileExt(ExtractFileName(ParamStrUTF8(0)), '.' + LangShortID) + '.po';
     
          if FileExistsUTF8(Result) then
            Exit;
        end;
     
        Result := EmptyStr;
      end;
     
    begin
      Result := EmptyStr;
      T:=EmptyStr;
     
      //Win32 user may decide to override locale with LANG variable.
      Lang   := GetEnvironmentVariableUTF8('LANG');
     
      if Lang = '' then
      begin
        for i := 1 to Paramcount - 1 do
          if (ParamStrUTF8(i) = '--LANG') or (ParamStrUTF8(i) = '-l') or
            (ParamStrUTF8(i) = '--lang') then
            Lang := ParamStrUTF8(i + 1);
      end; // if
     
      if Lang = EmptyStr then
        LCLGetLanguageIDs(Lang, T);
     
      Result := GetLocaleFileName(Lang);
      if Result <> EmptyStr then
        Exit;
     
      Result := ChangeFileExt(ParamStrUTF8(0), '.po');
      if FileExistsUTF8(Result) then
        Exit;
     
      Result := EmptyStr;
    end;
     
    { TDefaultTranslator }
     
    constructor TDefaultTranslator.Create(POFileName: string);
    begin
      inherited Create;
      FPOFile := TPOFile.Create(UTF8ToSys(POFileName));
    end;
     
    destructor TDefaultTranslator.Destroy;
    begin
      FPOFile.Free;
     
      //If someone will use this class incorrectly, it can be destroyed
      //before Reader destroying. It is a very bad thing, but in THIS situation
      //in this case is impossible. May be, in future we can overcome this difficulty
      inherited Destroy;
    end;
     
    procedure TDefaultTranslator.TranslateStringProperty(Sender: TObject;
      const Instance: TPersistent; PropInfo: PPropInfo; var Content: string);
    var
      s: string;
      Section: string;
      Component: TComponent;
    begin
      if not Assigned(FPOFile) then
        Exit;
     
      if not Assigned(PropInfo) then
        Exit;
     
      // do not translate at design time
      if Instance is TComponent then
        if csDesigning in (Instance as TComponent).ComponentState then
          Exit;
     
      if (UpperCase(PropInfo^.PropType^.Name) <> 'TTRANSLATESTRING') then
        Exit;
     
      s := FPOFile.Translate('', Content);
     
      if s = '' then
      begin
        Component := Instance as TComponent;
     
        if Component.Owner <> nil then
          Section := UpperCase(Component.Owner.Name) + '.';
     
        Section := 'T' + Section + UpperCase(Component.Name) + '.' +
          UpperCase(PropInfo^.Name);
     
        s := FPOFile.Translate('', Section + #4 + Content);
      end;
     
      if s <> EmptyStr then
        Content := s;
    end;
     
    procedure TDefaultTranslator.TranslateUnitResourceStrings(UnitName: string);
    begin
      Translations.TranslateUnitResourceStrings(UnitName, FPOFile);
    end;
     
    var
      lcfn: string;
     
    initialization
     
      //It is safe to place code here as no form is initialized before unit
      //initialization made
      //We are to search for all
      try
        lcfn := FindLocaleFileName;
      except
        lcfn := EmptyStr;
      end; // try
     
      LRSTranslator := TDefaultTranslator.Create(lcfn);
     
    finalization
     
      LRSTranslator.Free;
     
    end.

2) Luego deben activar la traducción automática del Lazarus en Opciones de Proyecto -> i18n. Donde deben indicar que la carpeta donde se almacenarán las traducciones se llame "Languages". Claro que deben crear esa carpeta luego. 

3) Agregen "Traductor.pas" a la lista de "uses" de su archivo ".lpr". 

4) Abran todos los formularios que tengan y háganle algún cambio pequeño como para que deban ser guardados nuevamente. 

5) Compilen su aplicación. 

6) Esto debió haber creado un archivo en la carpeta "Languages" con el mismo nombre que su aplicación pero con extensión ".po". 

7) Utilizando alguna herramienta de traducción de archivos .po (yo utilicé POEdit), deben generar los otros archivos de idiomas que deseen soportar. Ya saben "save as...". Los archivos deben tener la siguiente forma "<nombre de aplicación>.<idioma>.po". Ej.: MiAplicacion.es.po (para español), MiAplicacion.en.po (para inglés). Claro que deben ingresar en cada archivo y traducir los textos que se encuentren en los ítems llamados "msgstr" tomando como referencia lo escrito en "msgid". 

8) En este lugar la próxima vez que ejecuten su aplicación ya deberían aparecer los textos traducidos, tomando como base el idioma de su sistema operativo. Si quiere probar los otros idiomas debe escribir en la línea de comando "--LANG <idioma>" indicando el idioma que desee probar. Ej.: MiAplicación.exe --LANG en (Para inglés). 

## **PARA TRADUCIR CONSTANTES DE CARACTERES QUE NO SE ENCUENTRAN EN LOS FORMULARIOS**

Esta técnica explicada sólo funciona para traducir los formularios, los mensajes escritos en los programas deben ser traducidos de otra forma, la cual sería: 

1) Crear una nueva unidad que contendrá las constantes de caracteres para mensajes. Ej.: Constantes. 

2) Crear dentro de la unidad una sección "resourcestrings" y colocar dentro los mensajes a utilizar. Ej.: 
    
    
    unit Constantes;
     
    interface
     
    resourcestrings
     
      SSaludo = 'Hola que hacés';
      SDespedida = 'Un placer haber hablado contigo, saludos a la flía.';
     
    implementation
     
    end.

3) Para utilizar estas constantes, sólo deben utilizarse como si fueran constantes normales. Ej.: 
    
    
      MessageDlg(SSaludo, mtInformation, [mbOk], 0);
      MessageDlg(SDespedida, mtInformation, [mbOk], 0);

4) Como penultimo paso deben indicar que desean traducir esta unidad, para lo cual deben agregar un código de inicialización a la unidad creada: 
    
    
    unit Constantes;
     
    interface
     
    resourcestrings
     
      SSaludo = 'Hola que hacés';
      SDespedida = 'Un placer haber hablado contigo, saludos a la flía.';
     
    implementation
     
    { Traduce los resourcestrings }
    procedure TranslateResourceStrings;
    begin
      TDefaultTranslator(LRSTranslator).TranslateUnitResourceStrings('Constantes');
    end;
     
    initialization
     
      TranslateResourceStrings
     
    end.

4) Como último paso deben agregar esta unidad "Constantes" a su archivo lpr, tomando el recaudo de colocarlo luego de la unidad "Traductor", porque de no ser así "TranslateResourceStrings" dará un "Access Violation". 
    
    
      ...
      Traductor, // Tiene que estar antes que Constantes
      Constantes,
      ...

5) Ya está, disfruten.....

---

_Source: [https://wiki.freepascal.org/Traducci%C3%B3n_Autom%C3%A1tica_de_Formularios](https://web.archive.org/web/20190519175528/https://wiki.freepascal.org/Traducci%C3%B3n_Autom%C3%A1tica_de_Formularios)_
