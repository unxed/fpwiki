# Localization

## Contents

  * 1 Localization of programs and applications
  * 2 Localization of compiler messages
    * 2.1 How to start
    * 2.2 Source
    * 2.3 Message file structure
  * 3 Localization of other resources



## Localization of programs and applications

FPC delivers a broad range of functions and tools that support internationalization (i18n) and localization (l10n) of your application, e.g. in the [StrUtils](<http://lazarus-ccr.sourceforge.net/docs/rtl/strutils/index.html>) and [DateUtils](<http://lazarus-ccr.sourceforge.net/docs/rtl/dateutils/index.html>) units. 

An overview of how different languages are identified is at [W3C Language-tags](<http://w3c.org/international/articles/language-tags>) Unfortunately,different OS's use different internal methods for locale and language identification. In the long term, it is to be hoped that there will be more commonality. If you are coding to allow 'po' files to be added to an application, then you probably need to allow names to range from aa.po to zzz-zzzz.po. 

Additionally, some [i18n tips](<Translations_/_i18n_/_localizations_for_programs.md> "Translations / i18n / localizations for programs") may help to localize your software. 

## Localization of compiler messages

FPC was created in a way that allows easy localization of compiler messages to different languages. Quite a few translations are already available, but taking into account the number of languages used around the world, there's always room for improvement. ;-) 

### How to start

First of all, check which languages are already supported. In the installed tree, the message files are stored in a subdirectory called "msg". In the git working copy, these files are stored under /compiler/msg/. 

Every file contains all message in a particular language with a particular character encoding or "code page" (note that different platforms use different ways of encoding characters). As of now, the compiler has no restrictions regarding how the individual files are named, but the convention is that all files start with "error", followed with one or two letters identifying the particular language and then optionally followed with another letter used to distinguish the supported encoding (e.g. "w" for Windows, "i" for ISO character sets used for Unix platforms, or possibly "d" for the so called IBM or OEM codepages used with DOS and OS/2). All message files have an extension ".msg". 

If you don't find a message file for your language, or if you find that the existing one looks outdated, you can send an e-mail to fpc-devel mailing list stating that you are interested in providing translation to a new language. This e-mail is recommended to avoid having two people working on translation to the same language in parallel without knowing about each other. Once you're finished, you can send another e-mail to fpc-devel announcing so. You should be contacted by one of the FPC core team members shortly afterwards. He will provide you with instructions where to send the finished file (the mailing list has limits for maximum size of attachments) and then add the file to the git repository or you can also fill a GitLab merge request if you are familiar with GitLab. 

### Source

The primary language is English, which means that errore.msg is always the most up to date message file. Although you could use a different language as your basis, it isn't recommended (you can still use some other language as a reference if a particular message in English isn't clear to you and you know some other language better, of course). In addition, it's recommended to always start with the latest revision of errore.msg from trunk branch in SVN repository rather than e.g. the message file from some previous FPC release (which might be already several months old at that time). 

If you cannot use a Git client for accessing our Git repository for some reason, you can always use the GitLab WWW interface as an alternative. Some hints for using this interface follow: 

[List of message files](<https://gitlab.com/freepascal.org/fpc/source/-/tree/main/compiler/msg>)
    You can see the existing message files and information about their most recent update (hash and age) here. Clicking on the message file name provides you with more information about this file.

Outdated due to switch to git 

[Differences between two revisions](<http://svn.freepascal.org/cgi-bin/viewvc.cgi/trunk/compiler/msg/errore.msg?rev=3940&r1=1&r2=3940>)
    This link shows all changes performed in errore.msg between SVN revision 1 (roughly corresponding to FPC release 2.0.0) and revision 3940 (the latest SVN revision as of writing this text). Changing the revision numbers and/or file name in the link allows you to display changes for other revisions/files. As you can see, it's very useful to know the SVN revision number of the errore.msg file used as basis for your translation (and preferably put it to comments on the top of the translated file together with your contact information) - when updating a previously created message file, one can easily check changes in the English version since that revision and translate just the modified parts.

### Message file structure

Every message file consists of three types of lines: 

Comments
    All lines starting with "#", these are obviously not expected to be translated at all.
Documentation
    Lines starting with "%" provide description for individual messages for FPC documentation (immediately following the description). There's no need to translate these lines unless you plan to translate the complete documentation too (probably not - see below).
Messages
    All other lines are the real messages. These start with an identifier (not translated) followed with "=", message number, "_", optionally one letter specifying the verbosity level (error, warning, hint, note, information, etc.) and another "_" and then the real text. Special case are multi-line messages, these start with "[", end with "]" and all text in between belongs to that message (used e.g. for help).

Additional information regarding the format can be found as comments directly within the message files. 

## Localization of other resources

There are other texts related to FPC which can be translated to other languages: 

  * [RTL texts](<RTL.md> "RTL") (especially units rtlconsts and sysconst)
  * [Free Vision](<Free_Vision.md> "Free Vision") (strtxt.inc)
  * [IDE](<Textmode_IDE.md> "Textmode IDE") (fpstre.inc)
  * Possibly documentation (note that this is a **huge** task - FPC documentation is very extensive)
  * Localization of Lazarus

---

_Source: [https://wiki.freepascal.org/Localization](https://web.archive.org/web/20230603090028/https://wiki.freepascal.org/Localization)_
