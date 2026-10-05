# Using Google Translate

│ **English (en)** │  **[français (fr)](</Using_Google_Translate/fr> "Using Google Translate/fr")** │  **[português (pt)](</Using_Google_Translate/pt> "Using Google Translate/pt")** │    
****

## Contents

  * 1 Overview
  * 2 Info on the params
  * 3 Contacting Google
  * 4 Parsing the JSON Array based response
  * 5 Parsing the JSON Object based response
  * 6 External Links



## Overview

There are at least two way to access Google Translate: 

  * A free but metered way.
  * Using the OAuth2 authenticated API.



This is a usage example of the former. 

For information on the latter please consult: [Google Translate Getting Started](<https://developers.google.com/translate/v2/getting_started>)

## Info on the params

According to an answer on Stack Overflow ([What is the meaning of Google Translate query params?](<https://stackoverflow.com/a/29537590/8167>)) here's a list of the params to the HTTP GET call: 

  * sl - source language code (auto for auto detection)
  * tl - translation language
  * q - source text / word
  * ie - input encoding (a guess)
  * oe - output encoding (a guess)
  * dt - may be included more than once and specifies what to return in the reply
  * dj - JSON response with names instead of only arrays (dj=1)



Here are some value for dt: 

  * t - translation of source text
  * at - alternate translations
  * rm - transcription / transliteration of source and translated texts
  * bd - dictionary, in case source text is one word (you get translations with articles, reverse translations, etc.)
  * md - definitions of source text, if it's one word
  * ss - synonyms of source text, if it's one word
  * ex - examples
  * rw - See also list



  


## Contacting Google

This function will return the JSON response. 
    
    
    uses
      {...}, fpjson, fphttpclient, opensslsockets, {...}
    
    function CallGoogleTranslate(AURL: String): TJSONStringType;
    var
      client: TFPHTTPClient;
      doc: TStringList;
    begin
      Result:= EmptyStr;
      doc:=TStringList.Create;
      client:=TFPHTTPClient.Create(nil);
      try
        client.Get(AURL,doc);
        Result:=doc.Text;
      finally
        doc.Free;
        client.Free;
      end;
    end;
    

## Parsing the JSON Array based response
    
    
    uses
      {...}, fpjson, jsonparser, HTTPDefs, {...}
    
    const
      cArrayShortLanguages: Array [0..7] of String = (
        'auto',
        'en',
        'pt',
        'pl',
        'fr',
        'es',
        'it',
        'ru'
      );
    
    procedure ParseArraysTranslate;
    var
      URL: String;
      Index: integer;
      strResponse: TJSONStringType;
      jdResponse, jdTranslation, jdTranslationArray: TJSONData;
      jaTranslation, jaTranslationArray: TJSONArray;
    begin
    
      URL:='https://translate.googleapis.com/translate_a/single?client=gtx'
        +'&q='+HTTPEncode({** PUT TEXT TO TRANSLATE HERE **})
        +'&sl='+cArrayShortLanguages[0] // Auto Detect
        +'&tl='+cArrayShortLanguages[1] // English
        +'&dt=t'
        +'&ie=UTF-8&oe=UTF-8'
        ;
    
      strResponse:= CallGoogleTranslate(URL);
      try
        jdResponse:= GetJSON(strResponse);
    
        jdTranslation:= jdResponse.FindPath('[0]');
        if (jdTranslation <> nil) and (jdTranslation.JSONType = jtArray) then
        begin
          jaTranslation:= TJSONArray(jdTranslation);
          for index:= 0 to Pred(jaTranslation.Count) do
          begin
            jdTranslationArray:= jaTranslation[Index];
            if (jdTranslationArray <> nil) and (jdTranslationArray.JSONType = jtArray) then
            begin
              jaTranslationArray:= TJSONArray(jdTranslationArray);
              WriteLN(Trim(jaTranslationArray[0].AsString));
            end;
          end;
        end;
      finally
        jdResponse.Free;
      end;
    end;
    

## Parsing the JSON Object based response
    
    
    uses
      {...}, fpjson, jsonparser, HTTPDefs, {...}
    
    const
      cArrayShortLanguages: Array [0..7] of String = (
        'auto',
        'en',
        'pt',
        'pl',
        'fr',
        'es',
        'it',
        'ru'
      );
      cJSONSentences = 'sentences';
      cJSONTranslation = 'trans';
      cJSONSource = 'src';
    
    procedure ParseObjectTranslate;
    var
      URL: String;
      Index: integer;
      strResponse: TJSONStringType;
      jdResponse: TJSONData;
      joTranslation, joSentence: TJSONObject;
      jaSentencesArray: TJSONArray;
    begin
      Application.ProcessMessages;
        URL:='https://translate.googleapis.com/translate_a/single?client=gtx'
          +'&q='+HTTPEncode({** PUT TEXT TO TRANSLATE HERE **})
          +'&sl='+cArrayShortLanguages[0] // Auto Detect
          +'&tl='+cArrayShortLanguages[1] // English
          +'&dt=t&dj=1' // dj=1 makes the response be a JSON Object
          +'&ie=UTF-8&oe=UTF-8'
          ;
    
        strResponse:= CallGoogleTranslate(URL);
        try
          jdResponse:= GetJSON(strResponse);
    
          if (jdResponse <> nil) and (jdResponse.JSONType = jtObject) then
          begin
            joTranslation:= TJSONObject(jdResponse);
            jaSentencesArray:= TJSONArray(joTranslation.FindPath(cJSONSentences));
            for Index:=0 to Pred(jaSentencesArray.Count) do
            begin
              joSentence:= TJSONObject(jaSentencesArray[Index]);
              WriteLN(Trim(joSentence.Get(cJSONTranslation,'')));
            end;
          end;
        finally
          jdResponse.Free;
        end;
    end;
    

## External Links

[Test Google Translate](<https://github.com/gcarreno/TestGoogleTranslate>) \- A [GitHub](<https://github.com>) repository which demonstrates this example.

---

_Source: [https://wiki.freepascal.org/Using_Google_Translate](https://web.archive.org/web/20240121091010/https://wiki.freepascal.org/Using_Google_Translate)_
