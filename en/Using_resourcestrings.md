# Using resourcestrings

│ **English (en)** │  **[español (es)](</Using_resourcestrings/es> "Using resourcestrings/es")** │  **[Bahasa Indonesia (id)](</Using_resourcestrings/id> "Using resourcestrings/id")** │  **[русский (ru)](<../ru/Using_resourcestrings.md> "Using resourcestrings/ru")** │    
****

Resource strings provide a mechanism to internationalize (and to some degree localize) your application. 

## Contents

  * 1 rationale
  * 2 common steps
  * 3 GNU get text
    * 3.1 limitations
    * 3.2 procedure
  * 4 resource DLLs
  * 5 message files
  * 6 alternatives
  * 7 history
  * 8 see also



## rationale

Resource strings are constants with an extra level of indirection. The default value (as written in your [source code](<Source_code.md> "Source code")) is stored in the executable program. That means it is always available, even if loading the translation fails. 

## common steps

The first step is to _use_ resource strings for every string that potentially needs translation. In this tutorial we want to internationalize our Hello World program: 
    
    
    program helloWorld(input, output, stdErr);
    resourceString
    	hello = 'Hello world';
    begin
    	writeLn(hello);
    end.
    

The [FPC](<FPC.md> "FPC") creates a file ending in `.rsj` (resource string [JSON](<JSON.md> "JSON")) for every compiled module containing a `resourceString` section. For the above program it looks like this (pretty-format added): 
    
    
    {
      "version": 1,
      "strings": [
        {
          "hash": 20229140,
          "name": "helloworld.hello",
          "sourcebytes": [72, 101, 108, 108, 111, 32, 119, 111, 114, 108, 100],
          "value": "Hello world"
        }
      ]
    }
    

This intermediate file format was created, so the compiler does not need any knowledge of the utilized translation tool. It just needs to create `.rsj` files. 

The following steps differ on the used internationalization mechanism. As of 2021 the FPC only supports [GNU](<GNU.md> "GNU") gettext entirely. The reason gettext was chosen is that it is more or less standard on Unix-like systems, like [Linux](<Linux.md> "Linux") or [FreeBSD](<FreeBSD.md> "FreeBSD"). 

## GNU get text

### limitations

As mentioned, GNU gettext is widely available on Unix-like systems. However, gettext is horribly inefficient. Also, gettext is context insensitive, that means it operates on the string itself: Sometimes the same word/sentence must be translated differently according to the context, but this is not possible. 

### procedure

  1. Convert the `.rsj` file into GNU gettext’s portable object format:
         
         rstconv -f po -i helloWorld.rsj -o helloWorld.po
         

  2. Create a working copy of the generated file, e. g.
         
         cp helloWorld.po helloWorld-de.po
         

and translate it with your favorite translation tool or simply with a text editor.
         
         #: helloworld:hello
         msgid "Hello world"
         msgstr "Hallo Welt"
         

  3. Convert the PO file to an MO (machine object) file using gettext’s utility `msgfmt`:
         
         msgfmt -o helloWorld-de.mo helloWorld-de.po
         

  4. Integrate gettext in your program:
         
         program helloWorld(input, output, stdErr);
         uses
         	gettext;
         resourceString
         	hello = 'Hello world';
         begin
         	translateResourceStrings('helloWorld-%s.mo');
         	writeLn(hello);
         end.
         

The `%s` is substituted with the locale’s name. Examining an `strace` you will see, that for example first `helloWorld‑de.mo` is attempted to open. If it does not exist, it tries `helloWorld‑de_DE.mo`. If neither file exists, the resource string’s default values as specified in your source code are used.
  5. Run the program and celebrate your achievement:
         
         LANG='de_DE.UTF-8' ./helloWorld
         




For more details see the [gettext unit documentation](<https://www.freepascal.org/docs-html/fcl/gettext/index.html>). 

## resource DLLs

The FPC’s standard RTL does not yet contain portable functions for loading texts from resource [DLLs](<DLL.md> "DLL") containing translations (as [Delphi](<Delphi.md> "Delphi") does), but the first step would be: 

  1. Convert the `.rsj` file into a resource compiler script:
         
         rstconv -f rc -i helloWorld.rsj -o helloWorld.rc
         




## message files

The FPC currently does not support this, but the first step would be: 

  1. Convert the `.rsj` file in an IBM OS/2 message file:
         
         rstconv -f msg -c HWX -i helloWorld.rsj -o helloWorld.msg
         

The component identifier indicated by `‑c` may be any three upper-case [ASCII](<ASCII.md> "ASCII") letters, but it should be unique within your project.



## alternatives

To implement another mechanism, 3 things are needed: 

  1. Update rstconv so it can output another format.
  2. Tools to manipulate the other format.
  3. Implement a unit that loads the other format at [run-time](<runtime.md> "runtime").



[Routines](<Routine.md> "Routine") to directly manipulate a program’s resource string tables, i. e. the extra level of indirection for resource strings, were available in the objpas unit [until revision 31687](<https://gitlab.com/freepascal.org/fpc/source//commit/4494565a154b1c6b313c9ddc3a09f7102719c5b3>). 

## history

Formerly, the FPC created text files ending in `.rst`. While changing to the JSON format, the name of the [`rstconv` utility](<https://www.freepascal.org/docs-html/user/userse45.html>) was preserved. `rstconv` can still convert `.rst` files. 
    
    
    # hash value = 20229140
    helloworld.hello='Hello world'
    

## see also

  * [Step-by-step instructions for creating multi-language applications](<Step-by-step_instructions_for_creating_multi-language_applications.md> "Step-by-step instructions for creating multi-language applications")

---

_Source: [https://wiki.freepascal.org/Using_resourcestrings](https://web.archive.org/web/20250512121134/https://wiki.freepascal.org/Using_resourcestrings)_
