# Speech Synthesis

## Contents

  * 1 Overview
  * 2 Cross-platform solutions
    * 2.1 eSpeak
    * 2.2 sak
  * 3 macOS solutions
  * 4 Unix/Unix-like solutions
  * 5 Windows solutions
  * 6 See also



## Overview

Speech synthesis is the artificial production of human speech. A computer system used for this purpose is called a speech computer or speech synthesizer, and can be implemented in software or hardware products. A text-to-speech (TTS) system converts normal language text into speech; other systems render symbolic linguistic representations (phonemes) into speech. 

An intelligible text-to-speech program allows individuals with visual impairments or reading disabilities to listen to written words on their computer. This is known as an assistive technology because it allows an individual to perform a task that they would otherwise be unable to do, or increase the ease and safety with which a task can be performed, or anything that assists individuals to carry-out daily activities. 

## Cross-platform solutions

### eSpeak

[eSpeak](<espeak.md> "espeak") has 11 voices, (7 male and 4 female) and is cross platform supporting Android, FreeBSD, Linux, macOS, Solaris and Windows. It also has many [command line parameters](<espeak.md> "espeak") which can be used to further improve the speech. When coding, an eSpeak installation can be used and a [stand-alone option](<espeak.md> "espeak") is available as well. 

eSpeak does text to speech synthesis for the following languages, some better than others: Afrikaans, Albanian, Aragonese, Armenian, Bulgarian, Cantonese, Catalan, Croatian, Czech, Danish, Dutch, English, Esperanto, Estonian, Farsi, Finnish, French, Georgian, German, Greek, Hindi, Hungarian, Icelandic, Indonesian, Irish, Italian, Kannada, Kurdish, Latvian, Lithuanian, Lojban, Macedonian, Malaysian, Malayalam, Mandarin, Nepalese, Norwegian, Polish, Portuguese, Punjabi, Romanian, Russian, Serbian, Slovak, Spanish, Swahili, Swedish, Tamil, Turkish, Vietnamese, Welsh. 

### sak

[Speecher Assistive Kit](<Speecher_Assistive_Kit.md> "Speecher Assistive Kit") is based on eSpeak either as an executable or a library. 

## macOS solutions

macOS has system wide text-to-speech and screen reading functionality built in. See: 

  * [macOS Text-To-Speech](<macOS_Text-To-Speech.md> "macOS Text-To-Speech") for using text to speech directly (no screen reading).
  * [macOS VoiceOver](<LCL_Accessibility.md> "LCL Accessibility") for using screen reading.



## Unix/Unix-like solutions

On FreeBSD and Linux there are some other command line Text-To-Speech engines that may also offer access via APIs. An example is the [Festival Speech Synthesis System](<http://www.cstr.ed.ac.uk/projects/festival/>) engine. 

For Linux see also [Text-To-Speech or How to let my computer speak](<Linux_Programming_Tips.md> "Linux Programming Tips"). 

## Windows solutions

On Windows, there is Microsoft's [Speech Application Programming Interface](<SAPI.md> "SAPI") API which is used to perform text-to-speech (TTS). 

## See also

  * [Accessibility](<LCL_Accessibility.md> "LCL Accessibility") Planning and developing applications in a way that everyone will be able to use them.

---

_Source: [https://wiki.freepascal.org/Speech_Synthesis](https://web.archive.org/web/20250217104101/https://wiki.freepascal.org/Speech_Synthesis)_
