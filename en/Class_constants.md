# Class constants

FP supports typed class constants, if the compiler-switch `{$static on}` is set. There are no untyped class constants. 
    
    
    type
    	TCars = class(TVehicles)
    		private
    		public
    			wheelcount: integer; static;
    	end;
    
    begin
    	TCars.wheelcount := 4;
    	// further assignments are forbidden
    end.
    

## Weblinks

  * [“Static fields”](<https://www.freepascal.org/docs-html/ref/refse30.html>) in FPC-doc

---

_Source: [https://wiki.freepascal.org/Class_constants](https://web.archive.org/web/20241207104219/https://wiki.freepascal.org/Class_constants)_
