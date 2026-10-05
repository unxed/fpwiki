# espeak

## Contents

  * 1 Overview
  * 2 Using eSpeak command line
  * 3 Preparing to use eSpeak
  * 4 Coding for Free Pascal
  * 5 Coding for Lazarus
  * 6 See also



## Overview

[eSpeak](<http://espeak.sourceforge.net/>) is an open source Text-To-Speech (TTS) engine. It is an artificial speech synthesis software which converts text to audio. It supports a vast number of [languages](<http://espeak.sourceforge.net/languages.html>). Further languages can be developed with the help of [espeakedit](<http://espeak.sf.net/download.html>), a GUI interface for preparing and compiling phoneme data. In Windows, eSpeak implements Microsoft [SAPI](<SAPI.md> "SAPI") (Speech API). Besides Windows, it supports Mac and Linux platforms. 

eSpeak does text to speech synthesis for the following languages, some better than others: Afrikaans, Albanian, Aragonese, Armenian, Bulgarian, Cantonese, Catalan, Croatian, Czech, Danish, Dutch, English, Esperanto, Estonian, Farsi, Finnish, French, Georgian, German, Greek, Hindi, Hungarian, Icelandic, Indonesian, Irish, Italian, Kannada, Kurdish, Latvian, Lithuanian, Lojban, Macedonian, Malaysian, Malayalam, Mandarin, Nepalese, Norwegian, Polish, Portuguese, Punjabi, Romanian, Russian, Serbian, Slovak, Spanish, Swahili, Swedish, Tamil, Turkish, Vietnamese, Welsh. 

Lazarus or Freepascal has many ways of implementing it. The simplest way is to use an [sysutils.ExecuteProcess()](<http://www.freepascal.org/docs-html/rtl/sysutils/executeprocess.html>) call to execute eSpeak with command line parameters ([discussed below](<espeak.md> "espeak")). But this could result in a console window while speaking. A better solution is to use a TProcess (changing properties to poUsePipes and swoHide) to run the commands. 

## Using eSpeak command line

These are some simple examples of using the eSpeak command line. To use this [Download](<http://espeak.sourceforge.net/download.html>) and install eSpeak. Then open the command prompt and run `cd C:\Program Files\eSpeak\command_line\` then run the commands below: 
    
    
    espeak "Hello World!"
    

This is the simplest command. Speaks "Hello World!" 
    
    
    espeak -v +f2 "Hello World!"
    

Speaks the text in female voice (thus f2). There are 7 male voices (m1 to m7) and 4 female voices (f1 to f4) 
    
    
    espeak -v fr+f2 "Bonjour tout le monde"
    

Speaks the text in French accent and in a female voice. 
    
    
    espeak -g 10 "I have something to say."
    

Pauses for 10 milliseconds between words. A better understandable option. 
    
    
    espeak -s 400 "I have something to say."
    

Speaks fluently! The -s (speed) parameter could be 80 to 450. Default is 175. 
    
    
    espeak -v +whisper "I have something secret to say!"
    

Speaks the text as if in your ear!! (Notice the `+whisper` part.) 

  
More details about command line parameters can be found here: <http://espeak.sourceforge.net/commands.html>

## Preparing to use eSpeak

We can either use the installation path of eSpeak (path is retrieved from the Windows registry) or use a stand alone option (by using a copy of the installation directory). Example of the former can be found [here](<http://www.rolfware.de/delphi/espeak_example.html>) (for Delphi). For the rest of this article we will use the stand-alone option (by using `--path` parameter). 

To get the stand-alone flavor of eSpeak we will have to do the following: 

  1. [Download](<http://espeak.sourceforge.net/download.html>) and install eSpeak for windows. The file would be named something like: espeak-1.46.02-win.zip
  2. Assuming that you have installed eSpeak in `C:\Program Files\eSpeak` , copy the eSpeak directory in your freepascal/lazarus project directory.
  3. You may remove some unwanted files or folders such as sourcedict, espeaksapi.dll etc. if you want.
  4. Optionally, you may uninstall eSpeak (if you don't want to use the eSpeak installation).



Now remember to add `--path` parameter when calling eSpeak. For example: 
    
    
    espeak.exe --path=eSpeak "Hello World!"
    

## Coding for Free Pascal

Free Pascal can be used to call the eSpeak command line executable to retrieve spoken words. A simple example is given below: 
    
    
    uses sysutils
    
    ...
    
    var
      ExePath,MyText:String;
    begin
      ExePath := 'path\to\espeak\command_line\espeak.exe';
      MyText := 'Hello World!';
    
      ExecuteProcess(ExePath, ' --path=eSpeak "' + MyText + '"', []);
    

Here only `--path` parameter is used. There are other parameters that can speak a text file; set the volume, pitch, speed (words per minute), word gap etc. Info about command line parameters can be found here: <http://espeak.sourceforge.net/commands.html>

## Coding for Lazarus

For a tutorial, please see here: <http://www.lazarus.freepascal.org/index.php/topic,20153.0.html> Or here: <http://inkoflife.blogspot.com/2013/03/let-your-software-speak.html>

## See also

  * [Speech Synthesis](<Speech_Synthesis.md> "Speech Synthesis") for cross-platform and native operating system speech synthesis solutions.

---

_Source: [https://wiki.freepascal.org/espeak](https://web.archive.org/web/20250301000000/https://wiki.freepascal.org/espeak)_
