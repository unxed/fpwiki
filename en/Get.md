# Get

│ **English (en)** │ 

The [`procedure`](<Procedure.md> "Procedure") **`get`** retrieves a datum from a [`file of …`](<typed_files.md> "typed files") and advances the reading cursor. Although `get` is a standardized [Pascal](<Standard_Pascal.md> "Standard Pascal") routine, in the [FPC](<FPC.md> "FPC") `get` is only available in an ISO-compliant mode, such as [`{$mode ISO}`](<Mode_iso.md> "Mode iso"). 

## Contents

  * 1 behavior
    * 1.1 requirements
    * 1.2 effect
  * 2 application
  * 3 see also



## behavior

### requirements

Before `get` is executed, two requirements must be met: 

  1. The file must be in _inspection_ mode.
  2. The file’s “cursor” may not be past [`EOF`](<EOLN_and_EOF.md> "EOLN and EOF").



A [run-time error](<runtime_error.md> "runtime error") occurs if either condition is not satisfied. 

### effect

`Get`

  1. advances the cursor by one record
  2. makes the new current record accessible via the file’s buffer variable.



To put this into relation: 

  * `read(f, x)` is equivalent to `x := f^; get(f)`.
  * `readLn(f)` is equivalent to `while not EOLn(f) do get(f); get(f)`. The latter `get(f)` actually consumes the newline marker.
  * `reset(f)` is semantically equivalent to `seekRead(f, pred(firstRecord)); get(f)` where `firstRecord` is the index of the first record.



## application

`Get` is especially used to 

  * read the file buffer variable multiple times at various locations (e. g. subroutines),
  * eliminate the need for an _extra_ buffer variable.
        
        program echo(input, output);
        begin
        	while not EOF(input) do
        	begin
        		output^ := input^;
        		get(input);
        		put(output);
        	end;
        end.
        

  * bypass [`read`/`readLn`](<Read.md> "Read")’s interpretation/conversion capabilities and make nasty [type casts](<Typecast.md> "Typecast").



## see also

  * [`put`](</index.php?title=Put&action=edit&redlink=1> "Put \(page does not exist\)")

---

_Source: [https://wiki.freepascal.org/Get](https://web.archive.org/web/20250515125754/https://wiki.freepascal.org/Get)_
