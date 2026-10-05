# Polynomial evaluation

│ **English (en)** │  **[français (fr)](</Polynomial_evaluation/fr> "Polynomial evaluation/fr")** │    
****

# Introduction

It is often useful to evaluate a polynomial at a given point. The method shown below allows to calculate it by using only multiplication and addition, without any power. 

# Horner's method

Only 1-degree polynomial are evaluated, e.g. 3.x^2 + 2.x + 1, can be written, by means of an astute factorization, ((3.x +2).x + 1). 
    
    
    program PolyTest;
    
    type 
      // coef's rank = it's degree
      TDegres = 0..10; 
      TPolynome = record 
        Deg: TDegres; 
        Coefs: array[TDegres] of Double; 
      end;
    
    function PolyEval(const aPoly: TPolynome; X: Double): Double; 
    var 
      i: TDegres; 
    begin 
      with aPoly do 
      begin 
        Result := 0; 
        for i := Deg downto Low(Coefs) do 
          Result := Result * X + Coefs[i]; 
      end; 
    End; 
      
    (* Exemples *) 
    var 
      P: TPolynome; 
    begin 
      P.Deg := 2; 
      P.Coefs[2] := 3; 
      P.Coefs[1] := 2; 
      P.Coefs[0] := 1;   
      WriteLn('P(',7,') = ', PolyEval(7):10:2);  
      
      P.Deg := 0; 
      P.Coefs[2] := 0; 
      P.Coefs[1] := 0; 
      P.Coefs[0] := 1;   
      WriteLn('P(',7,') = ', PolyEval(7):10:2);  
    End
    

# See also

[Horner's method (wiki)](<https://en.wikipedia.org/wiki/Horner%27s_method%7C>)

---

_Source: [https://wiki.freepascal.org/Polynomial_evaluation](https://web.archive.org/web/20230323212735/https://wiki.freepascal.org/Polynomial_evaluation)_
