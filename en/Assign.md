# Assign

The [Borland Pascal](<Borland_Pascal.md> "Borland Pascal") [`procedure`](<Procedure.md> "Procedure") [`assign`](<https://www.freepascal.org/docs-html/rtl/system/assign.html>) associates a file variable with an external entity (e. g. a device file or a file on disk). `Assign` only operates on the program’s internal management data and has otherwise no immediate observable effect. 

## Contents

  * 1 usage
  * 2 application
  * 3 caveats
  * 4 notes



## usage

In non-ISO modes the [FPC](<FPC.md> "FPC") needs files to be explicitly associated with external entities. This can be achieved with `assign`. The first parameter is a file variable, the second parameter specifies a path. 
    
    
    program assignDemo(input, output, stdErr);
    var
    	FD: text;
    begin
    	assign(FD, '/tmp/assignDemo.txt');
    	assign(FD, '/root/.bashrc');
    end.
    

Note, you can run this program without any troubles as any user on any platform. Nonexistent path components (e. g. directories), missing privileges to eventually access the file or any path component, or unacceptable characters (usually [`chr(0)`](<Chr.md> "Chr")) do not pose a problem to `assign`. It merely operates on internal management data. 

## application

  * `Assign` is used before operating on any file variable.
        
        program assignApplication(input, output, stdErr);
        var
        	executable: file of Byte;
        begin
        	assign(executable, paramStr(0));
        	fileMode := 0; { 0 = read-only; only relevant for `file of …` variables }
        	reset(executable);
        	writeLn(paramStr(0), ' has a size of ', fileSize(executable), ' Bytes.');
        end.
        

  * `Assign` can be used to _redirect_ standard files [`input`](<https://www.freepascal.org/docs-html/rtl/system/input.html>)/[`output`](<https://www.freepascal.org/docs-html/rtl/system/output.html>). Our [Hello, World](<Hello,_World.md> "Hello, World") program needs only a few changes as highlighted:
        
        program redirectDemo(input, output, stdErr);
        const
        	blackhole = {$ifDef Linux}   '/dev/null' + {$endIf}
        	            {$ifDef Windows} 'nul'       + {$endIf}
        	            '';
        begin
        	assign(output, blackhole);
        	rewrite(output);
        	writeLn('Hello world!');
        end.
        




## caveats

  * Regardless of the utilized [string data type](<Character_and_string_types.md> "Character and string types"), `assign` can only process paths of up to 255 [Bytes](<Byte.md> "Byte"). This limitation comes from the internally used [`fileRec`](<https://www.freepascal.org/docs-html/rtl/system/filerec.html>)/[`textRec`](<https://www.freepascal.org/docs-html/rtl/system/textrec.html>). Relative paths in conjunction with working directories cannot overcome this. Cf. [FPC issue 35185](<https://gitlab.com/freepascal.org/fpc/source/-/issues/35185>).
  * Since `assign` only operates on internal management data, specifying illegal paths is accepted. It is even possible to supply an empty string as a path. The specific behavior on a file variable bearing an empty file name depends on the used I/O routine.



## notes

  * If `{$modeSwitch objPas+}` (automatically set via [`{$mode objFPC}`](<Mode_ObjFPC.md> "Mode ObjFPC") or [`{$mode Delphi}`](<Mode_Delphi.md> "Mode Delphi")), the `procedure` `assignFile` is available, too. It performs the very same task.
  * In [Extended Pascal](<Extended_Pascal.md> "Extended Pascal") a combination of `bindingType` and `bind` are usually used to achieve linking a file variable with an external entity.

---

_Source: [https://wiki.freepascal.org/Assign](https://web.archive.org/web/20240920213328/https://wiki.freepascal.org/Assign)_
