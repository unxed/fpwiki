# Levenshtein distance

│ **English (en)** │    
****

The Levenshtein distance is a string metric for measuring the difference between two sequences. Informally, the Levenshtein distance between two words is the minimum number of single-character edits (insertion, deletion, substitution) required to change one word into the other. 

This works on both [ASCII](<ASCII.md> "ASCII") and [UTF-8](<UTF-8.md> "UTF-8") encoding. This implementations use two-dimensional [dynamic array](<Dynamic_array.md> "Dynamic array") to store the distances of prefixes of the words compared. 

  

    
    
    uses
      Classes, SysUtils, Math, LazUTF8;
    
    function LevenshteinDistance(const s1 : string; s2 : string) : integer;
    function LevenshteinDistanceText(const s1, s2: string): Integer;
    
    implementation
    
    {------------------------------------------------------------------------------
      Name:    LevenshteinDistance
      Params: s1, s2 - UTF8 encoded strings
      Returns: Minimum number of single-character edits.
      Compare 2 UTF8 encoded strings, case sensitive.
    ------------------------------------------------------------------------------}
    
    function LevenshteinDistance(const s1 : string; s2 : string) : integer;
    var
      length1, length2, i, j ,
      value1, value2, value3 : integer;
      matrix : array of array of integer;
    begin
      length1 := UTF8Length( s1 );
      length2 := UTF8Length( s2 );
      SetLength (matrix, length1 + 1, length2 + 1);
      for i := 0 to length1 do matrix [i, 0] := i;
      for j := 0 to length2 do matrix [0, j] := j;
      for i := 1 to length1 do
        for j := 1 to length2 do
          begin
            if UTF8Copy( s1, i, 1) = UTF8Copy( s2, j, 1 )
              then matrix[i,j] := matrix[i-1,j-1]
              else  begin
                value1 := matrix [i-1, j] + 1;
                value2 := matrix [i, j-1] + 1;
                value3 := matrix[i-1, j-1] + 1;
                matrix [i, j] := min( value1, min( value2, value3 ));
              end;
          end;
      result := matrix [length1, length2];
    end;
    
    {------------------------------------------------------------------------------
      Name:    LevenshteinDistanceText
      Params: s1, s2 - UTF8 encoded strings
      Returns: Minimum number of single-character edits.
      Compare 2 UTF8 encoded strings, case insensitive.
    ------------------------------------------------------------------------------}
    function LevenshteinDistanceText(const s1, s2: string): integer;
    var
      s1lower, s2lower: string;
    begin
      s1lower := UTF8LowerCase( s1 );
      s2lower := UTF8LowerCase( s2 );
      result := LevenshteinDistance( s1lower, s2lower );
    end;
    
    end.

---

_Source: [https://wiki.freepascal.org/Levenshtein_distance](https://web.archive.org/web/20250401004023/https://wiki.freepascal.org/Levenshtein_distance)_
