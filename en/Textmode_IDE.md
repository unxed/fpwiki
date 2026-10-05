# Textmode IDE

│ **English (en)** │    
****

[How to get the fpc ide running under Mac OS X](<How_to_get_the_fpc_ide_running_under_Mac_OS_X.md> "How to get the fpc ide running under Mac OS X")  
  
Terminals, fonts, ESC codes, and why do I get all these funny characters: [Terminal & Fonts](<Terminal_&_Fonts.md> "Terminal & Fonts")

Noticed TCRT does not give proper results in Darwin terminal.app. Using DEC control codes, some of them can be done. 

White on black: Write( ESC, '[7m' ); Bright on black: Write( ESC, '[7;1m' ); 

## Contents

  * 1 How to get a bigger textmode screen under Windows?
  * 2 How to change codepage locally under Windows?
  * 3 Physical Terminal (udev) problems
  * 4 See also



## How to get a bigger textmode screen under Windows?

Adapt the properties of the IDE shortcut (set a 7 bit wide font if needed), or, if you prefer to execute from the command line, make a batch file that contains: 
    
    
    MODE CON COLS=132 LINES=70
    fp
    

## How to change codepage locally under Windows?

This has never been confirmed, but the sequence below was recommended to a Japanese IDE user who had the default codepage set to Japanese, while the IDE requires CP437 (Mantis 9649): 

  * set the console font to Lucida Console
  * execute chcp 850
  * start the IDE



## Physical Terminal (udev) problems

From <http://bugs.freepascal.org/view.php?id=10758> : 
    
    
    There appears to be an additional wrinkle when using a user virtual terminal.
    Symptoms: The User Screen overwrites the IDE screen.
    Analysis: In ~/ide/fpusrscr.pas source code, in 'constructor TLinuxScreen.Init;', where FName = '/dev/vcsa/x'
    
    
    
    TYFd:= fpOpen(FName, &666, O_RdWr); 
    
    
    
    always returns a '-1' due to the standard udev permissions for /dev/vcc/a* .
    Brute force work-around: As root, chmod 666 /dev/vcc/a*" or edit /etc/udev/rules.d/50-udev.rules to make the permissions at boot:
    original:
    KERNEL=="vcsa[0-9]*", NAME="vcc/a%n", SYMLINK+="%k", GROUP="tty"
    to:
    KERNEL=="vcsa[0-9]*", NAME="vcc/a%n", SYMLINK+="%k", GROUP="tty", MODE="666"
    Other: Maybe there is an underlying incompatibility between grab_vcsa and udev ?
    

## See also

  * [Textmode IDE development](<Textmode_IDE_development.md> "Textmode IDE development")

---

_Source: [https://wiki.freepascal.org/Textmode_IDE](https://web.archive.org/web/20240121091352/https://wiki.freepascal.org/Textmode_IDE)_
