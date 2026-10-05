# TTICheckListBox

For editing set of enumerated types. Let's assume that we have following definitions. 
    
    
       type
          TMonth = (Jan, Feb, Mar, Apr, May, Jun, Jul, Aug, Sep, Oct, Nov, Dec);
          TMonths = set of TMonth;
          //
          TForm1 = class(TForm)
          private
             FMonths: TMonths;
          published
             property Months: TMonths read FMonths write FMonths;
          end;
    

Properties of TTICheckListBox.Link can be assigned as following way. 
    
    
          with TICheckListBox1.LInk do begin
              TIObject:= Form1;
              TIPropertyName := 'Months';
          end;
    

Assigning TTICheckListBox.Link.AliasValues will change displayed string of items. So, if defined in following way, 
    
    
          with TICheckListBox1.LInk do begin
              with AliasValues do begin
                 Clear;
                 Add('Jan=January');
                 Add('Feb=February');
                 Add('Mar=March');
                 Add('Apr=April');
                 Add('Nov=November');
                 Add('Dec=December');
              end;
              TIObject:= Form1;
              TIPropertyName := 'Months';
          end;
    

The checklist items will be displayed in following way and order: 
    
    
        January
        February
        March
        April
        November
        December 
        May
        Jun
        Jul
        Aug
        Sep
        Oct
        
    

The "checked" months will be correctly stored in the FMonths field of Form1. That is, if you ckeck November, then 
    
    
          Nov in FMonths
    

will return true.

---

_Source: [https://wiki.freepascal.org/TTICheckListBox](https://web.archive.org/web/20240101000000/https://wiki.freepascal.org/TTICheckListBox)_
