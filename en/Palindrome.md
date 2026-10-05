# Palindrome

[![fpc source logo.png](https://wiki.freepascal.org/images/e/e1/fpc_source_logo.png)](</File:fpc_source_logo.png>)

│ **English (en)** │

## Contents

  * 1 Palindrome
  * 2 Examples
    * 2.1 English
    * 2.2 Arabic
    * 2.3 Armenian
    * 2.4 Finnish
    * 2.5 Russian
  * 3 Function IsPalindome



# Palindrome

A palindrome is a word, phrase, number, or other sequence of symbols or elements, whose meaning may be interpreted the same way in either forward or reverse direction 

# Examples

## English

  * racecar
  * A man, a plan, a canal: Panama
  * Madam I'm Adam



## Arabic

  * مودته تدوم لكل هول * وهل كل مودته تدوم



## Armenian

  * Արա իմ, առած դրամ՝ քաղաքեքաղաք մարդ ծառա մի արա
  * Սերամա՜հ եթե համարես



## Finnish

  * saippuakivikauppias
  * No niin, on aamu. Kehua sinua kai saan, ah, ihana Kristian. Nait Sirkan, ah, ihana asia, kaunis. Au, hekumaa. No niin on.
  * No, oikotie vei Tokioon.



## Russian

  * А роза упала на лапу Азора
  * луну дунул
  * лак резала зеркал



# Function IsPalindome

This function is suitable for both [ASCII](<ASCII.md> "ASCII") and [UTF-8](<UTF-8.md> "UTF-8") encoding. 
    
    
    function IsPalindome(const a_text: string): boolean;
    var
      i, j       :longint;
      s1, s2, s3 :string;
    begin
      s1 := UTF8LowerCase( a_text );
      j := length( s1 );
      i := 1;
      s2 := '';
      s3 := '';
      while ( i <= j ) do
        begin
          if s1[i] < #$C0 then  // ASCII
            begin
              if (s1[i] >= #$30) and (s1[i] < #$7B)then
                begin
                  if (s1[i] > #$60) or (s1[i] < #$3A) then
                    begin
                      s2 := s1[i] + s2;
                      s3 := s3 + s1[i];
                    end;
                  end;
                inc( i );
              end
            else
            begin
              begin
                if s1[i] < #$E0 then  // two byte
                  begin
                    if ((s1[i] > #$C2) and ( not
                      // Armenian punctuation
                      ((s1[i] = #$D5) and ((s1[i+1] >= #$99) and (s1[i+1] < #$A0)))
                      )) then
                      begin
                        s2 := s1[i]+ s1[i+1] + s2;
                        s3 := s3 + s1[i] + s1[i+1];
                      end;
                    inc( i ,2);
                  end
                else
                  begin
                    if s1[i] < #$F0 then  // three byte
                      begin
                        if not (
                          // General punctuation
                          (s1[i] = #$E2) and ((s1[i+1]= #$80)
                          or ((s1[i+1]= #$81) and (s1[i+2]< #$B0)))
                          ) then
                          begin
                            s2 := s1[i]+ s1[i+1] + s1[i+2]+ s2;
                            s3 := s3 + s1[i] + s1[i+1] + s1[i+2];
                          end;
                        inc( i ,3);
                      end
                    else
                      begin
                        s2 := s1[i]+ s1[i+1] + s1[i+2]+ s1[i+3] + s2;
                        s3 := s3 + s1[i] + s1[i+1] + s1[i+2] + s1[i+3];
                        inc( i ,4);
                      end
                  end;
              end;
            end;
        end;
      if s2 <> '' then result := s2=s3 else result := false;
    end;

---

_Source: [https://wiki.freepascal.org/Palindrome](https://web.archive.org/web/20241207111432/https://wiki.freepascal.org/Palindrome)_
