# spelling

│ **English (en)** │  **[русский (ru)](<../ru/spelling.md>)** │

## Contents

  * 1 Using Hunspell with Lazarus
  * 2 About Hunspell
  * 3 Note about dictionaries
  * 4 Platform Specific
    * 4.1 Linux
    * 4.2 Windows
    * 4.3 macOS
  * 5 The Hunspell Unit
    * 5.1 Demo 2 in a Full GUI
    * 5.2 Demo 3 - Lazspell - TMemo
    * 5.3 Demo 4 - Lazspell 2 - TRichMemo
    * 5.4 Demo 5 - Simple spelling checker
  * 6 Further Reading and Links



## Using Hunspell with Lazarus

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** This page is current as of August, 2018 but things change...

This page is about using the Hunspell library with Lazarus. It outlines a model that does, sort of, work. You will almost certainly need to make some changes for your specific purposes but hopefully, this page will give you a good start. 

Firstly, in the forum, there are several references to some code that will work with the Hunspell library. The hunspell.pas unit is based heavily on these blocks of code. Most have no license information and an assumption is being made that they are "common knowledge" and thus free of any constraint. I have added a bit that manages the problem of finding the library and dictionary files. And established a reasonable interface. 

Further, user **rvk** from the forum has build a windows 64 bit DLL as there seemed little alternative for Windows users. 

## About Hunspell

Hunspell is an active open source project, distributed under the Mozilla Public License. The hunspell library is used in products like Libra Office, Open Office and Firefox. Its can be made work on Windows, Linux and Mac (and probably heaps of other platforms). See the platform Specific pages below. Dictionaries for Hunspell are readily available and may already be installed on many machines. Even if you cannot access another application's library, you can use its dictionary. 

Dictionaries for Hunspell come as a *.dic and *.aff file pair. For example, the Australian dictionary consists of en_AU.dic and en_AU.aff . The 'en' indicates its english and the 'AU' says its specifically for Australia. As an English speaker, I note that the en_US dictionaries always seem to be installed and I add the Australian ones. I don't know how widespread this pattern is in non English speaking systems. You can find dictionaries here <https://github.com/LibreOffice/dictionaries> and also some information related to it here <https://wiki.documentfoundation.org/Development/Dictionaries>

## Note about dictionaries

