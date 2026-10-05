# Samples and permutations

Functions for samples and permutations deliver randomly ordered vectors of data. Random samples may be used for very diverse applications, including simulating rolls of a die, generating anonymised IDs (e.g. for clinical trials), Monte Carlo simulations and wide-range statistical applications. 

The following function delivers a vector of **longint** values, which are randomly ordered and range from **min** to **max** : 
    
    
    type
      TLongintvector_data = array of longint;   
    
    function sample(min, max: longint; replace: boolean): TLongintvector_data;
    var
      i, j, num1, num2: longint;
      Vector1, Vector2: TLongintvector_data;
    begin
      randomize;                        // initialise random number generator
      num1 := 1 + max - min;            // length of vector to be generated
      SetLength(Vector1, num1);         // vector for original (ordered) data
      SetLength(Vector2, num1);         // vector with permuted data
      if num1 > 0 then
      begin
        for i := 0 to num1 - 1 do
        begin                           
          Vector1[i] := min + i;        // initialise original vector
        end;
        num2 := num1;
        for i := 1 to num1 do
        begin
          j := random(num2);            // select random element of Vector1
          Vector2[i - 1] := Vector1[j]; // and assign to next element of Vector2
          if not replace then
          begin
            Delete(Vector1, j, 1);      // remove randomly selected element
            Dec(num2);                  // and adapt size
          end;
        end;
      end;
      result := Vector2;
    end;
    

Depending on the value of the parameter **replace** , the random elements of the vector are provided with or without replacement. With replacement, one number may occur several times. Without replacement all elements of the vector will contain unique numbers. 

## See also

  * [Ranking vectors of data](<Ranking_vectors_of_data.md> "Ranking vectors of data")
  * [Dev random](<Dev_random.md> "Dev random")
  * [Functions for descriptive statistics](<Functions_for_descriptive_statistics.md> "Functions for descriptive statistics")
  * [Generating Random Numbers](<Generating_Random_Numbers.md> "Generating Random Numbers")
  * [Marsaglia's pseudo random number generators](<Marsaglia's_pseudo_random_number_generators.md> "Marsaglia's pseudo random number generators")
  * [A simple implementation of the Mersenne twister](<A_simple_implementation_of_the_Mersenne_twister.md> "A simple implementation of the Mersenne twister")
  * [Delphi compatible LCG Random](<Delphi_compatible_LCG_Random.md> "Delphi compatible LCG Random")
  * [EverettRandom](<EverettRandom.md> "EverettRandom")

---

_Source: [https://wiki.freepascal.org/Samples_and_permutations](https://web.archive.org/web/20241213020217/https://wiki.freepascal.org/Samples_and_permutations)_
