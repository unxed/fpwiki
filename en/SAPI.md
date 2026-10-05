# SAPI

## Contents

  * 1 What is it
  * 2 Use with Free Pascal
  * 3 Assigning text to a variable. A WideString must used.
  * 4 Windows, Linux, OSX alternatives
  * 5 Linux/Unix alternatives
  * 6 OSX alternatives
  * 7 See also



## What is it

SAPI stands for Speech Application Programming Interface, a Microsoft Windows API used to perform text-to-speech (TTS). 

## Use with Free Pascal

Lazarus / Free Pascal can use this interface to perform TTS. Source: forum post: [[1]](<http://lazarus.freepascal.org/index.php/topic,17852.0>)

On Windows Vista and above, you will run in trouble with the FPU interrupt mask (see [[2]](<http://stackoverflow.com/questions/3032739/delphi-sapi-text-to-speech>)). The code dealing with SavedCW is meant to work around this. 
    
    
    uses
      ...,comobj;
    var
      SavedCW: Word;
      SpVoice: Variant;
    begin
      SpVoice := CreateOleObject('SAPI.SpVoice');
      // Change FPU interrupt mask to avoid SIGFPE exceptions
      SavedCW := Get8087CW;
      try
        Set8087CW(SavedCW or $4);
        SpVoice.Speak('hi', 0);
      finally
        // Restore FPU mask
        Set8087CW(SavedCW);
      end;

For the options available with SpVoice.Speak and other SpVoice methods see <http://msdn.microsoft.com/en-us/library/ms723609(v=vs.85).aspx>. 

[![Warning-icon.png](https://wiki.freepascal.org/images/b/b2/Warning-icon.png)](</File:Warning-icon.png>)

**Warning:** The OLE object created in above snippet will be destroyed automatically when the SpVoice variant goes out of scope. For larger texts the use of the asynchronous mode allows to keep your application reactive while speak is running. In that case it is especially important to control carefully the lifetime of the object and store it in a global or class variable. Destroying the object while speak is still running can cause crashes.

Changing the FPU interrupt mask can be done at any moment before loading the ole object and if your application is not using any floating point arithmetic there is no need to reset the FPU interrupt mask to its original value. 

  


## Assigning text to a variable. A WideString must used.
    
    
    File:procedure TForm1.Button1Click(Sender: TObject);
    Var
      SpVoice1: Variant;
      VoiceString: WideString; // WideString must be used to assign variable for speech to function, can be Global.
    begin
      SpVoice1 := CreateOleObject('SAPI.SpVoice'); // Can be assigned in form.create
      VoiceString := Button1.Caption;              // variable assignment 
      SpVoice1.Speak(VoiceString,0);
    end;

Thank you nsunny from 2013 post. 

## Windows, Linux, OSX alternatives

An alternative on Windows is to use [espeak](<espeak.md> "espeak"). eSpeak has 11 voices, (7 male and 4 female) and is cross platform supporting Windows, Mac and Linux. It also has many [command line parameters](<espeak.md> "espeak") which can be used to further improve the speech. When coding, an eSpeak installation can be used and a [stand-alone option](<espeak.md> "espeak") is available as well. 

For a code example, please see the [espeak](<espeak.md> "espeak") article. 

## Linux/Unix alternatives

Apart from [espeak](<espeak.md> "espeak"), on Linux/Unix, there are some other command line Text To Speech engines, that may also offer access via APIs. An example is the festival engine. 

See [Linux Programming Tips#Perform text-to-speech .28TTS.29 or how to let my computer speak](<Linux_Programming_Tips.md> "Linux Programming Tips")

## OSX alternatives

OSX has a system wide text to speech functionality built in: see [Mac OS X Text-to-Speech](<Mac_OS_X_Text-to-Speech.md> "Mac OS X Text-to-Speech") for using text to speech directly (no screenreading) 

There's also a screenreader, VoiceOver. See [LCL_Accessibility#Mac OS X VoiceOver](<LCL_Accessibility.md> "LCL Accessibility")

## See also

  * [espeak](<espeak.md> "espeak") \- Cross platform free Text to Speech engine

---

_Source: [https://wiki.freepascal.org/SAPI](https://web.archive.org/web/20220101000000/https://wiki.freepascal.org/SAPI)_
