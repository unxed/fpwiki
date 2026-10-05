# symmetric difference

><

The symmetric difference operator is applicable to [`set`](<Set.md> "Set") variables. By mathematical definition, `A >< B` is [math]\displaystyle{ \left( A \setminus B \right) \cup \left( B \setminus A \right) }[/math]. 
    
    
    procedure test_differ;
    var
      a: set of char = ['a', 'b', 'c'];
      b: set of char = ['b', 'c', 'x', 'y'];
      c: set of char;
    begin
      c:= a >< b; // c becomes ['a', 'x', 'y']
    end;
    

The symmetric difference operator is defined by [Extended Pascal](<Extended_Pascal.md> "Extended Pascal"), ISO standard 10206. In the [FPC](<FPC.md> "FPC") it is available in _any_ mode, not just [`{$mode extendedPascal}`](<Mode_extendedpascal.md> "Mode extendedpascal"). 

  


navigation bar: topic: Pascal symbols  single characters |  [`+` (plus)](<Plus.md> "Plus") • [`-` (minus)](<Minus.md> "Minus") • [`*` (asterisk)](<_.md> "*") • [`/` (slash)](<Slash.md> "Slash")   
[`=` (equal)](<Equal.md> "Equal") • [`>` (greater than)](<Greater_than.md> "Greater than") • [`<` (less than)](<Less_than.md> "Less than")   
[`.` (period)](<period.md> "period") • [`:` (colon)](<Colon.md> "Colon") • [`;` (semi colon)](</;> ";")   
[`^` (hat)](<^.md> "^") • [`@` (at)](<@.md> "@")   
[`$` (dollar sign)](<Dollar_sign.md> "Dollar sign") • [`&` (ampersand)](<&.md> "&") • [`#` (hash)](</index.php?title=Hash&action=edit&redlink=1> "Hash \(page does not exist\)")   
[`'` (single quote)](<'.md> "'")  
---|---  
character pairs |  [`<>` (not equal)](<Not_equal.md> "Not equal") • [`<=` (less than or equal)](<Less_than_or_equal.md> "Less than or equal") • [`:=` (becomes)](<Becomes.md> "Becomes") • [`>=` (greater than or equal)](<Greater_than_or_equal.md> "Greater than or equal") • `><` (symmetric difference) • [`//` (double slash)](<Slash.md> "Slash")  
  
  
****

---

_Source: [https://wiki.freepascal.org/symmetric_difference](https://web.archive.org/web/20230320125316/https://wiki.freepascal.org/symmetric_difference)_
