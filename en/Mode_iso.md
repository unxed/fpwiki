# Mode iso

│ **English (en)** │  **[français (fr)](</Mode_iso/fr> "Mode iso/fr")** │  **[русский (ru)](<../ru/Mode_iso.md> "Mode iso/ru")** │    
****

[FPC](<FPC.md> "FPC")’s [compatibility mode](<Compiler_Mode.md> "Compiler Mode") **`{$mode ISO}`** _intends_ to comply with the requirements of level 0 and 1 of the ISO/IEC standard 7185. It became available in [version 2.6.0](<FPC_New_Features_2.6.md> "FPC New Features 2.6.0"). The International Organization for Standardization standard 7185 is also known as [Standard “Unextended” Pascal](<Standard_Pascal.md> "Standard Pascal"). 

## Notable differences

This list highlights differences to the standard [`{$mode FPC}`](<Mode_FPC.md> "Mode FPC") compiler compatibility mode. 

  * `{$modeSwitch ISOProgramParas+}`: The [`program`](<Program.md> "Program") parameter list becomes significant. If not enabled via the `‑MISO` command-line switch, you will need to place `{$mode ISO}` _before_ the `program` header.
  * `{$modeSwitch ISOIO+}`: Files have associated “buffer variables” and [`get`](<Get.md> "Get") and [`put`](</index.php?title=Put&action=edit&redlink=1> "Put \(page does not exist\)") can operate on them. This functionality is not present in other modes.
  * `{$modeSwitch ISOUnaryMinus+}`: A unary [minus](<Minus.md> "Minus") has the same [operator precedence](<Operator.md> "Operator") as other addition operators. Usually _all_ unary operators have the highest precedence.
  * `{$modeSwitch ISOMod+}`: The [`mod` operator](<Mod.md> "Mod") yields a positive result (Euclidean-like definition).
  * `{$modeSwitch nonLocalGoto+}`: You can [`goto`](<Goto.md> "Goto") labels anywhere in the program.
  * `{$modeSwitch nestedProcVars+}`: Procedural variables may assume addresses of nested [routine](<Routine.md> "Routine") definitions.
  * Support for routine parameters and `{$modeSwitch classicProcVars+}` (i. e. no special syntax requirements to differentiate between routine activation and designation):
        
        {$mode ISO}
        program routineParameter(output);
        
        procedure fancyPrint(function f: integer);
        begin
        	writeLn('❧ ', f:1, ' ☙')
        end;
        
        function getRandom: integer;
        begin
        	{ chosen by fair dice roll: guaranteed to be random }
        	getRandom := 4
        end;
        
        begin
        	fancyPrint(getRandom);
        end.
        




## Status

  * As of version 3.2.0 the FPC does not yet support conformant-array parameters, thus level 1 requirements are not met, [FPC issue 38632](<https://gitlab.com/freepascal.org/fpc/source/-/issues/38632>).
  * It is not possible to mix block-comment delimiters. `{ }` and `(* *)` cannot be mixed although the standard specifically states so.



## Notes

  * The mode’s intention is to _at least_ compile an ISO-compliant `program` [source code](<Source_code.md> "Source code") file. The FPC will be able to compile a _superset_ of programs. 
    * For instance, ISO standard 7185 defines a _fixed order_ of sections, [`const`](<Const.md> "Const") → [`type`](<Type.md> "Type") → [`var`](<Var.md> "Var"), but the FPC accepts _any_ order, cf. [FPC issue 37739](<https://gitlab.com/freepascal.org/fpc/source/-/issues/37739>).
    * Delphi-style [`// end-of-line comments`](<Comments.md> "Comments") are accepted.
    * Or the mere fact that procedural _variables_ are allowed should be surprising enough.



    It is quite possible that a _different_ compiler rejects the same source code on grounds of non-compliance. Unlike the [GNU Pascal](<GNU_Pascal.md> "GNU Pascal") Compiler there is no way to disable such “extensions”.

  * The value of the constant [`maxInt`](<maxint.md> "maxint") is not necessarily identical to `high(ALUSInt)`. The permissible range of values of the [`integer`](<Integer.md> "Integer") data type still depends on the mode.

---

_Source: [https://wiki.freepascal.org/Mode_iso](https://web.archive.org/web/20250121223242/https://wiki.freepascal.org/Mode_iso)_
