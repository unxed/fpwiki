# Message files

│ **English (en)** │  **[русский (ru)](<../ru/Message_files.md>)** │

back to contents [FPC internals](<FPC_internals.md> "FPC internals")

## Basics

The compiler uses the files in fpc/compiler/msg to output translated error message. The default one, errore.msg, is compiled into the compiler, so it will work without any message file available. 

To add a new message, find the appropriate section (parser_, scan_ etc.) in errore.msg and add the new message at the end of this section with a new succeeding message id. Then rebuild the built-in message tables with 
    
    
    cd fpc/compiler; make msg
    

and commit the resulting msg*.inc files as well as the changed errore.msg file. The translators are responsible to add the new messages to the translated files. 

## Translating

In order to get a starting point for updating the message files of other languages, one can use the program msgdif from fpc/compiler/utils. It compares two message files and outputs problems as well as a file new.msg, which you can take as a starting point for an update. 

Example: 
    
    
    msgdif errore.msg errorr.msg

---

_Source: [https://wiki.freepascal.org/Message_files](https://web.archive.org/web/20210615090030/https://wiki.freepascal.org/Message_files)_
