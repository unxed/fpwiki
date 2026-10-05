# PTop

## Contents

  * 1 About
  * 2 Author
  * 3 Usage
  * 4 ptop.cfg
    * 4.1 Structure
    * 4.2 Example
  * 5 Links



## About

ptop - The FPC Pascal configurable source beautifier. Name means "Pascal-TO-Pascal". 

ptop is a more or less configurable source beautifier for pascal sources and specially the ones supported by FPC. ptop belongs to the FPC utils package, which currently also contains ppdep, h2pas, ppudump and ppumove. 

## Author

Michael van Canneyt. 

## Usage

ptop basically reformats "infile" and outputs the result to "outfile". It uses a configuration file explained further below, and can generate a default configuration file for you to edit. (not needed if you use the defaults) 
    
    
     ptop [-v]  [-i  indent]  [-b  bufsize ]  [-c optsfile] infile
    

  


  * -h : Writes a short description of these switches.
  * -c configfile : Read options from configuration file. A configuration file is not needed, ptop will revert to internal defaults then. See also -g.
  * -i ident : Sets the number of indent spaces used for BEGIN END; and other blocks.
  * -b bufsize : Sets the buffer size to bufsize. Default 255, 0 is considered non-valid and ignored.
  * -v : be verbose. Currently only outputs the number of lines read/written and some error messages.
  * -g configfile : Writes a default configuration file to be edited to the file configfile



Try to play with ptop and its configfile until you find the effect you desire. The configurability and possibilities of ptop are quite large compared to shareware source beautifier found on e.g. SIMTEL. 

## ptop.cfg

This is the main configuration file of the ptop. 

The configuration file for ptop(1) isn't necessarily called ptop.cfg, and is also not auto-loaded, so the name doesn't matter much. This man-page describes the structure of such a configuration file for ptop(1) 

### Structure

The structure of a ptop configuration file is a simple buildingblock repeated several (20-30) times, for each pascal keyword known to the ptop program. (see the default configuration file or ptopu.pp source to find out which keywords are known) 

The basic building block of the configuration file consists out of one or two lines, describing how ptop should react on a certain keyword. First a line without square brackets with the following format: 
    
    
     keyword=option1,option2,option3,...
    

If one of the options is "dindonkey" (see further below), a second line (with square brackets) is needed like this: 
    
    
     [keyword]=otherkeyword1,otherkeyword2,otherkeyword3,...
    

As you can see the block contains two types of identifiers , keywords(keyword and otherkeyword1..3 in above example) and options, (option1..3 above). 

Keywords are the built-in valid Pascal structure-identifiers like BEGIN, END, CASE, IF, THEN, ELSE, IMPLEMENTATION. The default configuration file lists most of these. Besides the real Pascal keywords, some other codewords are used for operators and comment expressions. These are listed in the following table: 

codeword  Name of codeword | operator   
---|---  
casevar | : in a case label (<>'colon')   
becomes | :=   
delphicomment | //   
dopencomment | (*   
dclosecomment | *)   
opencomment | {   
closecomment | }   
semicolon | ;   
colon | :   
equals | =   
openparen | [   
closeparen | ]   
period | .   
  
The Options codewords define actions to be taken when the keyword before the equal sign is found. 

Options  Option | does what   
---|---  
crsupp | suppress CR before the keyword.   
crbefore | force CR before keyword (doesn't go with crsupp :) )   
blinbefore | blank line before keyword.   
dindonkey | de-indent on assiociated keywords (see below)   
dindent | deindent (always)   
spbef | space before   
spaft | space after   
gobsym | Print symbols which follow a keyword but which do not affect layout.   
Prints until terminators occur.  
(terminators are hard-coded in pptop, still needs changing)   
inbytab | indent by tab.   
crafter | force CR after keyword.   
upper | prints keyword all uppercase   
lower | prints keyword all lowercase   
capital | capitalizes keyword: 1st letter uppercase, rest lowercase.   
  
The option "dindonkey" requires some extra parameters, which are set by a second line for that keyword (the one with the square brackets), which is therefore only needed if the options contain "dinkdonkey" (contraction of de-indent on assiociated keyword). "dinkdonkey" deindents if any of the keywords specified by the extra options of the square-bracket line is found. 

### Example

The lines 
    
    
     else=crbefore,dindonkey,inbytab,upper
     [else]=if,then,else
    

Mean: 

The keyword this is about is else , it's on the LEFT side of both equal signs. When the ptop parser finds ELSE, the options tell it to do the following things: 

  * (crbefore) Don't allow other code on the line before the keyword. (ELSE alone on a line)
  * (dindonkey) De-indent on the keywords in square brackets line (if,then,else)
  * (inbytab) indent by tab.
  * (upper) uppercase the keyword (ELSE)



Try to play with the configfile until you find the effect you desire. The configurability and possibilities of ptop are quite large compared to shareware source beautifier found on e.g. SIMTEL. 

## Links

Other FPC utils: 

  * [ppdep](</index.php?title=ppdep&action=edit&redlink=1> "ppdep \(page does not exist\)")
  * [ppudump](</index.php?title=ppudump&action=edit&redlink=1> "ppudump \(page does not exist\)")
  * [ppumove](</index.php?title=ppumove&action=edit&redlink=1> "ppumove \(page does not exist\)")
  * [H2Pas](<H2Pas.md> "H2Pas")



Original page: 

<http://www.us.freepascal.org/tools/ptop.html>

---

_Source: [https://wiki.freepascal.org/PTop](https://web.archive.org/web/20240930153708/https://wiki.freepascal.org/PTop)_
