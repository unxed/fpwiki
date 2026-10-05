# Ranking vectors of data

**Ranking** is an important method in statistics and data science. It is a preparative step that replaces each value by a rank in the order of magnitude of the original data. This operation is necessary for many non-parametric methods, e.g. the U test by Wilcoxon-Mann-Whitney or Spearman's rank correlation. 

## Contents

  * 1 Simple algorithm not caring for ties
  * 2 Advanced algorithm addressing potential ties
  * 3 See also
  * 4 References



## Simple algorithm not caring for ties

In the simplest case, where every value is distinct without identical data, the following frugal algorithm may be sufficient: 
    
    
    function rank(x: array of longint): array of real;
    var
      i, j, n: integer;
      lessCount: integer; { number of observations less than a given observation }
    begin
      n := length(x);
      SetLength(result, n);
      for i := 0 to n - 1 do
      begin
        lessCount := 0;
        for j := 0 to n - 1 do
          if x[j] < x[i] then
            inc(lessCount);
          result[i] := lessCount + 1;
      end;
    end;
    

This function delivers a list of ranks that replace the original data. It doesn't deliver satisfactory results if the data contain identical values, e.g. in the vector 3, 4, 4, 9, 0, 1. In statistical terminology, repeated observation-values are referred to as _ties_. The potential presence of ties requires a more complex algorithm: 

## Advanced algorithm addressing potential ties
    
    
    type
      TTiesMethod = (ascend, descend, average, min, max);
      
    function rank(x: array of longint; 
      TiesMethod: TTiesMethod = average): array of real;
    var
      i, j, n: integer;
      equalCount, lessCount: array of integer;
    begin
      n := length(x);
      SetLength(lessCount, n);
      SetLength(equalCount, n);
      SetLength(Result, n);
      for i := 0 to n - 1 do
      begin
        equalCount[i] := 0;
        lessCount[i] := 0;
        for j := 0 to n - 1 do
          case TiesMethod of
            average, min, max:
              if x[j] = x[i] then
                Inc(equalCount[i])
              else if x[j] < x[i] then
                Inc(lessCount[i]);
            ascend:
              if (j <= i) and (x[j] = x[i]) then
                Inc(equalCount[i])
              else if x[j] < x[i] then
                Inc(lessCount[i]);
            descend:
              if (j >= i) and (x[j] = x[i]) then
                Inc(equalCount[i])
              else if x[j] < x[i] then
                Inc(lessCount[i]);
          end;
      end;
      for i := 0 to n - 1 do
        case TiesMethod of
          average:
            Result[i] := lessCount[i] + (equalCount[i] + 1) / 2;
          min:
            Result[i] := lessCount[i] + 1;
          max, ascend, descend:
            Result[i] := lessCount[i] + equalCount[i];
        end;
    end;
    

This function delivers a vector of ranks that is corrected for ties. Similar to the function _rank_ of the statistical programming language S (e.g. in the environment R) it supports different methods to address potential ties. They include _average_ (default method, the rank for each occurrence of a repeated number is given as _rank_[_i_] = _lessCount_ \+ (_equalCount_ \+ 1) / 2), _ascend_ (permutation with increasing values at each index set of ties), _descend_ (decreasing values), _min_ (replaces values with ties with their minimum) and _max_ (replacement by maximum). 

## See also

  * [Samples and permutations](<Samples_and_permutations.md> "Samples and permutations")
  * [Dev random](<Dev_random.md> "Dev random")
  * [Functions for descriptive statistics](<Functions_for_descriptive_statistics.md> "Functions for descriptive statistics")
  * [Generating Random Numbers](<Generating_Random_Numbers.md> "Generating Random Numbers")
  * [Marsaglia's pseudo random number generators](<Marsaglia's_pseudo_random_number_generators.md> "Marsaglia's pseudo random number generators")
  * [A simple implementation of the Mersenne twister](<A_simple_implementation_of_the_Mersenne_twister.md> "A simple implementation of the Mersenne twister")
  * [Delphi compatible LCG Random](<Delphi_compatible_LCG_Random.md> "Delphi compatible LCG Random")
  * [EverettRandom](<EverettRandom.md> "EverettRandom")



## References

  1. Cooke D, Craven AH, Clarke GM. Statistical Computing in Pascal. Edward Arnold (Publishers), London 1985
  2. Becker, R. A., Chambers, J. M. and Wilks, A. R. (1988) The New S Language. Wadsworth & Brooks/Cole.
  3. R Documentation: [Sample Ranks](<https://stat.ethz.ch/R-manual/R-patched/library/base/html/rank.html>).

---

_Source: [https://wiki.freepascal.org/Ranking_vectors_of_data](https://web.archive.org/web/20241206084605/https://wiki.freepascal.org/Ranking_vectors_of_data)_