In some cases in order to display the suggested words correctly the dictionaries must be encoded in UTF-8. If you have notice that some words are not shown correctly--for example, 'apple' in Polish is 'jabłko' but you get 'jab�ko'--it means that dictionary pl_PL.aff and pl_PL.dic must be converted in UTF-8. To convert dictionaries into UTF-8 in Linux Debian (it should work in any Linux, converted dictionaries can be used in Windows, check also [this forum thread](<http://forum.lazarus.freepascal.org/index.php/topic,44298.0.html>)) follow below steps: 

1\. Start terminal as root 
    
    
      su
    

2\. If not yet installed, install hunspell (hunspell-pl is for Polish dictionary--more at [Debian Hunspell package](<https://packages.debian.org/search?keywords=hunspell>)) 
    
    
      apt-get install hunspell
      apt-get install hunspell-pl
    

3\. Go to /usr/share/hunspell/ where dictionaries are kept and create folder dic 
    
    
      cd /usr/share/hunspell/
      mkdir dic
    

4\. Convert for example dictionary pl_PL.aff and pl_PL.dic. Use [ISO-8859-2](<https://en.wikipedia.org/wiki/ISO/IEC_8859-2>) (for Eastern Europe), [ISO-8859-1](<https://en.wikipedia.org/wiki/ISO/IEC_8859-1>) (or ISO-8859-15) for Western Europe, ISO-8859-5 for (Russian?), etc. 
    
    
      iconv -f ISO-8859-2 -t UTF-8 /usr/share/hunspell/pl_PL.aff | sed 's/^SET ISO8859-2$/SET UTF-8/g' > dic/pl_PL.aff
      iconv -f ISO-8859-2 -t UTF-8 /usr/share/hunspell/pl_PL.dic > dic/pl_PL.dic
    

5\. Copy your 'dic' folder or dictionaries from 'dic' folder to your application. 

## Platform Specific

### Linux

Many Linux distributions will have Hunspell installed by default along with the appropriate language dictionaries. If not, its probably just a case of using the distribution's package manager. If all else fails, grab the source from the hunspell GitHub site and build it yourself. Linux users are like that. 

To see if you do in fact have a hunspell library installed, try this command. 
    
    
     ldconfig -p | grep hunspell
    

Similarly, you can probably find some dictionaries with 
    
    
     ls -l /usr/share/hunspell
    

If that does not work, try 
    
    
     find /usr -name *.aff
    

It will take a bit longer... 

### Windows

Installing the hunspell library on Windows is more of an issue. There is apparently no pre compiled 'kit' available and most Windows apps that use Hunspell appear to bind it staticly thus no hunspell.dll left lying around for you to use. But just to be sure, try searching for `*hunspell*.dll`. The Hunspell GitHub site lists a recipe to build one but it involves installing MSYS2 and is quite involved. The resulting DLL also needs a couple of GCC DLLs along as well. 

Fortunately user **rvk** on the Lazarus Forum has built us a nice statically (ie stand alone) linked DLL using Microsoft Visual Studio Community 2015. As such, you can use and distribute this DLL with your program subject to the Mozilla Public License. 

You will find this DLL bundled with its license file in <https://github.com/davidbannon/hunspell4pas>, click 'DLL', click the dll file, and you will get a 'Download' button. Don't forget to also get the license file, it should be distributed, one way or another with your app. 

### macOS

The author's Mac appears to have had the Hunspell library installed when Sierra was installed. But maybe, just maybe, it came along with Firefox. I'd like some feed back .... [already installed on Mojave and Catalina] 

![Light bulb](https://upload.wikimedia.org/wikipedia/commons/d/d8/Nuvola_apps_ktip.png) **Note:** See <https://opensource.apple.com/tarballs/hunspell/> for source download for v1.2.8.

To see if you aleady have a hunspell library installed, try this command 
    
    
     find / 2>&1 | grep "\hunspell"
    

It will run for some time, depending on how many files on your system. It will likely find several files including some in your XCode directory. However, end users will probably not have XCode installed. The one particularly interesting file to me was 
    
    
     /usr/lib/libhunspell-1.2.dylib
    

Version 1.2 is a bit older than elsewhere but it worked fine. If you don't find a usable library, I suggest you install one using a package manager like MacPorts, Fink or brew. Next issue issue is you will need some dictionaries. Similar command, 
    
    
     find / 2>&1 | grep "\.aff"
    

again, slow, it searches your whole disk. I found 
    
    
     /Applications/Firefox.app/Contents/Resources/dictionaries/en-US.aff
    

And a quick 'ls' assured me that there was a matching 'en-US.dic' files so all good. A more experienced Mac user might like to suggest better search strategies. Please ! 

## The Hunspell Unit

When this page was initially created (2017 ?) I made a serious mistake and used some code that did not have a clear license or history. It has been suggested that by doing so, I breached the Hunspell license as has anyone who has used it since then. 

To correct this situation, I have now released a new version of the Hunspell Wrapper that has a quite small part, just the function definitions, with the Hunspell (License: MPL 1.1/GPL 2.0/LGPL 2.1) and the Hunspell Unit (License: The Clear BSD License) in seperate files. From a coding point of view, its almost identical to what used to be here. 

Please see <https://github.com/davidbannon/hunspell4pas>

I have removed the hunspell wrapper that used to live on this page. The one on Github above is a drop in replacement, you need hunspell.pas and hunspell.inc. The remainder of the file make a basic test/demo command line Lazarus project that replaces Demo 1. 

### Demo 2 in a Full GUI

A Lazarus GUI demo makes a lot more sense and has been tested on Linux, Mac and Windows. But it is a bit harder to copy and paste. 

For this demo, you'll need a form with two TMemos, Memo1 and MemoMsg. A button, ButtonSpell, a Tlistbox, Listbox1. Make the following event handlers : FormCreate for the main form; a double click for the listbox and click on the Button. 

First you must create the hunspell object and see if it found a library its own way, here is an example in the FormCreate() method .... 
    
    
    uses hunspell;       // get hunspell.pas and hunspell.inc from  https://github.com/davidbannon/hunspell4pas
    var
        Form1: TForm1;
        Sp: THunspell;
        DictPath : AnsiString;              
    
    procedure TForm1.FormCreate(Sender: TObject);
    begin
        SetDefaultDicPath();
        Sp := THunspell.Create(True);
        if Sp.ErrorMessage = '' then begin
            MemoMsg.append('Library Loaded =' + Sp.LibraryFullName);
            ButtonSpell.enabled := CheckForDict();
        end else
            MemoMsg.append(SP.ErrorMessage);
    end;
    

In this example, we are writing some status message to MemoMsg, its an easy way to see whats happening. The ButtonSpell is NOT enabled until the dictionaries have been set. Wait for it .... 

We now need two methods, one reads a nominated directory looking for any likely dictionary files, the other manages the decisions. If we find just one dictionary set, use it, if we find none, complain. But if we find several, and thats most likely, we have to ask the user which dictionary (ie language) they wish to use. 
    
    
    function TForm1.FindDictionary(const Dict : TStrings; const DPath : AnsiString) : boolean;
    var
        Info : TSearchRec;
    begin
        Dict.Clear;
        if FindFirst(AppendPathDelim(DPath) + '*.dic', faAnyFile and faDirectory, Info)=0 then begin
            repeat
                Dict.Add(Info.Name);
            until FindNext(Info) <> 0;
        end;
        FindClose(Info);
        Result := Dict.Count >= 1;
    end;
    
    function TForm1.CheckForDict() : boolean;
    begin
        Result := False;
        EditDictPath.Caption := DictPathAlt;
        if not FindDictionary(ListBox1.Items, DictPath) then
            MemoMsg.Append('ERROR - no dictionaries found in ' + DictPath);
        if ListBox1.Items.Count = 1 then begin  // Exactly one returned.
            if not Sp.SetDictionary( AppendPathDelim(DictPath) + 
                                     ListBox1.Items.Strings[0]) 
            then
                MemoMsg.Append('ERROR ' + SP.ErrorMessage)
            else
                MemoMsg.Append('Dictionary set to ' + DictPath + 
                                ListBox1.Items.Strings[0]);
        end;
        Result := SP.GoodToGo;   // only true if count was exactly one or FindDict failed and nothing changed
    end;
    

Ah, you say, but where are we looking for the dictionaries ? Sadly, I don't have a good solution for that. Here is where I found mine - 
    
    
    procedure TForm1.SetDefaultDicPath();
    begin
        {$ifdef LINUX}
        DictPath := '/usr/share/hunspell/';
        {$ENDIF}
        {$ifdef WINDOWS}
        DictPath := ExtractFilePath(Application.ExeName);
        //DictPath := 'C:\Program Files\LibreOffice 5\share\extensions\dict-en\';
        {$ENDIF}
        {$ifdef DARWIN}
        DictPath := '/Applications/Firefox.app/Contents/Resources/dictionaries/';
        //DictPathAlt := ExtractFilePath(Application.ExeName);
        {$endif}
    end;
    

Maybe, if other users contribute where they found usable hunspell dictionaries, we can build a list for each platform. Or just take the easy way out and tell user to get some dictionaries and put them into the application directory on Windows and Mac. Your thoughts very welcome.... 

So far, if there is exactly one dictionary set in the indicated directory, all good. But what if there are several ? Our list box has a list of them, if the user double clicks one, they trigger this method - 
    
    
    procedure TForm1.ListBox1DblClick(Sender: TObject);
    begin
        if ListBox1.ItemIndex > -1 then
            ButtonSpell.enabled := Sp.SetDictionary( AppendPathDelim(DictPath) +
                                                     ListBox1.Items.Strings[ListBox1.ItemIndex]);
        if SP.ErrorMessage = '' then begin
            MemoMsg.Append('Good To Go =' + booltostr(Sp.GoodToGo, True));
            MemoMsg.Append('Dictionary set to ' + AppendPathDelim(DictPath) + 
                            ListBox1.Items.Strings[ListBox1.ItemIndex]);
        end else
            MemoMsg.append('ERROR ' + SP.ErrorMessage);
    end;
    

Assuming we now have everything "good to go" we can press the Spell button and trigger this - 
    
    
    procedure TForm1.ButtonSpellClick(Sender: TObject);
    begin
        if not Sp.Spell(Edit1.text) then begin
            Memo1.Lines.BeginUpdate;
            Sp.Suggest('badspeller', Memo1.lines);
            Memo1.Lines.EndUpdate;
        end else
            Memo1.Lines.Clear;
    end;
    

Memo1 now contains some suggestions for better ways to spell badspeller ! 

Important ! Don't forget to free up our hunspeller object, memory leaks are evil ! 
    
    
    procedure TForm1.FormDestroy(Sender: TObject);
    begin
        Sp.free;
        Sp := nil;
    end;
    

This module will do considerably more but its presented here in its most stripped down form for readability. 

_A note to people new to Lazarus, methods with "(Sender: TObject)" shown above cannot just be pasted into your source, use the Form's Object Inspector to create the events first, then paste my sample code into the method._

### Demo 3 - Lazspell - TMemo

In this example TMemo component was used. 

[![lazspell.gif](https://wiki.freepascal.org/images/7/71/lazspell.gif)](</File:lazspell.gif>)

Lazspell - an example of spelling checker - from <https://github.com/Raf20076/Lazspell>

### Demo 4 - Lazspell 2 - TRichMemo

In this example TRichMemo component was used. 

[![lazspell-ver2.gif](https://wiki.freepascal.org/images/3/36/lazspell-ver2.gif)](</File:lazspell-ver2.gif>)

Lazspell 2 version 2 - an example of spelling checker - from <https://github.com/Raf20076/Lazspell-2-version-2>

### Demo 5 - Simple spelling checker

1\. Start Lazarus IDE 

2\. Click Project -> New Project -> Choose -> Application 
    
    
      You have just created a new application. Now save it 
    

3\. Click File -> Save as 
    
    
      Choose a folder where your application will be saved
      First, project1.lpi will be saved, then unit1.pas
    

4\. Place hunspell.pas and hunspell.inc (from <https://github.com/davidbannon/hunspell4pas>) in your folder. 

5\. Place libhunspell.dll in your application folder (you can download it from here <https://github.com/Raf20076/Lazspell> or <https://github.com/davidbannon/hunspell4pas>) any options or compile by yourself. 

6\. Place dictionary in your application folder, (two files) like pl_PL.aff, pl_PL.dic. Dictionaries must be encoded in UTF8. You can download them from here <https://github.com/Raf20076/Lazspell/tree/master/dict>

7\. On Form1 place component TButton (Button1) from Standard Tab 

8\. On Form1 place component TMemo (Memo1) from Standard Tab 

9\. On Form1 place component TListbox (ListBox1) from Standard Tab (here misspell words will be shown) 

10\. Click once Button1 on Form1 then go to Object Inspector, click Events tab, and double click next to OnClick event: it creates OnClick event and paste code (see the code of the whole application) starts from 
    
    
       
    {Check spelling}
     var
         i : Integer;
         MAX : Integer;
         FillInArrayWithWords:TStringArray;
         FillInString1: String;
         FillInString2: String;
    

till 
    
    
    //If word is not in dictionary show it in Listbox as an error
    

11\. Go to Object Inspector and click Form1, then click Events tab then double click next to OnCreate it creates OnCreate event and paste code (see the code of the whole application) 
    
    
       SpellCheck := THunspell.Create(True);
       SpellCheck.SetDictionary('pl_PL.dic');//Load dictionary
       SpellCheck.GoodToGo := True;
    

12\. Still in ObjectInspector click Form1,then click Events tab then double click next to OnDestroy it creates OnDestroy event and paste code (see the code of the whole application) 
    
    
     SpellCheck.free;
     SpellCheck := nil;
    

13\. Then in your code place functions like ArrayValueCount etc 

See Below the code of the whole application to compare with your one. 
    
    
    //SpellChecker by Raf20076, Poland 2019
    
    unit Unit1;
    
    {$mode objfpc}{$H+}
    
    interface
    
    uses
      Classes, SysUtils, Forms, Controls, Graphics, Dialogs, StdCtrls,
      LazFileUtils, LCLProc, LazUtils, LazUtf8;
    
    type
      TForm1 = class(TForm)
        Button1: TButton;
        Label1: TLabel;
        Label2: TLabel;
        ListBox1: TListBox;
        ListBox2: TListBox;
        Memo1: TMemo;
        procedure Button1Click(Sender: TObject);
        procedure FormCreate(Sender: TObject);
        procedure FormDestroy(Sender: TObject);
      end;
    
    var
      Form1: TForm1;
    
    implementation
    
    uses hunspell;//place hunspell.pas and hunspell .inc in your application folder
                  //from https://github.com/davidbannon/hunspell4pas
                  
    var
      SpellCheck: THunspell;
    
    {$R *.lfm}
    
    procedure TForm1.FormCreate(Sender: TObject);
    begin
       SpellCheck := THunspell.Create(True);
       SpellCheck.SetDictionary('pl_PL.dic');//Load dictionary
       SpellCheck.GoodToGo := True;
    end;
    
    {Extract words from string: non characters, spaces, carriagereturn aware}
    
    function ArrayValueCount(const InputArray: Array of string): Integer;
    {Count elements in array}
    var
      i:Integer;
    begin
      result := 0;
      for i := low(InputArray) to high(InputArray) do
        if InputArray[i] <> ' ' then  // 'between them one space'
          inc(result);
    end;
    
    function StripOffNonCharacter(const aString: string): string;
    {Remove non characters from string}
    var
      a: Char;
    begin
      Result := '';
      for a in aString do begin //below punctuation marks, numbers to remove from string
          if not CharInSet( a, ['.', ',', ';', ':', '!', '/',
          '?', '@', '#', '$', '%', '&', '*', '(', ')', '{',
          '}', '[', ']', '-', '\', '|', '<', '>', '''', '"', '^',
          '1', '2', '3', '4', '5', '6', '7', '8', '9', '0', '_', '+',
          '=', '~']) then //'„', '”'])these marks make error: Ordinal expression expected, need to be fixed
          begin
            Result := Result + a;
          end;
      end;
    end;
    
    
    function ReplaceCarriageReturn(s: string) : string;
    {Replace carriagereturn with one space}
    var
      i: Integer;
    begin
      Result:=s;
      for i := 1 to Length(Result) do
        if Result[i] in [#3..#13] then
          Result[i] := ' ';//'Between them one space'
    end;
    
    
    procedure TForm1.Button1Click(Sender: TObject);
    {Check spelling}
    var
         i : Integer;
         MAX : Integer;
         FillInArrayWithWords:TStringArray;
         FillInString1: String;
         FillInString2: String;
    begin
        ListBox1.clear;
        ListBox1.Items.Clear;
    
        FillInString1 := ReplaceCarriageReturn(Memo1.Lines.Text);//take text from Memo1; replace carriage return
        //with one space (using ReplaceCarriageReturn function) and put into FillInString1
    
        FillInString2 := StripOffNonCharacter(FillInString1);//remove all non characters from FillInString1
        //(using StripOffNonCharacter function)  and put string without non charachters into FillInString2
    
        FillInArrayWithWords := FillInString2.split(' '); //Split string into words with ' ' one space
        //(using .split) and put separate words into array FillInArrayWithWords
    
        MAX := ArrayValueCount(FillInArrayWithWords); //Give how many elements are in array
        //(using function ArrayValueCount)
    
        for i := 0 to MAX -1 do
            if not SpellCheck.Spell(FillInArrayWithWords[i]) then 
                ListBox1.Items.add(FillInArrayWithWords[i]);
        //Take word from array and check in dictionary through hunspell (using SpellCheck.Spell function)
        //If word is not in dictionary show it in Listbox as an error
    end;
    
    procedure TForm1.FormDestroy(Sender: TObject);
    begin
        SpellCheck.free;
        SpellCheck := nil;
    end;
    
    end.
    

When you run your application and type some text in Memo and then press the button, the application will check if any errors are found, if found then shows them in ListBox. The main problem was how to remove carriage return and non characters and then how to split text into separate words. It's not an easy task. Therefore there are three functions to do this:ReplaceCarriageReturn function, StripOffNonCharacter function and FillInString2.split , function. The FillInString2.split function actually uses .split function from SysUtils. Hopefully the code of the whole application is self explanatory. 

## Further Reading and Links

  * <https://github.com/hunspell/hunspell>
  * <https://github.com/davidbannon/hunspell4pas> \- the (correctly licensed) hunspell object pascal unit.
  * <https://github.com/Homebrew> \- probably sensible way to get hunspell on your Mac if it's not already there.
  * <https://github.com/tomboy-notes/tomboy-ng/releases> \- Contains The 64bit Windows DLL in a tomboy-ng_win64_<ver>.zip
  * <https://github.com/cutec-chris/hunspell>
  * <https://forum.lazarus.freepascal.org/index.php/topic,50474.0.html> \- FastHighliter code to use for highlighting misspelt words
  * <https://github.com/Raf20076> \- Lazspell is an example of simple spelling checker application written in Lazarus IDE.
  * <https://forum.lazarus.freepascal.org/index.php/topic,46233.0.html>
  * <https://forum.lazarus.freepascal.org/index.php/topic,51569.0.html>
  * <https://forum.lazarus.freepascal.org/index.php?topic=44298.0>

---

_Source: [https://wiki.freepascal.org/spelling](https://web.archive.org/web/20250513073210/https://wiki.freepascal.org/spelling)_
